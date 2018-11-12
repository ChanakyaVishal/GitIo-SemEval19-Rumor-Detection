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
**Text data** (for text analysis for detection of interrogative, supportive and denial sentences)
Is the text interrogative?
Is the text supportive?
Is the text denial?
**Cosine Similarity with root rumourous tweets**

## Dataset & Method for Extraction

The Twitter data set was in the form of a directory structure in which the tweets were classified on the basis of the topic and each topic had a sub-folder which consisted of a set of tweets which had one source and one or more replies. We used python os calls to walk through the directory structure and used pythons JSON library to extract the necessary fields. Then after extracting the fields, we did further processing for a few fields: 
We did basic preprocessing like removal of 
Procedure for Text Analysis and Sentiment Analysis 

For detecting whether the statement is interrogative, denial or support (for the tweet replies), we used a classifier 

### For Interrogative sentences
+ For detecting whether the statement is interrogative, denial or support (for the tweet replies), we used a classifier 

+ For Interrogative sentences (Query), we used the following procedure:
- We looked for the following words in the tweet :
'What', 'where', 'when', 'how', 'why', '?': If any of these words are present in the tweet then definitely it is query. 
'did', 'do', 'does', 'have', 'has', 'am', 'is', 'are', 'can', 'could', 'may', 'would', 'will', 'should', 'didn't', 'doesn't', 'haven't', 'isn't', 'aren't', 'can't', 'couldn't', 'wouldn't', 'won't', 'shouldn't' : The presence of these words at the start of the tweet implies that the tweet is a query as these are helping verbs and whenever helping verbs occur at the start of the sentence then it is an interrogative sentence.

+ In the case when none of the above two conditions are matched then we parse the sentence into a word2vec model which is trained on NLTK’s nps_chat corpus and get a vector which is further passed to a 15-class neural network based classifier.
We trained a 15 class neural net classifier using a Word2Vec model in which the various classes are:
Accept: 1, Bye: 2, Clarify: 3, Continuer: 4, Emotion: 5, Emphasis: 6, Greet: 7, Other: 8, Reject: 9, Statement: 10, System: 11, nAnswer: 12, whQuestion: 13, yAnswer: 14, ynQuestion: 15
 
### For Denial sentence
If the tweet is not a question the we do a cosine similarity between the reply tweets’ vector and the source tweets’ vector. We set a certain threshold of 0.25, if the similarity was lesser than this threshold then we considered it to be a statement of denial. The dissimilarity between the source tweet and the reply gives an insight to the intent of the reply, the similarity between the source tweet and the reply tweet would be very low if the they had a difference in opinion, hence a lower cosine similarity would imply a greater difference in opinion and thus we can infer that the reply tweet was in denial to the source tweet.

### For Supportive sentence
We did a similar procedure to that of a denial tweet detection. If the cosine similarity was greater than 0.5. Then we considered it to be a statement of support. 


If the result of classification is ynQuestion, whQuestion or Clairfy, then it is definitely a question.

### For Commentative tweets
If the threshold was between 0.25 and 0.5 then we considered the tweet to be of the commentative type.


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
The accuracy of our current model is ___.


## Challenges Faced
The major challenge was to identify the features which had to be used, we tried on with the different set of features and through cross-validation, we finalized the current list of features. We also had to avoid overfitting to the model to the dataset being used. Some other minor challenges include
Understanding the Twitter dataset and the various fields in it.
