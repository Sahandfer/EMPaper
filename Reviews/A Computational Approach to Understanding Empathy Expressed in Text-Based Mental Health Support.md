### A Computational Approach to Understanding Empathy Expressed in Text-Based Mental Health Support

> Ashish Sharma, Adam S.Miner, David C. Atkins, and Tim Althoff
>
> Published on 17 Sep 2020 [ [arxiv](https://arxiv.org/abs/2009.08441) | [source code](https://github.com/behavioral-data/Empathy-Mental-Health) ]

#### Problem Statement and Significance

1. Empathy is essential for effective mental health support.
2. Millions of people use text-based platforms.
3. Around 20% of people worldwide have issues with mental health.
4. Many peer supporters lack sufficient training/feedback.
5. Due to resources, currently implemented levels of empathy are limited to speech-based therapy (face to face).
6. Previous work focused on reactions with warm and compassionate emotions. Thus, lacking emotions such as concern as well as cognitive understanding of the seeker's statement.  

#### Proposed Solution

1. EmPathy In Text-based, asynchrOnous MEntal health conversations (EPITOME) framework for asynchronous text-based conversations.
2. Create a new dataset of empathic conversations using EPITOME.
3. A RoBERTa-based bi-encoder model for identifying empathy.

#### Methodology

1. **EPITOME **, which is the framework for this project, and it contains:

   1. *Emotional Reactions*: expressing emotions such as warmth.
   2. *Interpretations*: expresses an understanding of the feelings/experiences inferred from the user's statement.
   3. *Explorations*: improving understanding by exploring the feelings/experience not stated in the user's statement (i.e. showing an active interest).

   Each of the above categories can have one of the following three mechanisms of communication:

   1. *No*: referring to advice, facts, and offensive or abusive language.
   2. *Weak*: implicit labeling of emotions, a mention of understanding, or generic questioning (e.g. Everything will be ok, I understand your feeling, what happened?, etc. ).
   3. *Strong*: Explicit and specific labeling of emotions, specific feeling/experience, or question specific feelings that the user might have  (e.g. I am really happy for you, I also had this problem which annoyed me a lot, are you feeling lonely?, etc.).

2. **Data Collection**

   1. TalkLife: peer-to-peer mental health support network.
   2. Mental Health Subreddits: e.g. r/depression

3. **Data Annotation**

   1. Acquired 8 trained crowedworkers from UpWork: trained through 30-60 minute-long phone calls. 
   2. Classified the (post, response) methods with given the three communication mechansims. 
   3. Highlighted portions of the response that formed rationale behind the annotation.
   4. Removed all personally identifiable information from dataset.
   5. *Result*: The corpus contained 10,143 ground truth (post, response) with average inter-annotator agreement of 0.6865 (in contrast, the average is 0.6 for face-to-face therapy).

4. **Bi-Encoder Model with Attention**

   Based on the RoBERTa for identifying empathy and extracting rationals.  

   ![RobertA](https://user-images.githubusercontent.com/35892312/95176237-2be5f100-07ef-11eb-8899-fc1e1b6471c8.png)

   Assuming  Si = Si1, ..., Sim is seeker's post and Ri = Ri1, .. Rin is the corresponding response, the model creates a joint modeling of (Si, Ri) instead of concatenating Si, Ri, and a [SEP] token to create a single input sequence. 

   **Two Encoders**:

   S-Encoder and R-Encoder, which are pre-trained and independant. S-Encoder encodes context from post while R-Encoder understands the empathy in the response.

   ![Two Encoder](https://user-images.githubusercontent.com/35892312/95175928-bc700180-07ee-11eb-9d74-36950bca413d.png)

   **Domain-Adaptive Pre-training**:

   This is implemented for adapting to conversational and mental health context, and it is initialized for both encoders by using the weights learned by the previous stage. 

   **Attention Layer:**

   This helps in provoding context from the post.

   ![Attention](https://user-images.githubusercontent.com/35892312/95175935-be39c500-07ee-11eb-8196-f6cfde0f708c.png)

   where d is the hidden size in previous stage and = 768. This gives a residual mapping hi, which forms the final seeker-context aware representation of the response.

#### Results

##### 1) Empathy

![RES1](https://user-images.githubusercontent.com/35892312/95175972-c98cf080-07ee-11eb-9fc2-5f3d90e0dc0a.png)

##### 2) Rationale

![RES2](https://user-images.githubusercontent.com/35892312/95175916-baa63e00-07ee-11eb-95a6-9670ba29e538.png)

##### 3) Ablation

![RES3](https://user-images.githubusercontent.com/35892312/95175922-bb3ed480-07ee-11eb-8700-afa8ff4bf1e6.png)

### Limitations

Lack of resources and methods for asynchronous text-based interactions.

#### Future Work

Develop empathy-based feedback and training systems for peer supporters. 

#### Definitions

*Seeker*: individuals seeking mental health support.

*Supporter*: individuals providing mental health support.

*Emotion*: relates to the emotional stimulation in reaction to experiences.

*Cognition*: interpreting the experiences and fellings of the user and conveying that understanding to the user.



