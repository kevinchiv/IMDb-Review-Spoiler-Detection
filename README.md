# IMDb Review Spoiler Detection

## Description: 
I recently developed a data pipeline to extract more insight from Amazon product reviews. Given several comparable options, it can be difficult to select the product that best fits your needs. When it comes to learning more about a product’s features, ratings can be too vague while comments can be too product-specific. 

## Objective: 
The goal is to make it easier to understand compare products on Amazon.

## Methodology: 
The data set was found at https://s3.amazonaws.com/amazon-reviews-pds/readme.html

Here are the features I started with:
- marketplace, the country the item is sold from 
- customer_id, the ID of the customer who wrote the review
- review_id, the internal ID of the review
- product_id, the ID of a specific product
- product_parent, the group ID that a specific product belongs to
- product_category, what Amazon product category a product belongs to
- product_title, the title of a product
- star_rating, the star rating given in the review
- helpful_votes, the number of upvotes a review received
- total_votes, the number of upvotes and downvotes a review received
- vine string, an indication of whether the review was written by someone in the Amazon Vine program
- verified_purchase, whether the customer bought the product before reviewing
- review_headline, the headline of the revoew
- review_body, the actual text in the review 
- review_date, the date the review was written
- year, the year the review was written

As mentioned before, ratings can be too vague while comments can be too product-specific. I used logistic regression, sentiment analysis, and word2vec to strike a balance between these two levels of granularity. 

As proof of concept, I processed Amazon product review data for laptops. For each laptop, I chose to focus on particularly helpful reviews and those written by the Amazon Vine program. I then decomposed each select review into sentence tokens to better capture consumer sentiment towards each product feature. 

Afterwards, I used logistic regression to identify bigrams and trigrams in each sentence token that were frequently associated with positive or negative sentiment. This allowed me to produce a list of product-specific pros or cons. I then used word2vec to filter and map these product-specific pros or cons to more general product features. For example, words like “glossy screen” and “full screen” in figure 1 were assigned to the “screen” feature label. 

I then collected the average sentiment about each general product feature to produce a more visual method of comparing products. This is encapsulated in the radar plots of sentiments I produced. I selected a few general product features laptop consumers might be interested in; these are the ones that appear on the circumference of the radar plot. The sentiment scale goes from 0 to 1, with 0 being most negative and 1 being most positive. 


## Results: <br>
I would refer to the images stored in the "img" folder.

Logistic regression paired with some text filtering produced some specific, human-interpretible pros and cons.

Generally speaking, a larger polygon area on the radar plot of sentiment represents a better product. In "macbook vs asus laptop.png", the Apple MacBook Pro 13.3 performs much better than the ASUS laptop in terms of ergonomics, build quality, battery life, and perceived value. On the other hand, the ASUS laptop performs better in terms of performance specs, cooling, and the number of ports.  

I further extended my work to compare between brands as well. This is helpful in cases where a customer wants to study brand reputations to better understand their potential purchases. In "apple vs asus.png", the blue polygon represents customer sentiment towards Apple laptops while the green polygon represents customer sentiment towards ASUS laptops for these features.

There are some caveats and room for future improvements, but the model seems generally extendable to other product categories.
