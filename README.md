# Music Recommendation Engine 🎵

Welcome to the **Music Recommendation Engine** repository! This project chronicles the journey of building a system capable of predicting how much a user will love a song. Grab your headphones, and let's dive into the story.

## What the repo is about 📖

Imagine scrolling through endless lists of songs, unable to decide what to play next. What if an intelligent system could just *know* what you're in the mood for? This repository is all about creating that magic.

The core goal of this project is to build an intelligent Music Recommendation Engine. To achieve this, it predicts not just if a user will 'like' a song, but also how long they will actually spend listening to it. By blending these two signals—the explicit "thumbs up" and the implicit "time spent"—we can rank songs in a way that truly resonates with the user's taste.

## What I did 🛠️

My journey started with a rich dataset containing user demographics, listening context, and track features (tempo, energy, acousticness, and more). Here is the step-by-step breakdown of how the engine was crafted:

1. **Data Wrangling and Exploration**: I dove deep into the data, handling missing values, encoding categorical strings (like user location, gender, and genre), and making sure the feature set was pristine for modeling.

2. **The Dual-Model Approach**: Predicting user preference isn't a single task. I decided to build two separate models using **XGBoost**:
   - A **Classifier** to predict if a user will explicitly 'like' a song (a binary outcome).
   - A **Regressor** to predict the 'time spent on a song' (a continuous measure of engagement).

3. **Hyperparameter Tuning**: Good models need fine-tuning. I used powerful search techniques:
   - *Hyperopt* for the initial exploration of the XGBoost classifier.
   - *RandomizedSearchCV* for deep-diving into the best parameters for both the classification and regression models, optimizing for log-loss and squared error, respectively.

4. **Learning-to-Rank (LTR)**: This was the crucial final piece! Having two models is great, but how do we combine them to actually *rank* the songs for a user? I created an LTR framework that blended the probability of a "like" with the predicted "duration".
   - I first did a sweep over different weight combinations on a validation set.
   - Then, I implemented a custom **Coordinate Ascent** algorithm to automatically fine-tune the weights `w1` (for liked probability) and `w2` (for duration) to maximize our ranking metric: **NDCG@10**.

## What's the result 🏆

The results were thrilling!

The individual models performed solidly, but the real star of the show was the **Learning-to-Rank** system.

By optimizing the blend of our classifier and regressor, the Coordinate Ascent algorithm pinpointed the perfect balance:
- **Weight 1 (Liked Probability):** 0.40
- **Weight 2 (Listening Duration):** 0.60

When put to the ultimate test on unseen data, the final LTR model achieved an impressive **NDCG@10 of 0.6068**! This means the engine is highly effective at surfacing the most relevant, engaging songs right at the top of a user's recommendations.

The journey from raw data to a tuned, two-pronged recommendation engine was a success, proving that when you listen to both what users say (likes) and what they do (time spent), you can hit the perfect note.
