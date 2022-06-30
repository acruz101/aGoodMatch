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

### Results & Interpretation: Attribute Predictions
<img width="625" alt="Screen Shot 2022-06-30 at 11 57 38 AM" src="https://user-images.githubusercontent.com/47541514/176756726-4235c660-bfba-4d13-b0bc-f1bb04b37f94.png">



