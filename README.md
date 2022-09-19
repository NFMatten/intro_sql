# SQL In Action 

Project problems and setup directions for the Week 6 Introduction to SQL project.

Make sure you have covered enough content to meet the prerequisites before starting on this project!


## PREREQUISITES

1. MySQL Server and MySQL Workbench, installed on your computer
2. Basic understanding of SQL queries
   1. SELECT, SELECT DISTINCT
   2. WHERE, operators with WHERE, complex expressions using AND, OR, NOT, and NULL
   3. Filtering and sorting (LIMIT, ORDER BY)
   4. Aggregate functions: COUNT, MIN, MAX, AVG, SUM, GROUP BY
   5. Aliasing (creating a new temporary column with the AS clause)
   6. SQL comments


## SETUP

1. Download the `.zip` file for this GitHub repository
2. Unzip the file on your computer
3. Open MySQL Workbench and choose your local connection
4. Run the `CREATE DATABASE intro_sql` query to create the database
5. Refresh your Schemas tab and right click on `intro_sql`
6. Select **Table Data Import Wizard**
7. When prompted to choose the file to import, click the **Browse** button and select `final_airbnb.csv` from the folder you unzipped out of our starter repository
8. Click Next five times - you will not need to change any settings when setting this project up for the first time.
9. You should see an **Import Results** screen that shows that the table was created with 146 records.
10. Now, go to **File > Open SQL Script** in the top menu of MySQL Workbench.
11. In the window that pops up, choose the `sql_in_action-starter.sql` file from the starter files, then click **Open**
12. You should see the script open in MySQL Workbench with all of the problems written out in SQL comments.
13. Write your queries underneath the problem they relate to, and execute those queries to check that they give you the expected output!


> ‼️ Make sure to check out the walk-through video on your course portal to see this setup process happening live!


-- <<<<<<<<<<<<<<<<<<<<<< PROBLEM 1 >>>>>>>>>>>>>>>>>>>>>>>
-- Find out how many rows are in the table "final_airbnb"
-- EXPECTED OUTPUT: 146

-- Solution:
 SELECT COUNT(*) AS Rows_count FROM final_airbnb;
 
 
 -- <<<<<<<<<<<<<<<<<<<<<< PROBLEM 2 >>>>>>>>>>>>>>>>>>>>>>>
-- Find out the name of the host for "host_id" 63613
-- HINT: "Where" could it be?

-- EXPECTED OUTPUT: Patricia

-- Solution:
  SELECT host_name FROM final_airbnb
  WHERE host_id = "63613";
  
  
  -- <<<<<<<<<<<<<<<<<<<<<< PROBLEM 3 >>>>>>>>>>>>>>>>>>>>>>>
-- Query the data to just show the unique neighbourhoods listed
-- HINT: This is a "distinct" operation...

-- EXPECTED OUTPUT: 40 neighbourhoods listed

-- Solution:
  SELECT DISTINCT neighbourhood FROM final_airbnb;
  
  
  -- <<<<<<<<<<<<<<<<<<<<<< PROBLEM 4 >>>>>>>>>>>>>>>>>>>>>>>

-- Find both the highest price listing and the lowest price listing, displaying the entire row for each
-- HINT: This can be two different queries.

-- FOOD FOR THOUGHT: Think about the results. Are the high and low prices outliers in this data set?

-- EXPECTED OUTPUT: Highest = 785, Lowest = 55

-- Solution:
 SELECT MAX(price) as Highest_price, MIN(price) as Lowest_price  FROM final_airbnb
 
 
 -- <<<<<<<<<<<<<<<<<<<<<< PROBLEM 5 >>>>>>>>>>>>>>>>>>>>>>>
-- Find the average availability for all listings in the data set (using the availability_365 column)
-- HINT: Aggregates are more than just big rocks...

-- EXPECTED OUTPUT: 165.3904

-- Solution:
 SELECT AVG(availability_365) AS Average_availability FROM final_airbnb


-- <<<<<<<<<<<<<<<<<<<<<< PROBLEM 6 >>>>>>>>>>>>>>>>>>>>>>>
-- Find all listings that do NOT have a review
-- HINT: There are a few ways to go about this. Remember that an empty cell is "no value", but not necessarily NULL

-- EXPECTED OUTPUT: 6 rows

-- Solution:
SELECT * FROM final_airbnb
WHERE number_of_reviews = "0";


-- <<<<<<<<<<<<<<<<<<<<<< PROBLEM 7 >>>>>>>>>>>>>>>>>>>>>>>
-- Find the id of the listing with a room_type of "Private room" that has the most reviews 
-- HINT: Sorting is your friend!

-- EXPECTED OUTPUT: 58059

-- Solution:
SELECT id from final_airbnb
WHERE room_type = "Private room"
ORDER BY number_of_reviews desc
LIMIT 1;


-- <<<<<<<<<<<<<<<<<<<<<< PROBLEM 8 >>>>>>>>>>>>>>>>>>>>>>>
-- Find the most popular neighbourhood for listings 
-- HINT: Look for which neighbourhood appears most frequently in the neighbourhood column
-- HINT: You are creating "summary rows" for each neighbourhood, so you will just see one entry for each neighbourhood

-- EXPECTED OUTPUT: Williamsburg
-- INVESTIGATE: Should Williamsburg be crowned the most popular neighbourhood?

-- Solution: (most popular shared at count of 16; LIMIT 3 to show top 3 most popular)
SELECT DISTINCT neighbourhood, COUNT(neighbourhood) AS Neighbourhood_count from final_airbnb
GROUP BY neighbourhood
ORDER BY neighbourhood_count desc
LIMIT 3;


-- <<<<<<<<<<<<<<<<<<<<<< PROBLEM 9 >>>>>>>>>>>>>>>>>>>>>>>
-- Query the data to discover which listing is the most popular using the reviews_per_month for all listings with a minimum_nights value of less than 7
-- HINT: Sorting is still your friend! So are constraints.

-- EXPECTED OUTPUT: 58059

-- Solution:
SELECT id from final_airbnb
WHERE minimum_nights < 7
ORDER BY reviews_per_month desc
LIMIT 1;


-- <<<<<<<<<<<<<<<<<<<<<< PROBLEM 10 >>>>>>>>>>>>>>>>>>>>>>>
-- Find out which host has the most listings. 
-- Create a NEW column that will show a calculation for how many listings the host for each listing has in the table
-- Display the column using aliasing.
-- HINT: Work this one step at a time. See if you can find a way to just display the count of listings per host first.

-- EXPECTED OUTPUT: The Box House Hotel with 6 listings

-- Solution:
SELECT DISTINCT host_name, COUNT(host_name) AS Total_listings FROM final_airbnb
GROUP BY host_name
ORDER BY Total_listings desc
LIMIT 1;


-- <<<<<<<<<<<<<<<<<<<<<< PROBLEM 11 >>>>>>>>>>>>>>>>>>>>>>>
-- <<<<<<<<<<<<<<<<<<<<<<< WRAP UP >>>>>>>>>>>>>>>>>>>>>>>>>
-- What do you think makes a successful AirBnB rental in this market? What factors seem to be at play the most?
-- Write a few sentences and include them with your project submission in the README file 

SELECT DISTINCT neighbourhood_group, COUNT(neighbourhood_group), SUM(number_of_reviews) FROM final_airbnb
GROUP BY neighbourhood_group;

-- Based off of this data output, location of the AirBnB determines the success. However, good reviews vs bad reviews are not determined; only total number of reviews. That being said, the individual AirBnB still is making profit based off of number of reviews, regardless of the review status. Furthermore, total nights stayed per individual host per year is not included, thus the conclusion based off of total reviews.
