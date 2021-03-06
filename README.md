# Identifying-Hate-Speech-in-Social-Media

## Introduction
Our aim in this project is to identify if a comment is toxic or not and flag toxic comments for removal. Toxic remarks are ubiquitous in Facebook pages, Instagram and YouTube comments, tweets and reddit threads. This will be extremely beneficial to social media companies (Facebook, YouTube, Tiktok) since it bypasses the need for someone to manually scrape out toxic content; our algorithm will help them do it in real-time. Researchers and social scientists can use the algorithm to identify patterns of hate speech and individuals who try to inflict them. Our model should work for generic conversations and using our machine learning algorithms we will eventually be able to build a robust system that can predict the level of toxicity on future unseen instances.

## Dataset Used
This project will utilize the "Jigsaw Unintended Bias in Toxicity Classification" dataset from Kaggle.com found at the following location: https://www.kaggle.com/c/jigsaw-unintended-bias-in-toxicity-classification/ . Jigsaw sponsored this effort and provided annotation of this data by human raters for various toxic conversational attributes. This dataset was extracted from the Civil Comments platform and public comments from their platform were available between September 2015 and November 2017. This data is stored in comma separated values (CSV) format consisting of 1.8 million records across 46 features. The features which provide the most valuable insight here are id, comment_text, and target.


### Install 

#### Checking required libraries
Keras - For building rnn model
```bash 
conda install -c conda-forge keras
```
nltk - for cleaning data
```bash 
conda install -c anaconda nltk
```
wordcloud : For visualizing words from train and test data.
```bash
conda install -c conda-forge wordcloud 
```
imbalanced-learn : Since the data is imbalanced towards positive class. we use imabalance-learn to create artificial samples
```bash
conda install -c conda-forge imbalanced-learn
```
gdown : downloading train data and glove embedding from google drive
```bash 
conda install -c conda-forge gdown 
```

#### Downloading Required train data and Golve Embedding files 
Dowloading files from google drive
```bash
gdown https://drive.google.com/uc?id=180hZuXbkeFmYuDkdWkdshOohbKm2-uSK

```


Extracing data files into data folder
```bash
unzip data.zip 
```
data/
 - filtered_test.csv                            : Cleaned samples for testing
 - train_filter.csv                             : Cleaned train data 
 - glove.twitter.27B.200d.gensim                : Glove Embeddings
 - glove.twitter.27B.200d.gensim.vectors.npy    : Glove Embeddings


## Conclusion

We have visualized certain instances of our toxic and nontoxic words using wordClouds. In the toxic comments, when we visualize it there isn???t a plethora of bad words, which was generally expected. So, we can attest that the model we are building isn???t a trivial one that just considers the presence or absence of toxic words, but rather builds upon the context of it. We have run a total of 5 different ML algorithms starting from the most simple Naive Bayes. Naive Bayes is generally expected to run well on textual data, because it has a tendency to capture the presence of core factors (here the toxic word vector). The shortcoming of Naive Bayes is that it fails to capture the context of the occurrence of the words. The reason we suspect to have gotten a low score on the under and oversampled synthetic data is because the number of the majority class is twice that of the minority class, affecting the Bayesian probability. We got a **mean F1-Score of 0.75, which forms our basis for comparison with other algorithms.**

SVM is known to be effective, however in the TF-IDF vectorizer, we are generating 10,000 top features, which makes our data very-high dimensional. Coupled with the fact that we have more than 200K records and computer resources at hand, we ran for a limited 40K records. **SVM performed decently well with a mean F1-Score of 0.839.**

Next we planned to run Decision Trees, which have the capability of identifying strong attributes. This will be helpful to identify the toxic attributes while classifying the record. We performed a plethora of hyperparameter tuning in Decision Trees. For 200 max_depth, we were able to get good scores. We tried using max_features to see if there was an improvement in F1 Score. However, it took a dip, but the model trained quickly. So we increased the depth to 500, but there wasn???t any significant improvement. So we tried using min_samples_split and changing the criterion to ???entropy??? and got an improvement but it was very slight. Then we bumped up the depth to 1000, still there wasn???t much improvement. So we decided that for max_depth=200, we have got a reasonably good model. Then, we bumped up size by using the one from **SMOTE and got the F1-score in cross-validation to be 0.86.**

Ideally, the next step would be to go to Random forests, since they???re an ensemble technique and use Decision trees for their base classifier. There is a huge scope for hyperparameter tuning, however we are significantly limited by resources to do a GridSearchCV on this. So we chose the **n_estimators to be 500 for our Random Forest and got a good F1-Score of 0.89.**

Then we took a turn towards Logistic regression, which is empirically known to perform well for text data sets. There isn???t any hyperparameter tuning to be done. **LR** performed extremely well, which took us a bit by surprise, but we believe it???s able to capture the toxic words' probability distribution quite well. We got a **mean F1-score of 0.90.**

For textual data prediction, deep learning has been the industry standard, which we wanted to test and replicate. **LSTM** is known to perform well on sequential data given it???s capability to remember the context of the words. We have used an empirically known ???adam??? optimizer that converges faster. We used two models, first where we trained the model with a **self-trained embedding layer and achieved a high F1-score of 0.91**. In the second model we leveraged the power of transfer learning, we retrained the model using the **Glove embedding layer** prepared from Twitter data. We achieved a minor increase in F1-Score which resulted in **our best F1 score 0.913, an increase of 0.3% from LSTM.**








