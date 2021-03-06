# Natural Language Processing and Pet Owner Classification on Video Comments

- This project aims to build up a classification for the pet owners from text comments. There are more than 3000 channels and over 2,500,000 comments recorded in the dataset. 

*This project is based on Apache Spark.*

## Classifier
- In order to train a model against the commetns, RegexTokenizer is applied to split each comment into a list of words and then applied Word2Vec to convert the list to a word vector. The algorithms are explained below:

### 1. Introduction
#### RegexTokenizer:
- [RegexTokenizer](https://spark.apache.org/docs/2.1.0/api/python/pyspark.ml.html#pyspark.ml.feature.RegexTokenizer) allows more advanced tokenization based on regular expression (regex) matching. By default, the parameter “pattern” is used as delimiters to split the input text. Alternatively, users can set parameter “gaps” to false indicating the regex “pattern” denotes “tokens” rather than splitting gaps, and find all matching occurrences as the tokenization result.

- A regex based tokenizer that extracts tokens either by using the provided regex pattern (in Java dialect) to split the text (default) or repeatedly matching the regex (if gaps is false). Optional parameters also allow filtering tokens using a minimal length. It returns an array of strings that can be empty.

**Example**  

  Input: 'I shared this to my friends and mom the were lol'   
  
  Output: '[i, shared, this, to, my, friends, and, mom, the, were, lol]'  
  
 
 #### [Word2Vec](https://en.wikipedia.org/wiki/Word2vec):
 - Word2vec is a group of related models that are used to produce word embeddings. These models are shallow, two-layer neural networks that are trained to reconstruct linguistic contexts of words. Word2vec takes as its input a large corpus of text and produces a vector space, typically of several hundred dimensions, with each unique word in the corpus being assigned a corresponding vector in the space.
 
 - In this case, the word2vec algorithm is employed to compute distributed vector representation of words. The main advantage of the distributed representations is that similar words are close in the vector space, which makes generalization to novel patterns easier and model estimation more robust. Distributed vector representation is showed to be useful in many natural language processing applications such as named entity recognition, disambiguation, parsing, tagging and machine translation.
 
In our implementation of Word2Vec, we used skip-gram model. The training objective of skip-gram is to learn word vector representations that are good at predicting its context in the same sentence. The mathematically deduction can be found in [here](https://spark.apache.org/docs/latest/mllib-feature-extraction.html#word2vec).

### 2. Classifier
- After mapping each word to a unique fixed-size vector, the next step is to set up a classifier to find out the labbeled pet owners. In this case, I only implemented and trained the logistic regression model with 5-fold cross-validation. However, from the code, I sealed a function as 'Classifier' to load the model, other algorithm or classifiers can be directly added within the function.

- The visualization result of the logistic regression model:
![](https://github.com/Jieyi-Deng/Video-Comments-Analysis/blob/master/training.png?raw=true)


### 3. Insight of the Users:
- At the same time, I explored the significant topics to the users by [Latent Dirichlet Allocation (LDA)](http://www.jmlr.org/papers/volume3/blei03a/blei03a.pdf). In natural language processing, Latent Dirichlet Allocation (LDA) is a generative statistical model that allows sets of observations to be explained by unobserved groups that explain why some parts of the data are similar. In this case, I tried to conclude 5 topics using LDA to Cluster the TF-IDF Matrix :

| Topics | Key Words |
| ------------- | ------------- |
| Topic 1 | ['feel', 'people', u'hate', 'school', 'time', 'life', 'day', 'last', 'also'] |
| Topic 2 | ['know', 'looks', 'please', 'back', 'see', 'want', 'died', 'help', 'video'] |
| Topic 3 | ['back', 'know', 'house', 'video', 'looks', 'time', 'old', 'lol', 'years'] |
| Topic 4 | ['pit', 'know', 'training', 'want', 'food', 'even', 'good', 'old', 'looks'] |
| Topic 5 | ['food', 'time', 'know', 'people', 'make', 'want', 'think', 'even', 'looks'] |

  From the topic clustering result we can know that the important topics include: a. food; school/training; life or age[died] b. the emotional expression mainly focuses on funny and interesting (e.g. lol).

