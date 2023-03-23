## 1. Understanding the Dataset

Our dataset for **Day 1** will be the [Whaling Crew List Database](https://www.whalingmuseum.org/online_exhibits/crewlist/). This is a list of crew members leaving the port of New Bedford on whaling voyages between the years 1840 and 1927. It was compiled by the [New Bedford Whaling Museum](https://www.whalingmuseum.org) based on records kept by the chaplains of the New Bedford Port Society. It includes names and physical descriptions alongside information about vessels and specific voyages.

### Discussion:
- Provenance and production context
- Data model and data types
- Completeness and consistency of data
- Transformations needed to make dataset amenable to analysis

#### Notes regarding columns:

Column | Description
-------|------------
| Rig  | Configuration of the rigging (system of ropes, cables, and chains + masts and sails), can be proxy for size and capabilities of vessel.
|Lay  |  For a given voyage, each man receives a lay or percentage of the profits, instead of wages. The size of the lay depends upon his rank and speciality: the captain always earns the largest share, usually around 1/8th, while a green hand (inexperienced crewman) earns the least, often as little as 1/350th. 
Rank | Includes both officers and crew, in descending order: Captain, officers (usually three or four mates, each commanding a whaleboat), boatsteerers (these are the harpooneers), specialists (blacksmith, carpenter, cook, cooper–i.e. caskmaker–, and steward), foremast hands (ordinary crewmen).

For more, see [Column Definitions and Code Tables](https://whalinghistory.org/av/voyages/columns/)