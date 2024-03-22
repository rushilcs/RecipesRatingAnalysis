# Recipe Ratings Analysis
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
  height="425"
  frameborder="0"
></iframe>  
It seems that the distribution is approximately Gaussian and right-skewed. This also shows us that the most common number of calories that recipes in our dataset have is around 130 - 230 calories.

*Bivariate Analysis*  
In our bivariate analysis, we explored the relationship between (1) number of steps and calories and (2) number of ingredients and calories.
<iframe
  src="assets/bivariate1.html"
  width="800"
  height="425"
  frameborder="0"
></iframe>  
This plot shows a relatively increasing correlation between number of steps and number of calories; however, this is not a strong correlation and cannot be confirmed by just this graph.
<iframe
  src="assets/bivariate2.html"
  width="800"
  height="425"
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
|   192.8  |    262.75 |     540.3  |     827.4  |     425.15 |    1562.25 | n/a        |

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
  height="425"
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
  height="425"
  frameborder="0"
></iframe> 
The above plot shows the simulated difference of means in protein amount when rating is and is not missing. It shows that the observed absolute difference in means is roughly near the middle of the distribution indicating that the results are likely not statistically significant. This was confirmed by our p-value of 0.204, which is greater than our significance level of 0.05. Therefore, we conclude this test by failing to reject the null, stating that the distributions of number of steps when rating is missing and when not are the same and thus the missingess of rating is not dependent on the protein amount (pdv) in our dataset.

## Hypothesis Testing

The question we tested for this part is: Do regular and complex recipes have the same number of calories? Here, we define complex recipes to be ones with more than 10 ingredients and 10 steps.  

Null Hypothesis: Complex and Simple recipes have the same number of calories.  
Alternate Hypothesis: Complex recipes have more calories.

We first plotted the distributions of number of calories of simple and complex recipes to get a better understanding of the data.
<iframe
  src="assets/hypo.html"
  width="800"
  height="425"
  frameborder="0"
></iframe>
The plot does not show a large difference in the two distributions, but we will move on with our test. Despite the distributions, there is a significant difference between the mean number of calories in simple and complex recipes (~180 calories) 
and thus we will use the absolute difference of means as the test statistic.
<iframe
  src="assets/hypo.html"
  width="800"
  height="425"
  frameborder="0"
></iframe>
After running our simulation, we find that our observed difference of means is far greater than our distribution, indicating that our results are not statistically significant. This is confirmed by our low p-value of 0.0, allowing us to reject the null at the 0.05 significance level. The results of the test appear to show that complex and simple recipes in our dataset tend to not have the same number of calories, but we cannot definitively say that complex recipes have more calories  than simple recipes because this is just a statistical analysis.

## Problem Identification
Here, we will be predicting the rating of a recipe from other relevant features. We identified these features as minutes, calories, n_steps, n_ingredients, total fat, and sugar. This is a regression problem and our response variable is rating. We chose this variable to predict because it has been a focus of our whole project and we wanted to see if the correlation between rating and the other columns was strong enough to build an accurate predictor for it. We felt that this would be useful for people whose recipes do not get many views as it takes a lot of people and ratings to determine an accurate rating for a recipe, which may not always be feasible to get. 

The metric we used to evaluate our model is RMSE, which we chose because it measures the root of the average difference in predicted versus actual values, allowing us to measure relatively how accurate our results are. We chose it over other metrics because it is still interpretable, with the RMSE having real meaning to our rating (ie. an RMSE of 0.5 means that the average difference in predicted versus actual values is 0.5^2^ = 0.25) and the same units.

At the time of prediction, we have access to all the features in our dataset except for rating itself, because rating is determined after the recipe is already published, meaning all the attributes about the recipe already exist.

## Baseline Model

Our model is a k-Neighbors Regressor with two features: minutes and calories. We used this model as the baseline because we thought recipes that took a similar amount of minutes and calories would have a similar rating. We transformed both of these features using a StandardScaler because a k-Neighbors Regressor calculates similarity via distance, and thus if the scales for both independent variables differed then it would affect the model prediction results/ Also, by standardizing we ensure that each point is taken into account by the model equally, as without features with larger variances would affect the model's prediction more than others. Both minutes and calories in theory would be quantitative continuous, but in the scope of our dataset and given how the data was collected, minutes is quantitative discrete and calories is quantitative continuous. Our model had a test RMSE of roughly 0.71, meaning that the average difference in predicted versus actual values is 0.71^2^ = ~ 0.50. While this seems low, ratings are given on a scale from 1-5 and thus being off by 0.5 on average is definitely a sizeable difference. Thus, we will not say our baseline model is "good", but just that it can roughly predict the rating.


## Final Model

For our final model, we engineered two new features: is_complex(), determined by if the number of steps or number of ingredients exceeeds 10 and is_unhealthy(), determined by if the percent daily value of total fat or sugar exceeds 100. We believed these new features would be tied to rating and thus would help improve the performance of our model by giving it more features to help predict rating on. We believed that more complex recipes would have higher ratings, as if the user included more steps it follows that the recipe was likely easier to follow or overall made a more complex dish that other users would be proud of, thus leading to higher ratings. We also thought that if the recipe was unhealthy, it was more likely to receive worse ratings as users likely wouldn't rate recipes that used an excessive amount of sugar or fat very highly and would prefer these to be more in moderation. 

We tested several different models and tuned them. These included Linear Regression, a Decision Tree Regressor, and a k-Neighbors Regressor.

Ultimately, our final model that performed the best on unseen test data was the Decision Tree Regressor. We tuned the hyperparameters max_depth and min_samples_split, and the most accurate ones for this tasked turned out to be 2 and 10, respectively. We found the most optimal hyperparameters using k-cross validation with 5 folds, which we carried out using GridSearchCV. We implemented this on all the above models mentioned and chose the Decision Tree Regressor because the test RMSE of 0.646 was the lowest of all of them. This gave us nearly a 0.7 decrease from our baseline model, and means that the average difference in predicted versus actual values is 0.646^2^ = ~ 0.41. This is better than our baseline model, but we will still not say that our final model is "good" because a 0.4 difference on average is still sizeable. However, this model is still able to roughly predict the rating, so if a 0.4 average difference is not significant to users, our model can be used for this task.

## Fairness Analysis

To continue with our recurring theme of complex vs non-complex recipes, we decided to do a fairness analysis to answer: "does our model perform the same for complex recipes and non-complex recipes?"

Since we made a regression model that predicted ratings, we chose the RMSE as our evaluation metric.
Our null and alternative hypothesis, along with our test statistic are:

Null Hypothesis: Our model is fair and its RMSE for complex and non-complex recipes is the same and any differences are due to random chance.
Alternative Hypothesis: Our model is unfair and its RMSE for complex recipes is lower than its RMSE for non-complex recipes. Since the lower the RMSE the better the model's performance, we're saying that the alternative hypothesis is that our model performs better for complex recipes.

Test Statistic: Since our evaluation criteria was RMSE, our test statistic will be the difference in RMSE's between complex recipes and non-complex recipes. 
We generated 1000 test statistics under the null hypothesis, and our p-value of seeing a test statistic as low or lower than our observed test statistic was 0.39



