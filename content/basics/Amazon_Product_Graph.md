
# Amazon Product Graph

## The need for retail knowlegde product graphs

- Knowledge Graphs have achieved success in several domains. However the domain of products has additional problematic aspects
  - sparsity and noise
  - complexity of the product types, attributes categories and growing amount products
- The proposed system Autoknow encounters these problems by using several techniques including:
  - taxonomy construction
  - product property identification
  - knowledge extraction
  - anomaly detection
  - synonym discovery

## Knowledge Graphs and the retail product domain
- Knowledge Graphs have a wide application in search engines, question answering and other domains
- Successful knowledge graphs have been created for domains like Music, Movies and Sport
  - Based on available rich and quality data structure
- The e-Business could be enhanced through the introduction of Knowledge Graphs in terms of product search, product recommendation or navigation
- However, this domain has its own challenges

## Challenges in the retail production domain
- C1-Structure-sparsity:
  - A large part of the used data is contributed by retailers → data is sparse and noisy
  - Structured knowledge has to be mined from textual product profiles 
- C2-Domain-complexity:
  - Designing an ontology is difficult
  - Millions of product types exist, which furthermore include relationships, sub-types, synonyms and overlapping types
  - Used attributes are complex and evolving
- C3-Product-type-variety:
  - The wide range of product types makes it difficult to train knowledge enrichment and cleaning models
  - Similar product types consist of different attributes

## How does AutoKnow combat these problems?
- Creating product type taxonomy
- Defining type specific product attributes
- Preprocessing data by → imputation of structured attributes, cleaning noisy values, identifying synonyms

- AutoKnow is:
  - automatic → human intervention is minimized
    - ML models
    - creating training data by using behavior logs and existing data
  - multi-scalable → scales towards many dimensions (domains, products, attributes)
    - extensible to new values and types
  - integrative → continuously gain information through customer behavior logs

# The Product Knowledge Graph
- Knowledge Graphs consist of triples (subject, predicate, object)
- Based on this, a tree-structured taxonomy can be build
  - Root → products
  - Nodes → product types
  - Edges → sub-type relationship
- Two sources of input:
  - Product catalog
  - Customer behavior logs
- Product Knowledge Graph Definition:
  - Graph G = (N1, N2, E)
    - N1: topic type → represent entities of one particular type
    - N2: entities → represent attribute values
    - E: edges → connect each entity with its attribute values.
  - Catalog C: (T_, A, P)
    - Taxonomy (T_): (T, H)
      - T: Types
      - H: Hypernyms
    - Attributes (A):  set of product attributes
    - Product (P): {P_ID, T, (A,V)}
      - P_ID: Product ID
      - (A,V): Attribute Value Pairs
      - Logs (L): customer logs behavior

 →  Product Knowledge Discovery: 
- Takes C (Catalog) and L (Customer logs)
- Enriches the product knowledge by adding new types and hypernym relationships to T (Taxonomy), 
and new product types and attribute values for each product in P (Product).


## AutoKnow: System Architecture
- AutoKnow consists of two function suites the ontology suite and data suite
1. The ontology suite
    - Taxonomy enrichment
    - Relation discovery
2. The data suite
    - Data imputation
    - Data cleaning
    - Synonym discovery

### Taxonomy Enrichment
- The basic problem: 
  - taxonomies of online catalogs are usually maintained by human experts
  - types and sub-types are rarely used in the same sentence 
- Key techniques:
  - Enrichment of the product taxonomy by
  - Type extraction → discovering new types through the product title or customer queries
  - Type attachment → adding extracted types to the taxonomy

## Relation Discovery
- The basic problem:
  - different attributes apply to different product types
  - some attributes have a higher impact on customer behavior than others (e.g.: brands)
- Key techniques:
  - Important attributes are mentioned more often by buyers and sellers
  - Using Classification for attribute applicability
  - Using Regression for attribute importance
  - Using Random Forest to analyze seller and buyer behaviors

## Enriching and Cleaning Knowledge
1. Data Imputation:
  - extracts new  (A, V) pairs for each product from its profile
  - There exist state-of-the art techniques  to address (T, V) pairs : 
    - sequential labeling combined with active learning
    - BilSTM-CRF learning:  uses word embedding -> fine-tuned via model training
    - ! does not scale up to address C3- Product-type-variety !
  - Adopt this approach to (A, V):
    - taxonomy aware sequence tagging
    - make prediction based on product type (T)
    - use pre-trained hyperbolic-space embedding for T
    - CondSelfAtt learning: conditional self attention layer
    - Multi-Task learning: training sequence tagging and product categorization 
   
->  Combination of CondSelfAtt and Multi-Task outperform Bilstim

2. Data Cleaning
  - indetifes (A,V) pairs that are incorrect
  - transformer based neural network:
      - The model is capable of learning from
      raw textual input without extensive feature engineering, making it
      ideal for scaling to thousands of types.
      - uses fasttext embedding
      - computes likelihood of input tripule (PID, A, V) to be correct
      - trained with distant supervision
          - automatically generate training labels from input catalog
          - generate positive example by high-frequency values from multiple brands
          1. build vocabulary for each Attribute A → replace randomly Values of A with vocab
          2. n-grams of product title without catalog value
          3. randomly pick values from another attribute and replace → label as incorrect
          
3. Synonym Discovery
  - includes spelling variants, acronyms or abbreviation and semantic equivalence
  - candidates: filtering of customer co-view behavior signals for product pairs
  - train simple logistic regression model
      - features:
          - edit distance
          - pre-trained MT-DNN model score
          - features regarding distinct vs. common
          words
              - words appearing only in the first candidate but not the second,
              - and vice versa
              - words shared by two candidates
              - for each: → edit distance and embedding similarity are
              computed and used as features.







