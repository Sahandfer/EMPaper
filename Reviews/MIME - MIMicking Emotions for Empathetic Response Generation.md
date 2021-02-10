### MIME: MIMicking Emotions for Empathetic Response Generation

>  Navonil Majumder, Pengfei Hong, Shanshan Peng, Jiankun Lu, Deepanway Ghosal, Alexander Gelbukh, Rada Mihalcea,  and Soujanya Poria
>
> Published on 4 Oct 2020 [ [arxiv](https://arxiv.org/pdf/2010.01454.pdf) | [source code](https://github.com/facebookresearch/EmpatheticDialogues) ]

#### Problem Statement and Significance

1) Empathy is a fundamental human trait that reflects our ability to understand.

2) The complexity of this behavior makes it difficult to emulate with computational approaches.

#### Proposed Solution

1) Introduce a new approach for empathetic generation.

2) Implement extensive feature ablation experiments to show the importance of emotion mimicry and grouping.

#### Methodology

>  Refer to the formulas in the paper for more details

##### MIME Model

![MIME](https://user-images.githubusercontent.com/35892312/95286474-05c75c00-0896-11eb-941b-d96e7d8a80e6.png)

1) Obtain context representation using a transformer encoder architecture.

​	1.1) Emotion understanding in context representation by emotion classification during training.

​	1.2) Identify response emotion:

​			1.2.1) Group the 32 emotions into 2 groups: positive and negative.

​			1.2.2) Sample a probability distribution of emotion for each group to form positive and negative response emotion representation.

​			1.2.3)  Appropriately combine these representations to form the emotional state of response.

2) ***Task Definition***:  Given a list of context utterances ([u0, u1, ..., un-1]), where each utterance consists of maximum m words,  generate an empathetic response to the last utterance, where odd-numbered and even-numbered utterances belong to the system and the user respectively. The emotion e of the context could also be predicted. The list of emotions are provided below:

![list of emotions](https://user-images.githubusercontent.com/35892312/95286485-0c55d380-0896-11eb-86f3-db359f966457.png)

3) ***Context Encoding***: 

​	3.1) Sequentially concatenate all contextual utterances to a string of k (<= mn) words.

​	3.2) Represent each word wij as three embeddings => Context EC = Semantic word EW + Positional EP + Speaker ES

​	3.3) Context propagation within utterances and words in context representation + BERT's CTX,  using a transformer encoder.

​	3.4) Similar to MoEL, train a emotion classifier on context representation c. Accordingly, train emotion embeddings to represent each emotion. Then, maximize the similarity between c and the user-emotion embeddings using cross-entropy loss.

4) ***Response Generation***

> This step is based on the assumption that the empathetic agent mimics the user's emotion to some extent.

​	4.1) Group positive and negative emotions (13 positives and 19 negatives).

​	4.2) Sample the positive and negative distributions from the context to find the emotion approximate posterior distribution.

​	4.3) Generate response-emotion-refined context representations (m): mimicking (pos for pos else neg) and non-mimicking (neg for pos else pos). Accordingly, concatenate m with the context-enriched word representations. Then, the result of the concatenation is fed to a transformer encoder to obtain mimicking (M) and non-mimicking (M bar) response-emotion-refined context.

​	4.4) Concatenate mimicking and non-mimicking contexts at word level to get M'. Then, M' is fed to a fully-connected layer with sigmoid activation, giving Mcontrib (the contribution of M and M bar). Accordingly, M' is multiplied with the gate output to give Madjust. Next, Madjust is fed to another fully-connected layer to get Mfused.

​	4.5) Mfused is fed to a transformer decoder to find number tokens (O) in response.  

​	4.6) Calculate categorical cross-entropy with respect to gold response (Rgold).

#### Results

##### Dataset: EMPATHETICDIALOGUES

![Comp1](https://user-images.githubusercontent.com/35892312/95312155-2953cc00-08c1-11eb-9db7-9df7964d1f5b.png)

![Comp2](https://user-images.githubusercontent.com/35892312/95312152-28bb3580-08c1-11eb-8c81-328df5ef21a0.png)

##### Performance on positive and negative context

![Comp3](https://user-images.githubusercontent.com/35892312/95312143-25c04500-08c1-11eb-9587-200c06ed016c.png)

##### Ablation Study

![Comp4](https://user-images.githubusercontent.com/35892312/95312147-26f17200-08c1-11eb-8fa9-377d6802b876.png)

#### Limitations

1) MIME shows lower fluency than MoEL

2) The model considers the emotion of 'surprise' to be positive, which is not the case in many situations.

3) Overall weaker emotion classification than MoEL.

#### Future Work

Improve fluency, and explicitly deal with emotions such as 'surprise' and 'anticipation' due to their double meanings.