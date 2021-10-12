# README

## Goal of this Project

I investigate how English Wikipedia's coverage of political figures varies between countries. In particular, I analyze two metrics:

* Coverage: Number of articles about a country's politicians per citizen of that country
* Quality: Fraction of articles about a country's politicians that are "high quality" (i.e. Featured Article or Good Article classes)

## Licenses

The data used in this project comes from three sources.

* The "Politicians by Country" dataset of articles was downloaded from [Figshare](https://figshare.com/articles/dataset/Untitled_Item/5513449) and is licensed [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/).
* The world population data is drawn from the [World Population Data Sheet](https://www.prb.org/international/indicator/population/table/) compiled by the Population Reference Bureau. The license for this data is not specified.
* The quality predictions for Wikipedia articles is obtained from the Wikimedia [ORES tool](https://ores.wikimedia.org/) via its [REST API](https://ores.wikimedia.org/v3/#!/scoring/get_v3_scores_context_revid_model).

## API Documentation

Documentation for the ORES [REST API](https://ores.wikimedia.org/v3/#!/scoring/get_v3_scores_context_revid_model).

## Data Files

### Input Data Files

* `page_data.csv` contains the "Politicians by Country" dataset. Its fields are:
	* page: The title of the Wikipedia article
	* country: The country associated with the article's subject
	* rev_id: A unique identifier containing the edit ID of the last edit to the page, used to identify the article for scoring
* `WPDS_2020_data.csv` contains the world population dataset from the Population Reference Bureau. Its fields are:
	* FIPS: A two-letter code identifying the country
	* Name: The country or sub-region name
	* Type: Identifies the data as pertaining to a country, sub-region, or world.
	* TimeFrame: The year the data was compiled
	* Data (M): The population, in millions
	* Population: The population
	
It should be noted that `page_data.csv` lists many pages that are not Wikipedia articles, such as templates and lists. These are removed from the dataset during the cleaning step.

### Output Data Files

* `unscored_pages.csv` lists the entries in the `page_data.csv` dataset for which ORES failed to return a score. Its fields are identical to that of `page_data.csv`.
* `wp_wpds_countries-no-match.csv` lists the entries in the 'page_data.csv' dataset whose `country` field had no matches in the world population dataset. Its fields are identical to that of `page_data.csv`.
	* Reasonable efforts were made to manually correct no-match cases resulting from errors in the `country` field, most commonly the accidental use of demonym in place of the country name (e.g. 'Salvadoran' rather than 'El Salvador').
	* Remaining no-match cases are primarily resulting from `country` field values corresponding to historical, disputed, unrecognized, or sub-national states.
* `wp_wpds_politicians_by_country.csv` is the result of merging the `page_data.csv` dataset with the `WPDS_2020_data.csv` dataset, adding country and regional populations to the list of articles about politicians. Its fields are:
	* country: Name of the country the article topic is associated with
	* article_name: Name of the politician or office
	* rev_id: A unique identifier containing the edit ID of the last edit to the page, used to identify the article for scoring
	* article_quality_est: The ORES tool's estimate of the article quality class.
	* country_pop: Population of the country
	* subregion: Name of the sub-region the country belongs to (e.g. Eastern Europe)
	* subregion_pop: Population of the sub-region
	* subregion2: Name of the larger region the country belongs to (e.g. Europe)
		* subregion2 and subregion may be identical if the larger region has no sub-regions, such as Oceania or North America
	* subregion2_pop: Population of the larger region.
