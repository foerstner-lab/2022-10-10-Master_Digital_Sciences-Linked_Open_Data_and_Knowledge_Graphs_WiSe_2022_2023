# Cypher: An Evolving Query Language for Property Graphs 

## What is Cypher
- Cypher is a declarative query language specialized for property graphs
- It was created as part of Neo4j, however commercial database products and researchers picked it up
- Nowadays, the openCypher project is mainly working on Cyphers further development
- The authors discuss the need for an standardized property graph query language
- Property graph databases have been widely adopted in the last decade in industry and academia
- The usage of graph databases have many benefits including:
  - support for modeling graph data
  - native indexing and storage
  - graph traversal operations
  - built-in graph algorithms (PageRank)
- Cypher began as a Neo4j product, it is now a well-established language for working with graph databases.
- The openCypher project was founded in 2015 with the aim to consolidate cypher as a standardized language for property graph querying such as SQL for relational data querying.
- Currently no standard language for property graph querying exists
- While Cypher does not have a standard for its language yet, it has a formalized semantics of its core construct

## The Cypher language
- The core idea of Cypher are linear queries
  - Input: property graphs, Output: tables
  - In contrast to SQL, the query does not start with a SELECT statement but ends with a RETURN
- Pattern matching
  - One of the core concepts written in “ASCII art” e.g.: Node A - [relation] -> Node B
Data modification
  - Similar to SQL, updating the graph is enabled by a update language including (Create, Delete, Set(Update), Merge)
  - Pragmatic, to aid the transition between Cypher and SQL

## Formal specification
- A language which vocabulary is defined through mathematic  
- A formal specification has many advantages compared to natural language
  - prove optimizations, reference implementation and less ambiguity
- Cypher has two key components
  - A data model consisting of values, graphs and tables
  - A query language consisting of expression, patterns, clauses and queries

## Data Model
- Values consist of: Identifiers, Base types (whole numbers), true, false and null, lists, maps, paths
- Property Graphs consist of:
  - Nodes
  - Relationships
  - Functions (e.g.: mapping source nodes to relationships, node ids to labels)
  - Tables (a bag or multiset of records including a set of name)

## Patterns and pattern matching
- Nodes are a triple consisting of: optional name, a finite set of labels, a map from keys to expression
  - e.g.: A is of type human with the attributes name and age
  - (A:human:{name: expression1, age: expression2}
- Relationships are a tuple consisting of:
  - the direction
  - optional name
  - set of relationship types
  - map from keys to expressions
  - nil or (m,n)
- Complexity:
  - only allowing traverses that visit a relationship once
  - By doing so, the Cypher semantic stays NP-complete

## Formal syntax and semantics
- Semantic expressions e.g.: values and variables, maps, lists, strings, logic
- Semantic of queries
  - a sequence of commands ending with return 
  - a union of two queries
- Semantics for clauses
  - Clauses take tables to tables
  - Matching clauses are statement for pattern matching using the keywords: MATCH, UNWIND, WITH

## Historical remarks	
- Cypher emerged from the Neo4j graph database which was founded in 2000
  - Initially, a schema-rigid relational database was used in Neo4j. 
  - By emphasising on relationship information a native property graph database system was created
- Since 2007, Neo4j is a open-source database management system
- In 2015 the openCypher project was announced, which is a open platform to enhance the standardization of Cypher

## Current development
- The openCypher group works on Cypher 10 currently, which will include three new features
- Multiple graphs
  - by using graph references, one can access external graphs or graphs created by queries
  - Already used in SPARQL 1.1
- Query composition
  - addition of single tables including multiple graphs as argument
  - named queries to create query libraries
- Temporal types

## Related Work
- Several graph query languages exist
- recursive queries in SQL offer graph navigation
  - however using complex graphs become difficult
- Regular path queries use regular expressions to describe paths between two nodes.
- SPARQL, supports regular path queries and is the standard for RDF data
- PGQL by Oracle
- Gremlin, a property graph dataflow language

## Future work
- Path patterns
- Configurable morphisms
- Standardized schema model
- Support time evolving data
- Theoretical vs real-life complexity
  - Worst case rarely happen in practice
  - Analyzing real-life query complexity in comparison to theoretical complexity

## Conclusion
- Cypher sees increasing adoption
- Evolving with new features
- openCypher Implementers Group works on different architectures, storage and query optimization
  - End goal: Establishing Cypher to be complementary to SQL
  - By doing so, offering integrated use of graph and relational data models

## Sources:
Francis, Nadime, et al. "Cypher: An evolving query language for property graphs." Proceedings of the 2018 International Conference on Management of Data. 2018.





