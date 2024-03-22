# Recipes Rating Analysis
By: Anish Kasam and Rushil Chandrupatla

## Introduction

There are many websites that allow users to publish recipes online and others to interact with them, posting their own reviews and ratings. In this project, we will explore trends and correlations present within a dataset containing recipe and rating data scraped from one such website, food.com. More specifically, the question we aim to answer is: Can we predict the average rating given to a recipe based on its attributes, including but not limited to the number of calories, duration to prepare, and amount of steps? We hope this question and project will provide insight into specific traits of recipes that tend to receive higher praise from the public and auto-determine ratings without requiring many responses to receive that rating.

Our final dataset for this question was derived from the combination of two datasets, recipes and interactions. The recipes dataset contained 83,782 unique recipes with information about the recipes, including steps, descriptions, number of minutes to prepare, and more. The interactions dataset contained 731,927 interactions that users had with the recipes, with columns such as rating, the date, and the actual review. Our final dataset had the original 83,782 recipes but also contained the average rating for the recipe, found from the interactions dataset. It had 13 columns, the relevant ones are described below:

- name: Name of the recipe
- minutes: Number of minutes it takes to prepare the dish
- nutrition: Contains information about number of macronutrients in dish
- n-steps: Number of steps in the recipe
- n_ingredients: Number of ingredients in the recipe
- rating: Average rating given to recipe on a scale of 1-5 where 5 is best


## Data Cleaning and Exploratory Data Analysis

### Data Cleaning and Preprocessing

*Creation of Final Dataset*
- We merged both datasets together to get the information into one dataset. We then determine the average rating per recipe and store it back into our rating column.
- We replaced missing values in the rating column with a 0 to use in future numerical analysis.
- We separated out the nutrition column into separate columns, one per macronutrient.

Here are the first few rows of the final dataset after all cleaning (only relevant columns shown for display purposes):

| name                                 |   n_steps |   n_ingredients |   calories |   sugar |   rating |
|:-------------------------------------|----------:|----------------:|-----------:|--------:|---------:|
| 1 brownies in the world    best ever |        10 |               9 |      138.4 |      50 |        4 |
| 1 in canada chocolate chip cookies   |        12 |              11 |      595.1 |     211 |        5 |
| 412 broccoli casserole               |         6 |               9 |      194.8 |       6 |        5 |
| millionaire pound cake               |         7 |               7 |      878.3 |     326 |        5 |
| 2000 meatloaf                        |        17 |              13 |      267   |      12 |        5 |

### Exploratory Data Analysis

We conducted an analysis of several of our variables to gain better insight into them for future tasks and answering our main question.

*Univariate Analysis*  
In our univariate analysis, we analyzed the distribution of number of calories.
<iframe
  src="assets/univariate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>  
It seems that the distribution is approximately Gaussian and right-skewed. This also shows us that the most common number of calories that recipes in our dataset have is around 130 - 230 calories.

*Bivariate Analysis*
In our bivariate analysis, we explored the relationship between (1) number of steps and calories and (2) number of ingredients and calories.
<iframe
  src="assets/bivariate1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>  
This plot shows a relatively increasing correlation between number of steps and number of calories; however, this is not a strong correlation and cannot be confirmed by just this graph.
<iframe
  src="assets/bivariate2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>  
This plot shows a relatively stronger increasting correlation between number of calories and number of ingredients; however, just like the previous graph, this doesn't seem like an overall strong correlation and cannot be confirmed with just this graph.

*Interesting Aggregates*
Here, we computed a pivot table to examine the number of calories in a recipe based on its number of steps and ingredients.
|   (0, 5] |   (5, 10] |   (10, 15] |   (15, 20] |   (20, 25] |   (25, 30] | (30, 40]   |
|---------:|----------:|-----------:|-----------:|-----------:|-----------:|:-----------|
|   193.65 |    272.4  |     328.5  |     389.05 |     413.1  |     505.2  | 338.2      |
|   244.25 |    322.8  |     390.8  |     452.25 |     534.4  |     459.5  | 766.3      |
|   286.3  |    347.65 |     444.15 |     567    |     608.15 |     660.2  | 555.9      |
|   395.35 |    316.15 |     469.25 |     583.7  |     580.2  |     956.6  | 1031.6     |
|   273    |    345.4  |     495.3  |     572.55 |     805.8  |     554    | n/a        |
|   192.8  |    262.75 |     540.3  |     827.4  |     600.15 |    1562.25 | n/a        |

This dataframe is indexed on the left by number of steps in the same bins as the columns: (0, 5], (5, 10], (10, 15] and so on. The columns shown are bins of number of ingredients, with the actual values being the median calories. Median was used as our aggregation function due to the presence of many outliers in this column, which would have impacted the mean value more. This table confirms our earlier suspicions that there seems to be an increasing correlation between number of steps and calories, especially when sorted by number of ingredients.

## Assessment of Missingness
In this section, we will assess the reasoning for some missing values in our dataset.

### NMAR Analysis
We believe that the rating column, which is missing 2609 values could be NMAR. Earlier in the project, we were instructed to replace rating values of 0 with np.NaN, which we believe is due to 0 being the default value of the rating and thus if it was left at 0, rating was not filled out. It is posssible that rating was not filled out because people are more likely to leave ratings for recipes that they either thoroughly liked or disliked but not as often if they just found it average. Another possibility is that recipes that are harder or niche are less likely to be tried by other people and thus will not have a rating value. In order to counterract this, additional data such as how difficult or niche a recipe is on some sort of quantitative scale could help explain this missingess and make the missing data MAR.

### Missingness Dependency
Here, we will test if the missingness of the rating column is dependent on the number of steps and amount of protein in the recipe. We will do this using permutation tests.

*Rating vs. Number of Steps*
Null Hypothesis: The distribution of the number of steps when rating is missing is the same as the distribution of the number of steps when rating is not missing.  
Alternate Hypothesis: The distribution of the number of steps when rating is missing is not the same as the distribution of the number of steps when rating is not missing.
We will use the absolute difference of means because both columns are numerical.  
<iframe
  src="assets/missing1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe> 
The above plot shows the simulated difference of means in number of steps when rating is and is not missing. It shows that the observed absolute difference in means is far greater than the distribution indicating that the results may be statistically significant. This was confirmed by our p-value of 0.0, which is less than our significance level of 0.05. Therefore, we conclude this test by rejecting the null, stating that the distributions of number of steps when rating is missing and when not are not the same. There could be many reasons for this, but we infer one may be that recipes with a lot of steps or very few steps are less likely to be replicated and rated, thus leading to missing values for those recipes.

*Rating vs. Protein Amount(PDV)*
Null Hypothesis: The distribution of protein amount when rating is missing is the same as the distribution of protein amount when rating is not missing.  
Alternate Hypothesis: The distribution of protein amount when rating is missing is not the same as the distribution of protein amount when rating is not missing.
We will use the absolute difference of means because both columns are numerical.  
<iframe
  src="assets/missing2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe> 
The above plot shows the simulated difference of means in protein amount when rating is and is not missing. It shows that the observed absolute difference in means is far roughly near the middle of the distribution indicating that the results are likely not statistically significant. This was confirmed by our p-value of 0.204, which is greater than our significance level of 0.05. Therefore, we conclude this test by failing to reject the null, stating that the distributions of number of steps when rating is missing and when not are the same and thus the missingess of rating is not dependent on the protein amount (pdv) in our dataset.

## Hypothesis Testing
