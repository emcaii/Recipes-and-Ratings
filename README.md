# Whats the relationship between ingredient and calorie count?
DSC80
Emily Cai

##Introduction
To many, the worst part of cooking is grocery shopping. Long grocery lists and the pressure to stay healthy are common stresses within this process. One common intuition is that more complex recipes with longer ingredient lists are often more calorie-dense, or unhealthy. How true is this statement? **In this project, we explore the relationship between a recipe's ingredient list length and its caloric content.** Does a longer ingredient list actually mean higher calories, or is the relationship more complex?

To investigate this, we analyzed two real-world datasets consisting of recipes and ratings posted since 2008 on [food.com](https://www.food.com/). The original purpose of the datasets was for the recommender system research paper, [Generating Personalized Recipes from Historical User Preferences](https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf) by Majumder et al., which has become a widlely used benchmark for studying food. The first dataset, `RAW_recipes.csvg` has 83,782 unique recipes with 10 unique columns with the following information:
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
