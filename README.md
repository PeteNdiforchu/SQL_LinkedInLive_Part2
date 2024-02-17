# SQL_LinkedInLive_Part2
`sql`
-- 1. Check for missing values in a specific column
WITH missing_emails AS (
    SELECT *
    FROM customer
    WHERE email IS NULL
)
SELECT *
FROM missing_emails;

-- 2. Validate numeric ranges for certain attributes
WITH invalid_lengths AS (
    SELECT *
    FROM film
    WHERE length < 0 OR length > 300 -- Assuming length should be within a reasonable range
)
SELECT *
FROM invalid_lengths;

-- 3. Ensure referential integrity between related tables
WITH missing_inventory AS (
    SELECT *
    FROM film
    WHERE film_id NOT IN (SELECT film_id FROM inventory)
)
SELECT *
FROM missing_inventory;

-- 4. Detect duplicate records within a table
WITH duplicate_rentals AS (
    SELECT rental_id, COUNT(*) AS num_rentals
    FROM rental
    GROUP BY rental_id
    HAVING COUNT(*) > 1
)
SELECT *
FROM duplicate_rentals;

-- 5. Validate format consistency for date fields
WITH invalid_dates AS (
    SELECT *
    FROM rental
    WHERE NOT (rental_date SIMILAR TO '####-##-## ##:##:##')
)
SELECT *
FROM invalid_dates;

-- 6. Check for outliers in numerical data
WITH outlier_payments AS (
    SELECT *
    FROM payment
    WHERE amount < 0 OR amount > 1000 -- Assuming payment amount should be within a reasonable range
)
SELECT *
FROM outlier_payments;

-- 7. Validate constraints on categorical variables
WITH invalid_ratings AS (
    SELECT *
    FROM film
    WHERE rating NOT IN ('G', 'PG', 'PG-13', 'R', 'NC-17')
)
SELECT *
FROM invalid_ratings;

-- 8. Identify inconsistencies in naming conventions
WITH invalid_names AS (
    SELECT *
    FROM actor
    WHERE first_name NOT SIMILAR TO '%[A-Z][a-z]%'
)
SELECT *
FROM invalid_names;

-- 9. Check for data completeness across multiple tables
WITH incomplete_films AS (
    SELECT *
    FROM film
    WHERE title IS NULL OR length IS NULL
)
SELECT *
FROM incomplete_films;

-- 10. Perform cross-tabulation checks to identify data discrepancies
WITH film_category_counts AS (
    SELECT c.name AS category, COUNT(fc.film_id) AS num_films
    FROM category c
    LEFT JOIN film_category fc ON c.category_id = fc.category_id
    GROUP BY c.name
)
SELECT *
FROM film_category_counts;
