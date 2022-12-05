# Sparsity and Noise: Where Knowledge Graph Embeddings Fall Short

``` 
The presentation notes for the discussion about the Paper 'Sparsity and Noise: Where Knowledge Graph Embeddings Fall Short' yb Jay Pujara, Eriq Augustine & Lise Getoor on the 28.11.2022
```

## 1. INTRODUCTION
	- KG as essential component for question answering, decision support, enable exploration and discovery
	- Initial effort: KG creation from structured information sourced or heavy manual curation
	- However: Diversity of knowledge available on the Web:
		○ require automatically constructing KGs from data of varying degrees of quality ---> difficult
	- Must overcome:
		○ complex, unreliable, incomplete data
	- Proposed: Machine Learning Methods to adress cleaning and completing KGs
		○ Example from external papers: Translate entities and relationships into a latent subspace
		○ Use this representation to derive additional, unobserved facts and score existing facts

	- Embedding methods:
		○ have shown state-of-the art results on several benchmark data sets
	- But: Benchmark Data Set differ from data in real KGs
	- Benchmark data set:
		1. restricted to the most frequently occuring entities in the KG  ->  dense
		2. consist of only highly reliable facts from curated knowledge bases -> accurate
	- Real KGs:
		1. entities are associated with a sparse set of observations -> sparse
		2. consists of knowledge extracted from unreliable, noise data such as text or images -> noisy, unreliable

	- In this paper:
		○ evalaute popular KG embedding approaches on KGs with
			§ sparse entities
			§ unreliabe candidate facts
		○ Apply embedding methods to an extracted KG
		○ modify existing benchmarks by varying the sparsity and reliability of training data used to learn embedding models
	- In order to:
		○ Where embedding approaches are successful
		○ and when they result in degrading results
	- Outcome:
		○ recommendations for improving embedding models and identify areas of future exploration

## 2. BACKGROUND AND RELATED WORK
	- Knowledge base construction includes:
		○ manually-crafted ontologies for common-sense reasoning
		○ community-driven collaborative efforts
		○ ontology-based extraction from structured and textual sources
		○ 'open' approaches relying on textual information
	- In this paper:
		○ contrast properties of two knowledge graphs with clean human-vetted facts
		○ compare with: 
		○ two KG extracted from textual data
	- Semantically meaningful embeddings of text
	- KG
		○ capture structured relationships between entities
		○ inspired:
			§ matrix factorization,
			§ tensor factorization
			§ deep learning
		○ that embed entities while preserving this relationship structure
	- We consider four state-of-the-art embedding methods
	- and assess their performance  on KG with different properties

## 3.1 COMPARING PROPERTIES OF KG


	- Introduce three KG and a parallel set of benchmark datasets derived from these KGs
		○ contains triplets for specifying relationship between: subject and object
	- Freebase and Wordnet KG: 
		○ human curation
		○ precisely defined entities and relationships
		○ highly reliable facts
	- NELL KG
		○ extracted from large Web text corpus
		○ iterative co-training process
		○ pre-defined set of relations and types
		○ dynamic dataset
		○ in Table 1: 1000th iteration
	- FB15K and WN18
		○ derived from Freebase and Wordnet KG
		○ used for training and evaluating embedding strategies
	- NELL156
		○ earlier iteration of NELL
		○ benchmark for probalistic models
	- Compare vital statistics of these six data sets

### 3.1 SIZE AND SAMPLING
	- Freebase
		○ largest KG with more Facts (T)
		○ unqiue entities (E)
		○ Relationships (R)
	- Wordnet: focused on NLP
		○ smallest KG
		○ only 27 Relationships between words
	- Derived benchmark datasets:
		○ substantially smaller than source KGs
		○ Nell165 containing 1M facts/Triplets
		○ FB15k 
			§ sample subset of KG around 15K entities
		○ WN18
			§ restricted to 18 relationships
### 3.2 DIVERSITY
	- To: understand distribution of entities and relationships in KG
	- Introduce: entropy-based measure
		○ probability an entity or relation will occur in a randomly selected Triple
		
	- For triples T of the form (s,p,o)
	- Relations R
	- Entities E
	- We define the entity and relations probabilities as the probability that:
		○ a randomly selected triplle will contain a particular relation or entity
	- Formally:
	- 
	- Compute: entity entropy (EE) and relationship entropy (RE) for each dataset
	- Higher entropy values: -> more uniform distribution of facts across entities and relations
	- lower values: -> biases in facts


	- Example:
		○ low RE values for Freebase KG and NELL165 due to
		○ abundance of facts specifying entitiy types such as person
		○ relative to other relation between entities
		○ --> Freebae has most facts and entities, but less diverse compared to other KGs
	- FB15k rebelances Freebase through sampling
		○ increasing diversity in E and R
	- Wordnet and WN18:
		○ similar diversity statistics
	- Nell165 & Nell1000
		○ NELL165 more diverse E, less diverse R
	- All KG: much higher EE than RE
		○ since they use manually defined set of relations but many diverse entities

### 3.3 SPARSITY

	- KG have differing levels of factual information for each entity/relation
	- Sparsity metric: Information Density
		○ average triples per entity or relation

	- Compare datasets with: Entity Density (ED) and Relational Density (RD)


	- Most datasets have similar ED
	- FB15K much higher ED
	- NELL165 much lower ED
	- NELL1000 highest RD
		○ since extractions are focused on small set of relations
	- FB15K low RD value
		○ due to entity-centric construction
		○ higher ED and lower RD than freebase

### 3.4 Reliability
	- Embedding approaches rely on facts that are reliable
	- Human-curated KG generally: high precision due to strong oversight
	- Extracted KG: noise, erroneous relationships between E
		○ often evaluated on small, manually labeled evaluations to estimate precision
	- Mitchel et. al 2015
	- NELL
		○ 0.75-0.85 precision for confident extractions
		○ 0.35-0.45 precision for broader set of extractions

## 4 EMPIRICAL EVALUATION
	- To understand embedding performance with sparse and unreliable data
	- Select four popular embedding approaches:
		○ TransE (Bordes et al., 2013)
		○ TransH (Wang et al., 2014)
		○ HolE ( Nickel et al., 2016)
		○ STransE (Nguyen et al., 2016)
	- increasingly sophisticated learning methods to represent entities and relations
	- To learn embeddings:
		○ public eimplementations of 
		Lin et al. (2015);
		Nickel et al. (2016b); Nguyen et al. (2016)

	- Conduct: 4 experiments
	1. Evaluate performance of embeddings on extracted NELL165 KG
	2. Modify existing FB15K to isolate the impact of sparsity on embedding quality
	3. Decrease reliability of FB15k, determine how performance degrades
	4. Explore Tradeoff between sparsity and reliability
		- by: beginning with sparse training set
		- incrementally adding unreliable triples at differing noise levels

	- AUPRC: Precision Recall Curve
	- F1 Score: Model's accuracy on a dataset -> positive/negative

### 4.1 EXTRACTED KNOWLEDGE GRAPHS
	- NELL165: Sparse and noisy
		- fewer facts per relation/entity than FB15k
		- lower precision
	- Try out Embedding Techniques on NELL165

	- Evaluate on 4.5K manually labeled facts
		- reporting AUPRC and F1 Score
	- Compare :against baseline
		- that simply applies treshhold to NELL extractor confidence with no scoring on novel facts
		- simply selects top extractions
	- Compare: NELL promotion strategy
	- Compare: PSL-KGI
		- probalistic approach
		- reasons collectively about KG facts using ontological constraints
		- supports open-world reasioning
	- Result:
		- embedding approaches cant cope with sparse and low-quality extractions
		- perform more poorly than baseline approaches and probabilistic models
		- Next: analyze whether poor performance due to sparsity or sensitivy to noise

### 4.2 SENSITIVY TO SPARSITY
	- Impact of Sparsity
	- Test:
	- Remove triples from FB15K using two different techniques
		1. sparse: removes triples uniformly at random
			- constraint: no elimination of entity or relation from data set
		2. stable: removes all triples for a particular relation
			- leave other relations intact
			- size doesnt exceed change of 2% between techniques

	- Fig.1
	
	- Shows filtered hits@10
		○ Proportion of correct triples in top ten triples, excluding training data
		○ for both sparse and stable
		○ using TransE, TransH, HolE and STransE Embeddings
	- Performance universally decreases as training set diminishes
		○ and triples are removes
	- However: in sparse treatment
		○ seen as the dotted lines in Figure 1
		○ performance deteriorates more rapidly than in stable
	- Also:
		○ Complex representations such as TransH and HolE suffer more from sparsity
		○ TransE and STransE have better performance
	- Ultimately:
		○ when half triples have been randomly removed
		○ to a RD value of 220
		○ stable outperforms sparse by as much as 60%
	- Contrast between 
		○ dense set of facts for each relation (stable)
		○ and sparse set of relational training data (sparse)
		○ means: embedding quality relies on dense training data !!!

### 4.3 Sensitivity to Unreliability
	- Sensity of embedding techniques to nodes
	- Modify:
	FB15K data set to include unreliable triples with different approaches:
		1. corrupt: 
			i. corrupt triples
			ii. substitutes replacement entity/relation for the true subject/predicate/orbject
			iii. train embedding approach with a corrupted version of the benchmark- 
	
	- In Figure 2:
	- Hits@10 metric suffers as more facts are either:
		○ corrupted (corrupt)
		○ removed (sparse)
	- Across all methods:
		○ providing incorrect training data negatively impacts performenca more (corrupt)
		○ as seen in the lines with long dashes
		○ than removing data (sparse)
	- deficit between sparse an corrupt is relatively stable across the board

### 4.4 TRADING OFF SPARSITY AND NOISE
	- Constructing Kg requires tradeoff between sparsity and noise
		○ sparse, high-quality set of extractions may be insufficient to learn meaningful embeddings
		○ incorporationg additional, unreliable facts may be even worse/questionable
	- Explore tradeoff
		1. By: randomly removing 300K triples from FB15K
		2. incrementally adding unreliable triples at differing noise levels
			i. noise level: probability of newly added triple being corrupted
		3. generate training sets for each noise level and size
		4. train TransE ( the best performing embedding technique)
		5. compute filtered Hits@10 metric on new test set
	
	- Fig. 3
	- All Embeddings have big initial benefit from added triples
	- noise level strongly effect the growth of improvement per triples added
	- Low Noise Level: 
		○ As seen in thick blue line
		○ performance climbs steadily
	- High noise Level:
		○ thin blue line
		○ performance stagnates after initial benefit
		○ at 90% noise level, small net improvement over no noisy triples added
		○ possible suggestion: large, unreliable corpus may be better than an extremely sparse, high-quality one

## 5. CONCLUSION
	- Analyzed several KG
	- discuss key metrics for diversity, sparsity and unreliability
	- Experimental evaluation concludes:
		○ KG embeddings are sensitive to sparse and unreliable data
		○ perform poorly on KGs extracted from text
	- Findings suggest
		○ new strategies to extend embeddings to cope with sparse and unreliable data
	- Promising Approaches
		○ Revising closed-world assumption frequently used in training embeddings
		○ combine embeddings and collective probabilistic models that perform well on extracted KGs
		○ devise an optimization approach for embeddings that exploits confidence from knowledge extraction systems

## 6. References
https://aclanthology.org/D17-1184/