
# Recommendation System for Videos

## Objective
The goal of this project was to create a recommendation system that:  
1. **Personalizes recommendations** based on user history and engagement patterns.  
2. **Handles cold start problems**, ensuring recommendations for new users without prior interaction history.  
3. **Suggests trending content** by identifying and prioritizing recently popular videos.

---

## Approach

### 1. Data Collection
- Data was retrieved using APIs providing information about videos/posts, including attributes like titles, tags, engagement metrics, genres, and popularity scores.
- Unfortunately, **user data could not be retrieved** due to persistent errors while accessing the APIs. Despite trying multiple solutions, this issue remained unresolved. 

---

### 2. Decision: User Profile Creation
- Due to the unavailability of user data, a **mock user profile** was designed using the data available from the posts dataset.  
- The user profile consisted of inferred genres, interests, and engagement metrics based on general trends in the dataset.
- **Key Attributes in the User Profile**:  
  - `genres`: Derived from trending and popular video genres.  
  - `interests`: Inferred from frequently occurring tags in the posts.  
  - `titles`: Popular video titles, used to simulate user preferences.  

---

### 3. Data Preprocessing
- Removed irrelevant columns to focus on meaningful attributes (`id`, `title`, `tags`, `popularity_score`, etc.).
- Addressed empty or null values to ensure data consistency.


---

### 4. Feature Engineering
To enhance the quality of recommendations, new columns were engineered:
- **Feature Engineering**: Two new columns, `targeted_audience` and `post_description`, were created or extracted from the `post_summary` column. The `post_summary` contained complex information that was broken down into simpler, more useful columns. 

 
 - **Popularity Score Calculation**: A new column called `popularity_score` was created from the `rating_count` and `average_rating` columns. The formula used was:

    ```
    popularity_score = average_rating × log(rating_count + 1)
    ```

   This formula provides a weighted measure of popularity, taking both average ratings and the number of ratings into account. 

---

### 5. Recommendation Algorithm
#### a) Personalized Recommendations
- Analyzed inferred user preferences (genres and interests) from the mock user profile.  
- Built a similarity matrix using the `tags` column to compute the similarity between videos.  
  Tools: **sklearn** and **numpy**.  
- Recommended videos with the highest similarity scores to the simulated user preferences.

#### b) Cold Start Problem
- For new users, recommendations were based on:  
  1. Genres included in the mock user profile.  
  2. Trending videos identified by filtering recent posts and sorting them by popularity scores.

#### c) Trending Content
- Defined "trending" based on the number of days since posting (`posted_n_days_ago`) and the engagement score.
- Sorted trending videos by `popularity_score` and removed duplicates based on the title to ensure diversity.

---

## Key Functionalities
1. **Personalization**:  
   - Tailored suggestions using the simulated user profile and similarity search.  
   - Example: A user profile indicating interest in "Educational" videos with tags like `['Learning', 'Development']` received top-rated videos from the same genre and tags.

2. **Cold Start Problem**:  
   - New users were recommended the most popular videos from their selected genres.  

3. **Trending Content**:  
   - Trending videos were dynamically updated based on recency and engagement scores.  

---

## Model Architecture
The system primarily relies on:
- **Content-Based Filtering**:  
   - Utilized the similarity matrix for personalized recommendations.  
   - Tags and genres formed the core attributes for similarity calculations.  

- **Feature-Based Ranking**:  
   - Trending videos were ranked by the engagement and popularity metrics.  

---

### Key Decisions
1. **Feature Engineering**:  
   - Incorporating an engagement score ensured videos with higher user interest were prioritized.  
2. **User Profile Creation**:  
   - The absence of user interaction data required the creation of a simulated profile based on general trends in the dataset.  
3. **Handling Duplicates**:  
   - Removed duplicate titles to improve diversity in recommendations.  
4. **Cold Start Mechanism**:  
   - Focused on genres and trending content for new users to mitigate the absence of interaction history.

---

## Challenges and Solutions
### Challenge 1: Unavailability of User Data
- **Issue**: Persistent errors while retrieving user interaction data made it impossible to access essential information.  
- **Solution**: A mock user profile was created using trends from the posts dataset. Although not ideal, this workaround enabled the system to simulate personalization.

### Challenge 2: Evaluating the Model
- **Issue**: Implementing evaluation metrics like **Precision@K**, **Recall@K**, and **NDCG** was challenging due to limited feedback data and the complexity of these metrics.  
- **Solution**: While these metrics remain a work in progress, trending content and cold start functionality were validated through manual analysis and user feedback simulations.

### Challenge 3: Cold Start Problem
- **Issue**: Recommending meaningful content to new users with no history.  
- **Solution**: Leveraged inferred genres and trending videos to provide relevant recommendations.

---