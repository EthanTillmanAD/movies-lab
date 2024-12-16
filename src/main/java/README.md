
+---------+---------+-----------------+------------+--------+--------------------------+
| MovieID | Runtime | Genre           | IMDB_Score | Rating | Title                    |
+---------+---------+-----------------+------------+--------+--------------------------+
|       1 |     156 | Action          |        5.3 | PG-13  | Robin Hood               |
|       2 |     113 | Sci-Fi          |        6.0 | R      | Project Power            |
|       3 |      98 | Sci-Fi          |        6.1 | TV-MA  | Code 8                   |
|       4 |     116 | Action&Adventur |        6.5 | R      | BadBoys RideOrDie        |
|       5 |     112 | Comedy          |        5.7 | R      | The Machine              |
|       6 |     149 | Comedy          |        7.2 | R      | 21 Jump Street           |
|       7 |     221 | Mystery         |        7.1 | R      | Glass Onion a knives out |
|       8 |     110 | Sci-Fi          |        4.6 | PG     | Howard the Duck          |
|       9 |      83 | Horror          |        4.7 | TV-14  | Lavalantula              |
|      10 |     129 | Sci-Fi          |        7.2 | PG-13  | Starship Troopers        |
|      11 |      90 | Documentary     |        8.0 | R      | Walts With Bashir        |
|      12 |      96 | Comedy          |        7.1 | PG     | Spaceballs               |
|      13 |      92 | Animation       |        8.1 | G      | Monsters Inc.            |
|      14 |     100 | Stand-Up        |        8.3 | TV-MA  | Bo Burnham Make Happy    |
+---------+---------+-----------------+------------+--------+--------------------------+

Query to find all Sci-Fi:
SELECT * FROM movies WHERE Genre LIKE 'Sci%';

Output
+---------+---------+--------+------------+--------+-------------------+
| MovieID | Runtime | Genre  | IMDB_Score | Rating | Title             |
+---------+---------+--------+------------+--------+-------------------+
|       2 |     113 | Sci-Fi |        6.0 | R      | Project Power     |
|       3 |      98 | Sci-Fi |        6.1 | TV-MA  | Code 8            |
|       8 |     110 | Sci-Fi |        4.6 | PG     | Howard the Duck   |
|      10 |     129 | Sci-Fi |        7.2 | PG-13  | Starship Troopers |
+---------+---------+--------+------------+--------+-------------------+

Query to find films scored at least 6.5:
SELECT * FROM movies WHERE IMDB_Score >= 6.5;

Output
+---------+---------+-----------------+------------+--------+--------------------------+
| MovieID | Runtime | Genre           | IMDB_Score | Rating | Title                    |
+---------+---------+-----------------+------------+--------+--------------------------+
|       4 |     116 | Action&Adventur |        6.5 | R      | BadBoys RideOrDie        |
|       6 |     149 | Comedy          |        7.2 | R      | 21 Jump Street           |
|       7 |     221 | Mystery         |        7.1 | R      | Glass Onion a knives out |
|      10 |     129 | Sci-Fi          |        7.2 | PG-13  | Starship Troopers        |
|      11 |      90 | Documentary     |        8.0 | R      | Walts With Bashir        |
|      12 |      96 | Comedy          |        7.1 | PG     | Spaceballs               |
|      13 |      92 | Animation       |        8.1 | G      | Monsters Inc.            |
|      14 |     100 | Stand-Up        |        8.3 | TV-MA  | Bo Burnham Make Happy    |
+---------+---------+-----------------+------------+--------+--------------------------+

or 

SELECT * FROM movies WHERE IMDB_Score = 6.5;

Output
+---------+---------+-----------------+------------+--------+-------------------+
| MovieID | Runtime | Genre           | IMDB_Score | Rating | Title             |
+---------+---------+-----------------+------------+--------+-------------------+
|       4 |     116 | Action&Adventur |        6.5 | R      | BadBoys RideOrDie |
+---------+---------+-----------------+------------+--------+-------------------+

Query to find pg and g movies less then 100 mins:
SELECT * FROM movies WHERE Rating IN ('PG','G') AND Runtime < 100;


Output
+---------+---------+-----------+------------+--------+---------------+
| MovieID | Runtime | Genre     | IMDB_Score | Rating | Title         |
+---------+---------+-----------+------------+--------+---------------+
|      12 |      96 | Comedy    |        7.1 | PG     | Spaceballs    |
|      13 |      92 | Animation |        8.1 | G      | Monsters Inc. |
+---------+---------+-----------+------------+--------+---------------+

Query to average runtimes by genres scored below 7.5:
SELECT Genre,AVG(Runtime) FROM movies WHERE IMDB_Score <= 7.5 GROUP BY Genre;

Output
+-----------------+--------------+
| Genre           | AVG(Runtime) |
+-----------------+--------------+
| Action          |     156.0000 |
| Sci-Fi          |     112.5000 |
| Action&Adventur |     116.0000 |
| Comedy          |     119.0000 |
| Mystery         |     221.0000 |
| Horror          |      83.0000 |
+-----------------+--------------+

Query to update Starship Troopers:
UPDATE movies SET Rating = 'R' WHERE MovieID = 10;

Output
+---------+---------+--------+------------+--------+-------------------+
| MovieID | Runtime | Genre  | IMDB_Score | Rating | Title             |
+---------+---------+--------+------------+--------+-------------------+
|      10 |     129 | Sci-Fi |        7.2 | R      | Starship Troopers |
+---------+---------+--------+------------+--------+-------------------+

Query to get id and rating of horror and documentary
SELECT MovieID,Rating FROM movies WHERE Genre IN ('Horror', 'Documentary');

Output
+---------+--------+
| MovieID | Rating |
+---------+--------+
|       9 | TV-14  |
|      11 | R      |
+---------+--------+

Query to find average min and max for movies of each rating
SELECT Rating,AVG(IMDB_Score),MIN(IMDB_Score),MAX(IMDB_Score) FROM movies GROUP BY Rating;

Output
+--------+-----------------+-----------------+-----------------+
| Rating | AVG(IMDB_Score) | MIN(IMDB_Score) | MAX(IMDB_Score) |
+--------+-----------------+-----------------+-----------------+
| PG-13  |         5.30000 |             5.3 |             5.3 |
| R      |         6.81429 |             5.7 |             8.0 |
| TV-MA  |         7.20000 |             6.1 |             8.3 |
| PG     |         5.85000 |             4.6 |             7.1 |
| TV-14  |         4.70000 |             4.7 |             4.7 |
| G      |         8.10000 |             8.1 |             8.1 |
+--------+-----------------+-----------------+-----------------+

Query adding in a count:
SELECT Rating,AVG(IMDB_Score),MIN(IMDB_Score),MAX(IMDB_Score) FROM movies GROUP BY Rating HAVING COUNT(*) > 1;

Output
+--------+-----------------+-----------------+-----------------+
| Rating | AVG(IMDB_Score) | MIN(IMDB_Score) | MAX(IMDB_Score) |
+--------+-----------------+-----------------+-----------------+
| R      |         6.81429 |             5.7 |             8.0 |
| TV-MA  |         7.20000 |             6.1 |             8.3 |
| PG     |         5.85000 |             4.6 |             7.1 |
+--------+-----------------+-----------------+-----------------+

Query deleting R rating movies
DELETE FROM movies WHERE Rating = 'R';

Output
+---------+---------+-----------+------------+--------+-----------------------+
| MovieID | Runtime | Genre     | IMDB_Score | Rating | Title                 |
+---------+---------+-----------+------------+--------+-----------------------+
|       1 |     156 | Action    |        5.3 | PG-13  | Robin Hood            |
|       3 |      98 | Sci-Fi    |        6.1 | TV-MA  | Code 8                |
|       8 |     110 | Sci-Fi    |        4.6 | PG     | Howard the Duck       |
|       9 |      83 | Horror    |        4.7 | TV-14  | Lavalantula           |
|      12 |      96 | Comedy    |        7.1 | PG     | Spaceballs            |
|      13 |      92 | Animation |        8.1 | G      | Monsters Inc.         |
|      14 |     100 | Stand-Up  |        8.3 | TV-MA  | Bo Burnham Make Happy |
+---------+---------+-----------+------------+--------+-----------------------+



