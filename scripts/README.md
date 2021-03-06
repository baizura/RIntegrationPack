## Off-the-shelf R scripts
These ready-to-use R scripts can easily be added to your R-enabled MicroStrategy environment. Simply save them to your desktop or Intelligence Server and reference them with a MicroStrategy metric.

#### Contents
| Forecasting                          | Classification                             | Descriptive                      |
| :----------------------------------- | :----------------------------------------- | :------------------------------- |
| [ARIMA][arima]                       | [k-Nearest Neighbors][knn]                 | [k-Means Clustering][kcluster]   |
| [Seasonal Forecasting][seasforecast] | [Neural Network][nn]                       | [k-Medoids Clustering][kmedoids] |
| [Stepwise Regression][stepreg]       | [Naive Bayes][nb]                          | [Pairwise Correlation][pairwise] |
| [Survival Analysis][survival]        | [Random Forest][rf]                        | [Sentiment Analysis][sentiment]  |
|                                      | [Stepwise Logistic Regression][steplogreg] |                                  |


## ARIMA

<img src="https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_ARIMA.png" width="250">

A time-series method that uses the Auto-Regressive Integrated Moving Average (ARIMA) model for forecasting values. It leverages the “auto.arima” function from R’s “forecast” package to search through a variety of ARIMA configurations in order to find the best one. This script generates the forecast value and confidence intervals, nominally set at 80% and 95%.

| Output | MicroStrategy Metric Expression |
| :----- | :------------------------------ |
| Forecasted value | `RScript<_RScriptFile="ARIMA.R", _InputNames="Target", SortBy=(Month), _Params="CycleLength=12, Horizon=12, Conf1=80, Conf2=95, ImageName='', FileName=''">(Target)` |
| First value of the lower confidence band | `RScript<_RScriptFile="ARIMA.R", _InputNames="Target", _OutputVar="ForecastLo1", SortBy=(Month), _Params="CycleLength=12, Horizon=12, Conf1=80, Conf2=95, ImageName='', FileName=''">(Target)` |
| Second value of the lower confidence band | `RScript<_RScriptFile="ARIMA.R", _InputNames="Target", _OutputVar="ForecastLo2", SortBy=(Month), _Params="CycleLength=12, Horizon=12, Conf1=80, Conf2=95, ImageName='', FileName=''">(Target)`|
| First value of the upper confidence band | `RScript<_RScriptFile="ARIMA.R", _InputNames="Target", _OutputVar="ForecastHi1", SortBy=(Month), _Params="CycleLength=12, Horizon=12, Conf1=80, Conf2=95, ImageName='', FileName=''">(Target)`|
| Second value of the upper confidence band | `RScript<_RScriptFile="ARIMA.R", _InputNames="Target", _OutputVar="ForecastHi2", SortBy=(Month), _Params="CycleLength=12, Horizon=12, Conf1=80, Conf2=95, ImageName='', FileName=''">(Target)`|

* [RScript][arima_script]
* [Documentation][arima_doc]
* [Back to the top][lnk_top]
<br></br>

## k-Means Clustering

<img src="https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_kMeans.PNG" width="250">

A clustering algorithm that separates observations into k-number of clusters, where each cluster is defined by its mean. The algorithm seeks to minimize the variance within each cluster; this is interpreted as observations belonging to the cluster with the "nearest mean".

| Output | MicroStrategy Metric Expression |
| :----- | :------------------------------ |
| Cluster to which each record belongs | `RScript<_RScriptFile="kMeansClustering.R", _InputNames="Vars", _Params="Exact_k=4, Max_k=10, FileName='', Seed=42">(Vars)` |

* [RScript][kcluster_script]
* [Documentation][kcluster_doc]
* [Back to the top][lnk_top]
<br></br>


## k-Medoids Clustering

<img src="https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_kMedoids.PNG" width="250">

A clustering algorithm that separates observations into k-number of clusters, where each cluster is defined by its medoid, a point within the cluster that is on average the least dissimilar to all the observations in the cluster. The algorithm seeks to minimize the absolute distance between the observations and the medoid of each cluster.

| Output | MicroStrategy Metric Expression |
| :----- | :------------------------------ |
| Cluster to which each record belongs | `RScript<_RScriptFile="kMedoidsClustering.R", _InputNames="Vars", _Params="Exact_k=4, Max_k=10, FileName='', Seed=42">(Vars)` |
| Cluster if a record is the medoid of that cluster, 0 otherwise | `RScript<_RScriptFile="kMedoidsClustering.R", _InputNames="Vars", _OutputVar="Medoids", _Params="Exact_k=4, Max_k=10, FileName='', Seed=42">(Vars)` |

* [RScript][kmedoids_script]
* [Documentation][kmedoids_doc]
* [Back to the top][lnk_top]
<br></br>


## k-Nearest Neighbors
A classification algorithm which assigns observations to a known classification group. Classifications for the test set are made by determining the k nearest observations in the training dataset, known as neighbors, and assigning the observation to a group based on the majority vote amongst those neighbors.

| Output | MicroStrategy Metric Expression |
| :----- | :------------------------------ |
| Predicted class as a string | `RScript<_RScriptFile="kNN.R", _InputNames="ID, Target, Training, Vars", _Params="TrainIncluded=TRUE, k=1, FileName='kNN', Seed=42">(ID, Target, Training, Vars)` |
| Predicted class as a number   | `RScript<_RScriptFile="kNN.R", _InputNames="ID, Target, Training, Vars", _OutputVar="ClassId", _Params="TrainIncluded=TRUE, k=1, FileName='kNN', Seed=42">(ID, Target, Training, Vars)` |

* [RScript][knn_script]
* [Documentation][knn_doc]
* [Back to the top][lnk_top]
<br></br>


## Naive Bayes

<img src="https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_NaiveBayes.PNG" width="250">

Naïve Bayes, also known as simple Bayes, is a classification technique that makes the assumption that the effect of the value of each variable feature is independent from all other features for each classification; this is known as naive independence. For each independent variable, the algorithm calculates the conditional likelihood of each potential classification given the value for each feature and multiplies the effects together to determine the probability that an observation belongs to each classification. The classification with the highest probability is returned as the predicted class.

| Output | MicroStrategy Metric Expression |
| :----- | :------------------------------ |
| Predicted class as a string | `RScript<_RScriptFile="NaiveBayes.R", _InputNames="Target, Vars", _Params="TrainMode=TRUE, FileName='NaiveBayes', Correction=1">(Target, Vars)` |
| Predicted class as a number | `RScript<_RScriptFile="NaiveBayes.R", _InputNames="Target, Vars", _OutputVar="ClassId", _Params="TrainMode=TRUE, FileName='NaiveBayes', Correction=1">(Target, Vars)` |

* [RScript][nb_script]
* [Documentation][nb_doc]
* [Back to the top][lnk_top]
<br></br>


## Neural Networks

<img src="https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_NeuralNetwork.PNG" width="250">

Neural networks are an advanced machine learning technique inspired by the innerworkings of the human brain. A neural network consists of “neurons”, weights, and an activation function. Data is passed from each layer of the network to the output layer through a series of weights and transformations defined by the activation function.

| Output | MicroStrategy Metric Expression |
| :----- | :------------------------------ |
| Predicted class as a string | `RScript<_RScriptFile="NeuralNetwork.R", _InputNames="Target, Vars", _Params="FileName='NeuralNetwork', TrainMode=TRUE, NumLayer=3, Seed=42">(Target, Vars)` |
| Predicted class as a number | `RScript<_RScriptFile="NeuralNetwork.R", _InputNames="Target, Vars", _OutputVar="ClassId", _Params="FileName='NeuralNetwork', TrainMode=TRUE, NumLayer=3, Seed=42">(Target, Vars)` |

* [RScript][nn_script]
* [Documentation][nn_doc]
* [Back to the top][lnk_top]
<br></br>


## Pairwise Correlation

<img src="https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_PairwiseCorr.PNG" width="250">

Pairwise correlation is a measure of the direction and strength of the relationship between pairs of metrics with respect to each other. This R script produces a correlation plot and a correlation table containing correlations of the variables when taken in pairs.

| Output | MicroStrategy Metric Expression |
| :----- | :------------------------------ |
| Correlation matrix plot | `RScript<_RScriptFile="PairwiseCorr.R", _InputNames="Labels, Vars", _Params="HasLabels=TRUE, WindowSize=0, ImageName='PairwiseCorr', FileName='PairwiseCorr'">(Labels, Vars)` |

* [RScript][pairwise_script]
* [Documentation][pairwise_doc]
* [Back to the top][lnk_top]
<br></br>


## Random Forest

<img src="https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_RandomForest.PNG" width="250">

Random Forest is a machine learning technique wherein numerous, independent decision trees are trained on randomized subsets of the training data in order to reduce overfitting. Data is passed into each individual decision tree for classification, and the class that is predicted by the majority of those decision trees is returned as the predicted class for that record.

| Output | MicroStrategy Metric Expression |
| :----- | :------------------------------ |
| Predicted class as a string | `RScript<_RScriptFile="RandomForest.R", _InputNames="Target, Vars", _Params="TrainMode=TRUE, FileName='RandomForest', NumTree=750, NumVar=3, Seed=42">(Target, Vars)` |
| Predicted class as a number | `RScript<_RScriptFile="RandomForest.R", _InputNames="Target, Vars", _OutputVar="ClassId", _Params="TrainMode=TRUE, FileName='RandomForest', NumTree=750, NumVar=3, Seed=42">(Target, Vars)`|

* [RScript][rf_script]
* [Documentation][rf_doc]
* [Back to the top][lnk_top]
<br></br>


## Seasonal Forecasting

<img src="https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_SeasonalForecast.PNG" width="250">

A time-series forecast method that uses R’s ordinary least squares regression algorithm to fit a function that captures the trend and seasonal variability of numerical data and can be used predict future values.

| Output | MicroStrategy Metric Expression |
| :----- | :------------------------------ |
| Forecasted value | `RScript<_RScriptFile="SeasonalForecasting.R", _InputNames="Target, Trend, Season", _Params="FileName=''">(Target, Trend, Season)` |

* [RScript][seasforecast_script]
* [Documentation][seasforecast_doc]
* [Back to the top][lnk_top]
<br></br>


## Sentiment Analysis

<img src="https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_SentimentAnalysis.png" width="250">

Sentiment analysis aims to measure the attitude of a writer’s words within a given text. It is an application of text mining and is used to extract sentiment from social media sources such as survey results, Twitter tweets, and Facebook comments. This metric uses R’s tidytext package to analyze text for sentiment by associating each word with classifications from two lexicons and returns the sentiment analyses as 14 different results, each can be represented by a MicroStrategy metric. In addition to these results which are returned “in-band” to MicroStrategy metrics in a report, dashboard or document, this metric optionally can persist output “out-of-band” to the file system, including a result table as a comma-separated-value file, a word cloud and a sentiment score histogram.

| Output | MicroStrategy Metric Expression |
| :----- | :------------------------------ |
| Sentiment Score (numeric, negative/positive) | `RScript<_RScriptFile="SentimentAnalysis.R", _InputNames="Text", _Params="FileName='SA_mstr', PlotWordCloud=TRUE, PlotHistogram=TRUE, RemoveRetweets=TRUE, SaveCSV=TRUE">(Text)` |
| Sentiment Grade (string, negative/positive) | `RScript<_RScriptFile="SentimentAnalysis.R", _InputNames="Text", _OutputVar="Grade", _Params="FileName='SA_mstr', PlotWordCloud=TRUE, PlotHistogram=TRUE, RemoveRetweets=TRUE, SaveCSV=TRUE">(Text)` |
| Anger (numeric, word count) | `RScript<_RScriptFile="SentimentAnalysis.R", _InputNames="Text", _OutputVar="Grade", _Params="FileName='SA_mstr', PlotWordCloud=TRUE, PlotHistogram=TRUE, RemoveRetweets=TRUE, SaveCSV=TRUE">(Text)` |
| Anger (numeric, word count) |	`RScript<_RScriptFile="SentimentAnalysis.R", _InputNames="Text", _OutputVar="Grade", _Params="FileName='SA_mstr', PlotWordCloud=TRUE, PlotHistogram=TRUE, RemoveRetweets=TRUE, SaveCSV=TRUE">(Text)` | 
| Anticipation (numeric, word count) | `RScript<_RScriptFile="SentimentAnalysis.R", _InputNames="Text", _OutputVar="Grade", _Params="FileName='SA_mstr', PlotWordCloud=TRUE, PlotHistogram=TRUE, RemoveRetweets=TRUE, SaveCSV=TRUE">(Text)`| 
| Disgust (numeric, word count) | `RScript<_RScriptFile="SentimentAnalysis.R", _InputNames="Text", _OutputVar="Grade", _Params="FileName='SA_mstr', PlotWordCloud=TRUE, PlotHistogram=TRUE, RemoveRetweets=TRUE, SaveCSV=TRUE">(Text)`|	
| Fear (numeric, word count) | `RScript<_RScriptFile="SentimentAnalysis.R", _InputNames="Text", _OutputVar="Grade", _Params="FileName='SA_mstr', PlotWordCloud=TRUE, PlotHistogram=TRUE, RemoveRetweets=TRUE, SaveCSV=TRUE">(Text)`| 
| Joy (numeric, word count) | `RScript<_RScriptFile="SentimentAnalysis.R", _InputNames="Text", _OutputVar="Grade", _Params="FileName='SA_mstr', PlotWordCloud=TRUE, PlotHistogram=TRUE, RemoveRetweets=TRUE, SaveCSV=TRUE">(Text)`|
| Negative (numeric, word count) | `RScript<_RScriptFile="SentimentAnalysis.R", _InputNames="Text", _OutputVar="Grade", _Params="FileName='SA_mstr', PlotWordCloud=TRUE, PlotHistogram=TRUE, RemoveRetweets=TRUE, SaveCSV=TRUE">(Text)`|
| Positive (numeric, word count) | `RScript<_RScriptFile="SentimentAnalysis.R", _InputNames="Text", _OutputVar="Grade", _Params="FileName='SA_mstr', PlotWordCloud=TRUE, PlotHistogram=TRUE, RemoveRetweets=TRUE, SaveCSV=TRUE">(Text)`|
| Sadness (numeric, word count)	| `RScript<_RScriptFile="SentimentAnalysis.R", _InputNames="Text", _OutputVar="Grade", _Params="FileName='SA_mstr', PlotWordCloud=TRUE, PlotHistogram=TRUE, RemoveRetweets=TRUE, SaveCSV=TRUE">(Text)`| 
| Surprise (numeric, word count) | `RScript<_RScriptFile="SentimentAnalysis.R", _InputNames="Text", _OutputVar="Grade", _Params="FileName='SA_mstr', PlotWordCloud=TRUE, PlotHistogram=TRUE, RemoveRetweets=TRUE, SaveCSV=TRUE">(Text)`|
| Trust (numeric, word count) | `RScript<_RScriptFile="SentimentAnalysis.R", _InputNames="Text", _OutputVar="Grade", _Params="FileName='SA_mstr', PlotWordCloud=TRUE, PlotHistogram=TRUE, RemoveRetweets=TRUE, SaveCSV=TRUE">(Text)`|
| WordCount (numeric, words with sentiment)	| `RScript<_RScriptFile="SentimentAnalysis.R", _InputNames="Text", _OutputVar="Grade", _Params="FileName='SA_mstr', PlotWordCloud=TRUE, PlotHistogram=TRUE, RemoveRetweets=TRUE, SaveCSV=TRUE">(Text)`|
| TotalWordCount (numeric, all words) | `RScript<_RScriptFile="SentimentAnalysis.R", _InputNames="Text", _OutputVar="Grade", _Params="FileName='SA_mstr', PlotWordCloud=TRUE, PlotHistogram=TRUE, RemoveRetweets=TRUE, SaveCSV=TRUE">(Text)`|

* [RScript][sentiment_script]
* [Documentation][sentiment_doc]
* [Back to the top][lnk_top]
<br></br>


## Stepwise Regression
A type of linear regression in which variables are only included in the model if they have a significant effect. Initially all variables are included and are eliminated one-by-one if a variable is statistically insignificant according to the specified significance criterion until the remaining model only includes variables which are deemed significant. This specification uses the Aikaike information criterion as the determination criterion. 

| Output | MicroStrategy Metric Expression |
| :----- | :------------------------------ |
| Forecasted value | `RScript<_RScriptFile="StepwiseRegression.R", _InputNames="Target, Vars", _Params="FileName='StepwiseRegression', Stepwise=TRUE">(Target, Vars)` |

* [RScript][stepreg_script]
* [Documentation][stepreg_doc]
* [Back to the top][lnk_top]
<br></br>


## Stepwise Logistic Regression
A type of regression in which the variable being forecasted is categorical ie. a class. This script creates a logistic regression model in which variables are only included in the model if they have a significant effect. Initially all variables are included and are eliminated one-by-one if a variable is statistically insignificant according to the specified significance criterion until the remaining model only includes variables which are deemed significant. This specification uses the Aikaike information criterion as the determination criterion. 

| Output | MicroStrategy Metric Expression |
| :----- | :------------------------------ |
| Probability of the target class | `RScript<_RScriptFile="StepwiseLogistic.R", _InputNames="Target, Vars", _Params="FileName='StepwiseLogistic', Stepwise=TRUE">(Target, Vars)` |

* [RScript][steplogreg_script]
* [Documentation][steplogreg_doc]
* [Back to the top][lnk_top]
<br></br>


## Survival Analysis
Survival Analysis can be used to predict the probability of an event occuring over time, such as a component failure or a customer being lost. This script uses the Cox Regression algorithm to quantify the effect of each independent variable on the likelihood that an event will occur at some point in the future.

| Output | MicroStrategy Metric Expression |
| :----- | :------------------------------ |
| Probability of an event occurring | `RScript<_RScriptFile="Survival.R", _InputNames="Time, Status, Vars", _Params="TrainMode=FALSE, FileName='Survival'">(Time, Status, Vars)`|

* [RScript][survival_script]
* [Documentation][survival_doc]
* [Back to the top][lnk_top]
<br></br>


## How to use these scripts in a MicroStrategy dashboard
These R metrics are deployed to MicroStrategy as metrics by copying the metric expressions provided here into any MicroStrategy Metric Editor. The shelf includes metric expressions for Version 1 of the R Integration Pack as well as variations that take advantage of the new features in Version 2.

Version 2 of the R Integration Pack makes it even simpler for end-users to control the metric's execution thanks to the _Params parameter that allows function parameters to be referenced by name using a string of name-value pairs. This is in addition to the original set of 27 pre-defined parameters (9 boolean, 9 numeric and 9 string).

After you've copied the metric expression for the metric you wish to deploy:
1. Paste the Metric Expression into any MicroStrategy metric editor
2. Match metric's inputs and function parameters for your application
3. Name and save the metric so it can be added to any MicroStrategy report or dashboard

## Disclaimers
This page provides programming examples. MicroStrategy grants you a nonexclusive copyright license to use all programming code examples from which you can use or generate similar function tailored to your own specific needs. All sample code is provided for illustrative purposes only. These examples have not been thoroughly tested under all conditions. MicroStrategy, therefore, cannot guarantee or imply reliability, serviceability, or function of these programs. All programs contained herein are provided to you "AS IS" without any warranties of any kind. The implied warranties of non-infringement, merchantability and fitness for a particular purpose are expressly disclaimed.


[lnk_top]: <https://github.com/MicroStrategy/RIntegrationPack/tree/master/scripts#off-the-shelf-r-scripts>

[arima]: <https://github.com/MicroStrategy/RIntegrationPack/tree/master/scripts#arima>
[arima_script]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/ARIMA.R>
[arima_doc]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/ARIMA.pdf>
[arima_img]: https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_ARIMA.png

[kcluster]: <https://github.com/MicroStrategy/RIntegrationPack/tree/master/scripts#k-means-clustering>
[kcluster_script]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/kMeansClustering.R>
[kcluster_doc]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/kMeansClustering.pdf>
[kcluster_img]: https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_kMeans.PNG

[kmedoids]: <https://github.com/MicroStrategy/RIntegrationPack/tree/master/scripts#k-medoids-clustering>
[kmedoids_script]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/kMedoidsClustering.R>
[kmedoids_doc]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/kMedoidsClustering.pdf>
[kmedoids_img]: https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_kMedoids.PNG

[knn]: <https://github.com/MicroStrategy/RIntegrationPack/tree/master/scripts#k-nearest-neighbors>
[knn_script]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/kNN.R>
[knn_doc]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/kNN.pdf>

[nb]: <https://github.com/MicroStrategy/RIntegrationPack/tree/master/scripts#naive-bayes>
[nb_script]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/NaiveBayes.R>
[nb_doc]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/NaiveBayes.pdf>
[nb_img]: https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_NaiveBayes.PNG

[nn]: <https://github.com/MicroStrategy/RIntegrationPack/tree/master/scripts#neural-network>
[nn_script]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/NeuralNetwork.R>
[nn_doc]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/NeuralNetwork.pdf>
[nn_img]: https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_NeuralNetwork.PNG

[pairwise]: <https://github.com/MicroStrategy/RIntegrationPack/tree/master/scripts#pairwise-correlation>
[pairwise_script]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/PairwiseCorr.R>
[pairwise_doc]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/PairwiseCorr.pdf>
[pairwise_img]: https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_PairwiseCorr.PNG

[rf]: <https://github.com/MicroStrategy/RIntegrationPack/tree/master/scripts#random-forest>
[rf_script]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/RandomForest.R>
[rf_doc]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/RandomForest.pdf>
[rf_img]: https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_RandomForest.PNG

[seasforecast]: <https://github.com/MicroStrategy/RIntegrationPack/tree/master/scripts#seasonal-forecasting>
[seasforecast_script]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/SeasonalForecasting.R>
[seasforecast_doc]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/SeasonalForecasting.pdf>
[seasforecast_img]: https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_SeasonalForecast.PNG

[stepreg]: <https://github.com/MicroStrategy/RIntegrationPack/tree/master/scripts#stepwise-regression>
[stepreg_script]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/StepwiseRegression.R>
[stepreg_doc]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/StepwiseRegression.pdf>

[steplogreg]: <https://github.com/MicroStrategy/RIntegrationPack/tree/master/scripts#stepwise-logistic-regression>
[steplogreg_script]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/StepwiseLogistic.R>
[steplogreg_doc]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/StepwiseLogistic.pdf>

[survival]: <https://github.com/MicroStrategy/RIntegrationPack/tree/master/scripts#survival-analysis>
[survival_script]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/Survival.R>
[survival_doc]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/Survival.pdf>
[survival_img]: https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_Survival.PNG

[sentiment]: <https://github.com/MicroStrategy/RIntegrationPack/tree/master/scripts#sentiment-analysis>
[sentiment_script]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/SentimentAnalysis.R>
[sentiment_doc]: <https://github.com/MicroStrategy/RIntegrationPack/blob/master/scripts/SentimentAnalysis.pdf>
[sentiment_img]: https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_SentimentAnalysis.png

[img_howtouse]: https://github.com/MicroStrategy/RIntegrationPack/blob/master/assets/RShelf_Params3.png