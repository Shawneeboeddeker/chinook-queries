# chinook-queries
Using the chinook database I queried several facts to answer specific questions. I cleaned the db where necessary.




/*Select artist name, track, and genre from the appropriate tables*/

SELECT a.Name AS 'Musician or Group', t.Name AS 'Track Name', g.Name AS 'Genre'
FROM chinook.artists a
JOIN chinook.albums
ON a.Artistid = albums.Artistid
JOIN chinook.tracks t
ON albums.albumid = t.Albumid
JOIN chinook.genres g
ON t.Genreid = g.Genreid;


/*What are the composer names and song tracks that cost less than $1.99? Clean the data*/

SELECT i.UnitPrice, t.Name, t.Composer
FROM chinook.tracks t
JOIN chinook.invoice_items i
ON t.Trackid = i.Trackid
WHERE i.UnitPrice < 1.99
AND t.Composer <> 'NULL';

/*List which Support Reps handle the customers in the USA, add the state and Supoort Rep ID,  */

SELECT * FROM chinook.customers;

SELECT Address, State, SupportRepid
FROM chinook.customers
WHERE Country = 'USA'
AND State <> 'NULL'
GROUP BY SupportRepid;

/*List the customers handled by each support rep.*/

SELECT FirstName, LastName, SupportRepid
FROM chinook.customers
Order BY SupportRepid;

/*Who are the customers each USA-based suppport rep has?*/

SELECT FirstName, LastName, SupportRepid
FROM chinook.customers
WHERE Country = 'USA'
Order BY SupportRepid;

/*Which employees are sales support reps?*/

SELECT * 
FROM chinook.Employees
WHERE Title = "Sales Support Agent";

/*Provide a query that shows the invoices associated with each sales agent. The resulting table should include the Sales Agent's full name.*/

SELECT e.FirstName, e.LastName, i.InvoiceId
FROM chinook.employees e
JOIN chinook.customers c
ON e.Employeeid = c.SupportRepid
JOIN chinook.invoices i
ON c.Customerid = i.Customerid
WHERE e.Title = 'Sales Support Agent';

/*Write a query that includes the purchased track name with each invoice ID.*/

SELECT * FROM chinook.invoice_items; 

SELECT items.TrackId, items.UnitPrice, items.InvoiceID, tracks.Name
FROM chinook.invoice_items items
JOIN chinook.tracks tracks
ON items.Trackid = tracks.Trackid
ORDER BY InvoiceID;

/*Write a query that includes the purchased track name with each invoice line ID*/

SELECT t.Name, i.InvoiceLineId
FROM chinook.Invoice_items i
JOIN chinook.Tracks t 
ON i.TrackId = t.TrackId;

/*Which Sales rep made the most dollars in sales in 2009?*/

SELECT * FROM chinook.invoices;
SELECT * FROM chinook.employees;

SELECT emp.FirstName, emp.LastName, ROUND(SUM(inv.Total), 2)
FROM chinook.employees emp

JOIN chinook.customers cust
ON emp.Employeeid = cust.SupportRepid

JOIN chinook.invoices inv
ON cust.Customerid = inv.Customerid

WHERE emp.Title = 'Sales Support Agent'
AND inv.InvoiceDate LIKE '2009%'
GROUP BY emp.FirstName
ORDER BY ROUND(SUM(inv.Total), 2)
 DESC LIMIT 1;
