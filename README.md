
# EZCommerce: Simplifying Online Shopping – SQL

## Objective: Design and implement a relational database system to model an e-commerce marketplace similar to Flipkart. Develop SQL queries to handle various aspects of the marketplace, including product management, order processing, user interactions, and analytics.

• Project involves creating an e-commerce site
for online product sales using Databases and other
storage options.
• Shows the user a catalog of various products available
for purchase in the store.
• Tech Stack: SQL

## https://rb.gy/q30hz



CREATE TABLE cart (
    CART_ID VARCHAR2(26) NOT NULL PRIMARY KEY
);


CREATE TABLE customer (
    CUSTOMER_ID VARCHAR2(12) NOT NULL PRIMARY KEY,
    FIRST_NAME VARCHAR2(11) NOT NULL,
    LAST_NAME VARCHAR2(10) NOT NULL,
    GENDER VARCHAR2(7) NOT NULL,
    EMAIL VARCHAR2(30) NOT NULL,
    ADDRESS VARCHAR2(30) NOT NULL,
    PHONE_NO NUMBER NOT NULL,
    CART_ID VARCHAR2(7) NOT NULL,
    FOREIGN KEY (CART_ID) REFERENCES cart(CART_ID)
);


 CREATE TABLE seller (
    SELLER_ID VARCHAR2(10) NOT NULL PRIMARY KEY,
    S_PASS VARCHAR2(10) NOT NULL,
    NAME VARCHAR2(20) NOT NULL,
    ADDRESS VARCHAR2(100) NOT NULL
);


CREATE TABLE payment (
    PAYMENT_ID VARCHAR2(10) NOT NULL PRIMARY KEY,
    PAYMENT_DATE DATE,
    PAYMENT_TYPE VARCHAR2(12),
    CUSTOMER_ID VARCHAR2(10),
    CART_ID VARCHAR2(7) NOT NULL,
    DISCOUNT NUMBER,
    TOTAL_AMOUNT NUMBER,
    FOREIGN KEY (CUSTOMER_ID) REFERENCES customer(CUSTOMER_ID),
    FOREIGN KEY (CART_ID) REFERENCES cart(CART_ID)
);

CREATE TABLE products (
    PRODUCT_ID VARCHAR2(20) PRIMARY KEY,
    TYPE VARCHAR2(12) NOT NULL,
    COLOR VARCHAR2(12) NOT NULL,
    P_SIZE VARCHAR2(12) NOT NULL,
    GENDER VARCHAR2(12) NOT NULL,
    COMMISSION NUMBER NOT NULL,
    COST NUMBER NOT NULL,
    QUANTITY NUMBER NOT NULL,
    SELLER_ID VARCHAR2(10) NOT NULL,
    FOREIGN KEY (SELLER_ID) REFERENCES seller(SELLER_ID)
);

   CREATE TABLE cart_item (
    QUANTITY_WISHED NUMBER NOT NULL,
    DATE_ADDED DATE NOT NULL,
    CART_ID VARCHAR2(12) NOT NULL,
    PRODUCT_ID VARCHAR2(12) NOT NULL,
    PURCHASED VARCHAR2(5) NOT NULL,
    PRIMARY KEY (PRODUCT_ID),
    FOREIGN KEY (CART_ID) REFERENCES cart(CART_ID)
);


CREATE TABLE seller_phone_number (
    PHONE_ID VARCHAR2(26) NOT NULL,
    SELLER_ID VARCHAR2(26) NOT NULL,
    PHONE_NUMBER VARCHAR2(26) NOT NULL,
    PHONE_TYPE VARCHAR2(26) NOT NULL,
    PRIMARY KEY (PHONE_ID),
FOREIGN KEY (Product_id) REFERENCES Product(Product_id)
);


CREATE TABLE review (
    REVIEW_ID VARCHAR2(11) NOT NULL PRIMARY KEY,
    PRODUCT_ID VARCHAR2(11) NOT NULL,
    CUSTOMER_ID VARCHAR2(11) NOT NULL,
    REVIEW_TEXT VARCHAR2(41) NOT NULL,
    RATING NUMBER NOT NULL,
    REVIEW_DATE DATE NOT NULL,
    FOREIGN KEY (PRODUCT_ID) REFERENCES products(PRODUCT_ID),
    FOREIGN KEY (CUSTOMER_ID) REFERENCES customer(CUSTOMER_ID)
);

Queries

1. Retrieve the names of the top 5 most expensive products.

SELECT PRODUCT_ID, COST
FROM PRODUCTS
ORDER BY COST DESC FETCH FIRST 15 ROWS ONLY;

2. Customer wants to see filtered products on the basis of size,gender,type

SELECT PRODUCT_ID, COLOR, COST, SELLER_ID FROM PRODUCTS WHERE TYPE='Shirt' AND P_SIZE='L';

3. Calculate total order amount for a specific customer
SELECT customer_id, SUM(total_amount) AS total_spent
FROM Orders
WHERE customer_id in('CUST006', 'CUST016', 'CUST008')
GROUP BY customer_id;

4.Get the latest orders:
SELECT * FROM Orders
ORDER BY order_date DESC;


5. Create the query to retrieve the total number of orders placed by each customer:

SELECT C.customer_id, C.first_name, C.last_name, COUNT(O.order_id) AS total_orders
FROM Customer C
JOIN Orders O ON C.customer_id != O.customer_id
GROUP BY C.customer_id, C.first_name, C.last_name
ORDER BY total_orders DESC;


6. Retrieve customer orders with details:
SELECT c.customer_id, c.customer_name, o.order_id, o.order_date
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id;


7. How much product sold on the particular date?
SELECT COUNT(product_id) count_pid,date_added FROM cart_item WHERE purchased='YES'  GROUP BY(date_added);


8. If a customer want to know the total price present in the cart
select sum(QUANTITY_WISHED * cost * COMMISSION/100) TOTAL_PROFIT from PRODUCTS p join CART_ITEM c on p.PRODUCT_ID=c.PRODUCT_ID where PURCHASED=’NO';

9. If admin want to see what are the product purchased on the particular date

select PRODUCT_ID from CART_ITEM where (PURCHASED='YES' and DATE_ADDED='17-01-23');

10. Retrieve pending orders for a specific payment method:
select o.order_id, o.order_date, c.first_name, c.last_name
from orders o
join customer c on o.customer_id = c.customer_id
where o.status = 'pending';


11. Retrieve customers who used a specific payment method:
SELECT c.customer_id, c.last_name
FROM customer c
JOIN orders o ON c.customer_id = o.customer_id
JOIN payment pm ON o.customer_id = pm.customer_id
WHERE pm.payment_type in ('Credit Card', 'Debit Card');



