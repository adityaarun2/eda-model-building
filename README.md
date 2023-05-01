# The Great Recipe Rating Race: Exploring the Connection between Cooking Ingredients and Recipe Ratings

---

## Introduction

Welcome to the exploratory data analysis project on the relationship between cooking ingredients and average rating of recipes! In this project, we will aim to investigate whether there is a correlation between the complexity of a recipe and how highly it is rated by users.

The dataset used for this analysis is collected from <a href='food.com'>food.com</a>. and contains information on a variety of recipes, including their ingredients, cooking times, and user ratings. By exploring this data, we hope to gain insights into the factors that contribute to a recipe’s success and popularity.

We initially started with two DataFrames: ``raw_recipes``, which contains information about recipes and their details, and ``interactions``, which contains information about user reviews and ratings for each recipe. We merged the two datasets on the recipe ``id`` and calculated the average rating for each recipe using aggregate statistics. Finally, we merged the Series containing the average rating for each recipe with the original ``raw_recipes`` DataFrame in the ``avg_rating`` column. This resulted in the ``recipes`` DataFrame which we will use for the rest of the project. 

<center><img src="recipe.jpeg" alt="Picture of a mobile phone and food" height="400" width="500"></center>

### Question

The question we are going to investigate with this data is: Do recipes with a higher number of ingredients (complex recipes) receive higher ratings on average than those with fewer ingredients (simple recipes)?

The reason we decided on this question is because we want to investigate whether people tend to rate more complicated recipes higher compared to simpler recipes. This would be important to consider when looking at the data above because the values of the ratings for a given recipe may be correlated with its complexity (in this case, the complexity is the number of ingredients since recipes with more ingredients are usually more complicated). However, complicated recipes might be more rewarding and ultimately a tastier dish compared to simpler recipes. After considering these nuances, answering this question can be helpful in determining why a certain recipe is rated a certain way.

<center><img src="food critic.jfif" alt="Picture of a food critic" height="500" width="500"></center>

Another important aspect of this question is determining the cutoff for a low or high recipe. In order to find this value, we decided to plot the distribution of the ``n_ingredients`` column so we could get a general sense of how the recipes in this dataset compare to one another in terms of their ingredient count. The histogram we generated is included below in the Cleaning and EDA section. As you can see, the median of this distribution is around 9. Therefore, we decided to classify recipes with an ingredient count of **9 or less as simple** and recipes with an ingredient count **greater than 9 as complex**. Additionally, choosing the median as the cutoff would help eliminate any sampling bias since we would have an equal amount of data on either side of the median (the median is the 50th percentile). Lastly, 9 ingredients is intuitively a good cutoff. It is reasonable to assume, in general, that a dish with less than 9 ingredients should be fairly simple and easy to make compared to dishes with 10 or more ingredients.

### About the data

The primary dataset used for this analysis is a <a href="food.com">food.com</a> dataset, which contains information on 83,781 recipes, including ingredients, cooking times, and user ratings.
The dataset contains 13 columns which are the following:


| column     |   description |
|:------------|--------:|
| name   |       the name of the recipe |
| id |     the unique recipe id in the website |
| minutes | time taken in minutes to cook recipe |
| contributor_id |     the unique id of the recipe contributor |
| submitted |       date of recipe submission |
| tags   |      keywords linked to the recipe for easy query |
| nutrition | nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)] |
| n_steps |  number of steps in recipe  |
| steps | text for recipe steps, in order|
| description | user-provided description |
| ingredients	| ingredients required for recipe |
| 


gredients |	number of ingredients required for recipe |
| avg_rating | average rating given by users |

We used Python and the following libraries to preprocess and analyze the data:

Pandas: for data manipulation and cleaning \
Plotly: for data visualization \
Scipy: for statistical analysis and hypothesis testing

### Why we replace 0 with NaN

The reason why we should replace the ratings of value 0 with NaN is because these ratings are often missing values from people who want to simply comment or forgot to leave an actual review. Thus, we wouldn't want extreme missing values, such as 0, to significantly alter our mean. By replacing 0's with NaN, Pandas automatically ignores the missing values and calculates the mean on the rest of the ratings.

---

## Cleaning and EDA (Exploratory Data Analysis)

<center><img src="Data-Cleaning.jpeg" alt="Picture of a sweeper cleaning data" height="500" width="500"></center>

### Data Cleaning

Here, we remove an outlier recipe that is not really a recipe, but rather dating advice. We also decided to split the ``nutrition`` column into the respective categories mentioned in the data dictionary, and convert them into ``float`` values. Then, we converted the tags and ingredients columns to actual arrays since they are currently represented as a strings. In order for us to conduct our hypothesis testing, we also need a way to classify dishes as ``simple`` or ``complex``. To do this, we have added a ``complexity`` column which will help us classify a dish as either simple or complex based on a cutoff value of 9 ingredients, as explained above. Lastly, we replaced missing values in columns such as the ``description`` column with ``NaN`` in order for us to properly conduct our missingness tests.

| name                                 |     id |   minutes |   contributor_id | submitted   | tags                                                                                                                                                                                                                                                                                                                                                          | nutrition                                                         |   n_steps | steps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | description                                                                                                                                                                                                                                                                                                                                                                       | ingredients                                                                                                                                                                                                                             |   n_ingredients |   avg_rating | complexity   |
|:-------------------------------------|-------:|----------:|-----------------:|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------|----------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-------------:|:-------------|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings']                                                                                          | ['138.4', ' 10.0', ' 50.0', ' 3.0', ' 3.0', ' 19.0', ' 6.0']      |        10 | ['heat the oven to 350f and arrange the rack in the middle', 'line an 8-by-8-inch glass baking dish with aluminum foil', 'combine chocolate and butter in a medium saucepan and cook over medium-low heat , stirring frequently , until evenly melted', 'remove from heat and let cool to room temperature', 'combine eggs , sugar , cocoa powder , vanilla extract , espresso , and salt in a large bowl and briefly stir until just evenly incorporated', 'add cooled chocolate and mix until uniform in color', 'add flour and stir until just incorporated', 'transfer batter to the prepared baking dish', 'bake until a tester inserted in the center of the brownies comes out clean , about 25 to 30 minutes', 'remove from the oven and cool completely before cutting']                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!                                                                                                              | ['bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour']                                                          |               9 |            4 | simple       |
| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11  | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                                                                                                                                       | ['595.1', ' 46.0', ' 211.0', ' 22.0', ' 13.0', ' 51.0', ' 26.0']  |        12 | ['pre-heat oven the 350 degrees f', 'in a mixing bowl , sift together the flours and baking powder', 'set aside', 'in another mixing bowl , blend together the sugars , margarine , and salt until light and fluffy', 'add the eggs , water , and vanilla to the margarine / sugar mixture and mix together until well combined', 'add in the flour mixture to the wet ingredients and blend until combined', 'scrape down the sides of the bowl and add the chocolate chips', 'mix until combined', 'scrape down the sides to the bowl again', 'using an ice cream scoop , scoop evenly rounded balls of dough and place of cookie sheet about 1 - 2 inches apart to allow for spreading during baking', 'bake for 10 - 15 minutes or until golden brown on the outside and soft & chewy in the center', 'serve hot and enjoy !']                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                                                                                                                                            | ['white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', 'vanilla', 'water', 'all-purpose flour', 'whole wheat flour', 'baking soda', 'chocolate chips']                                                                             |              11 |            5 | complex      |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                                                                                                                             | ['194.8', ' 20.0', ' 6.0', ' 32.0', ' 22.0', ' 36.0', ' 3.0']     |         6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']                                                                   |               9 |            5 | simple       |
| millionaire pound cake               | 286009 |       120 |           461724 | 2008-02-12  | ['time-to-make', 'course', 'cuisine', 'preparation', 'occasion', 'north-american', 'desserts', 'american', 'southern-united-states', 'dinner-party', 'holiday-event', 'cakes', 'dietary', 'christmas', 'thanksgiving', 'low-sodium', 'low-in-something', 'taste-mood', 'sweet', '4-hours-or-less'] | ['878.3', ' 63.0', ' 326.0', ' 13.0', ' 20.0', ' 123.0', ' 39.0'] |         7 | ['freheat the oven to 300 degrees', 'grease a 10-inch tube pan with butter , dust the bottom and sides with flour , and set aside', 'in a large mixing bowl , cream the butter and sugar with an electric mixer and add the eggs one at a time , beating after each addition', 'alternately add the flour and milk , stirring till the batter is smooth', 'add the two extracts and stir till well blended', 'scrape the batter into the prepared pan and bake till a cake tester or knife blade inserted in the center comes out clean , about 1 1 / 2 hours', 'cool the cake in the pan on a rack for 5 minutes , then turn it out on the rack to cool completely']                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | why a millionaire pound cake?  because it's super rich!  this scrumptious cake is the pride of an elderly belle from jackson, mississippi.  the recipe comes from "the glory of southern cooking" by james villas.                                                                                                                                                                | ['butter', 'sugar', 'eggs', 'all-purpose flour', 'whole milk', 'pure vanilla extract', 'almond extract']                                                                                                                                |               7 |            5 | simple       |
| 2000 meatloaf                        | 475785 |        90 |          2202916 | 2012-03-06  | ['time-to-make', 'course', 'main-ingredient', 'preparation', 'main-dish', 'potatoes', 'vegetables', '4-hours-or-less', 'meatloaf', 'simply-potatoes2']                                                                                                                                                                           | ['267.0', ' 30.0', ' 12.0', ' 12.0', ' 29.0', ' 48.0', ' 2.0']    |        17 | ['pan fry bacon , and set aside on a paper towel to absorb excess grease', 'mince yellow onion , red bell pepper , and add to your mixing bowl', 'chop garlic and set aside', 'put 1tbsp olive oil into a saut pan , along with chopped garlic , teaspoons white pepper and a pinch of kosher salt', 'bring to a medium heat to sweat your garlic', 'preheat oven to 350f', 'coarsely chop your baby spinach add to your heated pan , stir frequently for approximately 5 min to wilt', 'add your spinach to the mixing bowl', 'chop your now cooled bacon , and add it to the mixing bowl', 'add your meatloaf mix to the bowl , with one egg and mix till thoroughly combined', 'add your goat cheese , one egg , 1 / 8 tsp white pepper and 1 / 8 tsp of kosher salt and mix till thoroughly combined', 'transfer to a 9x5 meatloaf pan , and cook for 60 min or until the internal temperature is at least 160f', 'let stand for 5min', 'melt 1tbsp unsalted butter into a frying pan , and cook up to three eggs at a time', 'crack each egg into a separate dish , in order to prevent egg shells from reaching the pan , then add salt and pepper to taste', 'wait until the egg whites are firm looking , but slightly runny on top before flipping your eggs', 'after flipping , wait 10~20 seconds before removing each egg and placing it over your slices of meatloaf'] | ready, set, cook! special edition contest entry: a mediterranean flavor inspired meatloaf dish. featuring: simply potatoes - shredded hash browns, egg, bacon, spinach, red bell pepper, and goat cheese.                                                                                                                                                                         | ['meatloaf mixture', 'unsmoked bacon', 'goat cheese', 'unsalted butter', 'eggs', 'baby spinach', 'yellow onion', 'red bell pepper', 'simply potatoes shredded hash browns', 'fresh garlic', 'kosher salt', 'white pepper', 'olive oil'] |              13 |            5 | complex      |


### Univariate Analysis

Below is the histogram representing the distribution of values for the n_ingredients column. This helps give us a deeper understanding of how many ingredients are contained in each recipe and also explains why 9 ingredients is a good cutoff for determining whether a dish is complex or simple. The red line represents the median value of the distribution and, as you can see, it is centered around 9.

<iframe src="univariate1.html" width=800 height=600 frameBorder=0></iframe>

Below is a histogram showing the distribution of the complexity column for all the recipes. As we can see, there are slightly lesser recipes in the complex catrgory compared to the simple category.

<iframe src="univariate2.html" width=800 height=600 frameBorder=0></iframe> 

### Bivariate Analysis

Below is a bar chart which examines the mean ``avg_rating`` for simple recipes vs. complex recipes. We can infer that the mean for each complexity group is about the same.

<iframe src="bivariateplot1.html" width=800 height=600 frameBorder=0></iframe>

### Interesting Aggregates

Grouping the number of ingredients and examining the aggregate statistics mean can help us to understand if there is a relationship between the complexity of a recipe (as indicated by the number of ingredients) and its cooking time and rating. This is because recipes with a higher number of ingredients may require longer cooking times, but may also result in more complex and flavorful dishes, leading to higher ratings. By grouping the recipes based on the number of ingredients and calculating the average cooking time and rating for each group, we can identify any patterns or trends in the data and determine if there is a correlation between the number of ingredients, cooking time, and rating. This can help us to make more informed decisions about recipe development, as well as provide insights into the preferences of consumers when it comes to recipe complexity and cooking time.


| complexity   |   minutes |   n_steps |   n_ingredients |   avg_rating |   calories |   total_fat (PDV) |   sugar (PDV) |   sodium (PDV) |   protein (PDV) |   saturated_fat (PDV) |   carbohydrates (PDV) |
|:-------------|----------:|----------:|----------------:|-------------:|-----------:|------------------:|--------------:|---------------:|----------------:|----------------------:|----------------------:|
| complex      |   96.9493 |  12.6287  |        12.7353  |      4.62307 |    503.609 |           38.6573 |       69.3078 |        33.3543 |         41.6396 |               46.1697 |               15.71   |
| simple       |  106.663  |   8.20179 |         6.55755 |      4.62708 |    374.326 |           28.0727 |       68.1805 |        25.6126 |         26.7148 |               35.7703 |               12.3359 |

---

## Assessment of Missingness

### NMAR Analysis

A column that could be Not Missing at Random (NMAR) within our ``recipes`` DataFrame could be the ``avg_rating`` column. A possible reason why these values could be missing is that users simply did not want to rate the recipe. For example, if a person visited a recipe's webpage and did not find it appealing, then they would not leave a rating. In other words, a certain recipe may be missing its ``avg_rating`` if an insufficient amount of people try it and leave a rating.

An additional column of data that might help explain this missingness and make it Missing at Random (MAR) is the number of visits to the recipe's webpage. If this value is extremely low for a given recipe, then it might help explain why its ``avg_rating`` is missing, thus making the missingness MAR.

### Missingness Dependency 

The columns in the ``recipes`` DataFrame that contain missing values are: ``name``, ``description``, and ``avg_rating``.

Here, we will conduct permutation tests on two columns against the ``description_missing`` column in order to determine whether the ``description`` column is Missing at Random (MAR) since it would depend on the values of that other column. In order to conduct the test, we will consider the following hypotheses with an 𝛼 of 0.05:

* **Null Hypothesis:** There is no significant difference between the two distributions of the column when the description is missing or not missing.
* **Alternate Hypothesis:** There is a significant difference between the distributions of the column when the description is missing vs. when the description is not missing.

Additionally, we will be using the **absolute difference of means** as our test statistic in this permutation test since we are dealing with quantitative distributions.

We will run the permutation test on the following columns: ``n_ingredients`` and ``n_steps``. Below are the resulting p-values of conducting the permutation tests on both columns:

``n_ingredients p-value: 0.002`` \
``n_steps p-value: 0.218``

Clearly, the resulting p-value for the number of ingredients is 0.004 which is **less than** 0.05 (our 𝛼 level). This means that the test was statistically significant, and we can **reject** our null hypothesis. In other words, the missingness of the ``description`` column *likely* does depend on the values of the ``n_ingredients`` column.

On the other hand, the resulting p-value for the number of steps is 0.198 which is **greater than** 0.05. This means that the test was **not** statistically significant, and we **fail to reject** our null hypothesis. In other words, the missingness of the ``description`` column *probably* does not depend on the values of the ``n_steps`` column.

As you can see below, we have plotted the distribution of the ``n_ingredients`` column with and without the missing description values. Clearly, the distributions for both plots are quite different, meaning that the a missing description value likely **does** depend on ``n_ingredients`` column.

<iframe src="missingness1.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="missingness2.html" width=800 height=600 frameBorder=0></iframe>

---

## Hypothesis Testing

To answer our overarching question, we will conduct a permutation test because we are trying to determine whether complex and simple recipes are drawn from the same distribution. We will conduct the test utilizing the following pair of hypotheses under a significance level of 𝛼 = 0.05:

* **Null Hypothesis:** There is no difference between the mean average ratings of simple recipes versus the mean average ratings of complex recipes.
* **Alternate Hypothesis:** There is a difference between the mean average ratings of simple recipes versus the mean average ratings of complex recipes.

Lastly, in order to measure the difference, we will be using a **absolute difference of means** test statistic because we are comparing two quantitative distributions. Additionally, in our alternate hypothesis, we are simply measuring difference, not direction. Therefore, the absolute difference of means statistic is the correct choice for this test.

<iframe src="hypothesis_plot.html" width=800 height=600 frameBorder=0></iframe>

### Conclusion

As seen above, the resulting p-value is 0.395 which is **greater than** our significance level of 0.05. This means that the permutation test was statistically insignificant meaning that we **fail to reject the null**. In other words, there is *likely* no difference between the mean average ratings of simple recipes versus the mean average ratings of complex recipes. This aligns with our observations earlier from the bivariate plots since there was little to no observable visual difference in the group means between the complex vs. simple groups.

Additionally, the plot above clearly shows that our observed difference of means is an expected value under the null hypothesis. This aligns with the result of our p-value so we fail to reject the null hypothesis.

Therefore, this answers our main question of: Do recipes with a higher number of ingredients (complex recipes) receive different ratings on average than those with fewer ingredients (simple recipes)?

---

# Using Machine Learning to Help You Cook Like a Pro

Welcome to our model building project, a project focused on predicting ratings of recipes. The goal of this project is to develop a machine learning model that can accurately predict the ratings of recipes based on various features such as ingredients, cooking time, and complexity. This is a regression problem, as the predicted variable (ratings) is a continuous numerical variable.

Our response variable is the recipe rating, which ranges from 1 to 5. We chose this variable because it is a crucial factor for determining the success and popularity of a recipe. Our model will assist users in finding recipes that are likely to be highly rated, leading to a more satisfying cooking experience.

The dataset used for this analysis is collected from <a href="food.com">food.com</a>. and contains information on a variety of recipes, including their ingredients, cooking times, and user ratings. By exploring this data, we hope to gain insights into the factors that contribute to a recipe’s success and popularity.

We hope that this project will help food enthusiasts and cooking enthusiasts discover new and highly-rated recipes with ease.

<center><img src="cook.jpeg" alt="Picture of food getting cooked" height="300" width="600"></center>

---

## Framing the Problem

The prediction problem we are going to be exploring is predicting the average rating of a given recipe. Since this is a **regression** problem, we will be building a linear regression model using `sklearn` in order to solve it.

The response variable in our problem is the `avg_rating` column of the `recipes` DataFrame which describes the aggregate average rating of each recipe (merged from the `interactions` DataFrame). The reason we chose `avg_rating` is because we believe it is valuable to have a general idea of how well a certain recipe is going to perform based on a few features. Additionally, websites such as [food.com](https://www.food.com) and other organizations could utilize such a model in order to curate certain types of recipes which might perform better or simply to fill in missing values of recipes that haven't received any ratings yet.

In order to evaluate our regression model, we will be measuring the **coefficient of determination**, or $R^2$, because it is simple to interpret . While metrics such as Root Mean Squared Error (RMSE) are much more detailed since they tell us exactly how much our model's predictions deviate from the actual values on average, it varies by the scale and type of data. For example, the RMSE of a model predicting the revenue of a company might be in the billions, while the RMSE of a model predicting the price of a banana can vary by a few cents. In other words, it is difficult to gain a deep understanding of your model's performance without important context. On the other hand, $R^2$ is limited to a range of 0 to 1, which is convenient since we can easily understand how well our model fits with unseen data. 

Additionally, it is important to note what type of information will likely be provided at the time of prediction. In our case, since we are predicting the `avg_rating` of a recipe, we will probably have access to most of the features in the `recipes` DataFrame. A few features we plan on including as part of the model are: `complexity`, `n_steps` (number of steps), `calories`, and `saturated_fat (PDV)`. These features will be available to us at the time of prediction because we would have access to the recipe when predicting its rating.


| name                                 |     id |   minutes |   contributor_id | submitted   |   n_steps |   n_ingredients |   avg_rating |   calories |   total_fat (PDV) |   sugar (PDV) |   sodium (PDV) |   protein (PDV) |   saturated_fat (PDV) |   carbohydrates (PDV) | complexity   |
|:-------------------------------------|-------:|----------:|-----------------:|:------------|----------:|----------------:|-------------:|-----------:|------------------:|--------------:|---------------:|----------------:|----------------------:|----------------------:|:-------------|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27  |        10 |               9 |            4 |      138.4 |                10 |            50 |              3 |               3 |                    19 |                     6 | simple       |
| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11  |        12 |              11 |            5 |      595.1 |                46 |           211 |             22 |              13 |                    51 |                    26 | complex      |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30  |         6 |               9 |            5 |      194.8 |                20 |             6 |             32 |              22 |                    36 |                     3 | simple       |
| millionaire pound cake               | 286009 |       120 |           461724 | 2008-02-12  |         7 |               7 |            5 |      878.3 |                63 |           326 |             13 |              20 |                   123 |                    39 | simple       |
| 2000 meatloaf                        | 475785 |        90 |          2202916 | 2012-03-06  |        17 |              13 |            5 |      267   |                30 |            12 |             12 |              29 |                    48 |                     2 | complex      |

---

## Baseline Model

### Create and Split the Data
The first step in building the baseline model is to create our test and training sets. In order to do this, we will utilize `sklearn`'s `train_test_split` function. Additionally, we will use the default split proportion of 0.25.

The first couple of features we will build our baseline model on are: `complexity` (a categorical column) and `n_steps` (a quantitative column).

Our baseline model is going to be built on a Pipeline which utilizes a ColumnTransformer() for preprocessing and transforming the data as well as a Linear Regression model/estimator. Let's take a deeper look at the type of transformers we are going to be applying to our features.

Since complexity is a categorical column, we will have to transform it. Here, we will use the OneHotEncoder() because there are only two types of complexities: simple or complex. Moreover, we decided to drop one of the columns in order to prevent multicollinearity. This means that the OneHotEncoder() has only one category called x0_simple, indicating that a value of 1 means a recipe is simple (less than 9 ingredients), while a value of 0 means a recipe is complex (greater than 9 ingredients). Furthermore, this makes sense intuitively since we would expect simpler recipes which cater to the general public to have higher ratings compared to recipes that are complex and difficult to make.

Also, we will leave `n_steps` as it is because it is a quantitative column.

### Model Performance
After fitting our pipeline creating a prediction based on the input testing set, we end up with an $R^2$ of our model to be `-0.00017975548942428254`. This means that our model barely, if not, didn't fit with the testing data at all. This current baseline model is **not good** based off this performance. In other words, there was a very poor linear fit. Hopefully by adding more features, we can improve the performance in the Final Model.

<center><img src="ml.jpg" alt="Picture of a robot thinking" height="300" width="450"></center>

---

## Final Model

The next couple of features that we will be adding on top of our baseline model are: `calories` and `protein (PDV)`. These features could improve the performace of our model because they are important aspects people consider when rating recipes. For example, someone on a diet would be looking for recipes with a low calorie count and high protein content. Therefore, recipes with high calories and less protein might be rated lower when these additional features are considered. On the other hand, some delectable, but unhealthy, recipes such as desserts (which have lots of calories but less protein) might be rated higher simply because they taste incredible. Let's begin exploring these relationships with our model.

Since the `calories` data has such a wide range and tends to be in the hundreds if not thousands, we will apply `StandardScaler()` in order to standardize the data. Moreover, we will apply `Binarizer()` to the `protein (PDV)` column because it will be helpful to determine how much protein can be considered a significant amount. For the threshold of our `Binarizer()`, we will determine the optimal value using `GridSearchCV`.

### Tuning the Model
In order to optimize our model, we will utilize `GridSearchCV` in order to find the best combinations of hyperparameters. Specifically, we will be searching for the optimal `threshold` parameter value for the `Binarizer()` transformer since we are unsure what a good cutoff is. This is a great way to tune our model and maximize performance in a concise manner.

Based on the `GridSearch`, the optimal threshold for our `Binarizer()` is 30. This means that any values above 30 will be set to 1 and the rest will be set to 0. Now that we have the optimal threshold for our `Binarizer()`, we can create our Pipeline once again and evaluate the performance with $R^2$.

### Model Performance
The model chosen is a regression model, which predicts recipe ratings based on features such as `calories` and `total_fat (PDV)`. The dataset was split into training and testing sets and the data was standardized using `StdScaler` for the calories column. `Binarizer` was applied to the saturated fat column with the threshold value determined by `GridSearchCV`.

The hyperparameter tuned was the threshold value for the Binarizer() transformer. The method used to select hyperparameters was `GridSearchCV`. This method exhaustively searches through a specified parameter grid, fitting the estimator for each combination of parameters and returns the best combination. The performance of the Final Model was evaluated using RMSE, and the best performing hyperparameters were selected based on the lowest RMSE value.

The $R^2$ of our final model is `0.0003677010479545828`. This is a clear improvement over our baseline model which had a negative $R^2$. Undoubtedly, the addition of the new features `calories` and `protein` were critical to the improvement in performance of our Linear Regression model. We believe this is because these are important factors people consider when rating recipes. They are important indicators of the nutritional value and overall healthiness of a recipe. For instance, a recipe with a low calorie count and high protein content is considered a healthy meal. These are aspects everyone takes into consideration when rating a recipe.

The final model's performance was an improvement over the Baseline Model's performance as it included additional features such as calories and total fat. The addition of these features helped the model better predict recipe ratings, leading to a more accurate and comprehensive model.

---

## Fairness Analysis

For our Fairness Analysis, we will be choosing groups based on how long a recipe takes to prepare, in minutes. We will split our data based on the `minutes` column with a threshold of 40 minutes. That is, a `long` recipe is anything greater than 40 minutes while a `short` recipe is anything below.

Group X: `short` recipes that take 40 minutes or less. \
Group Y: `long` recipes that take over 40 minutes.

- **Null Hypothesis:** Our model is fair. Its RMSE for short recipes and long recipes are roughly the same, and any differences are due to random chance.
- **Alternative Hypothesis:** Our model is unfair. Its RMSE for short recipes is lower than its RMSE for long recipes.

In order to evaluate our model, we will be using the **Root Mean Squared Error (RMSE)** as the metric. Specifically, we will be calculating the difference in RMSE between the two Groups X and Y. If the difference is negative, it means that the RMSE for short recipes is lower than the RMSE for long recipes. Additionally, we will be using a significance level of 𝛼 = 5%.

### Conclusion
After running the test for a 1000 repitions, the resulting p-value from our permutation test is 0.0, which is **less than** our significance level of 5% or 0.05. This means that the test was statistically significant, so we **reject the null**. In other words, our model *likely* performs worse for recipes in Group X compared to recipes in Group Y because the RMSE of the short recipe predictions were lower than the RMSE of the long recipe predictions.
