# RDF Schema

by Patrick Thomasius and Tobias Esser

## Recap: Resource Description Framework (RDF)

- description of structured Framework
- first publication in 1999 by W3C
	- focus on metadata
- Republication in 2004
	- basic display format for generic data
- developed for electronic networks
	- decentrally managed information
- XML focuses on hierarchical structures
	- html-files
- RDF has no hierarchical structure
	- directed Graph
		- can be cyclic
	- graphs can be combined
	- relations are stored in triplets
		- Nodes describe subjects and objects
		- Edges describe predicates
	- Uniform Resource Identifier (URI) used as Identifier
	
## Why RDF Schema (RDF(S))?

- allows integration of RDF by a large number of different agents
	- a common vocabulary to interpret RDF is needed
- RDF(S) provides a vocabulary for specific domains of applications

-> RDF(S) is used to formalize ontologies

## Vocabularies

- set of terms stored in a standard format
	- different agents are able to reuse 
- vocabulary consisting of property names has its own namespace
	- allows to be used with other datasets
	
-> RDF(S) provide a vocabulary to define new vocabularies of properties

## Structure of RFD(S)

- adds a semantic layer to the RDF-Triplets
- contains information about the class of the subject and its relation to sub or parent classes
- predicates can be described to the extent to which classes of subjects and objects they can be applied to
- labels, comments and other meta-data can be given on any RDF-triplet. 

Image RDF+RDF(S) Layers

The most important components of RDF(S) are <b>classes</b> and <b>properties</b>.

## Classes

RDF(S) classes are <i>rdfs:classes</i> themselves. Everything described by RDF is a resource, instance of <i>rdfs:Resource</i>. 
Subclasses are assigned by using <i>rdfs:SubClassOf</i>.
Other instances of <i>rdfs:Class</i> are:

- <i>rdfs:literal</i> -> class of literal values like strings or integers
- <i>rdfs:DataType</i> -> instance and subclass of <i>rdfs:Class</i>, used to specify datatypes, subclass of <i>rdfs:literal</i>
- <i>rdfs:property</i> -> class to define RDF property

## Properties

Properties describe a relation between subject and object resources. They also support inheritance with <i>rdfs:subPropertyOf</i>. 
<i>rdfs:type</i> -> describe which class a resource is an instance of. 
<i>rdfs:range</i> -> instance of <i>rdfs:property</i> that states that a property refers to an object of a certain class
<i>rdfs:domain</i> -> instance of <i>rdfs:property</i> that an instance that has this property will be of a certain class

Example:
The property "makesMusicWith" that is being used as a predicate will have "musician" as domain and "instrument" as range.

<i>rdfs:label</i> and <i>rdfs:comment</i> are very often used to describe the meaning of other resources. The rdfs:range of both the label and comment is an rdfs:literal, so textual strings can be used.
There is further vocabulary in RDFS, such as Collections, Containers and Utility Properties, but these are usually specific to certain use cases and comparatively sparsely used.
## Application of RDF(S)
RDF(S) is an extension of RDF and cannot be used without RDF.
It works similar to the type systems of object-oriented programming languages.
Properties in terms of to what other classes they may apply can be described with RDF(S), e.g. by using the range and domain mechanisms.
The most common use case are knowledge graphs. RDF and RDF(S) are very efficient at merging data from multiple sources.
RDF-based knowledge graphs help when Search intentions have an implicit information need.