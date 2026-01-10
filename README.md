# IMDb-Movie-Analytics-SQL
Comprehensive SQL analytics project analyzing IMDb movie database (6 tables, 29+ advanced queries) covering genre trends, production insights, rating analysis, and business recommendations(from 2017-2019) using window functions, CTEs, and multi-table JOINs.
IMDb Movie Analytics SQL Project

Comprehensive SQL analysis of IMDb movie database covering data exploration, genre analysis, rating trends, production insights, and advanced window function queries for data analyst portfolios.

Project Overview
This project demonstrates end-to-end SQL analysis on a relational IMDb movie database built from UpGrad coursework. It includes database setup with ERD, comprehensive exploration queries (29+ questions), null value analysis, ranking functions, and business insights for movie production decisions.
​

Key Skills Demonstrated:

Multi-table JOINs across 6 tables (movie, genre, ratings, names, director_mapping, role_mapping)

Window functions (RANK, DENSE_RANK, ROW_NUMBER)

Complex aggregations with CASE statements

Null value analysis and data quality assessment

Business insights for production houses and casting

Project Structure
text
IMDb-Movie-Analytics/
├── IMDb-movies-Data-and-ERD.xlsx      (ERD + Sample data)
├── IMDB-dataset-import.sql           (Database schema + sample data)
└── IMDB-solution-Chaitra.H-1.sql     (29 SQL analysis queries)[file:59]
Database Schema (6 related tables):

movie: Core table (id, title, year, duration, country, worldwide_gross_income)

genre: Movie-genre mapping

ratings: avg_rating, total_votes, median_rating

names: Director/actor details

director_mapping, role_mapping: Relationship tables
​

Database Setup
sql
-- Run in MySQL Workbench
DROP DATABASE IF EXISTS imdb;
CREATE DATABASE imdb;
USE imdb;
-- Execute IMDB-dataset-import.sql for schema + sample data[file:58]
Data Exploration Queries
Sample Queries from Solution File
​

Q1. Row counts across all tables:

sql
SELECT 'movie' AS table_name, COUNT(*) AS row_count FROM movie
UNION ALL SELECT 'ratings', COUNT(*) FROM ratings;
Q2. Null value analysis:

sql
-- Identifies 3724 nulls in worldwide_gross_income, 528 in production_company
SELECT SUM(CASE WHEN worlwide_gross_income IS NULL THEN 1 ELSE 0 END) AS gross_nulls
FROM movie;
Q6. Top genre by movie count:

sql
SELECT genre, COUNT(*) AS movie_count
FROM genre GROUP BY genre ORDER BY movie_count DESC LIMIT 1;
-- Result: Drama (highest volume)
Key Analysis Queries
1. Top Movies & Ratings
Q11. Top 10 movies by average rating (with ties):

sql
WITH top_movies AS (
    SELECT m.title, r.avg_rating,
    RANK() OVER (ORDER BY r.avg_rating DESC) AS movie_rank
    FROM movie m LEFT JOIN ratings r ON m.id = r.movie_id
)
SELECT * FROM top_movies WHERE movie_rank <= 10;
2. Genre Analysis
Q8. Average duration by genre:

sql
SELECT g.genre, ROUND(AVG(m.duration),2) AS avg_duration
FROM genre g JOIN movie m ON g.movie_id = m.id
WHERE m.duration IS NOT NULL
GROUP BY g.genre ORDER BY avg_duration DESC;
-- Drama: 106.77 mins (ideal duration insight)
Q9. Thriller genre rank:

sql
-- Thriller ranks #3 by movie volume
SELECT genre, COUNT(*) AS movie_count,
RANK() OVER (ORDER BY COUNT(*) DESC) AS genre_rank
FROM genre GROUP BY genre HAVING LOWER(genre) = 'thriller';
3. Production Insights
Q13. Top production house for hit movies (avg_rating > 8):

sql
SELECT production_company, COUNT(*) AS hit_movies,
RANK() OVER (ORDER BY COUNT(*) DESC) AS prod_rank
FROM movie m JOIN ratings r ON m.id = r.movie_id
WHERE r.avg_rating > 8 AND production_company IS NOT NULL
GROUP BY production_company ORDER BY hit_movies DESC LIMIT 1;
4. Advanced Window Functions
Q22. Top Indian actors (weighted avg rating):

sql
-- Uses weighted average: SUM(rating*votes)/SUM(votes)
-- Filters: 5+ Indian movies, category='actor'
WITH actor_stats AS (
    SELECT n.name, COUNT(DISTINCT m.id) AS movie_count,
    ROUND(SUM(r.avg_rating*r.total_votes)/SUM(r.total_votes),2) AS weighted_avg
    FROM role_mapping rm JOIN names n ON rm.name_id = n.id
    JOIN movie m ON rm.movie_id = m.id JOIN ratings r ON m.id = r.movie_id
    WHERE LOWER(rm.category) = 'actor' AND m.country LIKE '%India%'
    GROUP BY n.name HAVING movie_count >= 5
)
SELECT *, RANK() OVER (ORDER BY weighted_avg DESC) AS actor_rank
FROM actor_stats ORDER BY actor_rank;
Key Findings
Drama dominates by volume, Thriller ranks #3

3724 movies lack gross income data (data quality insight)

USA/India produced 2000+ movies in 2019 alone

Production companies like Dream Warrior Pictures excel in hits (>8 rating)

Vijay Sethupathi tops Indian actors by weighted rating

Optimal movie duration: ~106 minutes (Drama average)

Business Insights
RSVP Movies Strategy: Focus on Drama/Thriller genres (high volume + demand)

Casting: Target Vijay Sethupathi, top Indian actors with proven ratings

Partnerships: Collaborate with hit production houses (Dream Warrior Pictures)

Duration: Target 100-110 minutes for optimal audience reception

Technologies Used
MySQL (schema design, complex queries,window functions)

Excel (ERD visualization, data validation)

SQL Window Functions (RANK/DENSE_RANK for leaderboards)

CTE (Common Table Expressions for complex analysis)

Perfect for Data Analyst interviews - demonstrates real-world SQL skills across exploration, cleaning, advanced analytics, and business decision-making.
​

