# data-512-a2

## Goal:
This project uses ORES API and the machine learning model they built to predict the quality of articles found on the english version of Wikipedia. It is important to note we used articles of people/politicians from various countries and have performed an analysis at both the country and geographical region granularity. We are looking into the question of the number of articles/high-quality articles from each country relative to that country's population.  

### Step 1: Data Acquisition
For this step we used the figshare link [here](https://figshare.com/articles/dataset/Untitled_Item/5513449) to get data of wikipedia politicians with their associated country. This was the first step. Next we used population data that is located [here](https://www.prb.org/international/indicator/population/table/) from the year 2020 to link those countries from the other data to populations so that we could find the proportion of articles to population.

### Step 2: Data Cleaning
In this step we removed articles with 'Template:' in the name as those were not wikipedia articles and therefore, not a part of our analysis. We also removed regional data from the population data to use later. In this step, we also added a column signifying which countries belonged to which regions so that it would be easy to merge together with our region populations later on in our analysis. 

### Step 3: Predicting Quality
Next we used an ORES model to predict the quality of the wikipedia articles we have. The model is obtainible through two ways: the first is their [github](https://github.com/wikimedia/ores) and the second is their [API](https://ores.wikimedia.org/v3/#!/scoring/get_v3_scores_context_revid_model). This step can take a while so we used the 'rev_id' column and performed the API calls in batches containing 50 articles per batch. We make sure to remove articles for which we did not have any value for a prediction.

### Step 4: Merge & Output
The next step was to merge our predictions with the population of the countries from which the article is based. This allows us to move onto our analysis step with confidence. Before moving on however we output our data to csv files. The first two csv files have the names 'wp_wpds_politicians_by_country.csv' and 'wp_wpds_countries-no_match.csv' which have the schema:

Column        | Value
------------- | -------------
country       | name of the country from which the article's politician is located
article_name  | The name of the politician which the article is about
revision_id   | The revision id of the article; used by ORES to predict the quality
article_quality_est.   | This is categorical variable containing the prediction of the article quality
population             | The number of humans in the country

The third csv output, our final data before analysis, has the following schema:

Column        | Value
------------- | -------------
country       | name of the country from which the article's politician is located
article_name  | The name of the politician which the article is about
article_quality_est.   | This is categorical variable containing the prediction of the article quality
population             | The number of humans in the country

### Step 5: Analysis
Next we create sql queries to create our different tables that are grouped by country. One of our tables has all articles while the other table only keeps the high end quality articles, namely, 'FA' and 'GA'. We count the number of articles from each country and also divide that number by the population to obtain the proportion of articles relative to population. Finally, we merge the countries with our regions from earlier and perform a grouping by regions, still counting the total number of articles in a region and dividing it by the population of that region to obtain the proportions. 

## API

### API Notes
- The url endpoint used in this project is "https://ores.wikimedia.org/v3/scores/enwiki/?models=articlequality&revids={rev_id}"
- There is only one response document schema that any path returns using the ORES API

### License and Terms of Use
Documentation for the ORES API can be found [here](https://ores.wikimedia.org/v3/#!/scoring/get_v3_scores_context)  
The license and terms of use for figshare can be found [here](https://help.figshare.com/article/copyright-and-license-policy)

#### If there are any questions, feel free to contact me at sampereb@uw.edu
