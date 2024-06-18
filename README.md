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
![Решение 1](https://github.com/DenioSa/INDEX/blob/82263a18a84bb3aec789ad950fff6800339198cc/img/1.bmp)`



### Задание 2

Выполните объяснение и анализ следующего запроса:

select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
* перечислите узкие места;
* Оптимизируйте запрос: внесите корректировки по использованию операторов при необходимости добавления индексов.

### Решение 2

Необходимые данные для вывода располагаются только в таблицах - payment p, customer c. Остальные же (rental r,  inventory i, film f) являются для данного запроса излишней нагрузкой, в том числе и неиспользуемыми данными в конечном выводе. 

```
SELECT
	distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id) 
FROM payment p, customer c 
WHERE DATE(p.payment_date) = '2005-07-30' and p.customer_id = c.customer_id;
```

![Решение 2](https://github.com/DenioSa/INDEX/blob/82263a18a84bb3aec789ad950fff6800339198cc/img/2.bmp)`



### ДОРАБОТКИ №2 СОГЛАСНО ЗАМЕЧАНИЯМ ПРЕПОДАВАТЕЛЯ:

##### Замечание:
Спасибо за выполненную работу.
Анализ запроса произведен верно, вы правильно определили, что таблица film присоединена некорректно без условия и убрали ее, по поводу таблиц inventory и rental - да, данные из них в финальном select не используются, но они задают определенные продажи, поэтому лучше их оставить. Также для оптимизация запроса стоит переписать соединение через запятую на inner join и оконную функцию (в ней нет смысла, когда вы убрали таблицу film) на group by.
Еще можно попробовать создать индекс на payment_date (индексы часто создают на поля в предикате where), но чтобы mysql смогла использовать созданный вами индекс, вам необходимо переписать условие where для payment_date следующим образом: payment_date >= ‘2005-07-30’ and payment_date < DATE_ADD(‘2005-07-30’, INTERVAL 1 DAY).
Приложите скрин explain analyze вашего запроса, где будет, что субд использует ваш индекс.


##### Решение:

```
SELECT
	distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id)
FROM payment p
    join rental r on p.payment_date = r.rental_date
    join customer c on r.customer_id = c.customer_id
    join inventory i on i.inventory_id = r.inventory_id 
WHERE payment_date >= '2005-07-30' and payment_date < DATE_ADD('2005-07-30', INTERVAL 1 DAY);
```
![Решение 2.1.-правки](https://github.com/DenioSa/INDEX/blob/cd1826cd03c200bed163ce5a123850380422d7dd/img/3.bmp)`
![Решение 2.2.-правки](https://github.com/DenioSa/INDEX/blob/cd1826cd03c200bed163ce5a123850380422d7dd/img/4.bmp)`
