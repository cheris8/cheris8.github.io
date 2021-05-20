## 1. Introduction

In this work we explore improvements to this approach for the class of tasks that require multi-sentence scoring: given an input context, score a set of candidate labels, a setup common in retrieval and dialogue tasks, amongst others. 
Performance in such tasks has to be measured via two axes: prediction quality and prediction speed, as scoring many candidates can be prohibitively slow.

The current state-of-the-art focuses on using BERT models for pre-training, which employ large text corpora on general subjects: Wikipedia and the Toronto Books Corpus (Zhu et al., 2015). 
Two classes of fine-tuned architecture are typically built on top: Bi-encoders and Cross-encoders. 
- Bi-encoders, which perform self-attention over the input and candidate label separately and combine them at the end for a final representation. 
- As the representations are separate, Bi-encoders are able to cache the encoded candidates, and reuse these representations for each input resulting in fast prediction times.
- Cross-encoders, which perform full (cross) self-attention over a given input and label candidate, tend to attain much higher accuracies than their counterparts, 
- Cross-encoders must recompute the encoding for each input and label; as a result, they are prohibitively slow at test time.

## 2. Related Work

The task of scoring candidate labels given an input context is a classical problem in machine learning. While multi-class classification is a special case, the more general task involves candidates as structured objects rather than discrete classes; in this work we consider the inputs and the candidate labels to be sequences of text.

**Bi-encoders**
There is a broad class of models that map the input and a candidate label separately into a common feature space wherein typically a dot product, cosine or (parameterized) non-linearity is used to measure their similarity. We refer to these models as Bi-encoders. Such methods include 
- vector space models, 
- LSI, 
- supervised embeddings 
- classical siamese networks (Bromley et al., 1994). For the next utterance prediction tasks we consider in this work, several Bi-encoder neural approaches have been con- sidered, in particular Memory Networks (Zhang et al., 2018a) and Transformer Memory networks (Dinan et al., 2019) as well as LSTMs (Lowe et al., 2015) and CNNs (Kadlec et al., 2015) which encode input and candidate label separately. 
A major advantage of Bi-encoder methods is their abil- ity to cache the representations of a large, fixed candidate set. Since the candidate encodings are independent of the input, Bi-encoders are very efficient during evaluation.

**Cross-encoders**
Researchers have also studied a more rich class of models we refer to as Cross-encoders, which make no assumptions on the similarity scoring function between input and candidate label. Instead, the concatenation of the input and a candidate serve as a new input to a nonlinear function that scores their match based on any dependencies it wants. This has been explored with 
- Sequential Matching Network CNN-based architectures (Wu et al., 2017), Deep Matching Networks (Yang et al., 2018), Gated Self-Attention (Zhang et al., 2018b), and most recently transformers (Wolf et al., 2019; Vig & Ramea, 2019; Urbanek et al., 2019). For the latter, concatenating the two sequences of text results in applying self-attention at every layer. This yields rich interactions between the input context and the candidate, as every word in the candidate label can attend to every word in the input context, and vice-versa. 
and finding that Cross-encoders perform better. However, the performance gains come at a steep computational cost. Cross-encoder representations are much slower to compute, rendering some applications infeasible.

## 3. TASKS

We consider the tasks of sentence selection in dialogue and article search in IR. 
- The former is a task extensively studied and recently featured in two competitions: 
    - the Neurips ConvAI2 competition (Dinan et al., 2020), and
    - the DSTC7 challenge, Track 1 (Yoshino et al., 2019; Jonathan K. Kummer- feld & Lasecki, 2018; Chulaka Gunasekara & Lasecki, 2019). 
    - We compare on those two tasks and in addition, we also test on the popular Ubuntu V2 corpus (Lowe et al., 2015)
- For IR, we use the Wikipedia Article Search task of Wu et al. (2018).

The ConvAI2 task is based on the Persona-Chat dataset (Zhang et al., 2018a) which involves dia- logues between pairs of speakers. Each speaker is given a persona, which is a few sentences that describe a character they will imitate, e.g. “I love romantic movies”, and is instructed to get to know the other. Models should then condition their chosen response on the dialogue history and the lines of persona. As an automatic metric in the competition, for each response, the model has to pick the correct annotated utterance from a set of 20 choices, where the remaining 19 were other randomly chosen utterances from the evaluation set. 

The DSTC7 challenge (Track 1) consists of conversations extracted from Ubuntu chat logs, where one partner receives technical support for various Ubuntu-related problems from the other.

Ubuntu V2 is a similar but larger popular corpus, created before the competition (Lowe et al., 2015); we report results for this dataset as well, as there are many existing results on it.

Finally, we evaluate on Wikipedia Article Search (Wu et al., 2018). Using the 2016-12-21 dump of English Wikipedia (∼5M articles), the task is given a sentence from an article as a search query, find the article it came from. Evaluation ranks the true article (minus the sentence) against 10,000 other articles using retrieval metrics. This mimics a web search like scenario where one would like to search for the most relevant articles (web documents). 

Table1

## 4. METHODS

### 4.1 Transformers and Pre-training Strategies

**Transformers**
Our Bi-, Cross-, and Poly-encoders, are based on large pre-trained transformer models with the same architecture and dimension as BERT-base, which has 12 layers, 12 attention heads, and a hidden size of 768. 
As well as considering the BERT pre-trained weights, we also explore our own pre-training schemes. Specifically, we pre-train two more transformers from scratch using the exact same architecture as BERT-base. 
- One uses a similar training setup as in BERT-base, training on 150 million of examples of [INPUT, LABEL] extracted from Wikipedia and the Toronto Books Corpus, 
- The former is performed to verify that reproducing a BERT-like setting gives us the same results as reported previously, 
- while the other is trained on 174 million examples of [INPUT, LABEL] extracted from the online platform Reddit, which is a dataset more adapted to dialogue. 
- while the latter tests whether pre-training on data more similar to the downstream tasks of interest helps. 
For training both new setups we used XLM (Lample & Conneau, 2019).

**Input Representation**
Our pre-training input is the concatenation of input and label [IN- PUT,LABEL], where both are surrounded with the special token [S], following Lample & Conneau (2019). 
- When pre-training on Reddit, the input is the context, and the label is the next utterance. 
- When pre-training on Wikipedia and Toronto Books, the input is one sentence and the label the next sentence in the text. 
Each input token is represented as the sum of three embeddings: the token embedding, the position (in the sequence) embedding and the segment embedding. Segments for input tokens are 0, and for label tokens are 1.

**Pre-training Procedure**
Our pre-training strategy involves training with a masked language model (MLM) task identical to the one in Devlin et al. (2019). 
In the pre-training on Wikipedia and Toronto Books we add a next-sentence prediction task identical to BERT training. 
In the pre-training on Reddit, we add a next-utterance prediction task, which is slightly different from the previous one as an utterance can be composed of several sentences. During training 50% of the time the candidate is the actual next sentence/utterance and 50% of the time it is a sentence/utterance randomly taken from the dataset. We alternate between batches of the MLM task and the next-sentence/next- utterance prediction task. Like in Lample & Conneau (2019) we use the Adam optimizer with learning rate of 2e-4, β1 = 0.9, β2 = 0.98, no L2 weight decay, linear learning rate warmup, and inverse square root decay of the learning rate. We use a dropout probability of 0.1 on all layers, and a batch of 32000 tokens composed of concatenations [INPUT, LABEL] with similar lengths. We train the model on 32 GPUs for 14 days.

**Fine-tuning**
After pre-training, one can then fine-tune for the multi-sentence selection task of choice, in our case one of the four tasks from Section 3. We consider three architectures with which we fine-tune the transformer: the Bi-encoder, Cross-encoder and newly proposed Poly-encoder.

### 4.2 BI-ENCODER


## 5. EXPERIMENTS

We perform a variety of experiments to test our model architectures and training strategies over four tasks. For metrics, we measure Recall@k where each test example has C possible candidates to select from, abbreviated to R@k/C, as well as mean reciprocal rank (MRR).

### 5.1 Bi-encoders and Cross-encoders

We first investigate fine-tuning the Bi- and Cross-encoder architectures initialized with the weights provided by Devlin et al. (2019), studying the choice of other hyperparameters (we explore our own pre-training schemes in section 5.3). 
In the case of the Bi-encoder, we can use a large number of neg- atives by considering the other batch elements as negative training samples, avoiding recomputation of their embeddings. On 8 Nvidia Volta v100 GPUs and using half-precision operations (i.e. float16 operations), we can reach batches of 512 elements on ConvAI2. Table 2 shows that in this setting, we obtain higher performance with a larger batch size, i.e. more negatives, where 511 negatives yields the best results. For the other tasks, we keep the batch size at 256, as the longer sequences in those datasets uses more memory. 
The Cross-encoder is more computationally intensive, as the embeddings for the (context, candidate) pair must be recomputed each time. We thus limit its batch size to 16 and provide negatives random samples from the training set. For DSTC7 and Ubuntu V2, we choose 15 such negatives; For ConvAI2, the dataset provides 19 negatives.

