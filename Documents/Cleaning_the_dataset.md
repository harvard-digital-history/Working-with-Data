## 1.4. Cleaning the Dataset

- **`Age`**: 
    - Go to record (from facet) and move `Portugal` to `Residence` column
    - Convert to number: *`Edit cells -> Common transforms -> To number`*
    - Change facet to numeric
- **`Voyage number`**:
    - Replace `AV` with blank string: *`Edit cells -> Replace…`*
    - Explain why converting to number is not appropriate.
- **`VesselNumber`**:
    - Replace `AS` with blank string: *`Edit cells -> Replace…`*
- **`Rig`**:
    - Clean spaces: *`Edit cells -> Common transforms -> Trim leading and trailing whitespace`*
    - Fix capitalization: *`Edit cells -> Common transforms -> To titlecase`*
    - Discuss clustering and fix abbreviations: *`Edit cells -> Cluster and edit…`*
        - Key collision/fingerprint:
            - `Bark Ship` / `Ship Bark` – same?, likely Schooner bark, although that's not on list
            - All to `Ship Bark`, then *`Merge selected and re-cluster`*
        - Key collision/cologne-phonetic:
            - `Ship Bark` / `Sbark` – these are not the same, Sbark == Steam Bark
            - All to `Ship Bark`, then *`Merge selected and re-cluster`*
        - Nearest neighbour/levenshtein:
            - Target is `Schr`/`Schooner`
            - Radius should be `4` and block chars `3` (overshoot to illustrate method)
            - Fix `Schr`/`Schooner` -> all to `Schooner`, then *`Merge selected and close`*
    - Direct edit fixes: *hover -> `edit`*:
        - `Sbark` -> `Steam Bark`
- **`ApproximateDeparture`**:
    - Convert to dates *`Edit cells -> Common transforms -> To date`*
        - Show problems: two-digit years assigned to 21st century -> **Undo!**
    - Let’s look at the **distribution of years**:
        - First, we split the date into its constituents:
            - *`Edit column -> Split into several columns`*:
                - *`By separator`* -> `/`
                - 3 columns at most
                - Check *`Do not remove this column`*

                    :information_source: | This won’t work for the dates formatted as `18XX-May-xx`, but at this point we don’t care: we just want to look at the years.
                    -|-
        - The two-digit years span a range from `01` to `27`, and we know that:
            > Records now span the years 1809 to 1927[^1].

            It seems likely that the two-digit years represent the dates in the 1900s. This assumption is reinforced by the fact that otherwise there are no dates in the 1900s at all.
            
            :warning: | This is ultimately an assumption, though, it could be verified by looking at how the consistency of the entire dataset is affected by it.
            -|-

    - We now fix the dates based on that assumption:
        - With RegEx [(expression explained here)](https://regex101.com/r/bxM4qW/1):
            - *`Edit cells -> Replace…`*: 
                ```regexp
                (\d{1,2})\/(\d{1,2})\/(\d{2})$ -> $1/$2/19$3
                ```
        - Without RegEx:
            - :left_speech_bubble: TBD
        - Next we convert the column to date format: *`Edit cells -> Common transforms -> To date`*
        - Create a timeline facet — notice `Non-time` category, these are the problem dates
        - Filter by that category - 281 of them
        - Create new text facet (it will be filtered by timeline facet)
        - Only 15 unique dates -> fix by hand
    - Alternative using [GREL](https://openrefine.org/docs/manual/grel):
        - *`Edit cells -> Transform...`*:
            ```js
            value.toDate('MM/dd/yy', 'yyyy-MMM', 'yyyy') .toString('dd/MM/yyyy')
            ```
        - *`Edit cells -> Replace...`*: 
            ```
            /20 -> /19
            ```
    - Convert the column to date format: *`Edit cells -> Common transforms -> To date`*

:warning: | The values/categories for `skin`, `hair`, and `eyes` are so mixed that it would make more sense to "bag" all three together and then reclassify based on a controlled vocab (likely derived from the entries themselves).
-|-

- **`Skin`**:
    - Remove brackets by replacing `(`, `)`, `[`, and `]` with blank strings: *`Edit cells -> Replace…`*
    - Remove other detritus, such as `-`, `?`, and `'` by replacing them with blank strings: *`Edit cells -> Replace…`*
    - Remove leading and trailing spaces: *`Edit cells -> Common transforms -> Trim leading and trailing whitespace`*
    - Create a text facet – then run *`cluster`*
        - Key collision/Fingerprint -> fix all
        - Key collision/Metaphone3 -> fix relevant matches
        - NN/Levenshtein | 3.0/5 ->
    - Manual fixes:
        - `BrownDark` -> `Brown Dark`
        - `of colour` -> `Colored`
        - `Mulatto or Indian` -> `Mulatto Indian`
        - `Yellow/Mullatto` -> `Yellow Mulatto`
        - `Tolerably fair` -> `Fair`
        - `B` and `bl` -> `Black`
        - `col` -> `Colored`
        - `sw` -> `Swarthy`
    - replace ` ` with `,` AND `,,` with `,`
    - re-order tokens with Python expression: *`Edit cells -> Transform...`*
        ```python
        if value and ',' in value:
            tokens = value.split(',')
            tokens.sort()
            return ','.join(tokens)
        else:
            return value
        ```

    - recluster;
        - Key collision/Fingerprint -> fix all
        - Key collision/Metaphone3 -> fix relevant matches
        - NN/Levenshtein | 6.0/3 ->
    - Eliminate: `Very`, `Unknown`, `Medium`, `man`, `Mac`
    - Rejoin with space separator: *`Edit cells -> Join multi-valued cells...`*

- **`Hair`**:

- **`Eye`**:

[^1]: https://www.whalingmuseum.org/online_exhibits/crewlist/about.php