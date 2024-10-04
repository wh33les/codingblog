---
layout: post
title: Translating using python.  
---
Today I started working on a project for the Erdös Institute's data science bootcamp that I attended last fall.  The first task is to find a data set, state a question that can be answered using the data, then identify stakeholders and key performance indicators (KPIs).  I decided to use a trending data set from [Kaggle](https://www.kaggle.com/datasets/thedevastator/mental-health-in-drug-users-during-covid-19) on the correlation between drug use and mental health during the COVID-19 pandemic.  The data is from two surveys conducted during spring and summer of 2020.  

There were two data files, I guess one for each survey, and each one had a column with a hash number that I think is the identifier for each respondent.  The Kaggle page says 5618 respondents took both surveys so I think those hash numbers link the two data files.  

One of the data files has its column names in Spanish.  At first I tried to use Google translate to rename the columns with the English translations manually.  Then when I realized there were 136 columns I thought, there must be a quicker way to do this.  So I did a quick Google search and it turns out, yes, of course there is an easier way to do this!  There is a python package called 'googletrans' that uses the Google translate API.  I needed to install it, as it's not in the package repository in Anaconda.  I wasn't sure how to do this, but it turned out I only needed to run the Anaconda command line and write the code:
```
pip install googletrans==3.1.0a0
```
From there I was able to write a loop that would translate each of the column entries from Spanish to English.  The translator is not perfect, there were columns that didn't get translated, but it was only four of them and I was able to use the rename function to change those manually.

Overall, I'm proud of myself for learning something new that was not covered in the bootcamp!
