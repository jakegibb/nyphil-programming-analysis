# New York Philharmonic Programming Analysis: New Works and Composers
This project analyses the programming practices of the [New York Philharmonic's](https://www.nyphil.org/). In particular, it is interested how often the Philharmonic programs works and composers that have not previously been programmed in the ensemble's history. 

For the purpose of this project, decisions to perform new works or composers are considered forms of "expansive programming," as they expand the orchestra's repertoire. 

### Disclaimer
This project is an independent analysis conducted using data from the [New York Philharmonic Archives](https://archives.nyphil.org/), which has released the organization's [performance history](https://github.com/nyphilarchive/PerformanceHistory) under a public domain license. The analysis and findings in this project are independent and do not represent official positions of the New York Philharmonic.


## General Information
 This README file was create on 2024-11-14 and was last updated on 2024-11-18.

### Creator
Jake Gibson<br>
Pratt Institute<br>
<jgibso92@pratt.edu><br>
ORCID: 0009-0005-0099-9197<br>

### Date of Analysis
September 2024 – December 2024

### Recommended Citation
Gibson, J. (2024). New York Philharmonic Programming Analysis: New Works and Composers [Dataset]. Git Hub. https://github.com/jakegibb/nyphil-programming-analysis


## Executing the Repository
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/jakegibb/nyphil-programming-analysis/HEAD)

A binder environment has been created for this project. Launch the project in binder to run the project's Jupyter Notebooks in an isolated computing environment.


Additionally, each notebook has an embed link to open the notebook in Google Colab if that is a preferred environment. However, to run a notebook in Colab, you will need to download the appropriate data source and at it to Colab's "content" folder.


## Repository Structure

### Folder: data_restructuring_and_analysis
* **File: '_nyphil_performance_history_data_restructuring.ipynb_'**
    * Functions:
        * Transforms the performance history JSON data from the New York Philharmonics to be structured around individual performances of orchestral works instead of unique programs.
        * Counts the number of new works and new composers performed in each season of the New York Philharmonic.
    * Output:
        * 'nyphil_programming_data.csv'
* **File: '_nyphil_expansive_programming_data_by_season.csv_'**
    * Counts of new works and new composers performed by the New York Philharmonic from 1842-2023, as well as basic descriptive statistics for this data.
    * See "Data Structure" section for variables and their definitions.
* **File: '_nyphil_programming_analysis.ipynb_'**
    * Plots data from 'nyphil_expansive_programming_data_by_season.csv' using matplotlib.
 * **File: '_nyphil_proramming_vizzu.ipynb_'**
    * Uses the ipyvizzu library to create an interactive visualization of new work and composer data.

### Folder: nyphilarchive_performance_history
* [GitHub repository](https://github.com/nyphilarchive/PerformanceHistory) cloned on 2024-09-10 from the New York Philharmonic Archives.
* Please see the README file enclosed in the folder for more information regarding the repository's structure, data structure, variable definitions, and usage guidelines.

## Data Processing Methodology

JSON data from the New York Philharmonic Archives is structured around unique programs performed by the orchestra. To analyze the frequency of "expansive programming" decisions, the data needs to be restructured around individual performances of single works.

The following steps are used to transform the performance history data:

1. JSON Data is "exploded" to create a record for every work performed on a unique program.
2. Each work is assigned a performance date based on the earliest date a unique program was performed.
3. The number of total and works and composers performed on each season are counted.
4. Copies of the dataset are filtered for only the first performance of a unique work or composer. These subsets are then grouped by season and counted.
5. Descriptive statistics are aggregated in a single DataFrame and exported to CSV.
6. Secondary Jupyter Notebooks import the CSV data and run statistical tests and create visualizations.

## Software and Packages

Jupyter Notebooks for this project were written using VSCode and Python 3.12.6. The following libraries and packages (beyond the standard Python library) were used:
* [pandas](https://pandas.pydata.org/)
* [matplotlib](https://matplotlib.org/)
* [scikit-learn](https://scikit-learn.org/stable/)
* [statsmodels](https://www.statsmodels.org/stable/index.html)
* [ipyvizzu](https://ipyvizzu.vizzuhq.com/latest/)

A "requirements.txt" file is included in the repository for ease of reproducing the Python environment.

## Data Structure: New York Philharmonic JSON Data

The original JSON data structure from the New York Philharmonic, as document in the enclosed README document, is as follows:
```
{
  "programs": [
    {
      "id": "38e072a7-8fc9-4f9a-8eac-3957905c0002", // GUID
      "programID": "3853", // NYP Local ID
      "orchestra": "New York Philharmonic",
      "season": "1842-43",
      "concerts": [
        {
           "eventType": "Subscription Season",
           "Location": "Manhattan, NY",
           "Venue": "Apollo Rooms",
           "Date": "1842-12-07T05:00:00Z",
           "Time": "8:00PM"
        },
        /* A program can have multiple concerts */
      ],
      "works": [
        {
          "ID": "8834*4", // e.g. "1234*1" - first part is the Work ID, second part is the NYP Movement ID
          "composerName": "Weber,  Carl  Maria Von",
          "workTitle": "OBERON",
          "movement": "\"Ozean, du Ungeheuer\" (Ocean, thou mighty monster), Reiza (Scene and Aria), Act II",
          "conductorName": "Timm, Henry C.",
          "soloists": [
            {
              "soloistName": "Otto, Antoinette",
              "soloistInstrument": "Soprano",
              "soloistRoles": "S"
            },
            /* more soloists, if applicable. If no soloists, this will be an empty array */
          ]
        },
        /* a program will usually have multiple works */
        {
          "ID": "0*",
          "interval": "Intermission",
          "soloists": []
        },
        /* Intermissions will also appear in the works array */
      ]
    },
    /* more programs */
  ]
}
```
The following are selected variable definitions from the New York Philharmonic that are particularly relevant for this analysis (more detailed definitions are documented in the README file enclosed within the cloned repository):
<table>
	<tr>
		<th>Field</th><th>Description</th>
	</tr>
	<tr>
		<td>Season</td><td>Defined as Sep 1 - Aug 31, displayed "1842-43"</td>
	</tr>
	<tr>
		<td>concerts.Date</td><td>Full ISO date used, but ignore TIME part (1842-12-07T05:00:00Z = Dec. 7, 1842)</td>
	</tr>
	<tr>
		<td>works.ComposerName</td><td>Composer Last name, first / TITLE (NYP short titles used)</td>
	</tr>
	<tr>
		<td>works.WorkTitle</td><td>Work Title as cataloged by NYP</td>
	</tr>
</table>

## Data Structure: nyphil_expansive_programming_data_by_season.csv
Data processing in 'programming analysis.ipynb' outputs a CSV file containing descriptive statistics for "expansive programming" decisions grouped by season.

The following is a list of variables and definitions for each field in the resulting CSV file.
<table>
	<tr>
		<th>Field</th><th>Description</th><th>Type</th>
	</tr>
	<tr>
		<td>season</td><td>Defined as Sep 1 - Aug 31, displayed as a two year range (e.g."1842-43")</td><td>str</td>
	</tr>
	<tr>
		<td>total_composers</td><td>The total number of composers performed within one season.</td><td>int</td>
	</tr>
	<tr>
		<td>new_composers</td><td>The number of composers within a season that are being performed for the first time in the orchestra's history.</td><td>int</td>
	</tr>
	<tr>
		<td>repeat_composers</td><td>The number of composers performed within a season that have previously been performed by the orchestra.</td><td>int</td>
	</tr>
    <tr>
		<td>new_composers_p</td><td>The proportion of composers performed within a season that are 'new_composers' (i.e. have never been performed by the orchestra).</td><td>int</td>
	</tr>
    <tr>
		<td>repeat_composers_p</td><td>The proportion of composers performed within a season that are 'repeat_composers' (i.e. have previously been performed by the orchestra).</td><td>int</td>
	</tr>
    <tr>
		<td>total_works</td><td>The total number of works performed within a season (if multiple movements of a single work are performed, that is counted as one work).</td><td>int</td>
	</tr>
    <tr>
		<td>new_works</td><td>The number of works within a season that are being performed for the first time in the orchestra's history.</td><td>int</td>
	</tr>
    <tr>
		<td>repeat_works</td><td>The number of works performed within a season that have previously been performed by the orchestra.</td><td>int</td>
	</tr>
    <tr>
		<td>new_works_p</td><td>The proportion of works performed within a season that are 'new_works' (i.e. have never been performed by the orchestra.)</td><td>float</td>
	</tr>
    <tr>
		<td>repeat_works_p</td><td>The proportion of works performed within a season that are "repeat_works" (i.e. have previously been performed by the orchestra).</td><td>float</td>
	</tr>
    <tr>
		<td>new_works_and_composers_p</td><td>The proportion of 'new_works' on a season that are also composed by 'new_composers'.</td><td>float</td>
	</tr>
    <tr>
		<td>season_year</td><td>The start year of an orchestral season. Useful for filtering and sorting.</td><td>int</td>
	</tr>
    <tr>
		<td>decade_bin</td><td>Range created by pandas.cut function to bin seasons by decade. Decades are defined from ###0 – ###9 (i.e. 1980-1989).</td><td>category</td>
	</tr>
    <tr>
		<td>decade</td><td>Single integer representing the start of a decade. Decades are defined from ###0 – ###9 (i.e. 1980-1989).</td><td>int</td>
	</tr>
</table>

## License
 This project is released under a [CC0 1.0 Universal License](https://creativecommons.org/publicdomain/zero/1.0/).

 The [New York Philharmonic](https://github.com/nyphilarchive/PerformanceHistory) data underlying this analysis was also released under the same [CC0](https://creativecommons.org/publicdomain/zero/1.0/) license. Please see the usage guidelines in dataset's original README (located in the "nyphilarchive_performance_history" folder) for more information. 
