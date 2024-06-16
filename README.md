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

![Решение 1](https://github.com/DenioSa/SQL-1/blob/9177d277bc0c450eec274b9fc3e9af13ecb02aab/img/1.bmp)`

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

### Решение 2

```
SELECT *
FROM payment
WHERE payment_date  BETWEEN '2005-06-15' and '2005-06-19'
AND amount > 10;
```
![Решение 2](https://github.com/DenioSa/SQL-1/blob/9177d277bc0c450eec274b9fc3e9af13ecb02aab/img/2.bmp)`

### Задание 3
Получите информацию о том, за какой месяц была получена самая большая сумма, и укажите информацию о размере арендной платы за этот месяц.

### Решение 3

```
SELECT *  
FROM rental   
ORDER by rental_date DESC 
LIMIT 5;
```
![Решение 3](https://github.com/DenioSa/SQL-1/blob/9177d277bc0c450eec274b9fc3e9af13ecb02aab/img/3.bmp)`

