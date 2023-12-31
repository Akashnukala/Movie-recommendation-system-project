# Movie-recommendation-system-project
import pandas as pd
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.metrics.pairwise import cosine_similarity

# Step 1: Data Collection (Assuming movie data is already available in a CSV file)
movies_data = pd.read_csv('movies.csv')

# Step 2: Data Preprocessing
# Clean and preprocess the movie data (e.g., handle missing values, convert data types)

# Step 3: Exploratory Data Analysis
# Generate statistics and create visualizations to gain insights into the movie dataset

# Step 4: Recommendation Algorithm (Content-Based Filtering)
# Create a TF-IDF vectorizer
vectorizer = TfidfVectorizer(stop_words='english')

# Apply the vectorizer on the 'genres' column to create the TF-IDF matrix
tfidf_matrix = vectorizer.fit_transform(movies_data['genres'].fillna(''))

# Compute the cosine similarity matrix
cosine_sim_matrix = cosine_similarity(tfidf_matrix, tfidf_matrix)

# Function to get top n similar movies based on cosine similarity
def get_similar_movies(movie_title, cosine_sim_matrix, movies_data, top_n=5):
    # Get the index of the movie
    movie_index = movies_data[movies_data['title'] == movie_title].index[0]

    # Get the pairwise similarity scores for the movie
    movie_scores = list(enumerate(cosine_sim_matrix[movie_index]))

    # Sort the movies based on similarity scores
    movie_scores = sorted(movie_scores, key=lambda x: x[1], reverse=True)

    # Get the top n similar movies (excluding the input movie itself)
    top_movies = [movies_data.iloc[score[0]]['title'] for score in movie_scores[1:top_n+1]]

    return top_movies

# Step 5: User Interface (Example function to interact with the recommendation system)
def recommend_movies(movie_title):
    similar_movies = get_similar_movies(movie_title, cosine_sim_matrix, movies_data)
    print(f"Recommended movies for '{movie_title}':")
    for movie in similar_movies:
        print(movie)

# Example usage
recommend_movies('The Dark Knight')
