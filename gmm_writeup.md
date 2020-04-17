One of the unsupervised models we used was a Gaussian Mixture Model (GMM). This method tried the binary classification option of the dataset, just trying to distinguish toxic from non toxic text. Overall, it did not seem to work well with the dataset we chose due to the following reasons:
The text, when vectorized with Count or TF/IDF, was a sparse matrix, and GMMs do not handle sparse data well.
GMMs also seem to much prefer datasets with a lower number of features (<100), but this one had about 190,000.
Converting such a large sparse dataset (about 130,000 by 190,000) caused timeout errors, so dimensionality reduction was needed (see PCA/TSVD). 
To try to fix  both of the issues above, PCA was attempted but it did not seem to have much of an effect on the accuracy. Another method was tried, called SelectKBest, which just orders the features according to a metric (we used the chi squared metric). Interestingly, this provided the best results, and even more suprisingly at lower dimensions: using between 10-100 features, around 93.5% accuracy was achieved, but too many more than that and the method gave similar results to PCA, around 90-91%. This is not good because with PCA it would just classify everything as non toxic, but 90% of the original data was non toxic so it was not an improvement at all. According to AIC methods, the ideal number of clusters is about 8 (when minimizing the metric) but this clearly does not work for binary classification. Linear Discriminant Analysis (LDA) was also attempted but did not work.

Some results:
method=SelectKBest, features=500:
accuracy: .9273 (missed 2320/31915), f1: .7988, precision: .4528
method=SelectKBest, features=400:
accuracy: .8812 (missed 3792/31915), f1: .7388, precision: .4116
method=TruncatedSVD, features=400:
accuracy: .9069 (missed 2971/31915), f1: .4756, precision: nan

Overall the TSVD appears to perform slightly better with the same number of features. However, note that this may just be because it is classifying everything as non toxic, which is about 90% of the dataset. Also, the f1 scores for the SelectKBest options are much higher but this may just be because TSVD's precision score doesnt exist because it classifies everything as just class 0.