# IMDb-Movie-Analytics-SQL
Comprehensive SQL analytics project analyzing IMDb movie database (6 tables, 29+ advanced queries) covering genre trends, production insights, rating analysis, and business recommendations(from 2017-2019) using window functions, CTEs, and multi-table JOINs.

🎯 Business Problem
RSVP Movies needs data-driven insights for:
Optimal movie genres & duration
Top production houses for partnerships
Best Indian actors/actresses for casting
Hit movie patterns (rating > 8)

📊 Database Schema
6 Tables: movie | genre | ratings | names | director_mapping | role_mapping
Core analysis: Multi-table JOINs + Window Functions

Quick Setup
-- MySQL Workbench
DROP DATABASE IF EXISTS imdb;
CREATE DATABASE imdb; USE imdb;

Key Queries Demonstrated
1. Data Quality Analysis
-- Null values across movie table (3724 gross income nulls found)
SELECT SUM(CASE WHEN worlwide_gross_income IS NULL THEN 1 ELSE 0 END)
FROM movie;

2. Top Insights
* Drama = #1 genre by volume
* Thriller = #3 genre ranking  
* USA/India: 2000+ movies (2019)
* Optimal duration: 106 mins (Drama avg)
* Vijay Sethupathi = Top Indian actor

3. Advanced Window Function
-- Top 10 movies by rating (with ties)
WITH top_movies AS (
  SELECT m.title, r.avg_rating,
  RANK() OVER(ORDER BY r.avg_rating DESC) AS rank
  FROM movie m JOIN ratings r ON m.id = r.movie_id
)
SELECT * FROM top_movies WHERE rank <= 10;

* Business Recommendations
Genre Focus: Drama + Thriller (highest volume)
Casting: Vijay Sethupathi (proven ratings)
Duration: 100-110 mins optimal
Partners: Dream Warrior Pictures (hit specialist)

* Skills Mastered
Multi-table JOINs (6 tables)
Window Functions (RANK/DENSE_RANK)
CTEs & Subqueries
Data Quality Analysis
Business Intelligence
