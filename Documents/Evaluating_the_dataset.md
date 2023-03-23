## Evaluating the Dataset

We can use OpenRefine's [text facets](https://openrefine.org/docs/manual/facets#text-facet) to check each column for issues:

<details>
<summary><h4><code>LastName</code> and <code>FirstName</code></h4></summary>

- Extracted (poorly) from `FullName` – :left_speech_bubble: not worth the bother?

</details>
<details>
<summary><h4><code>Vessel</code></h4></summary>

- Shockingly good!

</details>
<details>
<summary><h4><code>Rig</code></h4></summary>

- Leading/trailing spaces
- Inconsistent capitalization
- Use of abbreviations

</details>
<details>
<summary><h4><code>ApproximateDeparture</code></h4></summary>

- Two-digit years vs. four-digit years
- Inconsistent formatting: 
    ```
    1840-Sep-00
    1821—00
    8/4/1868
    08/04/1868
    ```
- Should we assume US date format?

</details>
<details>
<summary><h4><code>FullName</code></h4></summary>

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

</details>
<details>
<summary><h4><code>Age</code></h4></summary>

- Misaligned column: `Portugal` (1 record)

</details>
<details>
<summary><h4><code>Height</code></h4></summary>

- Outlier values: 
    ```
    0’ 0”
    1’ 0 1/2"
    8’ 8"
    ```
- `#VALUE!`: why Excel is utter rubbish

</details>
<details>
<summary><h4><code>Skin</code></h4></summary>

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

</details>
<details>
<summary><h4><code>Hair</code></h4></summary>

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

</details>
<details>
<summary><h4><code>Eye</code></h4></summary>

- Very sparse column - look at blanks
- Leading/trailing spaces: `Blue` vs. `Blue_`
- Inconsistent capitalization: `Blue` vs. `blue`
- Use of qualifiers: `light`, `dark`
- Repetition of parent category: `blue eyes`
- `packet`?

</details>
<details>
<summary><h4><code>Residence</code></h4></summary>

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

</details>
<details>
<summary><h4><code>Rank</code></h4></summary>

- Uncertainty: `?`, `(-man)`
- Inconsistent formatting: `2 mate` vs. `2nd mate`
- Capitalization: `mate` vs. `Mate`
- Mixed categories: *e.g.* rank/role `watch-header`
- Multiple categories: `3rd mate, boatsteerer, carpenter`
- Inconsistent nomenclature: `Greenhand` vs. `green hand`
- `Jan-60`?

</details>
<details>
<summary><h4><code>Lay</code></h4></summary>

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

</details>
<details>
<summary><h4><code>Remarks</code></h4></summary>

- Often includes data that should be in other columns, such as eye colour, rank or speciality.

</details>
<details>
<summary><h4><code>Voyage number</code></h4></summary>

- Values prefixed with `AV`

</details>
<details>
<summary><h4><code>VesselNumber</code></h4></summary>

- Values prefixed with `AS`

</details>