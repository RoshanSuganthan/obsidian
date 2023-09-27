  

## Transformer Architecture

- Transformers are a type of neural network architecture

- Transformers were developed to solve the problem of sequence transduction, or neural machine translation. That means any task that transforms an input sequence to an output sequence. This includes speech recognition, text-to-speech transformation, etc..

- In this example, the word “the band” in the second sentence refers to the band “The Transformers” introduced in the first sentence. When you read about the band in the second sentence, you know that it is referencing to the “The Transformers” band. That may be important for translation. There are many examples, where words in some sentences refer to words in previous sentences.

  

## RRN

-

  

### cons of RRN

- RNNs become very ineffective when the gap between the relevant information and the point where it is needed become very large. That is due to the fact that the information is passed at each step and the longer the chain is, the more probable the information is lost along the chain.

  

- When arranging one’s calendar for the day, we prioritize our appointments. If there is anything important, we can cancel some of the meetings and accommodate what is important.

  

## LSTM

- LSTMs make small modifications to the information by multiplications and additions. With LSTMs, the information flows through a mechanism known as cell states. In this way, LSTMs can selectively remember or forget things that are important and not so important.

  

### cons of LSTM

- The same problem that happens to RNNs generally, happen with LSTMs, i.e. when sentences are too long LSTMs still don’t do too well. The reason for that is that the probability of keeping the context from a word that is far away from the current word being processed decreases exponentially with the distance from it.

  

- That means that when sentences are long, the model often forgets the content of distant positions in the sequence.

  

- Hard to parallelize the process

  

## Attention

- Attention is a technique that is used in a neural network. For RNNs, instead of only encoding the whole sentence in a hidden state, each word has a corresponding hidden state that is passed all the way to the decoding stage. Then, the hidden states are used at each step of the RNN to decode

  

### Cons of Attention

- For example, processing inputs (words) in parallel is not possible. For a large corpus of text, this increases the time spent translating the text.

  

## Convolutional Neural Networks

  

Convolutional Neural Networks help solve these problems. With them we can

  

- Trivial to parallelize (per layer)

- Exploits local dependencies

- Distance between positions is logarithmic

  

- Some of the most popular neural networks for sequence transduction, Wavenet and Bytenet, are Convolutional Neural Networks.

  

### cons of CNN

-  The problem is that Convolutional Neural Networks do not necessarily help with the problem of figuring out the problem of dependencies when translating sentences. That’s why Transformers were created, they are a combination of both CNNs with attention.

  

## Transformers

- To solve the problem of parallelization, Transformers try to solve the problem by using encoders and decoders together with attention models. Attention boosts the speed of how fast the model can translate from one sequence to another.

  

- Transformer is a model that uses attention to boost the speed. More specifically, it uses self-attention.

  

- Transformer consists of six encoders and six decoders.

  

- Each encoder consists of two layers: Self-attention and a feed Forward Neural Network.

  

- The encoder’s inputs first flow through a self-attention layer. It helps the encoder look at other words in the input sentence as it encodes a specific word. The decoder has both those layers, but between them is an attention layer that helps the decoder focus on relevant parts of the input sentence.

  

## Self-Attention

- we begin by turning each input word into a vector using an embedding algorithm.

- Each word is embedded into a vector of size 512.

- Here we begin to see one key property of the Transformer, which is that the word in each position flows through its own path in the encoder. There are dependencies between these paths in the self-attention layer.

- The feed-forward layer does not have those dependencies, however, and thus the various paths can be executed in parallel while flowing through the feed-forward layer.

- to see how the scores are calculated in self attention [link](Calculate_self-attention.md)

-  The resulting vector is one we can send along to the feed-forward neural network. In the actual implementation, however, this calculation is done in matrix form for faster processing

  

### Positional Encoding

Another important step on the Transformer is to add positional encoding when encoding each word. Encoding the position of each word is relevant, since the position of each word is relevant to the translation.

## Sequence to sequence transduction

- Sequence Transduction, also known as sequence-to-sequence modeling, is a machine learning task that involves converting an input sequence into an output sequence, potentially of different lengths.

  

## BERT

- BERT (Bidirectional Encoder Representations from Transformers)

- state-of-the-art results in a wide variety of NLP tasks, including Question Answering (SQuAD v1.1), Natural Language Inference (MNLI), and others.

  

- resources about BERT:

  

the original paper

Jay Allamar's blog post as well as his tutorial

Chris Mccormick's Youtube channel

Abbishek Kumar Mishra's Youtube channel

  

## Transfer learning

- Transfer learning (TL) is a technique in machine learning (ML) in which knowledge learned from a task is re-used in order to boost performance on a related task.

- For example, for image classification, knowledge gained while learning to recognize cars could be applied when trying to recognize trucks.

  

- first pretraining a large neural network in an unsupervised way, and then fine-tuning that neural network on a task of interest.

  

-  BERT is a neural network pretrained on 2 tasks: masked language modeling and next sentence prediction. Now, we are going to fine-tune this network on a NER dataset. Fine-tuning is supervised learning, so this means we will need a labeled dataset.

  

# Fine-tuning BERT for named-entity recognition

- Named entity recognition (NER) uses a specific annotation scheme

- An annotation scheme that is widely used is called IOB-tagging. which stands for Inside-Outside-Beginning. Each tag indicates whether the corresponding word is inside, outside or at the beginning of a specific named entity. The reason this is used is because named entities usually comprise more than 1 word.

  

- Let's have a look at an example. If you have a sentence like "Barack Obama was born in Hawaï", then the corresponding tags would be [B-PERS, I-PERS, O, O, O, B-GEO]. B-PERS means that the word "Barack" is the beginning of a person, I-PERS means that the word "Obama" is inside a person, "O" means that the word "was" is outside a named entity, and so on. So one typically has as many tags as there are words in a sentence.

  

- There exist many annotation tools which let you create these kind of annotations automatically (such as Spacy's Prodigy, Tagtog or Doccano).

- Open-source [Doccano](https://github.com/doccano/doccano)

- docker pull doccano/doccano

docker container create --name doccano -e "ADMIN_USERNAME=admin" -e "ADMIN_EMAIL=admin@example.com" -e "ADMIN_PASSWORD=password" -v doccano-db:/data -p 8000:8000 doccano/doccano

  

- spacy's offset [training.biluo_tags_to_offsets](https://spacy.io/api/top-level#biluo_tags_from_offsets)

  

- after cleaning the Data

``` python

class dataset(Dataset):

    def __init__(self, dataframe, tokenizer, max_len):

        self.len = len(dataframe)

        self.data = dataframe

        self.tokenizer = tokenizer

        self.max_len = max_len

    def __getitem__(self, index):

        # step 1: tokenize (and adapt corresponding labels)

        sentence = self.data.sentence[index]  

        word_labels = self.data.word_labels[index]  

        tokenized_sentence, labels = tokenize_and_preserve_labels(sentence, word_labels, self.tokenizer)

        # step 2: add special tokens (and corresponding labels)

        tokenized_sentence = ["[CLS]"] + tokenized_sentence + ["[SEP]"] # add special tokens

        labels.insert(0, "O") # add outside label for [CLS] token

        labels.insert(-1, "O") # add outside label for [SEP] token

  

        # step 3: truncating/padding

        maxlen = self.max_len

  

        if (len(tokenized_sentence) > maxlen):

          # truncate

          tokenized_sentence = tokenized_sentence[:maxlen]

          labels = labels[:maxlen]

        else:

          # pad

          tokenized_sentence = tokenized_sentence + ['[PAD]'for _ in range(maxlen - len(tokenized_sentence))]

          labels = labels + ["O" for _ in range(maxlen - len(labels))]

  

        # step 4: obtain the attention mask

        attn_mask = [1 if tok != '[PAD]' else 0 for tok in tokenized_sentence]

        # step 5: convert tokens to input ids

        ids = self.tokenizer.convert_tokens_to_ids(tokenized_sentence)

  

        label_ids = [label2id[label] for label in labels]

        # the following line is deprecated

        #label_ids = [label if label != 0 else -100 for label in label_ids]

        return {

              'ids': torch.tensor(ids, dtype=torch.long),

              'mask': torch.tensor(attn_mask, dtype=torch.long),

              #'token_type_ids': torch.tensor(token_ids, dtype=torch.long),

              'targets': torch.tensor(label_ids, dtype=torch.long)

        }

    def __len__(self):

        return self.len

```

- Next, we define a regular PyTorch dataset class (which transforms examples of a dataframe to PyTorch tensors). Here, each sentence gets tokenized, the special tokens that BERT expects are added, the tokens are padded or truncated based on the max length of the model, the attention mask is created and the labels are created based on the dictionary which we defined above.

  

- multilingual NER [github](https://github.com/chambliss/Multilingual_NER/blob/master/python/utils/main_utils.py#L344)

  
  

- [Bert_NER_finetuning](https://github.com/NielsRogge/Transformers-Tutorials/blob/master/BERT/Custom_Named_Entity_Recognition_with_BERT.ipynb)