# A Good Match
For this project, I use speed dating data to predict whether two people would be a good match for each other. The purpose of this project is to improve the effectiveness and efficiency of the speed dating process. The data used for this project includes attribute ratings on how each person rated the other person after their date, on a variety of different character traits. Participants rated their date's attractiveness, intelligence, fun, ambition, sincerity, and shared interests.

## Study Motivation
In recent generations, speed dating and dating apps have become a common way for people to meet others and find a significant other. There are many factors involved in match making that can influence a person’s dating decisions, and the primary challenge in this space involves identifying mutually interested individuals out of millions of potential matches. 
As a result, this project focuses on finding a way to predict whether two people would be a good match for each other before they meet up, saving time and effort on both individual’s ends. This project uses attribute ratings representing how each person rated the other person in a variety of different character traits ranging from attractiveness, intelligence, fun, ambition, sincerity, to shared interests, and are all data that can be sourced from individuals independently. 
With these ratings and the respective match results, I hope to be able to make speed dating more effective by using our model to result in more matches and mutual interest across the board.

## Data Processing
+ The data used is sourced from a speed dating dataset collected by Fisman and Iyengar at Columbia University (source, documentation). 
+ For the purpose of this study, the data was relatively unusable in its raw form and required several steps of reformatting, filtering, and processing
+ The data cleaning involved the following:
  + Adding a  column to indicate a master’s degree using regex
  + Adding a college ranking column by sourcing external data
  + Filling NaN values with means/medians, or removing rows/columns with excessive NaNs
  + Creating “difference” columns: Since we reconstructed our data such that each row represents a pair instead of an individual, instead of using the raw values, for some columns, we computed the difference between the partner and the individual. For attribute rating predictions, we use the raw difference (since we would be interested in the direction of difference), and for matching predictions, we use the absolute value of the difference (since the direction of difference does not exist). e.g. Interest in Sports of the Individual - Interest in Sports of the Partner (or abs value if for matching)
  + Creating “matching” columns: Columns determining if certain categorical features matched between the partner and the individual. e.g. Partner finds religion important == Individual finds religion important
  + Removing columns with data collected after the meeting (invalid data for our use cases)
  + Merging both the individual’s and partner’s data into a single row    

## Data Analysis: Attribute Prediction Process
The first set of analytics models were developed to predict how an individual will rate six traits of a potential partner prior to meeting them in person: Attractiveness, Sincerity, Intelligence, Fun, Ambition, and Shared Interests. As suggested by course staff, I decided to try treat this data as both numeric with linear regression and categorical with linear discriminant analysis, since I was working with ordinal qualitative data (ratings from 0 to 10). I trained a total of 12 models, each with an appropriate set of data. I also performed feature engineering for each linear regression model, using VIF and p-values to test for multicollinearity and significance.

The ultimate goal with these models was to maximize the OSR2 for the linear regression model, and to maximize accuracy for the LDA models. To emphasize penalizing predictions that are further away from the true value (vs. those closer and incorrect), I use OSR2 for the regression model. I also utilize two methods of rescaling OSR2 (considering our response variable can only take on integer values between 0 and 10). The first rescaling method takes all the predicted values <0 and sets them to 0, and all the predicted values >10 and sets them to 10. Our second rescaling method linearly rescales and proportionally compresses all predictions to be between 0 and 10. 

After performing thorough feature engineering, I calculated the performance of the training set on the test set. I then performed bootstrapping to get a better sense of how well the model would perform on new data. From the bootstrapped results, I calculated 95% confidence intervals and means and plotted performance differences to help us understand how effective our models truly were.

## Results & Interpretation: Attribute Predictions

<img width="625" alt="Screen Shot 2022-06-30 at 11 57 38 AM" src="https://user-images.githubusercontent.com/47541514/176756726-4235c660-bfba-4d13-b0bc-f1bb04b37f94.png">

First, I will discuss the results and OSR2 performance of the linear regression models for predicting how an individual will rate each trait. The bootstrapped results indicate that the linear regression models for each trait perform poorly. For each trait prediction model, the mean of the bootstrapped OSR2 results are all close to 0, indicating poor performance on new data.  Compared to the OSR2 results of the baseline models, the linear regression models perform only slightly better with differences very close to 0. These results indicate that the percentage of the dependent variable variation that our linear model explains is small. Although I computed low OSR2 values for each linear regression model on each trait, independent variables for each of the final linear regression models are statistically significant. (Reference example of non-final model OLS results for attractiveness below.) Furthermore, after feature engineering, the independent variables for each of the final linear regression models compute low VIFs (below 5)–indicating no presence of multicollinearity. As such, the results of low OSR2 values for each linear regression model indicate poor performance for predicting how an individual will rate each trait. However, since each linear regression model results indicate statistically significant independent variables, there still may exist some relationship between our independent and response variables, albeit one that is not well captured by a linear model.

<img width="519" alt="Screen Shot 2022-06-30 at 12 00 57 PM" src="https://user-images.githubusercontent.com/47541514/176757273-d0b05c2e-665e-4ac6-bb1e-9f7d102c7d90.png">

Second, I will discuss the results and accuracy performance of the LDA models for predicting how an individual will rate each trait. The bootstrapped results indicate that the LDA models for each trait perform poorly as well. For each trait prediction model, the mean of the bootstrapped accuracy results are around 23%. Although this may seem high considering a 10 point scale, I note that the distribution of scores in our data are not uniform. Similar to linear regression performance, the LDA models perform only slightly better than accuracy results of our baseline models (predicting the most common rating, usually 6 or 7 with ~20% of the observations)–with differences very close to 0. Although I computed low accuracies for each LDA model on each trait, the confusion matrix results of each model suggest potentially usable predictions. Each LDA model's predictions tend to be close to the diagonal of the confusion matrix, where the diagonal represents a correct prediction. (Reference confusion matrix example below.) Since the data is ordinal, this suggests that the LDA output of each model may catch the general sentiment of the true value. Therefore, even although the raw accuracy is relatively low, the LDA model results may be sufficient enough for our use cases.

<img width="462" alt="Screen Shot 2022-06-30 at 12 02 26 PM" src="https://user-images.githubusercontent.com/47541514/176757489-0df97cd8-e709-49c4-a704-e5f46dbfe568.png">

Overall, neither the linear regression and LDA models perform well at predicting how an individual will rate six traits of a potential partner prior to meeting them in person: attractiveness, sincerity, intelligence, fun, ambition, and shared interests. This interpretation is based on OSR2 performance for the linear regression models and accuracy performance for the LDA models. These results may be partially attributed to the notion that the field of dating and how people rate others’ traits has an inherently great amount of unexplainable variation, in the sense that individuals may not have a realistic or consistent perception of themselves or what they want. Furthermore, given time constraints, these scores were unnormalized, and a score that may be low for one individual may be high for another (e.g. a pickier person might have a 6 be the high score, whereas a more generous scorer might have 6 as a low). These facets make it difficult, or perhaps even infeasible, to use such a model to predict these types of scores, especially when considering that the data itself may have a dubious relationship with the response variables.

Dataset: http://www.stat.columbia.edu/~gelman/arm/examples/speed.dating/

