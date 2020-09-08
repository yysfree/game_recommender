## game_recommender

### Problem Statement: a recommender system for users, based on their preferences and their gaming habits

Recommender systems are widely used these days to push potential interested products for customers, such as books, videos and movies. Companies across many different areas have implemented recommendation systems to enhance their customer’s shopping experience, increase sales and retain customers. The game industry is constantly evolving, just the PC gaming market is set to be worth $45.5 billion in 2021, and there are about 1.2 million different games in the market. As a decent former gamer, I am wondering if I can build a recommender system to produce a list of games, I have never played but I may have interest in.

## Dataset: Steam, Kaggle
This project will be using two datasets, both are available to download on Kaggle.
The first dataset is the user dataset. It contains the user id, the game title, the behavior ('purchase' or 'play') and a value associated to the behavior, the amounts of hours played. Each row of the dataset represents the behavior of a user towards a game. The user dataset contains a total of 200,000 rows, including 5,155 unique games and 12,393 unique users.
The second one is the game dataset, it contains a list of games, their unique id, the name, release date, developer, publisher, platforms, required age, categories, popular tags, achievements, genre, number of positive and negative ratings, playtime, number of owners, and price. Each row represents a single game with 17 features, and there are 27075 different games in the dataset.


## Preprocess: Cleaning and Reformatting
For the user dataset, first dropped the unrelated column, which only has value "0", and won't be used in the project. Then the ‘behaviour’ column has been reformatted into two new columns ‘purchase’ and ‘play’, so it become more clearly to represent the interaction between user and each game.
Two histograms were created to show if there is a correlation between most played and most purchased game. Therefore, I can have a better understanding of user data distribution and user's playing habits.

In most cases, there is a relation between most played and most purchased. But for some cases a high number of users does not represent an equivalent high total of hours played. These games might be purchased as part of game bundle. 
For the game dataset, it was combined from steam dataset and description dataset. The genre and category columns were converted to numeric values of 0s and 1s, then it’s better to demonstrate the breakdown of genres, and how the average amount of owners per game compares to the proportion of games in each genre.


## Build Recommender: two collaborative filtering and one content-based filtering
The data we have is implicit data. Unlike explicit data, there is no negative feedback, the dataset has only positive feedback, in this particular case is the total hours a user has played in a game, we couldn’t tell the missing value is because the customer did not like the game or just doesn’t know it.
And the numerical value refers to a frequency which might not necessarily reflect the customer’s preference. So, we need to deduce a metric that shows customer’s confidence.


## Collaborative recommender
The model is based a user's past behaviour as well as similar decisions made by other users. 

The first one is a user based collaborative filtering I chose to deduce a metric that shows customer’s confidence by using matrix factorization. By using the singular value decomposition, it decomposes the user matrix into the best lower rank approximation of the original matrix, and create vectors underlying tastes and preferences. I use the SVD from SciPy, the function returns one user ratings matrix, shows how much user like the game, one game matrix shows how relevant each to each game, and one diagonal matrix of singular values (essentially weights). Then add the user means back to get the actual game ratings prediction, then make the recommendation from highest predicted rating the user haven’t played before.

The second one is item based, and it uses the patterns of users who played the same game to recommend new games. I choose K-nearest neighbour as a go-to model, and the model use a pre-defined distance metric to find clusters of similar games based on play hours. By using CSR Metrix function from SciPy to transform the pivot table, then fit the KNN model and make recommendations using the distance metric in item ratings of top k nearest neighbors.


## Content based recommender
The last one is a content-based filtering, it gives recommendation based on the similarity between the game a user already has and other games. I select game description as input to the recommender algorithmI use the NTLK to create a tokenizer and use TF-IDF to convert the game description into numeric format.Then build the similarity matrix using cosine similarity, and make recommendations rank the similarity between each game.
