# Make sure you download the starter code and run the following

```sh
  psql < movies.sql
  psql movies_db
```

In markdown, you can place a code block inside of three backticks (```) followed by the syntax highlighting you want, for example

\```sql

SELECT \* FROM users;

\```

Using the `movies_db` database, write the correct SQL queries for each of these tasks:

1. The title of every movie.
   \```sql
   SELECT title FROM movies;
   \```
2. All information on the G-rated movies.
   \```sql
   SELECT \* FROM movies WHERE rating LIKE 'G%';
   \```
3. The title and release year of every movie, ordered with the
   oldest movie first.
   \```sql
   SELECT title, release_year FROM movies ORDER BY release_year ASC;
   \```
4. All information on the 5 longest movies.
   \```sql
   SELECT \* FROM movies ORDER BY runtime DESC LIMIT 5;
   \```
5. A query that returns the columns of`rating`and`total`, tabulating the
   total number of G, PG, PG-13, and R-rated movies.
   \```sql
   SELECT rating, COUNT(\*) AS total FROM movies WHERE rating IN ('G', 'PG', 'PG-13', 'R') GROUP BY rating;
   \```
6. A table with columns of`release_year`and`average_runtime`,
   tabulating the average runtime by year for every movie in the database. The data should be in reverse chronological order (i.e. the most recent year should be first).
   \```sql
   SELECT release_year, AVG(runtime) AS average_runtime FROM movies GROUP BY release_year ORDER BY release_year DESC;
   \```
7. The movie title and studio name for every movie in the
   database.
   \```sql
   SELECT m.title, s.name
   FROM movies m
   JOIN studios s ON m.studio_id = s.id;
   \```
8. The star first name, star last name, and movie title for every
   matching movie and star pair in the database.
   \```sql
   SELECT s.first_name, s.last_name, m.title
   FROM movies m
   JOIN stars s ON m.id = s.id;
   \```
9. The first and last names of every star who has been in a G-rated movie. The first and last name should appear only once for each star, even if they are in several G-rated movies. _IMPORTANT NOTE_: it's possible that there can be two _different_ actors with the same name, so make sure your solution accounts for that.
   \```sql
   SELECT s.first_name, s.last_name
   FROM stars s
   JOIN roles r ON s.id = r.star_id
   JOIN movies m ON r.movie_id = m.id
   WHERE m.rating = 'G';
   \```
10. The first and last names of every star along with the number
    of movies they have been in, in descending order by the number of movies. (Similar to #9, make sure
    that two different actors with the same name are considered separately).
    \```sql
    SELECT s.first_name, s.last_name, COUNT(\*) AS total
    FROM stars s
    JOIN roles r ON s.id = r.star_id
    JOIN movies m ON r.movie_id = m.id
    GROUP BY s.id, s.first_name, s.last_name
    ORDER BY total DESC;
    \```

## The rest of these are bonuses

11.The title of every movie along with the number of stars in
    that movie, in descending order by the number of stars.
    \```sql
    SELECT m.title, COUNT(\*) AS stars
    FROM stars s
    JOIN roles r ON s.id = r.star_id
    JOIN movies m ON r.movie_id = m.id
    GROUP BY m.title
    ORDER BY stars DESC;
    \```
12. The first name, last name, and average runtime of the five
    stars whose movies have the longest average.
    \```sql
    SELECT s.first_name, s.last_name, AVG(m.runtime) AS avg_runtime
    FROM stars s
    JOIN roles r ON s.id = r.star_id
    JOIN movies m ON r.movie_id = m.id
    GROUP BY s.id
    ORDER BY avg_runtime DESC
    LIMIT 5;
    \```
13. The first name, last name, and average runtime of the five
    stars whose movies have the longest average, among stars who have more than one movie in the database.
    \```sql
    SELECT s.first_name, s.last_name, AVG(m.runtime) AS avg_runtime
    FROM stars s
    JOIN roles r ON s.id = r.star_id
    JOIN movies m ON r.movie_id = m.id
    GROUP BY s.id
    HAVING COUNT(DISTINCT m.id) > 1
    ORDER BY avg_runtime DESC
    LIMIT 5;
    \```
14. The titles of all movies that don't feature any stars in our
    database.
    \```sql
    SELECT title
    FROM movies
    WHERE id NOT IN (
    SELECT movie_id
    FROM roles
    );
    \```
15. The first and last names of all stars that don't appear in any movies in our database.
    \```sql
    SELECT first_name, last_name
    FROM stars
    WHERE id NOT IN (
    SELECT star_id
    FROM roles
    );
    \```
16. The first names, last names, and titles corresponding to every
    role in the database, along with every movie title that doesn't have a star, and the first and last names of every star not in a movie.
    \```sql
    SELECT
    s.first_name,
    s.last_name,
    m.title AS movie_title
    FROM stars s
    FULL JOIN movies m
    ON s.id = m.id;
    \```
