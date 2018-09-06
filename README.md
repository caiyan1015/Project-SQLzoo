# Project-SQLZOO tutorials' solutions

# SELECT basics
# Introducing the "world" table of countries
1. Show population of Germany
SELECT population FROM world WHERE name='Germany';

2. Show name and population for Sweden, Norway and Denmark
SELECT name, population FROM world WHERE name IN ('Sweden','Norway','Denmark');

3. Show the country and the area for countries with an area between 200,000 and 250,000
SELECT name, area FROM world WHERE area BETWEEN 200000 AND 250000;
