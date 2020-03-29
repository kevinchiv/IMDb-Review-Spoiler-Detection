# IMDb Review Spoiler Detection

## Description:
Typically, when searching for a good movie to watch, we visit sites like IMDb to identify some potential picks or learn more about them. And typically, we also look at the review section in order to see how others felt about a movie. However, reviews can contain spoilers and some people prefer to not have any plot points revealed. In some extreme cases, some people will eschew watching a movie all together during a premiere if the plot has been spoiled. Hence, this can hurt ticket sales for movies, especially those that were filmed with the intent of becoming the highest-grossing film of all time.

## Objective:
Not everyone labels their review their review with a spoiler alert. There is also no moderation of the reviews.

The goal is to use unsupervised learning techniques like natural language processing and clustering to flag reviews with spoiler. Essentially, we are creating spoiler alerts.

## Methodology:
The data set was found at https://www.kaggle.com/rmisra/imdb-spoiler-dataset/data

Here are the features I started with for IMDB_movie_details.json:
- movie_id, unique id of the movie/tv-show.
- plot_summary, plot summary of the item and does not contain spoilers.
- duration, item runtime duration.
- genre, associated genres.
- rating, overall rating for the item.
- release_date, release date of the item.

Here are the features I started with for IMDB_reviews.json:
- review_date, date the review was written.
- movie_id, unique id for the item.
- user_id, unique id for the review author.
- is_spoiler, indication of whether review contains a spoiler
- review_text, text review about the item.
- rating, rating given by the user to the item.
- review_summary, short summary of the review.

I chose to ignore 'is_spoiler' since this project was an unsupervised learning task and the is_spoiler was subjectively determined.

In addition, I used the data at https://datasets.imdbws.com/
- movie_id, unique id of the movie/tv-show
- title, English title of the movie/tv-show

I solely focused on movies with decently long plot synopses i.e. 50 sentences. I then used natural language processing techniques like vectorizing, topic modeling (via NMF, LDA, and LSA), and cosine similarity to compare each review to its respective plot synopsis. If a review had relatively high similarity scores to the plot synopsis, I considered it a review with spoiler content. Afterwards, I supplemented the similarity scores with some text metrics. I then used a subset of the scores and text metrics as features for clustering to identify two groups of reviews - those with spoiler content and those without.

## Results:
Since I was only interested in having two clusters, metrics like inertia and sihoulette coefficient were not valid for my project. I mainly relied on visual inspection through visualizations like a UMAP projection to see how distinct the clusters were. I found that the two clusters were visually distinct and separate enough from each other.

The results indicate that:
- reviews with higher similarity scores were flagged as ones with spoiler content
- reviews flagged as spoiler content also had higher word difficulty scores

I think high similarity scores go hand in hand with high word difficult scores. A higher word difficulty score does not necessarily mean a user is using more advanced vocabularly. Rather, they are most likely mistyping words as they write their reviews. This carelessness in grammar could go hand in hand with a disregard for spoiling a movie to those who have yet to watch.

Afterwards, I fed the non-spoiler reviews into a text generative model i.e. gpt2 to see if I could produce more data. Admittedly, I did not have the gpu performance to produce many reviews, but the generated reviews are very coherent. Absolute loss is not a measure of text generation performance, but the change in the absolute loss does indicate whether the text will improve or not.
