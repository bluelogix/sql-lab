# Database Queries

## find all customers that live in London. Returns 6 records.
SELECT * FROM customers
where city = "London"



## find all customers with postal code 1010. Returns 3 customers.
SELECT * FROM customers
where postalCode = 1010



## find the phone number for the supplier with the id 11. Should be (010) 9984510.
SELECT SupplierId, phone, ContactName, SupplierName FROM Suppliers
where phone like '%998%'



## list orders descending by the order date. The order with date 1997-02-12 should be at the top.
SELECT * FROM [Orders] order by  OrderDate desc




## find all suppliers who have names longer than 20 characters. You can use `length(SupplierName)` to get the length of the name. Returns 11 records.
SELECT SupplierName FROM [Suppliers]
where length(SupplierName) > 20



## find all customers that include the word "market" in the name. Should return 4 records.
SELECT * FROM [Customers]
where CustomerName like '%ma___t%'



## add a customer record for _"The Shire"_, the contact name is _"Bilbo Baggins"_ the address is _"1 Hobbit-Hole"_ in _"Bag End"_, postal code _"111"_ and the country is _"Middle Earth"_.
INSERT INTO [Customers] (
CustomerName, ContactName, Address, City, PostalCode, Country)
VALUES (
'The Shire', 'Bilbo Baggins', '1 Hobbit Hole', 'Bag End', '111', 'Middle Earth')


## update _Bilbo Baggins_ record so that the postal code changes to _"11122"_.
UPDATE [Customers]
SET PostalCode = '11122'
where PostalCode = '111'


## list orders grouped by customer showing the number of orders per customer. _Rattlesnake Canyon Grocery_ should have 7 orders.
SELECT Orders.OrderID, Customers.CustomerName,
COUNT (Orders.CustomerId)
FROM [Orders]
INNER JOIN Customers ON Orders.CustomerId =Customers.CustomerID
GROUP BY Orders.CustomerId;


## list customers names and the number of orders per customer. Sort the list by number of orders in descending order. _Ernst Handel_ should be at the top with 10 orders followed by _QUICK-Stop_, _Rattlesnake Canyon Grocery_ and _Wartian Herkku_ with 7 orders each.

SELECT Customers.CustomerName,
COUNT (Orders.CustomerId)
FROM [Customers]
INNER JOIN Orders ON Customers.CustomerId =Orders.CustomerID
GROUP BY Customers.CustomerId
ORDER BY COUNT (Customers.CustomerId) DESC


## list orders grouped by customer's city showing number of orders per city. Returns 58 Records with _Aachen_ showing 2 orders and _Albuquerque_ showing 7 orders.
SELECT Customers.City,
COUNT (Customers.City)
FROM [Customers]
INNER JOIN Orders ON Customers.CustomerId =Orders.CustomerID
GROUP BY Customers.City


## delete all users that have no orders. Should delete 17 (or 18 if you haven't deleted the record added) records.

DELETE FROM [Customers]
where CustomerID NOT IN (SELECT CustomerID FROM[ORDERS])
