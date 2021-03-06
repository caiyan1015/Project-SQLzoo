# Project-SQLzoo
Solutions to SQLzoo tutorials
The Odin Project. SQLZoo tutorials’ solutions
# Select basic
1. Show population of Germany
SELECT population FROM world
   WHERE name = ‘Germany’;

2. Show name and population for Sweden, Norway and Denmark
SELECT name, population FROM world
   WHERE name IN (‘Sweden’,’Norway’,’Denmark’);

3. Show the country and the area for countries with an area between 200,000 and 250,000
SELECT name, area FROM world
   WHERE area BETWEEN 200000 AND 250000;

# Select from World 
table: world / variables: name, continent, area, population, gdp, capital
1. Show the name, continent and population of all countries
SELECT name, continent, population FROM world;

2. Show the name for the countries that have a population of at least 200000000
SELECT name FROM world
   WHERE population >= 200000000;

3. Show the name and per capita gdp for those countries with a population of at least 200 million
SELECT name, gdp/population FROM world
   WHERE population >= 200000000;

4. Show the name and population in millions for the countries of the continent 'South America'. Divide the population by 1000000 to get population in millions.
SELECT name, population/1000000 FROM world
   WHERE continent = 'South America';

5. Show the name and population for France, Germany, Italy
SELECT name, population FROM world
   WHERE name IN ('France', 'Germany', 'Italy');

6. Show the countries which have a name that includes the word 'United'
SELECT name FROM world
   WHERE name LIKE '%United%';

7. Show the countries that are big by area or big by population. Show name, population and area
SELECT name, population, area FROM world
   WHERE area > 3000000 OR population >= 250000000;

8. Show the countries that are big by area or big by population but not both. Show name, population and area
SELECT name, population, area FROM world
   WHERE area > 3000000 XOR population >= 250000000;

9. Show the name and population in millions and the GDP in billions for the countries of the continent 'South America'. Round to 2 decimal places.
SELECT name, ROUND(population/1000000, 2), ROUND(gdp/1000000000,2) FROM world
   WHERE continent = 'South America';

10. Show per-capita GDP for the trillion dollar countries to the nearest $1000.
SELECT name, ROUND(gdp/population, -3) FROM world
   WHERE gdp > 1000000000000;

11. Show the name and capital where the name and the capital have the same number of characters.
SELECT name, capital FROM world
   WHERE LENGTH(name) = LENGTH(capital);

12. Show the name and the capital where the first letters of each match. Don't include countries where the name and the capital are the same word.
SELECT name, capital FROM world
   WHERE LEFT(name,1)= LEFT(capital,1) AND name <> capital;

13. Find the country that has all the vowels and no spaces in its name.
SELECT name FROM world
  WHERE name LIKE '%a%'
  AND name LIKE '%e%'
  AND name LIKE '%i%'
  AND name LIKE '%o%'
  AND name LIKE '%u%'
  AND name NOT LIKE '% %';

# Select from Nobel
table: nobel / variables: yr, subject, winner
1. Show Nobel prizes for 1950
SELECT * FROM nobel
   WHERE yr = 1950;

2. Show who won the 1962 prize for Literature
SELECT winner FROM nobel
   WHERE yr = 1962 AND subject = 'Literature';

3. Show the year and subject that won 'Albert Einstein' his prize
SELECT yr, subject FROM nobel
   WHERE winner = 'Albert Einstein';

4. Show the name of the 'Peace' winners since the year 2000, including 2000
SELECT winner FROM nobel
   WHERE subject = 'Peace' AND yr >=2000;

5. Show all details of the Literature prize winners for 1980 to 1989 inclusive.
SELECT * FROM nobel
   WHERE subject = 'Literature' AND yr BETWEEN 1980 AND 1989;

6. Show all details of the presidential winners
SELECT * FROM nobel
   WHERE winner IN ('Theodore Roosevelt','Woodrow Wilson','Jimmy Carter','Barack Obama');

7. Show the winners with first name John
SELECT winner FROM nobel
   WHERE winner LIKE 'John%';

8. Show the year, subject, and name of Physics winners for 1980 together with the Chemistry winners for 1984
SELECT * FROM nobel
   WHERE (subject = 'Physics' AND yr = 1980)
	OR (subject ='Chemistry' AND yr = 1984);

9. Show the year, subject, and name of winners for 1980 excluding Chemistry and Medicine
SELECT * FROM nobel
   WHERE subject NOT IN ('Chemistry','Medicine')
   AND yr = 1980;

10. Show all who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004)
SELECT * FROM nobel
   WHERE (subject = 'Medicine' AND yr < 1910)
   OR (subject = 'Literature' AND yr >= 2004);

11. Find all details of the prize won by PETER GRÜNBERG
SELECT * FROM nobel
   WHERE winner = 'Peter Grünberg';

12. Find all details of the prize won by Eugene O'Neill
SELECT * FROM nobel 
   WHERE winner = "Eugene O''Neill";

13. List all details where the winner starts with Sir. Show the most recent first then by name order
SELECT winner, yr, subject FROM nobel
   WHERE winner LIKE 'Sir%' 
	ORDER BY yr DESC;

14. Show the 1984 winners and subject ordered by subject and winner name; but list Chemistry and Physics last.
SELECT winner, subject FROM nobel
   WHERE yr= 1984
	ORDER BY CASE WHEN subject IN ('Chemistry', 'Physics'), subject, winner;

# Select within Select
table: world 
1. List each country name where the population is larger than that of 'Russia'
SELECT name FROM world
   WHERE population > (SELECT population FROM world WHERE name = 'Russia');

2. Show the countries in Europe with a per capita GDP greater than 'UK'
SELECT name FROM world
   WHERE gdp/population > (SELECT gdp/population FROM world WHERE name = 'United Kingdom')
	AND continent = 'Europe';

3. List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
SELECT name, continent FROM world
  WHERE continent IN(SELECT continent FROM world WHERE name IN ('Argentina', 'Australia')
   ORDER BY name;

4. Which country has a population that is more than Canada but less than Poland? Show the name and the population.
SELECT name, population FROM world
WHERE population > (SELECT population FROM world WHERE name = 'Canada')
 AND population < (SELECT population FROM world WHERE name = 'Poland');

5. Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany
SELECT name, CONCAT(ROUND((population/(SELECT population FROM world WHERE name = 'Germany')*100),0),'%')
   FROM world 
   WHERE continent = 'Europe';

6. Show the countries that have a GDP greater than every country in Europe
SELECT name FROM world
   WHERE gdp > ALL( SELECT gdp FROM world WHERE continent = 'Europe' AND gdp >0);

7. Find the largest country (by area) in each continent, show the continent, the name and the area
SELECT continent, name, area FROM world x
   WHERE area >= ALL(SELECT area FROM world y WHERE y.continent = x.continent AND area>0);

8. List each continent and the name of the country that comes first alphabetically.
SELECT continent, name FROM world x
   WHERE name = ( SELECT name FROM world y WHERE y.continent=x.continent ORDER BY name LIMIT 1)

9. Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population
SELECT name, continent, population FROM world
 WHERE continent NOT IN (SELECT continent FROM world WHERE population > 25000000);

10. Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents
SELECT name, continent FROM world x
WHERE population > ALL (SELECT population*3 FROM world y WHERE x.continent = y.continent and x.name <> y.name);

# Sum and Count
table: world
1. Show the total population of the world
SELECT SUM(population) FROM world;

2. List all the continents- just once each
SELECT DISTINCT continent FROM world;

3. Give the total GDP of Africa
SELECT SUM(gdp) FROM world
   WHERE continent ='Africa';

4. How many countries have an area of at least 1000000
SELECT count(name) FROM world
   WHERE area>= 1000000;

5. What is the total population of ('Estonia', 'Latvia', 'Lithuania')
SELECT SUM(population) FROM world
   WHERE name IN ('Estonia', 'Latvia', 'Lithuania');

# Using GROUP BY and HAVING
6. For each continent show the continent and number of countries
SELECT continent, COUNT(name) FROM world
   GROUP BY continent;

7. For each continent show the continent and number of countries with populations of at least 10 million
SELECT continent, count(name) FROM world
   WHERE population >= 10000000
   GROUP BY continent;

8. List the continents that have a total population of at least 100 million
SELECT continent FROM world
   GROUP BY continent
   HAVING SUM(population) >= 100000000;

# JOIN
table: game / variables: id, mdate, stadium, team1, team2
table: goal / variables: matchid, teamid, player, gtime
table: eteam / variables: id, teamname, coach

1. Show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'
SELECT matchid, player FROM goal
WHERE teamid='ger';

2. Show id, stadium, team1, team2, for just game 1012
SELECT id,stadium,team1,team2 FROM game
   WHERE id=1012;

3. Show the player, teamid, stadium and mdate for every German goal
SELECT player,teamid, stadium, mdate FROM game 
   JOIN goal ON (id=matchid)
   WHERE teamid='ger' AND teamid=(team1 or team2);

4. Show the team1, team2 and player for every goal scored by a player called Mario
SELECT team1, team2, player FROM game
   JOIN goal on id=matchid
   WHERE player LIKE 'Mario%';

5. Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10
SELECT player, teamid, coach, gtime FROM goal join eteam on teamid=id
   WHERE gtime<=10;

6. List the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach
SELECT mdate, teamname FROM game 
   JOIN eteam ON (team1=eteam.id)
WHERE team1='Are';

7. List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'
SELECT player FROM goal 
   JOIN game ON id=matchid
   WHERE stadium = 'National Stadium, Warsaw';

8. Show the name of all players who scored a goal against Germany
SELECT distinct player FROM game 
   JOIN goal ON matchid = id 
   WHERE teamid!='ger' and (team1='ger' or team2='ger');

9. Show teamname and the total number of goals scored
SELECT teamname, count(player) FROM eteam 
   JOIN goal ON id=teamid
  GROUP BY teamname
 ORDER BY teamname;

10. Show the stadium and the number of goals scored in each stadium
SELECT stadium, count(player) FROM game
   JOIN goal ON id=matchid
   GROUP BY stadium;

11. For every match involving 'POL', show the matchid, date and the number of goals scored
SELECT matchid, mdate, count(player) FROM game 
  JOIN goal ON matchid = id 
  WHERE (team1 = 'POL' OR team2 = 'POL')
  GROUP BY matchid, mdate;

12. For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'
SELECT matchid, mdate, count(player) FROM game 
   JOIN goal ON id=matchid
   WHERE teamid='ger'
   GROUP BY matchid, mdate;

13. List every match with the goals scored by each team as shown. This will use "CASE WHEN" which has not been explained in any previous exercises.Sort your result by mdate, matchid, team1 and team2.
SELECT mdate, team1, SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END)score1, team2, SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END)score2
  FROM game JOIN goal ON matchid = id
   GROUP BY mdate, matchid, team1, team2
   ORDER BY mdate, matchid, team1, team2;

# More JOIN
table: movie / var: id, title, yr, director, budget, gross
table: actor / var: id, name
table: casting / var: movieid, actorid, ord

1. List the films where the yr is 1962 [Show id, title]
SELECT id, title FROM movie
 WHERE yr=1962;

2. Give year of 'Citizen Kane'.
SELECT yr FROM movie
   WHERE title='citizen kane';

3. List all of the Star Trek movies, include the id, title and yr (all of these movies include the words Star Trek in the title). Order results by year.
SELECT id, title, yr FROM movie
   WHERE title like'star trek%'
   ORDER BY yr;

4. What id number does the actor 'Glenn Close' have?
SELECT id FROM actor
 WHERE name='glenn close';

5. What is the id of the film 'Casablanca'
SELECT id FROM movie
   WHERE title='casablanca'

6. Obtain the cast list for 'Casablanca'
SELECT name FROM movie 
   JOIN casting ON movie.id=movieid
   JOIN actor ON actorid=actor.id
   WHERE title='casablanca';

7. Obtain the cast list for the film 'Alien'
SELECT name FROM movie
   JOIN casting ON movie.id=movieid
   JOIN actor ON actorid=actor.id
   WHERE title='alien';

8. List the films in which 'Harrison Ford' has appeared
SELECT title FROM movie 
   JOIN casting ON movie.id=movieid
   JOIN actor ON actorid=actor.id
   WHERE name='harrison ford';

9. List the films where 'Harrison Ford' has appeared - but not in the starring role
SELECT title FROM movie 
   JOIN casting ON movie.id=movieid
   JOIN actor ON actorid=actor.id
   WHERE name='harrison ford' AND ord!=1;

10. List the films together with the leading star for all 1962 films.
SELECT title, name FROM movie
   JOIN casting ON movie.id=movieid
   JOIN actor ON actorid=actor.id
   WHERE yr='1962' AND ord=1

11. Which were the busiest years for 'John Travolta', show the year and the number of movies he made each year for any year in which he made more than 2 movies.
SELECT yr, COUNT(title) FROM casting
   JOIN movie ON movieid = movie.id
   JOIN actor ON actorid = actor.id
   WHERE actor.name = 'John Travolta'
   GROUP BY yr 
   HAVING COUNT(title) > 2;

12. List the film title and the leading actor for all of the films 'Julie Andrews' played in.
SELECT title, name FROM casting
   JOIN movie ON movieid = movie.id
   JOIN actor ON actorid = actor.id
   WHERE movie.id IN (SELECT movieid FROM casting
    JOIN movie ON movieid = movie.id
    JOIN actor ON actorid = actor.id
    WHERE actor.name = 'Julie Andrews')
   AND ord = 1;

13. Obtain a list, in alphabetical order, of actors who've had at least 30 starring roles.
SELECT name FROM casting
   JOIN movie ON movieid = movie.id
   JOIN actor ON actorid = actor.id
   WHERE ord=1
   GROUP BY name
   HAVING COUNT(moved)>= 30
   ORDER BY name;

14. List the films released in the year 1978 ordered by the number of actors in the cast, then by title.
SELECT title, COUNT(actorid) FROM casting
   JOIN movie ON movieid = movie.id
   WHERE yr = 1978
   GROUP BY title
   ORDER BY COUNT(actorid) DESC;

15. List all the people who have worked with 'Art Garfunkel'.
SELECT name FROM casting
   JOIN actor on actorid = actor.id
   WHERE movieid IN (SELECT movieid FROM casting
     JOIN actor ON actorid = actor.id
    WHERE name = 'Art Garfunkel')
   AND name != 'Art Garfunkel';

# Using NULL
table: teacher / var:id, dept, name, phone, mobile
table: dept / var: id, name

1. List the teachers who have NULL for their department.
SELECT name FROM teacher
   WHERE dept IS NULL;

2. Note the INNER JOIN misses the teachers with no department and the departments with no teacher.
SELECT teacher.name, dept.name FROM teacher 
   INNER JOIN dept ON (teacher.dept=dept.id);

3. Use a different JOIN so that all teachers are listed.
SELECT teacher.name, dept.name FROM teacher 
   LEFT JOIN dept ON (teacher.dept=dept.id);

4. Use a different JOIN so that all departments are listed.
SELECT teacher.name, dept.name FROM teacher
   RIGHT JOIN dept ON (teacher.dept=dept.id);

5. Use COALESCE to print the mobile number. Use the number '07986 444 2266' if there is no number given. Show teacher name and mobile number or '07986 444 2266'
SELECT teacher.name, COALESCE(mobile, '07986 444 2266') FROM teacher; 

6. Use the COALESCE function and a LEFT JOIN to print the teacher name and department name. Use the string 'None' where there is no department.
SELECT teacher.name, COALESCE(dept.name, 'None') FROM teacher 
   LEFT JOIN dept ON (teacher.dept=dept.id);

7. Use COUNT to show the number of teachers and the number of mobile phones.
SELECT COUNT(id), COUNT(mobile) FROM teacher;

8. Use COUNT and GROUP BY dept.name to show each department and the number of staff. Use a RIGHT JOIN to ensure that the Engineering department is listed.
SELECT dept.name, COUNT(teacher.name) FROM teacher
   RIGHT JOIN dept ON (teacher.dept=dept.id)
   GROUP BY dept.name;

9. Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2 and 'Art' otherwise.
SELECT name, CASE WHEN dept IN (1, 2) THEN 'Sci'ELSE 'Art' END FROM teacher;

10. Use CASE to show the name of each teacher followed by 'Sci' if the teacher is in dept 1 or 2, show 'Art' if the teacher's dept is 3 and 'None' otherwise
SELECT name, 
CASE WHEN dept IN (1, 2) THEN 'Sci'
WHEN dept = 3 THEN 'Art'
ELSE 'None' END FROM teacher;

# Self Join
table: stops / var: id, name
table: route / var: num, company, pos, stop)

1. How many stops are in the database.
SELECT COUNT(id) FROM stops;

2. Find the id value for the stop 'Craiglockhart'
SELECT id FROM stops
   WHERE name = 'Craiglockhart';

3. Give the id and the name for the stops on the '4' 'LRT' service.
SELECT id, name FROM stops
   JOIN route ON id=stop
   WHERE company = 'lrt' AND num = 4;

4. The query shown gives the number of routes that visit either London Road (149) or Craiglockhart (53). Run the query and notice the two services that link these stops have a count of 2. Add a HAVING clause to restrict the output to these two routes.
SELECT company, num, COUNT(*) FROM route
   WHERE stop=149 OR stop=53
   GROUP BY company, num
   HAVING count(*)=2;

5. Execute the self join shown and observe that b.stop gives all the places you can get to from Craiglockhart, without changing routes. Change the query so that it shows the services from Craiglockhart to London Road.
SELECT a.company, a.num, astop.id , bstop.id 
  FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
  JOIN stops astop ON (a.stop = astop.id)
  JOIN stops bstop ON (b.stop = bstop.id)
  WHERE astop.name='Craiglockhart'
  AND bstop.name='London Road';

6. The query shown is similar to the previous one, however by joining two copies of the stops table we can refer to stops by name rather than by number. Change the query so that the services between 'Craiglockhart' and 'London Road' are shown. If you are tired of these places try 'Fairmilehead' against 'Tollcross
SELECT a.company, a.num, stopa.name, stopb.name
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
  JOIN stops stopa ON (a.stop=stopa.id)
  JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name='Craiglockhart' AND stopb.name='London Road';

7. Give a list of all the services which connect stops 115 and 137 ('Haymarket' and 'Leith')
SELECT DISTINCT a.company, a.num
FROM route a JOIN route b ON
  a.company=b.company AND a.num=b.num
WHERE a.stop=115 AND b.stop=137;

8. Give a list of the services which connect the stops 'Craiglockhart' and 'Tollcross'
SELECT DISTINCT a.company, a.num
FROM route a JOIN route b ON
    a.company=b.company AND a.num=b.num
  JOIN stops stopa ON stopa.id=a.stop
  JOIN stops stopb ON stopb.id=b.stop
WHERE stopa.name='Craiglockhart'
  AND stopb.name='Tollcross';

9. Give a distinct list of the stops which may be reached from 'Craiglockhart' by taking one bus, including 'Craiglockhart' itself, offered by the LRT company. Include the company and bus no. of the relevant services.
SELECT DISTINCT stopb.name, a.company, a.num
FROM route a JOIN route b ON
    a.company=b.company AND a.num=b.num
  JOIN stops stopa ON stopa.id=a.stop
  JOIN stops stopb ON stopb.id=b.stop
WHERE stopa.name='Craiglockhart';





