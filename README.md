# «SQL. Часть 2» Акиньшин С.Г.

## Задание-1

### 1. `Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей и выведите в результат следующую информацию:`
```
SELECT concat(s.first_name  , ' ', s.last_name) as name , c.city,  count(c2.customer_id) as users 
FROM staff s 
JOIN address a  ON s.address_id = a.address_id 
JOIN city c  ON a.city_id = c.city_id 
JOIN store s2 ON s2.store_id = s.store_id 
JOIN customer c2 ON s2.store_id = c2.store_id 
GROUP BY s.first_name , s.last_name , c.city 
HAVING users > 300;
```


![Link](https://github.com/akinya1974/SQL-2/blob/main/JPG/Задание-1.jpg)


## Задание-2

### 2. `Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.`
```
SELECT  count(`length`) 
FROM film 
WHERE `length` > (SELECT avg(`length`)FROM film )
```


![Link](https://github.com/akinya1974/SQL-2/blob/main/JPG/Задание-2.jpg)


## Задание-3

### 3. `Получите информацию, за какой месяц была получена наибольшая сумма платежей и добавьте информацию по количеству аренд за этот месяц.`
```
SELECT DATE_FORMAT(p.payment_date, '%Y-%M') AS Дата , (sum(p.amount )) AS Сумма , count((p.rental_id )) AS Аренд
FROM payment p 
GROUP BY Дата
ORDER BY Сумма DESC
LIMIT 1;
```


![Link](https://github.com/akinya1974/SQL-2/blob/main/JPG/Задание-3.jpg)

## Задание-4

### 4. `Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».`
```
SELECT CONCAT(s.first_name, ' ', s.last_name) AS salesman,
 COUNT(p.amount) AS sales,
CASE
	WHEN COUNT(p.amount) > 8000 THEN 'YES'
	WHEN COUNT(p.amount) < 8000 THEN 'NO'
END AS bonus,
SUM(p.amount) AS revenue
FROM payment p
JOIN staff s ON s.staff_id = p.staff_id
WHERE p.amount > 0
GROUP BY salesman
ORDER BY sales DESC;
```


![Link](https://github.com/akinya1974/SQL-2/blob/main/JPG/Задание-4.jpg)

## Задание-5

### 5. `Найдите фильмы, которые ни разу не брали в аренду.`
```
SELECT f.title, f.release_year, p.amount, p.payment_date, r.return_date
FROM payment p
JOIN rental r ON r.rental_id = p.rental_id
JOIN inventory i ON i.inventory_id = r.inventory_id
JOIN film f ON f.film_id = i.film_id
WHERE p.amount = 0
ORDER BY f.title;
```


![Link](https://github.com/akinya1974/SQL-2/blob/main/JPG/Задание-5.jpg)