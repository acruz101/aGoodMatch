# A Good Match
Used speed dating data to predict whether two people would be a good match for each other. Data includes attribute ratings on how each person rated the other person in a variety of different character traits ranging from attractiveness, intelligence, fun, ambition, sincerity, to shared interests.

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
