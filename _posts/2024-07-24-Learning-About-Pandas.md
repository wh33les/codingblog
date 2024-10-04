---
layout: post
title: Learning about pandas.
date: 2024-07-24 12:00:00 -06:00
---
I've taken a break from the Node to SQL project to get a few quicker projects on my resum&eacute;.  The current one is a data visualization in Tableau.  I downloaded some polling data from fivethirtyeight that gives presidential polling results dating from 7 April 2021 to 18 July 2024.  I may do a post on Tableau later but for now I'd like to do one on pandas, since it's been my main method of cleaning the data.

I've gone to the pandas documentation site, just to see if there are quicker ways to do things I've been trying to do by brute force.  Here are some things I've learned.

### Narrowing down a data frame by row or column conditions

I *finally* figured out the difference between `.loc` and `.iloc`, and how to use them!  Basically, they each take two arguments, `.loc` uses the respective labels of rows or columns, and `.iloc` uses indices.  I've noticed often I have the row index, but not the column index, and I can just use `df.iloc[index, df.columns.get_loc('Column name')]`.  Or if I want a row identified by a particular entry in a column, I can use `data.loc[data['candidate_name'] == 'Joe Biden', :]`, for example.  That gives all the rows with 'Joe Biden' in the 'candidate_name' column in my data.  I'm realizing with `.loc` I can use lots of booleans in the entries for row and column labels.

### Parsing dates in pandas

At first I was importing datetime for this, but I learned that I can read the dates in my data using pandas!  So, for example, I can use `data['end_date'] = pd.to_datetime(data['end_date'])` to parse the dates in the 'end_date' column of my data (this column gives the date each poll ended).  One command, and now I can sort by date without python sorting the dates lexicographically.

### Other misc

- I can use `.nunique()` to get the number of unique entries of each column in my data frame.  This was helpful in identifying redundant columns.
- Awhile back I learned about `inplace=True` as an argument for many data frame operations, so I don't have to keep setting the data frame equal to what I want changed.  I get the impression `inplace=True` is more efficient memory-wise, too.  Maybe.
- Oftentimes row indices can be cumbersome, for example if I sort the rows or if I export the data frame to a file.  The command `df = df.reset_index(drop=True)` reorders the indices if I want to permanently sort the rows, and `index=False` as an argument in `.to_csv` keeps the .csv file from creating a new column with the row indices.

I've got a couple of challenges with cleaning the Tableau data but the big one is imputing missing dates.  More to come in the next post.