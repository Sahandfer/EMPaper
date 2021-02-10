### Empathetic Dialogue Generation via Knowledge Enhancing and Emotion Dependency Modeling

> Qintong Li, Piji Li, Zhumin Chen,  and Zhaochun Ren
>
> Published on 21 Sep 2020 [ [arxiv](https://arxiv.org/pdf/2009.09708.pdf) ]

#### Problem Statement and Significance

1. Empathy is a crucial factor of humanized dialogue systems.

2. Challenges of empathetic dialogue generation, mainly due to lack of external knowledge (e.g. knowledge of emotional interactions between the two parties) :

   1.1 Perceived nuanced emotions implied in the dialogue context.

   1.2 Modelling emotional dependancies.

3. Current systems are only able to implement empathy with explicit emotion labels.

4. Humans rely on experience and external knowledge to reason and express emotions.

#### Proposed Solution

A knowledge-enhanced framework "Know-EDG", consisting of 3 components:

1. *Knowledge-enhanced context encoder* for conducting external-knowledge acquirement.
2.  *Emotion identifier* for jointly modeling the dynamic emotion transition mechanism.
3. *Emotion-focused response generator* for generating the empathetic response by jointly considering the emotional information in dialogue context and the predicted ones.

#### Methodology

>  Refer to the formulas in the paper for further  details.

##### The model

![model]()

##### Knowledge-Enhanced Context Encoder

1. Construct a context graph: flat M utterances to token sequence S. Then, for each non-stopword token x in S, retrieve a set of candidate tuples and filter them via the following:
   1. Relation filtering => Create an excluded relation list Lex to filter the set of tuples and reserve tuples whose relation vector r is not in the list.
   2. Concept ranking => Derive a matching score for each tuple. Then, select the top K' tuples as the emotional knowledge for each token.
2. Encode context graph: 
   1. Use a word embedding layer and a positional embedding layer and convert each node v into vector Ew(v) and Ep(V). 
   2. Use a dimensionality emdding to get Ed(V). The vector representation of  node v would be the sum of these 3 embeddings.
   3. Apply a multi-head graph-attention mechanism to update node representations with emotional knowledge.
3. Emotion Identifier:
   1. Calculate emotion intensity values for all nodes and derive the emotion-focused context representation Ee from them by weighted summation of all node representations.
   2. Identify emotion signal (ep) for target response by using a linear layer with softmax operation on Ee.
   3. Employ cross-entropy as the loss function to learn parameters.
4. Emotion-focused Response Generator:

##### Emotion Identifier



##### Emotion-Focused Response Generator



#### Results



#### Limitations



#### Future Work



#### Definitions

**External Knowledge**: the commonsense knowledge in ConceptNet and the emotional knowledge NRC_VAD.

**ConceptNet**: a large-scale knowledge graph that describes general human knowledge in natural language.

**NRC_VAD**: a list of Valence-Arousal-Dominance (VAD) 3D vectors, which are culture-independent and widely adopted in Psychology.

