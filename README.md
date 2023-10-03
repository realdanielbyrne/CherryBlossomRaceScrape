# CherryBlossomRaceScrape

The code and the report in this repository scrapes a 5K run website for data.  It was an exercise in grad school on learning how to perform web scrapping in R.  jduranengineer was my collaborator on the project, and I'm sure I couldn't have done it without her.  For smoe reason we always worked well together.  I think she is one of those engineers that everyone just likes working for.


# Modeling Runner's Times in Cherry Blossom Race

Daniel Byrne, Joanna Duran
9/19/19

## Abstract

We analyze the Cherry Blossom 10 Mile race results in order to assess if the age distributions of racers changes over the years. We compare the age distributions of the runners across the years 1999-2012. We utilize box plots and density curves to make our comparisons. We will answer "How do the distributions change over the years?" and "Was it a gradual change?"

## Introduction

Running for pleasure in a slightly competitive run, often for charity has become a weekend pleasure for many a health enthusiast across the United States. Races tend to be sponsored by corporate benefactors, held on or around the same time every year, and often on the same course. The successful races have continued to thrive and grow over the years, attracting more and more participants every year.

Races of this type come in any varieties. Common race configurations are 1M, 5K, 10K, 10M, half marathons and marathons. In most of these races, lots of data is collected by the race organizers and racing services. Common data points usually include the participant's sex, age, and recorded times. This data is used by the race organizers to determine winners in overall and age and sex based divisions, as well as course records and basic attendance.

The advent and growth of the Internet has coincided with the growth in popularity of these races. Coincidently, this data is often made publicaly available on the individual race's website. Ostensibly to allows race participants and interested parties to gather information on the performance of the participants.

This data then becomes a freely accessible repository of rich sample datasets. Thus, possibilities to extract insights from this data around for example demographic shifts, year over year trends, and age-related performance metrics abound.

In this paper we examine the data from the Cherry Blossom Ten Mile Run held in Washington D.C. held annually in early April when the cherry trees in bloom. The Cherry Blossom race started in 1973. It is used as a lead in competition for elite runners planning to compete in the Boston Marathon.

Earlier [research][1] by Kaplan and Nolan had shown that 1999 runners were typically older than the 2012 runners. We compare the age distributions of male runners across the years 1999 - 2012. We will answer "How do the distributions change over the years?" and "Was it a gradual change?"

We also improved upon the original code used to scrape the website, by implementing a modern REST API based approach to gathering and compiling the data across the years.

## Methods

### REST Based Mining Method

We began by first investigating the data mining approach used by Nolan and Kaplan. It was an extensive and comprehensive approach that worked well for the race data as it was presented on the [Cherry Blossom Race Results website][2] at the time that this study was first conducted. However, since then, the source website has changed data repository format, and so we took this opportunity to explore new ways to mine the data that could possibly be extended across all years to include the original study range and the years competed since and also prior to 1999.

The new repository is featured a searchable directory of race results from 1978 to 2018. The search engine was built with 3 selectable filters for event (5M or 10M), division (male and female age groups, and race year. As search parameters are varied by the user, the website makes RESTful API calls back to the server with these query string parameters to retrieve which then returns with the relative data, 20 records at a time.

The mining approach we took was to use the first identify the HTML table presenting the data on the website, and then use the r package rvest to convert the table data into an r data.frame. We then varied the query string parameters to cycle through the available pages to then iteratively captured all the data we were interested in. Each successive year was saved in a UTF-8 csv files for later consumption and evaluation.

The code to scrape the data in its newest form was easier to implement and thus less error prone than the original. The code consists of 2 simple r functions, comprising less than 45 lines of code.

- `getResults(uri, year = 1999)` - Loops over every available page of data for the selected year.
- `getAll()` - Loops over the years 1999-2014 and saves each year's data in a csv file.

The scraping code r code is listed in [cbreaddata.r](https://github.com/realdanielbyrne/CherryBlossomRaceScrape/blob/master/Proj%202%20Cherry%20Blossom/Byrne_Duran_Hwk_2.ipynb).

### Investive Technique

We limit our data to the Men's datasets. The variables available in our data were: 

- `Race`: the year of the race.
- `Name`: name of participant * “Age”- age of participant
- `Time`: total run time of participant
- `Pace`- pace per mile
- `PiS.TiS`: Position of participant in sex category/ total participants in sex category
- `Division`: Division; M/F+ age range
- `PiD.TiD`: Position of participant in division/ total participants in division
- `Hometown`: Participant's Hometown

In this paper we limited our investigation to the analysis of Age and Race. We first investigated the combined dataset from all years and tested the data against normality constraints. We then used multi-class ANOVA to determine if the overall groups mean varied from inter year means. We also tested this data for serial auto-correlation using the standard [Mann Whitney U][3] test to determine if there was evidence to suggest that this time series data is not independent of prior distributions. We also used density plots, box plots and qq plots to visualize the metrics we calculated.

## Results
See [ipython notebook](https://github.com/realdanielbyrne/CherryBlossomRaceScrape/blob/master/Proj%202%20Cherry%20Blossom/cbreaddata.r)
