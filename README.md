# Homonymy disambiguation project

### Introduction

In the field of Natural Language Processing, Word Sense Disambiguation (WSD) is a challenging task in which the objective is to assign the correct mean- ing to ambiguous target words, given their belonging context. Homonymy disambiguation can be considered a particular instance of this task, where related senses are clusterized together, hence lead- ing to a form of Coarse-Grained WSD. In this con- text, two words are homonyms if they hold the same lexical form, but have unrelated meanings. In the field of WSD, BERT-based models such as GlossBERT [5] have been extensively used, demonstrating the capability of producing contextualized embeddings that are able to capture word senses. The work presented describes a series of experiments involving BERT-based architecures, describing the process of fine-tuning and the implementation choices.

### Proposed architecture

The proposed architecture is constituted by two main modules: DeBERTa used to extract word embeddings for each token and a classifier head, constituted by a Multi-Layer Perceptron, which takes as input the Transformer’s embeddings and outputs the logits for each possible class. Moreover, the model implements some operations both at the embeddings level and at the logits level, which will be highlighted in the following paragraphs.

![Screenshot 2023-11-28 alle 15.23.29.png](/Users/ludocomito/Desktop/Università/AIRO/passed_exams/mnlp/Homonymy-Disambiguation-NLP/image_assets/general_view.png)

**Averaging hidden states** 

As different layers of BERT are expected to encode information at different levels, the proposed method considers the representation obtained by averaging the last four hidden states of BERT, as a technique to obtain a more informative representation. 

**Sub-token pooling** 

During tokenization, certain words can be split by BERT’s tokenizer in sub- tokens. After being fed to the transformer the resulting sub-token embeddings for each word are
averaged, in order to obtain a single representation for the entire word. This operation is performed for each word.

![Screenshot 2023-11-28 alle 15.23.45.png](/Users/ludocomito/Desktop/Università/AIRO/passed_exams/mnlp/Homonymy-Disambiguation-NLP/image_assets/token_handling.png)

**Logits mask** In this kind of task, the classifier could potentially deal with thousands of possible senses. As a solution to this problem, the candidate senses for target words present in the datasets are used to create a logits mask in the form of a list of zeros and ones. In particular, ones are assigned to all the candidate senses for each target word, while all the others will correspond to zero. This will have the effect of putting a constraint on the model in order to put the focus on the actual candidates.

![Screenshot 2023-11-28 alle 15.24.02.png](/Users/ludocomito/Desktop/Università/AIRO/passed_exams/mnlp/Homonymy-Disambiguation-NLP/image_assets/logits_mask.png)