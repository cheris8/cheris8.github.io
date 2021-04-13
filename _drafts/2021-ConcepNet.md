


## 1. Introduction

ConceptNet : a knowledge graph that connects words and phrases of natural language (terms) with labeled, weighted edges (assertions)
ConceptNet represents relation between words
represent assertions as triples of their start node, relation label, and end node
the assertion that “a dog has a tail” can be represented as (dog, HasA, tail)
ConceptNet represents links between knowledge resources
In addition to its own knowledge about the English term. astronomy, ConceptNet contains links to URLs that define astronomy in WordNet, Wiktionary, Open-Cyc, and DBPia

The graph-structured knowledge in ConceptNet can be particularly useful to NLP learning algorithms
We can use ConceptNet to build semantic spaces that are more effective than distributional semantics alone
The most effective semantic space is one that learns from booth distributional semantics and ConceptNet, using a generalization of the “retrofitting” method. 
We call this hybrid semantic space “ConceptNet Numberbatch”

## 2. Related Work
ConceptNet is the knowledge graph version of the Open Mind Common Sense project, a common sense knowledge base of the most basic things a person knows.

ConceptNet’s role compared to these other resources is to provide a sufficiently large, free knowledge graph that focuses on the common-sense meaning of words (not named entities) as they are used in natural language. This focus on words make it particularly compatible with the idea of representing word meanings as vectors.

## Structure of ConceptNet

### Knowledge Sources
ConceptNet contains over 21 million edges and over 8 million nodes.

### Relations
ConceptNet uses a closed class of selected relations.
ConceptNet’s edges are directed, but some relations are designated as being symetric.
Symmetric relations:
Antonym,DistinctFrom,Etymolog- icallyRelatedTo, LocatedNear, RelatedTo, SimilarTo, and Synonym 
Asymmetric relations: 
AtLocation, CapableOf, Causes, CausesDesire, CreatedBy, DefinedAs, DerivedFrom, De- sires, Entails, ExternalURL, FormOf, HasA, HasCon- text, HasFirstSubevent, HasLastSubevent, HasPrerequi- site, HasProperty, InstanceOf, IsA, MadeOf, MannerOf, MotivatedByGoal, ObstructedBy, PartOf, ReceivesAc- tion, SenseOf, SymbolOf, and UsedFor 


### Term Representation
ConceptNet represents terms in a standardized form.
ConceptNet 5.5 removes the lemmatizer, and instead relates inflections of words usin the FormOf relation.


### Vocabulary
In ConceptNet, a node is a word or phrase of a natural language, often a common word in its undisambiguated form.
c/en/lead even thou it has multiple meanings.
ConceptNet’s representation allows for more specific, dis- ambiguated versions of a term.
The URI /c/en/lead/n refers to noun senses of the word “lead”, and is effectively included within /c/en/lead when searching or travers- ing ConceptNet, and linked to it with the implicit relation SenseOf. Many data sources provide information about parts of speech, allowing us to use this as a common representa- tion that provides a small amount of disambiguation. 


### Linked Data
A term that is imported from another knowledge graph will be connected to ConceptNet nodes via the relation ExternalURL.
ConceptNet terms can also be represented as absolute URLs


## Applying ConceptNet to Word Embeddings
1. Computing ConceptNet Embeddings Using PPMI
We can represent the ConceptNet graph as a sparse, symmetric term-term matrix. Each cell contains the sum of the weights of all edges that connect the two corresponding terms
the context is the other nodes it is connected to in ConceptNet.
We can calculate word embeddings directly from this sparse matrix by following the practical recommendations of L

As in L,
determine the pointwise mutual information of the matrix entries with context distributional smoothing
clip the negative values to yield positive pointwise mutual information(PPMI)
reduce the dimensionality of the result to 300 dimensions with truncated SVD
and combine the terms and contexts symmetrically into a single matrix of word embeddings.

This gives a matrix of word embeddings we call ConceptNet-PPMI.


### Combining ConceptNet with distributional Word Embeddings
we would now like to create a more robust set of embeddings that represents both ConceptNet and distributional word embeddings learned from text. 
Retrofitting is a process that adjusts an existing matrix of word embeddings using a knowledge graph.
목적 함수 수식
The process of “expanded retrofitting” 
can optimize this objective over a larger vocabulary, including terms from the knowledge graph that do not appear in the vocabulary of the word embeddings. 
The particular benefit of expanded retrofitting to ConceptNet is that it can benefit from the multilingual connections in ConceptNet. 
We add one more step to retrofitting, which is to subtract the mean of the vectors that result from retrofitting, then re- normalize them to unit vectors. 
Retrofitting has a tendency to move all vectors closer to the vectors for highly-connected terms such as “person”. 
Subtracting the mean helps to ensure that terms remain distinguishable from each other. 


### Combining Multiple Sources of Embeddings
the way that we can benefit from them the most is by taking both of them as input. 
To do this, we apply retrofitting to both matrices, 
then find a globally linear projection that aligns the results on their common vocabulary. 
We find the projection by concatenating the columns of the matrices and reducing them to 300 dimensions using truncated SVD. 
We then use this alignment to infer compatible embeddings for terms that are missing from one of the vocabularies. 



## Evaluation
Evaluation
To compare the performance of fully-built systems of word embeddings, 
we will first compare their results on intrinsic evaluations of word relatedness, 
then apply the word embeddings to the downstream tasks of solving proportional analogies and choosing the sensible ending to a story, to evaluate whether better embeddings translate to better performance on semantic tasks. 
Evaluation
성능 비교 표, 그래프
