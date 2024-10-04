---
layout: post
title: Machine learning&#58; PCA II 
--- 
Check out my [previous post](https://wh33les.github.io/Machine-Learning-PCA-I/) on the idea and math behind PCA.

## PCA in my project

For my project I collected data from the last 500 posts from the [538 features page](https://fivethirtyeight.com/politics/features/): post type (article, video, or podcast), post title, post url, author(s), date and time posted, tags (according to 538), and number of comments.  The initial intent of this project was to determine which tags get the most comments.  There were 494 distinct tags so I pruned them a little bit by removing tags like "Podcast Video" and "Video" (these tags don't give information about the content of the posts) and then throwing out the tags that appear less than 2% of the time.  This left 39 tags but Matt said it was still possible that certain tags appear together frequently enough that they can be combined into "supertags".  Doing PCA on the tags would reveal which tags are correlated.

To prepare the data I made a matrix $X$ with posts v. the final tags list, where each entry is a $1$ if the tag appears in the post, and a $0$ otherwise.  The `PCA` tool is part of the `sklearn.decomposition` module and the scaling tool, `StandardScaler`, is part of `sklearn.preprocessing`.  

Here is a snippet of the code used:
```
# Make the PCA objects
scaler = StandardScaler()
pca = PCA()

# Fit the PCA
features_scaled = scaler.fit_transform(posts_v_features_matrix[posts_v_features_matrix.columns[1:]])
pca.fit(features_scaled)

# Get the PCA components
component_vectors = pd.DataFrame(pca.components_.transpose(), index = posts_v_features_matrix.columns[1:])
component_vectors.sort_values(by = component_vectors.columns[0], ascending = False)
```
The result is a matrix whose columns are the vectors $X\vec w$, where $\vec w$s are the PCA components.  The rows are ordered in descending order of the contributions to the first vector $X\vec w$ from each tag vector.  The top five positive contributions come from the tags "2024 Republican Primary", "Donald Trump", "Ron DeSantis", "2024 Presidential Election", and "2024 Election", and it makes sense that these tags frequently appear together.  Similarly, the top five negative contributions come from the tags "2022 Election", "2022 Midterms", "2022 Senate Elections", "Election Update", and "Georgia Senate".  It makes sense that these tags are correlated and at the same time rarely appear with any of the other five tags.  To find other correlated features we just look at the top contributions to the other vectors $X\vec w$.

My inclination was to run PCA on all the tags, to see if, for example, certain topics are more likely to appear as videos or podcasts.  I did it and found correlations between "Qatar", ""US Mens National Team"", "FIFA World Cup", "World Cup", and "Controversy"; and "2022 Election", "2022 Midterms", "Politics Podcast", "Video", and "Donald Trump".  I also ran PCA on authors v. all tags, but I'm not sure how to interpret the results.  Are the top contributions tags that one author tends to write about?  How can I tell which author?

## Lingering questions

PCA is a helpful tool that I hadn't known about before.  I've done my best to try to understand what it is and how it works, but still, some questions remain.

**1. How does PCA reveal correlated features?**
This was my biggest question and I didn't address it at all in my previous post.  Since then I've thought about it and here's my guess.  PCA components point in the direction of the highest variance of the data, and so if certain features have high contributions, then observations with those features tend to appear at the edge of the variance in the data.

**2. What vectors are output with the command `pca.components_`?**
Because these vectors have contributions from the features vectors I thought they were the vectors $X\vec w$.  However, I think the documentation for `PCA` says they are just the PCA components $\vec w$.

**3. How strong are the correlations of features contributing to later vectors $X\vec w$?**  My guess is that they are not as strong because the later PCA components $\vec w$ correspond to smaller singular values, i.e., directions with smaller variance.

**4. How many PCA components should there be and what are their dimensions?**  `PCA` gives vectors that are linear combinations of the features vectors, but when there are fewer observations than features, the features vectors cannot be linearly independent.  Also I think I've seen when there are fewer observations than features, the number of PCA components is the number of observations.  That means there are repeated eigenvalues of the covariance matrix.  Is this obvious?  What does it mean?   

Unfortunately I have to put this project on pause for the next couple of weeks while I work on another project for Matt's data visualization minicourse.  Maybe after stepping away for a bit I can come back with a clearer mind and be able to answer these questions myself.  Or maybe as I gain more experience with data science and data analysis things will become more intuitive to me.  Matt has also suggested I use another unsupervised learning technique on my data: clustering.  I'll be sure to share what I learn about it.


