# Using OpenRefine with the *Whaling Crew List*

## 1. Understanding the Dataset
- Whaling Crew List Database from the New Bedford Whaling Museum[^1]
- Importance of context, provenance, production
- Data model and categories
- Notes regarding columns:

    Column | Description
    -------|------------
    | Rig  | Configuration of the rigging (system of ropes, cables, and chains + masts and sails), can be proxy for size and capabilities of vessel.
    |Lay  |  For a given voyage, each man receives a lay or percentage of the profits, instead of wages. The size of the lay depends upon his rank and speciality: the captain always earns the largest share, usually around 1/8th, while a green hand (inexperienced crewman) earns the least, often as little as 1/350th. 
    Rank | Includes both officers and crew, in descending order: Captain, officers (usually three or four mates, each commanding a whaleboat), boatsteerers (these are the harpooneers), specialists (blacksmith, carpenter, cook, cooper–i.e. caskmaker–, and steward), foremast hands (ordinary crewmen).

## 2. OpenRefine
- Excel that doesn’t suck
- can manipulate much bigger datasets
- non-destructive: [OpenRefine](https://openrefine.org) project stores not just data, but history of changes
- well [documented](https://openrefine.org/docs)
- extensive support for import and export formats
- besides advanced manipulation tools it supports basic but very useful visualization (and filtering) through facets

## 3. Initial Exploration/Assessment
- show how quick look can help you see gaps in data and patterns - <mark>better in Excel?</mark>
- Use text facets to check columns for consistency:
    - **`LastName`** and **`FirstName`**:
        - Extracted (poorly) from `FullName` – <mark>not worth the bother?</mark>
    - **`Vessel`**:
        - Shockingly good!
    - **`Rig`**:
        - Leading/trailing spaces
        - Inconsistent capitalization
        - Use of abbreviations
    - **`ApproximateDeparture`**:
        - Two-digit years vs. four-digit years
        - Inconsistent formatting: 
            ```
            1840-Sep-00
            1821—00
            8/4/1868
            08/04/1868
            ```
        - Should we assume US date format?
    - **`FullName`**:
        - Uncertainty inconsistently marked – prob. transcriber?
            ```
            ?, Surname
            [?] [?]
            Abner Hov?eland
            ```
        - Inconsistent formatting:
            ```
            (Surname), Name
            [Surname], Name
            Name Surname
            [Name] Surname
            ```
        - Use of abbreviations: e.g. `Wm.` == `William`
        - Initialisms, spacing, capitalization: `A. H. Leonard`, `A.h. Leonard`
        - Other: `Abraham 2d Cook`
        - Blanks - there are 70
    - **`Age`**: 
        - Misaligned column: `Portugal` (1 record)
    - **`Height`**: 
        - Outlier values: 
            ```
            0’ 0”
            1’ 0 1/2"
            8’ 8"
            ```
        - `#VALUE!`: why Excel is utter rubbish
    - **`Skin`**:
        - Formatting inconsistencies: *e.g.* use of `()`
        - Blanks/uncertainty: 
            ```
            (-)
            -
            ??
            ?
            ```
        - Leading/trailing spaces: `black` vs. `black_`
        - Inconsistent capitalization: `Black` vs. `black`
        - Use of abbreviations: 
            ```
            B
            bl
            Black
            Black man
            blk
            ```
        - Use of multiple categories: `Colored (Black)`
        - Spelling: `coloured` vs. `colored`
        - `Tolerably fair`!?
    - **`Hair`**:
        - Formatting inconsistencies: `()` vs. `[]`
        - Uncertainty: 
            ```
            [--]
            [-------]
            ?
            ```
        - Use of multiple categories: `Black & Curly`
        - Inconsistent nomenclature: `curly` vs. `Curled`, `blond`/`blonde` vs. `Yellow`
        - Use of abbreviations: `Blk` vs. `Bl`
        - Inconsistent capitalization: `curly` vs. `Curly`
        - Spelling: `curly` vs. `curley`, `wooly` vs. `woolly`
        - Use of qualifiers: `little curley` or `light brown`
        - Misaligned columns: `blue` – unlikely!
        - Aggregated data: `dark hair and eyes`
    - **`Eye`**:
        - Very sparse column - look at blanks
        - Leading/trailing spaces: `Blue` vs. `Blue_`
        - Inconsistent capitalization: `Blue` vs. `blue`
        - Use of qualifiers: `light`, `dark`
        - Repetition of parent category: `blue eyes`
        - `packet`?
    - **`Residence`**:
        - Uncertainty: `?`, `CT`
        - Inconsistent formatting: `Lansingburgh Ny` vs. `Lansingburgh, Ny`
        - Inconsistent data structure: 
            ```Athens
            Athens, GA
            Athens, Greece
            Greece
            Columbia, South America
            Green, Shenango County, NY
            ```
        - Inconsistent capitalization: `NY` vs. `Ny` vs. `ny`
        - Inconsistent spelling: 
            ```
            Ascention Island
            Ascencion Island
            Assension Island
            ```
        - Normalization: `Arabia/New Bedford`
        - Typos? `Astonia, NY` -> Astoria?
    - **`Rank`**:
        - Uncertainty: `?`, `(-man)`
        - Inconsistent formatting: `2 mate` vs. `2nd mate`
        - Capitalization: `mate` vs. `Mate`
        - Mixed categories: *e.g.* rank/role `watch-header`
        - Multiple categories: `3rd mate, boatsteerer, carpenter`
        - Inconsistent nomenclature: `Greenhand` vs. `green hand`
        - `Jan-60`?
    - **`Lay`**:
        - Inconsistent formatting: 
            ```
            1-110
            1/110
            20/M
            .25c/M
            ```
        - Typos: `1-1190` – likely 1-119
        - OCR detritus: 
            ```python
            1-12O # should be 1-120
            1-2OO # should be 1-200
            ```
        - Notes: `+ 1/4 of slush` or `+ $25 advance`
        - Unclear data: 
            ```
            12-Jan
            AGREE
            Jan-32
            Jan-33
            JOHN
            ```
    - **`Remarks`**:
        - Often includes data that should be in other columns, such as eye colour, rank or speciality.
    - **`Voyage number`**: 
        - Values prefixed with `AV`
    - **`VesselNumber`**: 
        - Values prefixed with `AS`

## 4. Data Cleaning
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
            - `Bark Ship` / `Ship Bark` – same, likely Schooner bark
            - All to `Ship Bark`, then *`Merge selected and re-cluster`*
        - Key collision/cologne-phonetic:
            - `Ship Bark` / `Sbark` – same
            - All to `Ship Bark`, then *`Merge selected and re-cluster`*
        - Nearest neighbour/levenshtein:
            - Target is `Schr`/`Schooner`
            - Radius should be `4` and block chars `3` (overshoot to illustrate method)
        - Fix `Schr`/`Schooner` -> all to `Schooner`, then *`Merge selected and close`*
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
            > Records now span the years 1809 to 1927[^2].

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
            - <mark>TBD</mark>
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
    - **`Skin`** / **`Hair`** / **`Eye`**:
        - Create a text facet – then run *`cluster`*
        - *`Merge and recluster`* as necesary

[^1]: https://www.whalingmuseum.org/online_exhibits/crewlist/
[^2]: https://www.whalingmuseum.org/online_exhibits/crewlist/about.php