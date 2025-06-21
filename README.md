# indecs

## Домашнее задание к занятию «Индексы» - Aleksandr Romanenko

Задание 1
Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.

Задание 2
Выполните explain analyze следующего запроса:
select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id

перечислите узкие места;
оптимизируйте запрос: внесите корректировки по использованию операторов, при необходимости добавьте индексы.




### Задание 1
SELECT SUM(index_length) / SUM(data_length) * 100
FROM INFORMATION_SCHEMA.TABLES;

### Задание 2



CREATE INDEX IF NOT EXISTS pay_date_idx ON payment(payment_date);
SELECT 
    c.last_name,
    c.first_name,
    f.title AS film,
    SUM(p.amount) AS total

FROM payment p                

JOIN rental r ON p.rental_id = r.rental_id
JOIN customer c ON r.customer_id = c.customer_id

JOIN inventory i ON r.inventory_id = i.inventory_id 
JOIN film f ON i.film_id = f.film_id

WHERE p.payment_date >= '2005-07-30'
  AND p.payment_date < '2005-07-31'    


GROUP BY 
    c.customer_id,
    f.film_id,
    c.last_name,
    c.first_name;  



