1a)
SELECT first_name, last_name
FROM actor

1b)
SELECT CONCAT(first_name, ' ', last_name) AS 'Actor Name'
FROM actor

2a)
SELECT first_name, last_name, actor_ID
FROM actor
WHERE first_name = 'Joe'

2b)
SELECT first_name, last_name, actor_ID
FROM actor
WHERE last_name LIKE '%GEN%'

2c)
SELECT first_name, last_name, actor_ID
FROM actor
WHERE last_name LIKE '%LI%'
ORDER BY TRIM(last_name) ASC
       , TRIM(first_name) ASC;

2d)
SELECT country_id, country
FROM country
WHERE country IN ('Afghanistan', 'Bangladesh', 'China')

3a)
ALTER TABLE actor
ADD middle_name VARCHAR(50);

SELECT first_name, middle_name, last_name
FROM actor

3b)
ALTER TABLE actor
MODIFY COLUMN middle_name blob;

3c)
ALTER TABLE actor
DROP COLUMN middle_name;

4a)
SELECT Count(last_name), last_name
FROM actor
GROUP BY last_name

4b)
SELECT Count(last_name), last_name
FROM actor
GROUP BY last_name
Having COUNT(last_name) > 1

4c)
UPDATE actor
SET first_name = 'HARPO'
WHERE actor_ID = 172

4d) 
UPDATE actor
SET first_name = 'GROUCHO'
WHERE actor_ID = 172;

UPDATE actor
SET first_name = 'MUCHO GROUCHO'
WHERE actor_ID = 78;

UPDATE actor
SET first_name = 'MUCHO GROUCHO'
WHERE actor_ID = 106

5a)
SHOW CREATE TABLE address

6a) 
SELECT staff.first_name, staff.last_name, address.address
FROM staff
INNER JOIN address ON staff.address_id=address.address_id

6b)
SELECT sum(payment.amount), staff.first_name, staff.last_name
FROM payment
INNER JOIN staff ON payment.staff_id=staff.staff_id
WHERE payment.payment_date like '2005-08-%'
GROUP BY payment.staff_id

6c)
SELECT film.title, count(film_actor.actor_id)
FROM film_actor
INNER JOIN film ON film_actor.film_id=film.film_id
GROUP BY film_actor.film_id

6d)
SELECT title, film_id
FROM film
Having title = 'Hunchback Impossible'

SELECT inventory_id, count(film_id)
FROM inventory
WHERE film_id = 439
GROUP BY inventory_id

6e)
SELECT customer.customer_id, customer.first_name, customer.last_name, sum(payment.amount)
FROM payment
INNER JOIN customer ON payment.customer_id=customer.customer_id
GROUP BY customer.customer_id
ORDER BY TRIM(last_name) ASC
       , TRIM(first_name) ASC;

7a)
SELECT title
  FROM film
  WHERE language_id IN
  (
      SELECT language_id
      FROM language
      WHERE name = 'English'
     )
Having title LIKE 'K%' OR title LIKE 'Q%'    

7b)
SELECT first_name, last_name
FROM actor
WHERE actor_id IN
(
SELECT actor_id
FROM film_actor
WHERE film_id IN
(
SELECT film_id
FROM film
WHERE title = 'Alone Trip'
))

7c) 
select first_name, last_name, email
 from customer
  join address
 using (address_id)
  join city
 using (city_id)
  join country 
 using (country_id)
 WHERE country = 'Canada'

7d)
SELECT title
FROM film
WHERE film_id IN
(
SELECT film_id
FROM film_category
WHERE category_id IN
( 
SELECT category_id
FROM category
WHERE name = 'family'
))

7e)
 select film.title,  count(rental.rental_id)
  from film
  join inventory
 using (film_id)
  join rental
    on (inventory.inventory_id = rental.inventory_id)
group by (film.title)
ORDER BY (count(rental.rental_id)) DESC

7f)
select store_id, concat('$',format(sum(amount),2)) as business_renenue
  from store
  join customer
 using (store_id)
  join payment 
    on (customer.customer_id = payment.customer_id)
group by (store_id);

7g) 
select store_id, city, country 
 from store
  join address
 using (address_id)
  join city
 using (city_id)
  join country 
 using (country_id)

7h)
select category.name, sum(amount) as gross_revenue
 from category 
  join film_category
 using (category_id)
  join inventory
 using (film_id)
  join rental 
 using (inventory_id)
  join payment
 using (customer_id)
group by (category.name)
ORDER BY (gross_revenue) DESC
limit 5

8a) 
CREATE VIEW top_five_genres
AS select category.name, sum(amount) as gross_revenue
 from category 
  join film_category
 using (category_id)
  join inventory
 using (film_id)
  join rental 
 using (inventory_id)
  join payment
 using (customer_id)
group by (category.name)
ORDER BY (gross_revenue) DESC
limit 5

8b)
SELECT * FROM top_five_genres

8c)
DROP VIEW top_five_genres
