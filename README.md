# Домашнее задание к занятию "`Индексы`" - `Сайфиев Денис`
---

### Задание 1

Напишите запрос к учебной базе данных, которая вернёт процентное соотношение общего размера всех индексов к общему размеру всех таблиц.


### Решение 1
```
SELECT  
     TABLE_SCHEMA AS 'База данных',
     ROUND((SUM(index_length)/(SUM(data_length)+SUM(index_length))*100), 1) AS 'Процентное отношение'
FROM information_schema.TABLES
WHERE TABLE_SCHEMA = 'sakila';
```

### Задание 2

Выполните объяснение и анализ следующего запроса:

select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
перечислите узкие места;
Оптимизируйте запрос: внесите корректировки по использованию операторов при необходимости добавления индексов.

### Решение 2



