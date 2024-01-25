# java-DB-sub-queries

1. Return all customers listed in the orders table who placed orders on the most recently recorded day. 

2. Select all product names and prices that have unit price which is bigger than price of product with name 'Carnarvon Tigers'

3. Select customer ids and ther contact names that made order that was shipped to Brazil

4. Select customer ids who ordered more than 20 items of product with name = 'Tofu' on a single order.

5. Select only discounted products names, dicount value, category of it, and shipped date of order where this product occurs. Use join + sub query structure.

6. Select orders for customers that come from London and are shipped via United Package. Remember to use sub queries (not joins). 



SELECT c.customer_id, c.company_name, c.contact_name FROM customers c
	JOIN orders o ON c.customer_id = o.customer_id
	WHERE o.order_date = (SELECT MAX(order_date) FROM orders);

SELECT product_name, unit_price FROM products 
	WHERE unit_price >
	(SELECT unit_price FROM products WHERE product_name = 'Carnarvon Tigers');
	
SELECT c.customer_id, c.contact_name FROM customers c
	LEFT JOIN orders o ON c.customer_id = o.customer_id
	WHERE o.ship_country = 'Brazil';
	
SELECT c.customer_id FROM customers c
	JOIN orders o ON c.customer_id = o.customer_id
	JOIN order_details od ON o.order_id = od.order_id
	JOIN products p ON od.product_id = p.product_id
	WHERE p.product_name = 'Tofu'
	GROUP BY c.customer_id, o.order_id
	HAVING SUM(od.quantity) > 20;
	
SELECT p.product_name, od.discount, c.category_name, o.shipped_date FROM products p
	JOIN categories c ON p.category_id = c.category_id
	JOIN order_details od ON p.product_id = od.product_id
	JOIN orders o ON od.order_id = o.order_id
	WHERE od.discount > 0;
	
SELECT * FROM orders 
	WHERE customer_id IN (SELECT customer_id FROM customers WHERE city = 'London')
	AND ship_via = (SELECT shipper_id FROM shippers WHERE company_name = 'United Package');
