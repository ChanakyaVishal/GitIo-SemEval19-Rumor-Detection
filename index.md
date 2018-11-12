# IRE PROJECT REPORT
## SemEval19-Rumor-Detection
[GitHub Repo](https://github.com/mohit3011/SemEval19-Rumor-Detection)

Mohit Chandra (201502199)

Nikita Garg (20172004)

Chanakya Vishal KP (20161116)

Aayush Tiwari(201531031)


# Overview
The main task of this project is to deal with the objective of determining how the other
sources/agents react to a rumourous story/ text. The goal here is to label the type of
interaction between a given statement and the reply posts. Given a tree-structured
thread of tweets, this project intends to classify each of these tweets into one of the four
categories: Support, Deny, Query, Comment.

# Data Extraction and Pre-Processing
In this project, we considered the data from Twitter which is in JSON format. The dataset includes the post tweet (with ID and other metadata) its nested replies (with IDs and other metadata).

For creating our dataset we analyzed tweets and replies to get what all features are necessary for classification purpose. Hence, for our dataset, we considered the following features:
+ **Text data** (for text analysis for detection of interrogative, supportive and denial sentences)
Is the text interrogative?
Is the text supportive?
Is the text denial?
+ **Cosine Similarity with root rumourous tweets**

## Dataset & Method for Extraction

The Twitter data set was in the form of a directory structure in which the tweets were classified on the basis of the topic and each topic had a sub-folder which consisted of a set of tweets which had one source and one or more replies. We used python os calls to walk through the directory structure and used pythons JSON library to extract the necessary fields. Then after extracting the fields, we did further processing for a few fields: 
We did basic preprocessing like removal of 
Procedure for Text Analysis and Sentiment Analysis 

For detecting whether the statement is interrogative, denial or support (for the tweet replies), we used a classifier 

### For Interrogative sentences
+ For detecting whether the statement is interrogative, denial or support (for the tweet replies), we used a classifier 
+ For Interrogative sentences (Query), we used the following procedure:
+ We looked for the following words in the tweet :
'What', 'where', 'when', 'how', 'why', '?': If any of these words are present in the tweet then definitely it is query. 
+ 'did', 'do', 'does', 'have', 'has', 'am', 'is', 'are', 'can', 'could', 'may', 'would', 'will', 'should', 'didn't', 'doesn't', 'haven't', 'isn't', 'aren't', 'can't', 'couldn't', 'wouldn't', 'won't', 'shouldn't' : The presence of these words at the start of the tweet implies that the tweet is a query as these are helping verbs and whenever helping verbs occur at the start of the sentence then it is an interrogative sentence.
+ In the case when none of the above two conditions are matched then we parse the sentence into **a word2vec model which is trained on NLTK’s nps_chat corpus and get a vector which is further passed to a 15-class neural network based classifier.**
We trained a 15 class neural net classifier using a Word2Vec model in which the various classes are:
Accept: 1, Bye: 2, Clarify: 3, Continuer: 4, Emotion: 5, Emphasis: 6, Greet: 7, Other: 8, Reject: 9, Statement: 10, System: 11, nAnswer: 12, whQuestion: 13, yAnswer: 14, ynQuestion: 15
+ If the result of classification is **ynQuestion, whQuestion or Clarify**, then it is definitely a question.
 
### For Denial sentence
If the tweet is not a question, then we count the number of positive and negative words in the tweet. We found that if the number of denial terms is greater than that of the positive words then there it implied that the tweet was more likely to be a tweet denying the source tweet. The negative terms in our corpus include terms like: “abuse”,”false”,”belittle”,”issue”,”laughable”.

### For Supportive sentence
Similar to the process of obtaining denial tweets we found out that the reply tweet was overwhelmingly supportive if the number of positive terms was greater than that of the negative terms. The positive terms in our corpus include terms like: “right”,”undisputable”,”true”,”wow”,”wonderous”

### For Commentative tweets
If the number of positive and negative tweets were equivalent then that meant that the author of the reply tweet was neutral in the opinion. Hence its has to be comment.


## Word2Vec based Neural Network Classifier
We trained a 15 class neural net classifier using a Word2Vec model in which the various classes are:
Accept: 1
Bye: 2
Clarify: 3
Continuer: 4
Emotion: 5
Emphasis: 6
Other: 8
Greet: 7
Reject: 9
Statement: 10
System: 11
nAnswer: 12
whQuestion: 13
yAnswer: 14
ynQuestion: 15


## Results
Our previous tests with the use of Naive Bayes classifier gave an accuracy of between 65-70% (Combined for all 3 types of sentences). 
The accuracy of our current model is **70.37%**.

**We observed the following shortcomings of our model while testing:**
In case when the replies contain “WH”-words and still the sentence is not interrogative (eg: I teared up when I heard (actual tweet)) then our model would still classify it as interrogative.
In case the replies have sarcasm or oxymoron involved, then we cannot catch that.
Most of the errors came for the comment class tweets as we didn’t get any specific way to separate them out.  



## Challenges Faced
Below are some of the challenges we faced:

+ The major challenge was to identify the features which had to be used, we tried on with the different set of features and through cross-validation, we finalized the current list of features. We also had to avoid overfitting to the model to the dataset being used.
+ There was no as such annotated dataset available for twitter data, so we had to work with NLTK’s inbuilt dataset (like nps_chat, Brown) but these didn’t give the desired result. So, we had to create our own dataset and word2vec & fasttext model.
+ Understanding the Twitter dataset and the various fields in it.

