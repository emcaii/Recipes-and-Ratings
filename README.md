# Whats the relationship between ingredient and calorie count?
DSC80
Emily Cai

## Introduction
To many, the worst part of cooking is grocery shopping. Long grocery lists and the pressure to stay healthy are common stresses within this process. One common intuition is that more complex recipes with longer ingredient lists are often more calorie-dense, or unhealthy. How true is this statement? **In this project, we explore the relationship between a recipe's ingredient list length and its caloric content.** Does a longer ingredient list actually mean higher calories, or is the relationship more complex?

To investigate this, we analyzed two real-world datasets consisting of recipes and ratings posted since 2008 on [food.com](https://www.food.com/). The original purpose of the datasets was for the recommender system research paper, [Generating Personalized Recipes from Historical User Preferences](https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf) by Majumder et al., which has become a widlely used benchmark for studying food. The first dataset, `recipes` has 83,782 unique recipes with 10 unique columns with the following information:
| Column             | Description                                                                                                                                                                                       |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `'name'`           | Recipe name                                                                                                                                                                                       |
| `'id'`             | Recipe ID                                                                                                                                                                                         |
| `'minutes'`        | Minutes to prepare recipe                                                                                                                                                                         |
| `'contributor_id'` | User ID who submitted this recipe                                                                                                                                                                 |
| `'submitted'`      | Date recipe was submitted                                                                                                                                                                         |
| `'tags'`           | Food.com tags for recipe                                                                                                                                                                          |
| `'nutrition'`      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'`        | Number of steps in recipe                                                                                                                                                                         |
| `'steps'`          | Text for recipe steps, in order                                                                                                                                                                   |
| `'description'`    | User-provided description                                                                                                                                                                         |
| `'ingredients'`    | Text for recipe ingredients                                                                                                                                                                       |
| `'n_ingredients'`  | Number of ingredients in recipe   

The second dataset, `ratings`, contains 731927 rows and each row contains a review from the user on a specific recipe. The columns it includes are:

| Column        | Description         |
| :------------ | :------------------ |
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction |
| `'rating'`    | Rating given        |
| `'review'`    | Review text         |

**Given the datasets, we are investigating whether greater ingredients is correlated with greater calories.** To facilitate the investigation of our question, we extracted 'calories'`from the `nutrition` column so we could effectively compare it against `n_ingredients` for exploratory data analysis and hypothesis testing.The most relevant columns to answer our question are `'calories'` and `'n_ingredients'`.
Understanding this relationship could provide valuable insights into how recipe complexity relates to energy content. This is particularly useful for users who want to estimate the healthiness of a meal without analyzing every nutrient label.


## Data Cleaning and Exploratory Data Analysis
To make our analysis of the dataset more efficient and convenient, we conducted the following data cleaning steps.

1. Left merge the recipes and interactions datasets on id and recipe_id.

   - This step helps match the unique recipes with their rating and review.

1. Check data types of all the columns.

   - This step helps us evaluate what data cleaning steps are appropriate for the dataset and if we need to conduct data type conversion.
   - | Column             | Description |
     | :----------------- | :---------- |
     | `'name'`           | object      |
     | `'id'`             | int64       |
     | `'minutes'`        | int64       |
     | `'contributor_id'` | int64       |
     | `'submitted'`      | object      |
     | `'tags'`           | object      |
     | `'nutrition'`      | object      |
     | `'n_steps'`        | int64       |
     | `'steps'`          | object      |
     | `'description'`    | object      |
     | `'ingredients'`    | object      |
     | `'n_ingredients'`  | int64       |
     | `'user_id'`        | float64     |
     | `'recipe_id'`      | float64     |
     | `'date'`           | object      |
     | `'rating'`         | float64     |
     | `'review'`         | object      |

1. Fill all ratings of 0 with np.nan.

   - Rating is on a scale from 1 to 5, 1 meaning the lowest rating while 5 means the highest rating. However, a rating of 0 indicates missing values in rating and skews the averages. To avoid bias in the ratings, we filled the value 0 with np.nan so that averages are unaffected by missing variables.

1. Add column `'average'` containing average rating per recipe.

   - Since a recipe can have numerous ratings from different users, we take an average of all the ratings to get a more comprehensive understanding of the rating of a given recipe.

The cleaned dataframe ended up with 234429 rows and 18 columns. Here are the first 5 rows of `food` of our cleaned dataframe for reference. Since there are many columns in the merged dataframe, we selected the most relevant to display here. 

|   | name                                | id     | minutes | contributor_id | date       | rating | average | calories |
|---|-------------------------------------|--------|---------|----------------|------------|--------|---------|----------|
| 0 | 1 brownies in the world best ever   | 333281 | 40.0    | 985201         | 2008-11-19 | 4.0    | 4.0     | 138.4    |
| 1 | 1 in canada chocolate chip cookies  | 453467 | 45.0    | 1848091        | 2012-01-26 | 5.0    | 5.0     | 595.1    |
| 2 | 412 broccoli casserole              | 306168 | 40.0    | 50969          | 2008-12-31 | 5.0    | 5.0     | 194.8    |
| 3 | 412 broccoli casserole              | 306168 | 40.0    | 50969          | 2009-04-13 | 5.0    | 5.0     | 194.8    |
| 4 | 412 broccoli casserole              | 306168 | 40.0    | 50969          | 2013-08-02 | 5.0    | 5.0     | 194.8    |

### Univariate Analysis

We examined the distribution of ingredient counts across all recipes in our dataset. We created a new dataframe `unique` to conduct analysis without repeats by dropping duplicates in the 'id' column. Each row with the same 'id' has the same 'calories' value so the drop does not affect the analysis. The plot demonstrates that the majority of recipes use between 4-13 ingredients and recipes with 20+ ingredients are quite rare. 
<iframe
  src="assets/ingredientshist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
