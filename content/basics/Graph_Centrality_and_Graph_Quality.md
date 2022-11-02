# Graphs, Centrality and Graph Quality

Authors: *Mathis Loevenich*, *Nicolas Rehbach*

## On Graphs

Terminology Definition (finite Graphs):
- consists of Vertices (V), Edges (E)
- can be weighted 
- directed or undirected
- can contain loops
- acyclic or cyclic
- connected or disconnected

    Example Graph G(V,E):
- V = {V1, V2, V3, V4}
- E = {E1, E2, E3, E4}

![alt text](img/graph03.png)

With walks you relate how to get from one vertice to another

![alt text](img/walks.png)

From [`5`](https://www2.math.utah.edu/mathcircle/notes/MC_Graph_Theory.pdf) [p.3]
 


## On Graph centrality

In the paper "A Graph-theoretic perspective on centrality" [`1`], Stephen P. Borgatti Martin G. Everett present a review of centrality measurement methods.

- Centrality is a core concept used for graph and network analysis
- By analyzing centrality key aspects of the graph can be understood like:
    - Network performance
    - Error occurance
    - Network efficiency

### Key terms used in Graph centrality
- **Walk** $\rightarrow$ a sequence of nodes and edges when moving from one node to another
- **Trail** $\rightarrow$ a walk without repeating any edges. However, nodes can be visited several times
- **Path** $\rightarrow$ describes a walk without repeating any nodes
- **Geodesic** $\rightarrow$ the shortest path between two nodes

### Measurements for centrality
- There are three best-known measures of centrality: degree, closeness and betweenness. While there are more measurements, these will be described in detail in the following paragraph.

#### Degree-like measures
- These measures use the number/amount of walks that join the nodes to eachother
    - Also called volume measures   
- According to Freeman[[`2`]](https://www.bebr.ufl.edu/sites/default/files/Centrality%20in%20Social%20Networks.pdf) degree centrality is the number of other nodes, a node is adjacent to.
- Mathematically, it can be stated as followed:
$
c_i^{DEG} = \sum_j a_{ij}
$
- Simply put: the degree-like measurements take into account how many adjacent nodes a node does have. The node with the highest amount of adjacent nodes has the highest degree centrality.
- The node with the highest amount of adjacent nodes will have the biggest reach inside the graph.


#### Closeness-like measures
- These measures use the length of the walks the nodes are involves in
    - Also called length measures
- Freeman[[`2`]](https://www.bebr.ufl.edu/sites/default/files/Centrality%20in%20Social%20Networks.pdf) defines closeness centrality as the sum of all geodesic distances from a given node to all other nodes
- Mathematically, it can be stated as followed:
$
c_i^{CLO} = \sum_j d_{ij}
$
- The closeness centrality is the mean distance of a node to other nodes.
- In closeness centrality larger values are refering to less centrality or importance inside the graph. Therefore, the closeness measures decentrality, rather than centrality.
- Simply put: the closeness-like measurements point out the importance of a node inside a graph. The smaller the closeness centrality, the closer a node is to all other nodes.

#### Betweenness-like measures
- These measures use the number of walks that pass through a node
    - Also called medial measures
- Betweenness-centrality is the sum of walks, any node need another node to reach a desired node.
- Mathematically, it can be stated as followed:
$
c_i^{BET} = \sum_i \sum_j \frac{d_{ikj}}{g_{ij}}
$
- Simply put: betweenness-like measurements analyze how many times a node occurs in the geodesic path of two nodes. Therefore, nodes with a high betweenness are important for the graph flow. It is worth analyzing betweenness-like measures to understand the importance of certain nodes and counter errors or performance issues when deleting nodes.

#### Other centrality measures used by Neo4j [`4`](https://neo4j.com/developer/graph-data-science/centrality-graph-algorithms/)
- Harmonic Centrality
- Eigenvector Centrality
- PageRank
- ArticleRank

#### Adoption of Centrality [`4`](https://neo4j.com/developer/graph-data-science/centrality-graph-algorithms/)

**Degree Centrality**
- Determine the most important node inside a social network. 
    - Can be used for marketing a product
    - Reach the most people in a social network by using few nodes

**Closeness Centrality**
- Determine the nodes with the most efficient information throughput
    - Organizational networks, people with high closeness centrality are in control of the organization (e.g.: Terrorist Networks)
    - Infection spreading
    - Importance of words inside a document
    
**Betweeness Centrality**
- Determine the node with the highest impact on information flow
    - Package delivery and telecommunication networks
    - Determine influencial nodes to disrupt networks

## On Graph Quality

Issa, Subhi, et al. present a literature review on the term completeness/quality of a graph in their paper Knowledge Graph Completeness: A Systematic Literature Review [`3`].
The authors present a review of the state of Graph completeness measurements before presenting seven aspects to ensure graph complenteness based on 56 articles.

- The fitness for use of a graph is defined by the graph's quality
    - Graph quality includes: accuracy, timeliness, consistency, correctness, completeness, etc. 
- The term completeness is a data quality measure.
    - How much information is presented in a given dataset?
    
    
## Graph Quality

    - quality measures
        - accuracy
        - completeness
        - timeliness
        - provenance
        - accessibility
        - consistency
        - [term quality (fitness for use)]
            - encompasses accuracy, timeliness, consistency, correctness,
            completeness, etc.
    - quality issues
        - with RDF at the schema or instance level
            - leaks of completeness and provenance
    - completeness
        - can be tackeled during data collection or within the integration
        - one of most essential dimension in data quality
        - measure that refers to the
        amount of information present in a particular dataset
        - missing data completeness → inaccurate analysis
        - affects other dimensions (accuracy, timeliness and consistency)

### Presented types of completeness
**Schema completeness**
   
   - Are all required properties of an entity given?
    
**Property completeness**
   
   - Are missing values for an instance's property validated? For example, several places of residence over time
   
**Population completeness**
    
   - Does the instance reflect the real-world entity correctly?
    
**Interlinking completeness**
   
   - Are instances analogous presented in other datasets or do they represent diffent instances?
    
**Currency completeness**
    
   - How did the property values of an instance evolve? For example, does the dataset contain the several places of residence over time?
    
**Metadata completeness**
    
   - Is adequate metadata for the dataset given? For example, the author or last modifier? 
    
**Labelling completeness**
    
   - Are all labels human and machine readable?

# References:


[`1`](https://www.sciencedirect.com/science/article/pii/S0378873305000833) Borgatti, S. P., & Everett, M. G. (2006). A graph-theoretic perspective on centrality. Social networks, 28(4), 466-484.

[`2`](https://www.bebr.ufl.edu/sites/default/files/Centrality%20in%20Social%20Networks.pdf) Freeman, L. C. (1978). Centrality in social networks conceptual clarification. Social networks, 1(3), 215-239.

[`3`](https://ieeexplore.ieee.org/abstract/document/9344615) Issa, S., Adekunle, O., Hamdi, F., Cherfi, S. S. S., Dumontier, M., & Zaveri, A. (2021). Knowledge graph completeness: A systematic literature review. IEEE Access, 9, 31322-31339.

[`4`](https://neo4j.com/developer/graph-data-science/centrality-graph-algorithms/) Neo4j (2012). Neo4j - The World’s Leading Graph Database, Documentation (Last visited: 02.11.22)

[`5`](https://www2.math.utah.edu/mathcircle/notes/MC_Graph_Theory.pdf) APA 
(2006). Allen Dickson - Introduction to Graph Theory (Last visited: 02.11.22)
