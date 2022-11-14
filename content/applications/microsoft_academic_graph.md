# Microsoft Academic Graph (MAG)

## Problem:
- Research is complex
- Advanced search operators are rarely used due to their arcane syntax
- New publications every day
- Not processable by humans anymore

`(Sinha et al., 2015)`

---
## Solution:
#### Microsoft academic:
> - Research more, search less
> - Helping researchers stay on top of their game
> - (Microsoft, 2021b)

- Use computers power (memory, computation, sensing, attention and endurance) to assist humans in research
- Process crawled documents from Bing
- Search engine for semantic retrieval of academic knowledge:
    - Academic Search
    - Recommendation

`(Microsoft, 2021b)`

### Microsoft Academic Graph

* Microsoft Academic Knowledge Graph (MAKG) `(Sinha et al., 2015)`
* Heterogeneous graph 
* Established: June 5, 2015
* Contains:
    * scientific publication records
    * citation relationships publications
    * authors
    * institutions
    * journals
    * conferences 
    * fields of study. 
* freely available
* API Interface
---
### Current state:
> *"No longer providing updated data or access to old releases after Dec. 31, 2021; however, existing copies can still be used under license."*
> *`Microsoft (2020)`*

>The discontinuation *"shows the fragility of infrastructures that depend on the goodwill of companies for which creating and maintaining these infrastructures does not represent a core activity, and does not bring in significant revenues"*
> *`Ludo Waltman, deputy director of the Centre for Science and Technology Studies at Leiden University as cited in Chawla (2021)`*


&rarr; major implications for all those who have relied on Microsoft Academic as a data source
&rarr; not a good idea to rely on the availability of a proprietary data source

`(Hauschke, 2021)`

---

<?xml version="1.0" encoding="utf-8"?>
<svg viewBox="150 150 200 200" preserveAspectRatio="xMidYMid slice" xmlns="http://www.w3.org/2000/svg">
  <g transform="matrix(0.532287, 0, 0, 0.532287, 112.534081, 88.245583)">
    <ellipse style="fill: rgb(80, 167, 221);" cx="246.736" cy="243.868" rx="125" ry="125"/>
    <ellipse style="fill: rgb(255, 194, 108); fill-opacity: 0.85;" cx="164.933" cy="242.511" rx="75" ry="75"/>
    <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 20px; white-space: pre;" x="97.535" y="253.761">Scopus</text>
    <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 20px; white-space: pre;" x="256.072" y="239.69" transform="matrix(1, 0, 0, 1, 0, 0)">Microsoft <tspan x="256.0719909667969" dy="1em">​</tspan>Academic <tspan x="256.0719909667969" dy="1em">​</tspan></text>
    <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 20px; white-space: pre;" transform="matrix(1, 0, 0, 1, -73.187881, 18.374723)"><tspan x="186.59" y="394.553">Scopus: 27.0M</tspan><tspan x="186.58999633789062" dy="1em">​</tspan><tspan>Microsoft Academic: 73.3M </tspan><tspan x="186.58999633789062" dy="1em">​</tspan><tspan>Overlap: 22.0M</tspan></text>
  </g>
</svg>

Comparison of scientific documents covered from the period 2008–2017.
Own visualization based on `Visser et al. (2021)`.
### MAKG.ORG
#### Key facts from 2020-05-29:
- **Publications**: *238,670,900*
- **Authors**: *151,355,324*
- **Affiliations**: *25,767*
- **References**: *1,635,169,990*
- **Citations with citation contexts**: *234,337,833*
- **Fields of study**: *740,460*
- **ORCID IDs**: *34,863*
- **Conference instances**: *16,142*
- **Conference series**: *4,468*
- **Journals**: *48,942*
- **Paper abstracts**: *139,227,097*
- **Paper tags**: *677,389,638*
- **Linked datasets and code**: *75,091*


https://makg.org
`(Ghidini et al., 2019)`

---
## Creating the Knowledge Graph:

### Data Acquisition
- Sources:
    - Publisher feeds (ACM, IEEE...) (high quality)
    - Bing web crawler
- Processing pipeline to clean `titles` and `author` names
- Merge papers with identical name and venue but different sources 
    - Papers may be published in different sources (ArXiv, ResearchGate, Personal Website...)
    - Combine information to fill in gaps
    - Create a more comprehensive information about the entity

`(Sinha et al., 2015)`

### Data Annotation
- Six central academic domain entities are discovered:
`Author`, `Title`, `Field of Study`, `Venue`, `Event`, `Institution`

#### `Author` and `Title`
- Aquired during the discovery process


#### `Field of Study` (FOS)
- Source:
    - Available in an in-house knowledge base of candidate FOS
    - Aquired from content, hyperlinks, web-clicks
- Majority not marked as FOS entity
- Label new entities by
    - assigning known labels
    - using keyword attributes from publications

#### `Institution`
- *Journal* and `Institution` mostly aggregated from the in-house knowledge base

#### `Venue`, `Event`
- Source:
    - semi-structured websites indexed by *Bing*
- Various signals to detect:
    - `event`: Recognize the *conference* instance, *journal* issue 
    - `venue`: Combine *conference* instances from different web-page
- Combine category attributes of conferences with `FOS` entities
- Add new academic conference instances and series to in-house knowledge base
- Combine with other (non-academic) entities (e.g. location, city or country)

`(Sinha et al., 2015)`


<?xml version="1.0" encoding="utf-8"?>
<svg viewBox="30 80 400 170" xmlns="http://www.w3.org/2000/svg" xmlns:bx="https://boxy-svg.com">
  <rect x="35.486" y="148.48" width="94.679" height="32.559" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);"/>
  <rect x="165.5" y="148.5" width="94.679" height="32.559" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);"/>
  <rect x="295.5" y="148.5" width="94.679" height="32.559" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);"/>
  <rect x="165.5" y="208.5" width="94.679" height="32.559" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);"/>
  <rect x="35.5" y="88.5" width="94.679" height="32.559" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);"/>
  <rect x="295.5" y="88.5" width="94.679" height="32.559" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);"/>
  <path style="stroke: rgb(0, 0, 0); fill: none;" d="M 216.387 115.403 C 227.693 115.403 234.761 127.231 229.107 136.692 C 223.453 146.155 209.32 146.155 203.666 136.692 C 202.166 134.181 201.498 131.287 201.751 128.397" transform="matrix(-1, 0, 0, -1, 432.795547, 259.192245)"/>
  <rect x="517.171875" y="246.14453125" width="1" height="1" rx="0" ry="0" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);"/>
  <rect x="584.21875" y="423.203125" width="1" height="1" rx="0" ry="0" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);"/>
  <path d="M 261.893 164.977 H 286.469 L 286.469 160.477 L 294.469 165.977 L 286.469 171.477 L 286.469 166.977 H 261.893 V 164.977 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(1, 0.00089, -0.00089, 1, 0.147858, -2.233074)" bx:shape="arrow 261.893 160.477 32.576 11 2 8 0 1@baf2660f"/>
  <path d="M -261.893 -155.977 H -237.317 L -237.317 -160.477 L -229.317 -154.977 L -237.317 -149.477 L -237.317 -153.977 H -261.893 V -155.977 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(-1, 0.00089, 0.00089, 1, -98.055435, 320.03331)" bx:shape="arrow -261.893 -160.477 32.576 11 2 8 0 1@dfabfc9d"/>
  <path d="M -173.326 -168.514 H -157.06 L -157.06 -173.375 L -151.767 -167.433 L -157.06 -161.491 L -157.06 -166.353 H -173.326 V -168.514 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(-0.00089, -1, -1, 0.00089, -85.854114, -27.065131)" bx:shape="arrow -173.326 -173.375 21.56 11.884 2.161 5.294 0 1@5a5a40e7"/>
  <path d="M 173.326 178.237 H 189.592 L 189.592 173.375 L 194.886 179.317 L 189.592 185.259 L 189.592 180.398 H 173.326 V 178.237 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(-0.00089, -1, -1, 0.00089, 525.890796, 318.231654)" bx:shape="arrow 173.326 173.375 21.56 11.884 2.161 5.294 0 1@2149401c"/>
  <path d="M -173.326 -168.514 H -157.06 L -157.06 -173.375 L -151.766 -167.433 L -157.06 -161.491 L -157.06 -166.353 H -173.326 V -168.514 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(-0.00089, 1, -1, -0.00089, 46.455669, 357.027344)" bx:shape="arrow -173.326 -173.375 21.56 11.884 2.161 5.294 0 1@da0cca24"/>
  <path d="M 350.013 184.26 H 435.225 L 435.225 179.76 L 443.225 185.26 L 435.225 190.76 L 435.225 186.26 H 350.013 V 184.26 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(-0.887782, 0.460263, -0.460263, -0.887782, 746.017517, 188.632172)" bx:shape="arrow 350.013 179.76 93.212 11 2 8 0 1@ee0c8ab8"/>
  <path d="M 231.252 -137.834 L 236.382 -131.643 L 226.121 -131.643 L 231.252 -137.834 Z" style="fill: rgb(216, 216, 216); stroke: rgb(0, 0, 0);" transform="matrix(1, 0, 0, -1, 0, 0)" bx:shape="triangle 226.121 -137.834 10.261 6.191 0.5 0 1@9e028955"/>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 12px; white-space: pre;" x="195.896" y="169.599">Paper</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 12px; white-space: pre;" x="173.892" y="229.807">Field of Study</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 12px; white-space: pre;" x="63.309" y="169.688">Author</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 12px; white-space: pre;" x="53.283" y="108.673">Institution</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 12px; white-space: pre;" x="303.485" y="103.516" transform="matrix(1, 0, 0, 1, 0, -1)">Events (Conf.<tspan x="303.4849853515625" dy="1em">​</tspan>Instances)</text>
  <text style="fill: rgb(51, 51, 51); font-family: Arial, sans-serif; font-size: 12px; white-space: pre;" x="299.176" y="159.403" transform="matrix(1, 0, 0, 1, 0, 3)">Venue (Journal<tspan x="299.1759948730469" dy="1em">​</tspan>/ Conf. Series)</text>
</svg>

*Own visualization based on `Sinha et al. (2015)`.*

---
## Applications:
### Recommendation Model
e.g.:
- Given a field of study, find:
&rarr; most prominent authors
&rarr; most influential papers
&rarr; potential publishing venues 
&rarr; upcoming events

- Given a field of study; find:
&rarr; other relevant fields of study

### Advanced academic search
- Highly constraint queries
- Suggestions related to the user query:
    - several fields of study
    - authors in a given field
- Can show papers in the intersection of two fields, e.g.: 
    - *Papers about <field of study 1> citing <field of study 2>* 

`(Sinha et al., 2015)`

---


## Availability (currently)
- SPARQL Endpoint
- URI Resolution (part of Linked Open Data Cloud)
- RDF Dumps (1.2 TB Total, slices available)
- Entity Embeddings (ComplEx and RDF2Vec)

`(Ghidini et al., 2019)`

---

## References
* Chawla, D. S. (2021, 15. Juni). Microsoft Academic Graph is being discontinued. What’s next? Nature Index. https://www.nature.com/nature-index/news-blog/microsoft-academic-graph-discontinued-whats-next
* Ghidini, C., Hartig, O., Maleshkova, M., Svátek, V., Cruz, I., Hogan, A., Song, J., Lefrançois, M. & Gandon, F. (Hrsg.). (2019). The Semantic Web – ISWC 2019. Lecture Notes in Computer Science. https://doi.org/10.1007/978-3-030-30796-7
* Hauschke, C. (2021, September 14). Microsoft Academic – eine bedeutende Datenquelle versiegt. TIB-Blog. https://blogs.tib.eu/wp/tib/2021/05/07/microsoft-academic-eine-bedeutende-datenquelle-versiegt/
* Microsoft. (2020, 20. November). Next Steps for Microsoft Academic - Expanding into New Horizons. Microsoft Research. https://www.microsoft.com/en-us/research/project/academic/articles/microsoft-academic-to-expand-horizons-with-community-driven-approach/
* Microsoft. (2021a, 4. Mai). Microsoft Academic Graph. Microsoft Research. https://www.microsoft.com/en-us/research/project/microsoft-academic-graph/
* Microsoft. (2021b, 10. Juni). Microsoft Academic. Microsoft Research. https://www.microsoft.com/en-us/research/project/academic/
* Sinha, A., Shen, Z., Song, Y., Ma, H., Eide, D., Hsu, B.-J. (Paul), & Wang, K. (2015). An Overview of Microsoft Academic Service (MAS) and Applications. Proceedings of the 24th International Conference on World Wide Web, 243–246. https://doi.org/10.1145/2740908.2742839
* Visser, M., van Eck, N. J. & Waltman, L. (2021). Large-scale comparison of bibliographic data sources: Scopus, Web of Science, Dimensions, Crossref, and Microsoft Academic. Quantitative Science Studies, 2(1), 20–41. https://doi.org/10.1162/qss_a_00112


