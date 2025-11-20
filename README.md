<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <title>Movie Recommendation System – Derek Lamb</title>
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <!-- If you're using this in your portfolio site, keep this line: -->
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="wrap">
    <!-- Optional: Back to portfolio button -->
    <p><a href="index.html" class="btn btn-secondary">← Back to Portfolio</a></p>

    <h1>🎬 Movie Recommendation System (Content-Based)</h1>
    <p>
      This project implements a <strong>content-based movie recommendation system</strong> using the
      MovieLens dataset. It recommends movies based on their <strong>genres</strong>, finding titles that are
      most similar to a given input movie. Recommendations are generated using 
      <strong>cosine similarity</strong> on a binary genre feature matrix.
    </p>

    <hr />

    <h2>🧠 What This Project Does</h2>
    <ul>
      <li>Loads movie metadata (titles and genres) and ratings data.</li>
      <li>Merges the datasets on <code>movieId</code> so titles and genres are aligned with user ratings.</li>
      <li>Converts the <code>genres</code> column into a set of binary indicator features (one column per genre).</li>
      <li>Builds a genre matrix and computes cosine similarity between all pairs of movies.</li>
      <li>Provides a <code>recommend_movies()</code> function that returns the top <em>N</em> most similar movies to a given title.</li>
    </ul>
    <p>
      This is a classic example of <strong>content-based filtering</strong>, where we recommend items based on
      their attributes rather than on user–user similarity.
    </p>

    <h2>📂 Data</h2>
    <p>This project expects the standard MovieLens-style input:</p>
    <ul>
      <li>
        <strong><code>movies.csv</code></strong>
        <ul>
          <li><code>movieId</code></li>
          <li><code>title</code></li>
          <li><code>genres</code> (pipe-separated list, e.g., <code>Adventure|Children|Fantasy</code>)</li>
        </ul>
      </li>
      <li>
        <strong><code>ratings.csv</code></strong>
        <ul>
          <li><code>userId</code></li>
          <li><code>movieId</code></li>
          <li><code>rating</code></li>
          <li><code>timestamp</code></li>
        </ul>
      </li>
    </ul>
    <p>The code merges <code>movies.csv</code> and <code>ratings.csv</code> on <code>movieId</code> to create a combined DataFrame with titles, genres, and ratings.</p>

    <h2>🛠 Methods &amp; Approach</h2>

    <h3>1. Load datasets</h3>
    <pre><code>import pandas as pd

movies_df = pd.read_csv("movies.csv")
ratings_df = pd.read_csv("ratings.csv")

movie_ratings_df = ratings_df.merge(movies_df, on="movieId")
</code></pre>

    <h3>2. One-hot encode genres</h3>
    <p>Genres are split and converted into a binary feature matrix using <code>str.get_dummies(sep="|")</code>:</p>
    <pre><code>genres_split = movies_df["genres"].str.get_dummies(sep="|")
genre_matrix = genres_split.values
</code></pre>

    <h3>3. Compute cosine similarity</h3>
    <pre><code>from sklearn.metrics.pairwise import cosine_similarity

cosine_sim = cosine_similarity(genre_matrix)
</code></pre>

    <h3>4. Recommendation function</h3>
    <pre><code>def recommend_movies(input_movie, num_recommendations=10):
    # Check if the movie exists
    if input_movie not in movies_df["title"].values:
        return "Movie not found in the dataset."

    # Find the index of the input movie
    idx = movies_df[movies_df["title"] == input_movie].index[0]

    # Get similarity scores for that movie
    sim_scores = list(enumerate(cosine_sim[idx]))

    # Sort by similarity (highest first)
    sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True)

    # Skip the first (it’s the movie itself) and take top N
    sim_scores = sim_scores[1 : num_recommendations + 1]

    # Get the corresponding movie indices
    movie_indices = [i[0] for i in sim_scores]

    # Return the titles of the recommended movies
    return movies_df["title"].iloc[movie_indices].tolist()
</code></pre>

    <h2>▶️ Example Usage</h2>
    <pre><code># Example usage of the recommender system
recommended_movies = recommend_movies("Jumanji (1995)", num_recommendations=5)
print(recommended_movies)
</code></pre>

    <p>Example output:</p>
    <pre><code>[
  "Indian in the Cupboard, The (1995)",
  "NeverEnding Story III, The (1994)",
  "Escape to Witch Mountain (1975)",
  "Darby O'Gill and the Little People (1959)",
  "Return to Oz (1985)"
]
</code></pre>

    <h2>💻 Requirements</h2>
    <ul>
      <li>Python 3.x</li>
      <li>Libraries:
        <ul>
          <li><code>pandas</code></li>
          <li><code>numpy</code></li>
          <li><code>scikit-learn</code> (for <code>cosine_similarity</code>)</li>
          <li><code>jupyter</code> (if running the notebook)</li>
        </ul>
      </li>
    </ul>

    <pre><code>pip install pandas numpy scikit-learn jupyter
</code></pre>

    <h2>🚀 How to Run</h2>
    <ol>
      <li>
        <strong>Clone this repository</strong><br />
        <pre><code>git clone https://github.com/USERNAME/REPO_NAME.git
cd REPO_NAME
</code></pre>
      </li>
      <li>
        <strong>Place your <code>movies.csv</code> and <code>ratings.csv</code> files</strong> in the project directory.
      </li>
      <li>
        <strong>Launch Jupyter Notebook</strong><br />
        <pre><code>jupyter notebook
</code></pre>
      </li>
      <li>
        Open the notebook (or script) containing the code and run all cells.
      </li>
      <li>
        Call <code>recommend_movies("&lt;movie title&gt;", num_recommendations=N)</code> to get recommendations.
      </li>
    </ol>

    <h2>📈 Possible Extensions</h2>
    <ul>
      <li>Incorporate ratings more directly (e.g., weighting by average user rating).</li>
      <li>Combine genres with text features from movie descriptions.</li>
      <li>Add user-based or item-based collaborative filtering as a second model.</li>
      <li>Wrap the recommender in a Flask/FastAPI service to expose it as an API.</li>
    </ul>

    <h2>🔗 Links</h2>
    <ul>
      <li><strong>Project repository:</strong> <em>(update with your actual GitHub repo URL)</em></li>
      <li><strong>Portfolio page:</strong> <em>(link to this HTML page from your main portfolio)</em></li>
    </ul>

    <footer style="margin-top:40px;color:var(--muted);">
      © <span id="year"></span> Derek Lamb
    </footer>
  </div>

  <script>
    document.getElementById('year').textContent = new Date().getFullYear();
  </script>
</body>
</html>
