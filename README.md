# project_for_cricket_Data

**Question 1.**

1. Data Retrieval: Downloading files directly from cricsheet.org.
2. Preprocessing: Parsing and cleaning the data if required.
3. Database Setup: Creating a database schema to store match results, innings data, and player information.
4. Ingestion Process: Developing a script to read data files, transform data as needed, and insert it into the database

**Question 2**

**a)** Win Records for Each Team by Year and Gender
-- SQL Query for win records for each team by year and gender
-- Exclude ties, no results, and matches decided by DLS
SELECT 
    t.team_name,
    m.year,
    m.gender,
    COUNT(CASE WHEN m.match_result = 'won' THEN 1 END) AS total_wins,
    ROUND((COUNT(CASE WHEN m.match_result = 'won' THEN 1 END) / COUNT(*)) * 100, 2) AS win_percentage
FROM matches m
JOIN teams t ON m.team_id = t.team_id
WHERE m.match_result NOT IN ('tie', 'no_result', 'DLS')
GROUP BY t.team_name, m.year, m.gender
ORDER BY m.year, m.gender, win_percentage DESC;


**b)** Male and Female Teams with the Highest Win Percentages in 2019
-- SQL Query to find male and female teams with the highest win percentages in 2019
-- Exclude specific match types
SELECT 
    t.team_name,
    m.gender,
    ROUND((COUNT(CASE WHEN m.match_result = 'won' THEN 1 END) / COUNT(*)) * 100, 2) AS win_percentage
FROM matches m
JOIN teams t ON m.team_id = t.team_id
WHERE m.year = 2019
    AND m.match_result NOT IN ('tie', 'no_result', 'DLS')
GROUP BY t.team_name, m.gender
ORDER BY m.gender, win_percentage DESC
LIMIT 1; -- for each gender, this will give the team with the highest win percentage in 2019

**c)** Players with the Highest Strike Rate as Batsmen in 2019 
-- SQL Query to identify players with the highest strike rate as batsmen in 2019
-- Handling extras properly
SELECT 
    p.player_name,
    ROUND(SUM(i.runs) / (SUM(i.balls_faced) - SUM(i.extras)) * 100, 2) AS strike_rate
FROM innings i
JOIN players p ON i.player_id = p.player_id
JOIN matches m ON i.match_id = m.match_id
WHERE m.year = 2019
GROUP BY p.player_name
ORDER BY strike_rate DESC
LIMIT 1; -- This will give the player with the highest strike rate in 2019, accounting for extras

**Question 3**: Incremental Loading
If the use case involves incrementally loading new match data, the solution would change by implementing mechanisms like:

1.Using timestamp or version identifiers to track and load only new or updated data.
2.Employing change data capture (CDC) techniques to identify and sync only modified or new records.
3.Building scheduled jobs or triggers to handle regular data updates without reloading the entire dataset.

**Question 4**: Learning Experience
Discuss an instance where I learned a new technique during a project, I also got to explore a lot of things.

**Question 5** Troubleshooting Experience
when I begin writing code. I have a lot of difficulty taking the data table and its columns into consideration. I attempted to answer the issue after taking into consideration the table's column.
