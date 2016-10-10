1. Provide a query showing Customers (just their full names, customer ID and country) who are not in the US.
```
SELECT FirstName || ' ' || LastName AS 'Full Name', CustomerId, Country FROM Customer
WHERE Country != 'US';
```

2. Provide a query only showing the Customers from Brazil.
```
SELECT FirstName || ' ' || LastName AS 'Full Name', CustomerId, Country FROM Customer
WHERE Country == 'Brazil';
```

3. Provide a query showing the Invoices of customers who are from Brazil. The resultant table should show the customer's full name, Invoice ID, Date of the invoice and billing country.
```
SELECT FirstName || ' '||  LastName AS 'Customer Name', Invoice.InvoiceId, Invoice.InvoiceDate, Invoice.BillingCountry
FROM (SELECT * FROM Customer WHERE Country == 'Brazil') AS 'BrazilCustomers'
JOIN  Invoice ON BrazilCustomers.CustomerId == Invoice.CustomerId;
```

4. Provide a query showing only the Employees who are Sales Agents.
```
SELECT * FROM Employee
WHERE Title LIKE '%Sales%Agent%';
```

5. Provide a query showing a unique list of billing countries from the Invoice table.
```
SELECT DISTINCT BillingCountry FROM Invoice;
```

6. Provide a query showing the invoices of customers who are from Brazil.
```
SELECT * FROM Invoice
JOIN (SELECT * FROM Customer WHERE Customer.Country == 'Brazil') AS 'BrazilCustomers'
ON BrazilCustomers.CustomerId == Invoice.CustomerId;
```

7. Provide a query that shows the invoices associated with each sales agent. The resultant table should include the Sales Agent's full name.
```
SELECT Employee.FirstName || ' ' || Employee.LastName AS 'Support Rep', InvoiceId, Invoice.CustomerId, InvoiceDate, BillingAddress, BillingCity, BillingState, BillingCountry, BillingPostalCode, Total FROM Invoice
JOIN Customer ON Invoice.CustomerId == Customer.CustomerId
JOIN Employee ON Employee.EmployeeId == Customer.SupportRepId;
```
```
SELECT Employee.FirstName || ' ' || Employee.LastName AS 'Support Rep', Invoice.*FROM Invoice
JOIN Customer ON Invoice.CustomerId == Customer.CustomerId
JOIN Employee ON Employee.EmployeeId == Customer.SupportRepId;
```

8. Provide a query that shows the Invoice Total, Customer name, Country and Sale Agent name for all invoices and customers.
```
SELECT Total, Customer.FirstName || ' ' || Customer.LastName AS 'Customer', Customer.Country, Employee.FirstName || ' ' || Employee.LastName AS 'Support Rep' FROM Invoice
JOIN Customer ON Invoice.CustomerId == Customer.CustomerId
JOIN Employee ON Employee.EmployeeId == Customer.SupportRepId;
```

9. A. How many Invoices were there in 2009 and 2011?
  ```
  SELECT COUNT(InvoiceDate) FROM Invoice
  WHERE InvoiceDate LIKE '%2009%' OR InvoiceDate LIKE '%2011%'
  ```
   B. What are the respective total sales for each of those years?
  ```
  SELECT TOTAL(Total) FROM Invoice
  WHERE InvoiceDate LIKE '%2009%';
  ```
  ```
  SELECT TOTAL(Total) FROM Invoice
  WHERE InvoiceDate LIKE '%2011%';
  ```
10. Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for Invoice ID 37.
```
SELECT COUNT(InvoiceLineId) From InvoiceLine
WHERE InvoiceId == 37;
```
11. Looking at the InvoiceLine table, provide a query that COUNTs the number of line items for each Invoice. HINT: GROUP BY
```
SELECT COUNT(InvoiceLineId), InvoiceId From InvoiceLine
GROUP BY InvoiceId;
```
12. Provide a query that includes the track name with each invoice line item.
```
SELECT Track.Name, InvoiceLineId FROM InvoiceLine
JOIN Track on InvoiceLine.TrackId == Track.TrackId;
```
13. Provide a query that includes the purchased track name AND artist name with each invoice line item.
```
SELECT Track.Name, Artist.Name, InvoiceLineId FROM InvoiceLine
JOIN Track ON InvoiceLine.TrackId == Track.TrackId
JOIN Album ON Track.AlbumId == Album.AlbumId
JOIN Artist ON Album.ArtistId == Artist.ArtistId;
```
14. Provide a query that shows the # of invoices per country. HINT: GROUP BY
```
SELECT COUNT(InvoiceId), BillingCountry From Invoice
GROUP BY BillingCountry;
```
15. Provide a query that shows the total number of tracks in each playlist. The Playlist name should be include on the resultant table
```
SELECT COUNT(Track.TrackId), Playlist.Name From Track
JOIN PlaylistTrack ON Track.TrackId == PlaylistTrack.TrackId
JOIN Playlist ON PlaylistTrack.PlaylistId == Playlist.PlaylistId
GROUP BY PlaylistTrack.PlaylistId;
```
16. Provide a query that shows all the Tracks, but displays no IDs. The resultant table should include the Album name, Media type and Genre.
```
SELECT Album.Title AS 'Album Title', MediaType.Name AS 'Media Type', Genre.Name AS 'Genre' from Track
JOIN Album ON Track.AlbumId == Album.AlbumId
JOIN MediaType ON Track.MediaTypeId == MediaType.MediaTypeId
JOIN Genre ON Track.GenreId == Genre.GenreId;
```
17. Provide a query that shows all Invoices but includes the # of invoice line items.
```
SELECT Invoice.*, COUNT(InvoiceLine.InvoiceId) AS 'Invoice Lines' FROM Invoice
JOIN InvoiceLine ON Invoice.InvoiceId == InvoiceLine.InvoiceId
GROUP BY Invoice.InvoiceId;
```
18. Provide a query that shows total sales made by each sales agent.
```
SELECT TOTAL(Invoice.Total) AS 'Sales Total', Employee.FirstName || ' ' || Employee.LastName AS 'Sales Rep' FROM Invoice
JOIN Customer ON Invoice.CustomerId == Customer.CustomerId
JOIN Employee ON Customer.SupportRepId == Employee.EmployeeId
GROUP BY Employee.EmployeeId;
```
19. Which sales agent made the most in sales in 2009? HINT: MAX
```
SELECT TOTAL(Invoice.Total) AS 'Sales Total', Employee.FirstName || ' ' || Employee.LastName AS 'Sales Rep' FROM Invoice
JOIN Customer ON Invoice.CustomerId == Customer.CustomerId
JOIN Employee ON Customer.SupportRepId == Employee.EmployeeId
WHERE strftime('%Y', Invoice.InvoiceDate ) == '2009'
GROUP BY Employee.EmployeeId
ORDER BY 'Sales Total' DESC
LIMIT 1;
```

20. Which sales agent made the most in sales in 2010?
```
SELECT TOTAL(Invoice.Total) AS 'Sales Total', Employee.FirstName || ' ' || Employee.LastName AS 'Sales Rep' FROM Invoice
JOIN Customer ON Invoice.CustomerId == Customer.CustomerId
JOIN Employee ON Customer.SupportRepId == Employee.EmployeeId
WHERE strftime('%Y', Invoice.InvoiceDate ) == '2010'
GROUP BY Employee.EmployeeId
ORDER BY 'Sales Total' DESC
LIMIT 1;
```
```
SELECT Employee.FirstName || ' ' || Employee.LastName AS 'Sales Rep',
MAX(SalesTotal)
FROM (SELECT TOTAL(Invoice.Total) AS SalesTotal
FROM Invoice
JOIN Customer ON Invoice.CustomerId == Customer.CustomerId
JOIN Employee ON Customer.SupportRepId == Employee.EmployeeId
WHERE strftime('%Y', Invoice.InvoiceDate ) == '2010'
GROUP BY Employee.EmployeeId
), Invoice
JOIN Customer ON Invoice.CustomerId == Customer.CustomerId
JOIN Employee ON Customer.SupportRepId == Employee.EmployeeId
```
21. Which sales agent made the most in sales over all?
```
SELECT TOTAL(Invoice.Total) AS 'Sales Total', Employee.FirstName || ' ' || Employee.LastName AS 'Sales Rep' FROM Invoice
JOIN Customer ON Invoice.CustomerId == Customer.CustomerId
JOIN Employee ON Customer.SupportRepId == Employee.EmployeeId
GROUP BY Employee.EmployeeId
ORDER BY "Sales Total" DESC
LIMIT 1;
```
22. Provide a query that shows the # of customers assigned to each sales agent.
```
SELECT TOTAL(Customer.CustomerId) AS 'Total Customers', Employee.FirstName || ' ' || Employee.LastName AS 'Sales Rep' FROM Customer
JOIN Employee ON Customer.SupportRepId == Employee.EmployeeId
GROUP BY Employee.EmployeeId;
```
23. A. Provide a query that shows the total sales per country.
  ```
  SELECT TOTAL(Invoice.Total) AS 'Total Spent Per Country', Invoice.BillingCountry FROM Invoice
  GROUP BY Invoice.BillingCountry;
  ```
  B.  Which country's customers spent the most?
  ```
  SELECT TOTAL(Invoice.Total) AS 'Total Spent Per Country', Invoice.BillingCountry FROM Invoice
  GROUP BY Invoice.BillingCountry
  ORDER BY 1 DESC
  LIMIT 1;
  ```
  ```
  /* **** WORKED **** */
  SELECT TOTAL(Invoice.Total) AS 'Total Spent Per Country', Invoice.BillingCountry FROM Invoice
  GROUP BY Invoice.BillingCountry
  ORDER BY "Total Spent Per Country" DESC
  LIMIT 1;
  ```
  ```
  /* **** DID NOT WORK WHAT THE HELL **** */
  SELECT TOTAL(Invoice.Total) AS 'Total Spent Per Country', Invoice.BillingCountry FROM Invoice
  GROUP BY Invoice.BillingCountry
  ORDER BY 'Total Spent Per Country' DESC
  LIMIT 1;
  ```
24. Provide a query that shows the most purchased track of 2013.
```
/* WOULD WORK BUT ALL TRACKS WERE ONLY PURCHASED ONCE??*/
SELECT COUNT(InvoiceLine.InvoiceLineId), Track.Name FROM InvoiceLine
JOIN Invoice ON InvoiceLine.InvoiceId = Invoice.InvoiceId
JOIN Track ON InvoiceLine.TrackId == Track.TrackId
WHERE strftime('%Y', Invoice.InvoiceDate ) == "2013"
GROUP BY InvoiceLine.TrackId
ORDER BY 1 DESC
LIMIT 1;
```
25. Provide a query that shows the top 5 most purchased tracks over all.
```
SELECT COUNT(InvoiceLine.InvoiceLineId) AS 'Number Sold', Track.Name AS 'Track Name' FROM InvoiceLine
JOIN Invoice ON InvoiceLine.InvoiceId = Invoice.InvoiceId
JOIN Track ON InvoiceLine.TrackId == Track.TrackId
GROUP BY InvoiceLine.TrackId
ORDER BY 1 DESC
LIMIT 5;
```
26. Provide a query that shows the top 3 best selling artists.
```
SELECT COUNT(InvoiceLine.InvoiceLineId) AS 'Number Sold', Artist.Name AS 'Artist Name' FROM InvoiceLine
JOIN Invoice ON InvoiceLine.InvoiceId = Invoice.InvoiceId
JOIN Track ON InvoiceLine.TrackId == Track.TrackId
JOIN Album ON Track.AlbumId == Album.AlbumId
JOIN Artist ON Album.ArtistId == Artist.ArtistId
GROUP BY Artist.ArtistId
ORDER BY 1 DESC
LIMIT 5;
```
27. Provide a query that shows the most purchased Media Type.
```
SELECT COUNT(InvoiceLine.InvoiceLineId) AS 'Number Sold', MediaType.Name AS 'Media Type' FROM InvoiceLine
JOIN Invoice ON InvoiceLine.InvoiceId = Invoice.InvoiceId
JOIN Track ON InvoiceLine.TrackId == Track.TrackId
JOIN MediaType ON Track.MediaTypeId == MediaType.MediaTypeId
GROUP BY MediaType.MediaTypeId
ORDER BY 1 DESC
LIMIT 1;
```
