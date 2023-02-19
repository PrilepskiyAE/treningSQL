### Простая выборка
SELECT * FROM products;

### Простая выборка по полям product_id, product_name, unit_price
SELECT product_id,product_name, unit_price  FROM products;

### расчет полной стоимости товара unit_price * units_in_stock
SELECT product_id,product_name,unit_price * units_in_stock  FROM products;

### DISTINCT вывод уникальных городов
SELECT DISTINCT city FROM employees;

### DISTINCT вывод уникальных городов и стран
SELECT DISTINCT city, country FROM employees;

### COUNT Подсчет строк
SELECT COUNT(*) FROM orders;

### Количество городов в которых работают наши сотрудники
SELECT COUNT(DISTINCT city) FROM employees;

### Фильтрация Where

### Все компании в 'USA'

SELECT company_name, contact_name, phone
FROM customers
WHERE country = 'USA';

### price > 20
SELECT product_id,product_name, unit_price  
FROM products
WHERE unit_price > 20;

SELECT COUNT(*) 
FROM products
WHERE unit_price > 20;

### Вывод не продоваемого товара
SELECT product_id,product_name, unit_price  
FROM products
WHERE discontinued = 1;

### Заказщики не в Берлине

SELECT company_name, contact_name, phone, city
FROM customers
WHERE city <> 'Berlin';

### работа с датами
SELECT order_id,customer_id, order_date 
FROM orders
WHERE order_date > '1998-03-01';

### AND, OR

### дороже 25$ и Больше 40 штук на складе
SELECT product_id,product_name, unit_price, units_in_stock  
FROM products
WHERE unit_price > 25 AND units_in_stock >40;

### Вывести все customer в Берлине, Лондоне и Сан-Франциско 
SELECT company_name, contact_name, phone, city
FROM customers
WHERE city = 'Berlin' OR city = 'London' OR city = 'San Francisco';

### BETWEEN
### вес отгрузки от 20 до 40

SELECT order_id,customer_id, order_date, freight 
FROM orders
WHERE freight BETWEEN 20 AND 40;

### заказы между 20 мартом 1998г и 03 апрелем 1998г

SELECT order_id,customer_id, order_date, freight 
FROM orders
WHERE order_date BETWEEN '1998-03-20' AND '1998-04-03';

###  IN & NOT IN
### компании в Mexico, Germany, USA и в Canada
SELECT company_name, contact_name, phone, country
FROM customers
WHERE country IN ('Mexico','Germany','USA','Canada');

### реверсируем логику и выводим компании которых нет в Mexico, Germany, USA и в Canada
SELECT company_name, contact_name, phone, country
FROM customers
WHERE country NOT IN ('Mexico','Germany','USA','Canada');

### ORDER BY

### дороже 25$ и Больше 10 штук на складе отсортировано по полю product_name ASC
SELECT product_id,product_name, unit_price, units_in_stock  
FROM products
WHERE unit_price > 25 AND units_in_stock >10 ORDER BY product_name;

### дороже 25$ и Больше 10 штук на складе отсортировано по полю product_name по убыванию
SELECT product_id,product_name, unit_price, units_in_stock  
FROM products
WHERE unit_price > 25 AND units_in_stock >10 ORDER BY  unit_price DESC;

###  MIN, MAX, AVG
### самый старый заказ из London
SELECT MIN(order_date)
FROM orders
WHERE ship_city = 'London';

### самый новый заказ из London
SELECT MAX(order_date)
FROM orders
WHERE ship_city = 'London';

### самый новый заказ из London
SELECT MAX(order_date)
FROM orders
WHERE ship_city = 'London';

### Средняя цена
SELECT AVG (unit_price) 
FROM products;

### всего единиц в продаже
SELECT SUM(units_in_stock)
FROM products
WHERE discontinued <> 1;

### LIKE
### Найти всех сотрудников имя которых заканчивается на n
SELECT last_name, first_name 
FROM employees
WHERE first_name LIKE '%n';

### Найти всех сотрудников имя которых начинется на A
SELECT last_name, first_name 
FROM employees
WHERE first_name LIKE 'A%';

### LIMIT
### вывести первые 10 продуктов из таблицы
SELECT * FROM products LIMIT 10;

### Check on NULL
SELECT order_id,customer_id, order_date, ship_city, ship_region, ship_country 
FROM orders
WHERE ship_region IS NOT NULL;

SELECT order_id,customer_id, order_date, ship_city, ship_region, ship_country 
FROM orders
WHERE ship_region IS NULL;

### GROUP BY
### сколько в каждую страну отправляется посылок вес которых более 50 кг
SELECT  ship_country, COUNT(*) 
FROM orders
WHERE freight > 50
GROUP BY ship_country
ORDER BY COUNT(*) DESC;

### HAVING
### Посчитать сумму товара по категориям которые будут продоватся и сумма будет более 5000 отсортировать по сумме 
SELECT category_id, SUM(unit_price * units_in_stock)
FROM products
WHERE discontinued <> 1
GROUP BY category_id
HAVING SUM(unit_price * units_in_stock) >5000
ORDER BY SUM(unit_price * units_in_stock) >5000 DESC;

### UNION, INTERSECT, EXCEPT
### Вывести страны всех сотрудников и всех заказщиков 
SELECT country FROM customers
UNION
SELECT country FROM employees;

### список стран в которых одвременно и customers и suppliers
SELECT country FROM customers
INTERSECT
SELECT country 
FROM suppliers;

### Вывести страны всех сотрудников и нет заказщиков 
SELECT country FROM customers
EXCEPT
SELECT country FROM suppliers;
