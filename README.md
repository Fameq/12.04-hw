# Домашнее задание к занятию 12.4 "Реляционные базы данных: SQL. Часть 2"

Домашнее задание нужно выполнить на датасете из презентации.

*Для проверки домашнего задания в личном кабинете прикрепите и отправьте ссылку на ваш Github репозиторий, содержащий запросы по всем заданиям.  
Для оформления вашего решения можете воспользоваться [шаблоном](https://github.com/netology-code/sys-pattern-homework)*.

Любые вопросы по решению задач задавайте в чате учебной группы.

---

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1.

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина,
- город нахождения магазина,
- количество пользователей, закрепленных в этом магазине.

![alt text](https://github.com/Fameq/12.04-hw/blob/master/img/task1.png)

```sql
SELECT  
	CONCAT_WS (' ', s.first_name, s.last_name) as Staff,
	c2.city as City,
	COUNT(c.store_id) as Customers
FROM staff s 
JOIN customer c ON c.store_id = s.store_id
JOIN address a  ON a.address_id  = s.address_id
JOIN city c2 ON c2.city_id =  a.city_id 
GROUP BY s.staff_id  HAVING Customers > 300
```

### Задание 2.

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

![alt text](https://github.com/Fameq/12.04-hw/blob/master/img/task2.png)

```sql
SELECT COUNT(1)
FROM film
WHERE `length`  > (SELECT AVG (`length`) FROM film)
```

### Задание 3.

Получите информацию, за какой месяц была получена наибольшая сумма платежей и добавьте информацию по количеству аренд за этот месяц.

![alt text](https://github.com/Fameq/12.04-hw/blob/master/img/task3.png)

```sql
SELECT my_date, sum_price, COUNT(r.rental_id) as my_count
FROM
	(SELECT my_date, sum_price
	FROM
		(
		SELECT  DATE_FORMAT(payment_date, '%Y-%M') as my_date, SUM(amount) as sum_price 
		FROM payment p  
		GROUP BY DATE_FORMAT(payment_date, '%Y-%M')
		)
	mytable 
	ORDER BY sum_price  DESC 
	Limit 1
	)
mytable_2
JOIN rental r on DATE_FORMAT(r.rental_date, '%Y-%M') = mytable_2.my_date 
GROUP BY my_date, sum_price 
;
```


## Дополнительные задания (со звездочкой*)
Эти задания дополнительные (не обязательные к выполнению) и никак не повлияют на получение вами зачета по этому домашнему заданию. Вы можете их выполнить, если хотите глубже и/или шире разобраться в материале.

### Задание 4*.

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», 
иначе должно быть значение «Нет».

### Задание 5*.

Найдите фильмы, которые ни разу не брали в аренду.

