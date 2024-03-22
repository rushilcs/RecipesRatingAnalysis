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
