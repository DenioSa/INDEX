# Домашнее задание к занятию "`SQL. Часть 2`" - `Сайфиев Денис`
---
Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1
Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и в результате вы получите следующую информацию:

* фамилия и имя сотрудника этого магазина;
* город нахождения магазина;
* количество пользователей, закрепленных в этом магазине.

### Решение 1

```
SELECT CONCAT(s1.first_name, ' ', s1.last_name) AS Name , c.city AS City, COUNT(c2.store_id)
FROM staff s1
JOIN address a ON a.address_id = s1.address_id 
JOIN city c ON c.city_id = a.city_id 
JOIN store s ON s.store_id = s1.store_id 
JOIN customer c2 ON c2.store_id = s1.store_id 
GROUP BY s1.first_name , s1.last_name , c.city 
HAVING COUNT(c2.store_id) > 300;
```

![Решение 1](https://github.com/DenioSa/SQL-2/blob/4e9d7edd198aba221ad3c0c354ec4298c7c39e29/img/1.bmp)`

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

### Решение 2

```
SELECT count(`length`) 
FROM film 
WHERE `length` > (SELECT AVG(`length`) FROM film );
```
![Решение 2](https://github.com/DenioSa/SQL-2/blob/4e9d7edd198aba221ad3c0c354ec4298c7c39e29/img/2.bmp)`

### Задание 3
Получите информацию о том, за какой месяц была получена самая большая сумма, и укажите информацию о размере арендной платы за этот месяц.

### Решение 3

```
SELECT DATE_FORMAT(p.payment_date, '%Y-%M') AS data, (sum(p.amount )) AS sum , count((p.rental_id )) AS rent
FROM payment p 
GROUP BY data 
ORDER BY sum DESC
LIMIT 1;
```
![Решение 3](https://github.com/DenioSa/SQL-2/blob/4e9d7edd198aba221ad3c0c354ec4298c7c39e29/img/3.bmp)`

