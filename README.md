# Movie-Recommendation System

A practical, end-to-end movie recommendation system implemented in a Jupyter Notebook. It showcases three complementary approaches:
- User-based collaborative filtering: recommends movies enjoyed by users with similar tastes.
- Item-based collaborative filtering: recommends movies similar to a selected title.
- Pixie-inspired random walks (graph-based): explores a user–movie bipartite graph via weighted random walks to surface diverse, serendipitous titles.

Clean data handling, clear methodology, and reproducible outputs make this a solid foundation for learning and extension.

## Dataset

MovieLens 100K (ml-100k):
- 100,000 ratings (1–5) from 943 users on 1,682 movies.
- Files used:
  - u.data: user_id, movie_id, rating, timestamp
  - u.item: movie_id, title, release_date (other columns not required)
  - u.user: user_id, age, gender, occupation, zip_code

This project primarily uses interaction data (user_id, movie_id, rating, timestamp) and minimal metadata (movie_id, title, release_date) for readable outputs.

## Features

- Data preparation
  - Correct delimiter/encoding handling (tabs/pipes, latin-1)
  - Timestamp conversion to readable datetime
  - Missing value checks and dataset summaries
  - Exports cleaned DataFrames to CSV for reuse

- User-based Collaborative Filtering
  - User–movie pivot matrix
  - Cosine user–user similarity
  - Top-N recommendations for a given user

- Item-based Collaborative Filtering
  - Transposed matrix (movies × users)
  - Cosine item–item similarity
  - Top-N similar movies for a given title

- Pixie-Inspired Random Walks (Graph-Based)
  - Merge, aggregate, and user-mean normalization of ratings
  - Bipartite adjacency list (user ↔ movie)
  - Degree-weighted random walks (user- or movie-started)
  - Rank movies by visit frequency

## Repository Structure

- Programming_Assignment-1.ipynb — Main notebook (data loading, modeling, results)
- ratings.csv — Cleaned ratings (exported)
- movies.csv — Cleaned movies (3-column version, exported)
- users.csv — Cleaned users (exported)

## Getting Started

Requirements:
- Python 3.9+
- Jupyter Notebook/Lab
- Packages: pandas, numpy, scikit-learn

Setup:
1. Clone the repo:
   - git clone https://github.com/Rachietaa/Movie-Recommendation
   - cd Movie-Recommendation
2. Create a virtual environment (optional) and install packages:
   - pip install pandas numpy scikit-learn
3. Open the notebook:
   - jupyter notebook Programming_Assignment-1.ipynb
4. Run cells in order:
   - Data loading → Exploration → CF methods → Graph-based method

## Usage

- User-based recommendations:
  - recommend_movies_for_user(user_id=10, num=5)
- Item-based similar movies:
  - recommend_movies("Jurassic Park (1993)", num=5)
- Pixie-style recommendations:
  - weighted_pixie_recommend(user_id=1, walk_length=15, num=5)
  - weighted_pixie_recommend_movie("Jurassic Park (1993)", walk_length=10, num=5)

Each function returns a clean, ranked DataFrame with columns: Ranking, Movie Name.

## Methodology (Summary)

- User-based CF:
  - Build a user–movie matrix, fill NaNs with 0.0, compute cosine similarity between user vectors.
  - Aggregate neighbor ratings for movies unseen by the target user (unweighted mean) and rank.

- Item-based CF:
  - Transpose to movies × users, compute cosine similarity between movie vectors.
  - Rank the most similar movies to the queried title (self excluded).

- Pixie-inspired Random Walks:
  - Construct a user–movie bipartite graph from normalized ratings.
  - Simulate degree-weighted random walks; count movie visits; rank by frequency.

## Example Results

- User-based CF (e.g., user 10):
  - Returns a personalized top-5 aligned with the preferences of highly similar users.
- Item-based CF (“Jurassic Park (1993)”):
  - Surfaces highly similar action/adventure titles (e.g., Top Gun, The Empire Strikes Back).
- Random walks (user 1 or “Jurassic Park (1993)”):
  - Produces diverse sets spanning genres and decades through multi-hop exploration.

## Notes and Limitations

- User-based CF uses unweighted neighbor averaging; can be upgraded to similarity-weighted aggregation.
- Cosine similarity is used without shrinkage/significance weighting.
- Random walks use degree-based weighting; adding restarts, multi-seed walks, or edge/weight designs can improve control and quality.

## Roadmap

- Add similarity-weighted CF and shrinkage
- Introduce evaluation metrics (Precision@K, Recall@K, MAP)
- Parameter sweeps for walk length and sampling strategies
- Hybridize CF and graph signals
- Include content/genre features for cold-start handling and re-ranking

## Data Source

- MovieLens 100K dataset by GroupLens (University of Minnesota). Please refer to the official MovieLens site for license and citation details.

## Contributing

Contributions are welcome! Open an issue to discuss improvements or submit a pull request with a clear description and minimal, focused changes.

## Contact

For questions or suggestions, please open an issue in this repository.
