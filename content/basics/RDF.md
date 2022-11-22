# RDF - the ressource description framework

##

![RDF Introduction] (images/RDF_03.png)

RDF is a Framework for a combination of Resources and Descriptions. This framework is used to store, publish and logically interlink metadata. Based on the idea of making statements about resources in expressions of triples. The entities codify a statement about semantic data. Published in 1999 it was designed to represent information in a minimally constraining and flexible way. The outcome is a graph based model which interconnects all kinds of bits and pieces that can be defined.  


![RDF Data Model] (images/RDF_04.png)
Unlike relational databases where a tuple is allocated to an attribute, RDF adds another dimension to stored metadata.
It is undoubtable that the multifactoric linkage of metadata creates a whole new web of connections. 
RDF adds a SPARQL to the classical SQL based databases.

Still there are some limitations when it comes to the seek time. As the more information is combined in within a graph and the more the graphs are connected, the more complex is the output. 

![RDF Data Model] (images/RDF_05.png)
Thinking about interlinked resources it becomes clear, that not only a specific field can be connected in within itself, but also the different fields can be interlinked with one another.

In the picture a combination of two sets “fields of resources” can be observed: Bob, who is a person, born on the 14th of July 1990 who is a friend of Alice is interested in the Mona Lisa. What connects Bobs interests to Leondardo Da Vince. Just like that a spiderweb of interconnected resources starts to be visible. Even tho it wasnt as ambigious as it seems now.

Putting metadata into directed perspective makes this framework as powerful as it is. 

![RDF Triple] (images/RDF_06.png)
So how are the resources interlinked?

Every piece of information expressed in RDF is represented as a triple. 

Subject – a resource which is identified with an URI/IRI
Predicate – represents the nature of the relationship
Object – a resource or literal to which the subject is related.

URIs are global identifiers, so other people can re-use this URI to identify the same thing. Uniform Resource Identifiers can appear in all three positions of a triple.


Literals:
Basic values that are not URIs. Examples are dates such as “the 4th of july” and the number Pi and so on. Literals may only apperar in the object position of a triple.
The Literals can be PLAIN ones, where a string is combined with an optional language tag. 
And the string can be combined with a datatype URI, what is then called a typed Literal. 

![RDF Vocabularies] (images/RDF_07.png)

A Class is a construct that represents things in the real and/or
information world, e.g. a person, an organisation, a concepts such as
“health” or “freedom”.

Relationships are the link between two classes; for the link between a
document and the organisation that published it (i.e. organisation
publishes document), or the link between a map and the geographic
region it depicts (i.e. map depicts geographic region). In RDF, relationships are encoded as object type properties.

Property. A characteristic of a class in a particular dimension such as
the legal name of an organisation or the date and time that an
observation was made.

![RDF Vocabularies] (images/RDF_08.png)
There are many different vocabularies to be found. 

The ones referred to here are the most popular ones so far.

Vocabularies get their value from reuse: the more vocabulary IRIs are reused by others, the more valuable it becomes to use the IRIs. This is called the network effect.
Meaning you should prefer re-using someone else's IRI instead of inventing a new one.

 
![RDF Syntax] (images/RDF_09.png)
As there are various ways to define resource, there are various syntax available to make the entities queriable. 

- RDF/XML is the only standartized Syntax by the W3C so far
- Turtle will be standartized sers like N-Triple, RDFa, SACL, R2RML are also availabe and here pointed at for completion.

![Query Language] (images/RDF_10.png)
Query Language SPARQL
