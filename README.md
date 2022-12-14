![Wine Picture](https://github.com/elizabeth524/Capstone-Project/blob/main/Images/Wine_Image.jpg)

# Wine Reviews

### Author: [Elizabeth Webster](https://github.com/elizabeth524)

## Overview

Natural Language Processing to discover which attributes are indicative of a highly scoring wine and recommendation systems for Wine Enthusiast's tasters using Surprise.

## Business Problem

This project is being prepared for a small winery in Walla Walla.  They are just starting out and currently only producing a few wines. Their wine maker wants to gain insight on which wine qualities generate highly rated wines.

In this project, I will create a model to predict wine scores that can be used to assess how well the current wines would be rated. 

I will also pick out keywords and flavor profiles from wine descriptions that relate to high and low scoring wines in order for the wine maker to understand what direction to move in and look into what wine varieties are currently receiving high scores.

Finally, I will create a recommendation system for Wine Enthusiast's tasters in order to suggest wines to certain tasters. By understanding the wines that are recommended, the winery will get an idea of what type of wines to create and who to market them to.

## Dataset

![Wine Enthusiast](https://github.com/elizabeth524/Capstone-Project/blob/main/Images/Wine_Enthusiast.png)

The data that I am using comes from 2017 Wine Enthusiast and includes information on 130,000 different wines.  This information includes the description, variety, winery, country, taster name, etc.  

The target value is the 'points' column.  This column contains a score from 80 to 100 for each wine.  All the wines in this range are perfectly drinkable, but I have broken down this category into four ranges:
* 80-84 points: 0 - Acceptable
* 85-89 points: 1 - Good
* 90-94 points: 2 - Very Good
* 95-100 points: 3 - Outstanding

I will be looking at what sets 'Outstanding' wines apart from the 'Acceptable' ones.

For the recommendation systems piece of this project, it is important to note that there are 19 different tasters that we will be generating information on.

## Building a Model

I will build a Multinomial Naive Bayes model that the wine maker can utilize to predict how well their current wines will perform.

### Data Cleaning

I am dropping columns that are missing high numbers of values:
* designation
* region_1
* region_2 

I am also dropping columns should not impact point value.:
* taster_name
* taster_twitter_handle 
* title 

![Point Distribution](https://github.com/elizabeth524/Capstone-Project/blob/main/Images/Point_Distribution.jpeg)

Our target is the number of points that a wine has scored out of 100.  Unsurprisingly, it follows a normal distribution. Since we are creating a classification model we will break our target up into 4 categories:
* 80-84 points: 0 - Acceptable
* 85-89 points: 1 - Good
* 90-94 points: 2 - Very Good
* 95-100 points: 3 - Outstanding

I will drop NaNs for:
* country (63 - .04%)
* province (63 - .04%)
* variety (1)

I will use mean for filling NaN prices (8996 - 6% of dataset) after the train test split.

In order to use all of the data, I need to One Hot Encode the following categorical columns:
* Country
* Province
* Variety
* Winery

### Baseline Model

Before performing any Natural Language Processing, I created a baseline Multinomial Naive Bayes model with the raw text.  After vectorizing our text and joining it with our other variables, the model created performed with a 64% accuracy.

**Confusion Matrix:**

![Baseline Confusion Matrix](https://github.com/elizabeth524/Capstone-Project/blob/main/Images/ConfusionMatrix_1.jpg)

This model is not terrible, but I will try to increase it's accuracy with Natural Language Processing.

### Natural Language Processing

The steps taken for Natural Language Processing (NLP) of our 'description' column were:
* Lowercase Text
* Tokenize and Remove Punctuation
* Remove Stopwords - Both English stopwords and stopwords specific to this dataset
* Lemmatize

### Final Model

After NLP, I created the final Multinomial Naive Bayes model.

**Confusion Matrix:**

![Final Confusion Matrix](https://github.com/elizabeth524/Capstone-Project/blob/main/Images/ConfusionMatrix_2.jpg)

Even though this model has some difficulty identifying the outlier targets (0 and 3) it does a good job predicting whether a wine will be in the upper (2 and 3) or lower (0 and 1) half.  This model can be used reliably to predict if a wine will make it into the 'Very Good' tier, which would be very helpful for a wine maker.

## Wine Description Keywords

Since I have a model that can tell us how well the current wines would be perceived, I would next like to explore keywords for highly scoring wines.  This will help the wine maker know what flavor profiles to aim for in future wines.

I will be using the points range that we established above:
* 80-84 points: 0 - Acceptable
* 85-89 points: 1 - Good
* 90-94 points: 2 - Very Good
* 95-100 points: 3 - Outstanding

### Acceptable Wines

![Acceptable Cloud](https://github.com/elizabeth524/Capstone-Project/blob/main/Images/Acceptable_Cloud.jpg)

In this word cloud, we are seeing words like 'green', 'simple', 'acidity', 'bitter', and 'light'.  These words seem to be indicative of a wine that is lacking balance and body.

### Good Wines

![Good Cloud](https://github.com/elizabeth524/Capstone-Project/blob/main/Images/Good_Cloud.jpg)

This word cloud shows us many fruit notes; 'apple', 'cherry', 'berry', 'citrus' as well as 'fresh' and 'crisp'.  These wines could be lacking depth of flavor or complexity.

### Very Good Wines

![Very Good Cloud](https://github.com/elizabeth524/Capstone-Project/blob/main/Images/Very_Good_Cloud.jpg)

In this word cloud we are starting to see bolder words; 'black', 'ripe', 'rich', 'plum', 'texture', and 'chocolate'.  These wines seem to be more complex and less fruit forward.

### Outstanding Wines

![Outstanding Cloud](https://github.com/elizabeth524/Capstone-Project/blob/main/Images/Outstanding_Cloud.jpg)

One interesting word that immediately jumps to the eye is 'cabernet'. This variety would be worth exploring since it is the only variety that made it into any of our word clouds. These outstanding wines are also characterized by the words 'complex', 'spice', 'dense', 'time', 'aging' and 'pepper'.  These words seem to point us in the direction of full bodied and complex red wines that have been aged for a decent period of time.

### Keyword Conclusions

Based on our findings from the wine descriptions, higher scoring wines seem to be ones that have greater complexity and body and spend more time aging. It also seems as though Cabernets could be a good direction to move in.  Outstanding wines also move past the straightforward fruit flavors and into more unusual wine flavors such as spice, pepper, and chocolate.

## Wine Varieties

Finally, I would like to look into which wine varieties are currently receiving high scores.  To do this, I will look at mean scores of varieties as well as which varieties are most commonly receiving outstanding or perfect scores.

Since there are so many different wine varieties in this dataset (707), we will limit our scope to the 15 that had the most reviews.  It is important to note that these are not necessarily the 15 highest rated wines, they are just the 15 that we have the most information on.  

I chose to go this route because I believe that the more ratings a wine has, the more common the wine.  I believe there will be many wines in this dataset that are very obscure and we would like to focus on wines that a new winery could potentially produce.

### Mean Scores

![Mean Wine Scores](https://github.com/elizabeth524/Capstone-Project/blob/main/Images/Mean_Variety_Scores.jpg)

The wines with the top 5 highest mean scores are:
* Nebbiolo - 90.25
* Riesling - 89.45
* Pinot Noir - 89.41
* Syrah - 89.29
* Bordeaux-style Red Blend - 89.11

### Oustanding Scores

Varieties that are common in the 'Outstanding' point range (95-100):
* Pinot Noir - 402 times
* Cabernet Sauvignon - 274 times
* Bordeaux-style Red Blend - 256 times
* Chardonnay - 235 times
* Riesling - 194 times

### Perfect Scores

There are only two wines that have received a perfect score more than once in this dataset:
* Bordeaux-style Red Blend - 5 times
* Syrah - 2 times

### Variety Conclusions

Based on these findings, the three wines that I believe have the best potential to receive high scores are:
* **Bordeaux-style Red Blend** - high mean score, commonly in the outstanding point range, five perfect scores
* **Pinot Noir** - high mean score, commonly in the outstanding point range
* **Syrah** - high mean score, two perfect scores

## Recommendation Systems

I will create a recommendation system for Wine Enthusiast's tasters in order to suggest wines to certain tasters. By understanding the wines that are recommended, the winery will get an idea of what type of wines to create and who to market them to.

### Data Cleaning

For this section of the project, I am only interested in three columns from our dataset: taster name (users), title (items), and points (target).  Since I am interested in user's preferences, I dropped the values that were missing taster names.

### Builing a Model

For the recommendation system, I explored three different models to use:
* SVD - Singular Value Decomposition
* KNN Basic
* KNN Baseline

The metric I am interested in is RMSE - Root Mean Squared Error.  The model with the lowest RMSE is the SVD Model.

### Model Predictions

From the model, I created a function to predict the top five recommended wines for each taster.

The red varieties that are showing up the most are:
* Pinot Noir
* Cabernet Sauvignon
* Syrah

The white varieties that are showing up the most are:
* Riesling
* Chardonnay

The tasters that are recommended wines from our city, Walla Walla, or region, the Pacific Northwest are:
* Sean Sullivan
* Paul Gregutt
* Virginie Boone


## Recommendations

**Naive Bayes Model** - This model can be used reliably to predict if a wine will make it into the 'Very Good' tier, which will helpful for a wine maker to assess their current wines. I would recommend using this model to understand who well current wines would be received.

**Keywords** - Based on our findings for wine description keywords, I would recommend producing complex, aged red wines with deep flavor profiles.  I would also suggest exploring flavors such as spice, pepper, and chocolate in full bodied wines.

**Varieties** - The three varieties that I would recommend producing are:
* Bordeaux-style Red Blend - Full bodied red wine with hints of chocolate and black current.
* Pinot Noir - Versatile red wine loved for its red fruit and spice flavors and smooth finish.
* Syrah - Rich, meaty red wine characterized by tobacco, plum, and peppercorn. 

These wines historically score highly and also correspond with the keywords we discovered above.

**Recommendation Systems** - From the recommendation system I would recommend producing Pinot Noirs, Cabernets, Syrahs, Rieslings, and Chardonnays. Once the wines have been produced, I would recommend sending them to Sean, Paul, or Virginie for tasting. These tasters enjoy the above varieties from our region, so they are most likely to highly rate our wines.


## Next Steps

**Putting Model into Action** - The first step I would take is gathering the data from our current wines to predict their score level.

**Exploring Wineries** - Another step we could take is exploring the different wineries in the dataset.  If certain wineries are receiving high scores for their wines, we can look into their processes for creating wines.

**Cold Start Problem** - Finally, I would be address the cold start problem, or how to recommend wines for a user that we have no information on. The strategy I would use is having the new user provide preferences on a few of the wines to get us started.


## For More Information

See the full analysis in the [NLP Jupyter Notebook](https://github.com/elizabeth524/Capstone-Project/blob/main/Wine-Reviews-NLP.ipynb) and [Recommendation Juptyer Notebook](https://github.com/elizabeth524/Capstone-Project/blob/main/Wine-Reviews-Recommendation-Systems.ipynb) or review my [presentation](https://github.com/elizabeth524/Capstone-Project/blob/main/Wine-Reviews-Presentation.pdf).

For additional information, contact Elizabeth Webster at [eaw524@gmail.com](eaw524@gmail.com)

## Repository Structure

```
????????? Data
????????? Images
????????? README.md
????????? Wine-Reviews-NLP.ipynb
????????? Wine-Reviews-Presentation.pdf
????????? Wine-Reviews-Recommendation-Systems.ipynb
```