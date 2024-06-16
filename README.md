# Домашнее задание к занятию "`Индексы`" - `Сайфиев Денис`
---

### Задание 1

Напишите запрос к учебной базе данных, которая вернёт процентное соотношение общего размера всех индексов к общему размеру всех таблиц.


### Решение 1



### Задание 2

Выполните объяснение и анализ следующего запроса:

select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
перечислите узкие места;
Оптимизируйте запрос: внесите корректировки по использованию операторов при необходимости добавления индексов.

### Решение 2



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

