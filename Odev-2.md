## Müşterilerden elde edilen ciroya göre, 20000-29999 Silver, 30000-49999 Gold, 49999 Üzeri 'Platinum' olacak şekilde raporlayın. Musteri | Ciro | Tip
 
`SELECT   c.ContactName AS Musteri,
          SUM(od.UnitPrice * od.Quantity * (1 - od.Discount)) AS [Ciro],
		CASE
		   WHEN  SUM(od.UnitPrice * od.Quantity * (1 - od.Discount)) < 20000 THEN 'Bilinmiyor'
	           WHEN  SUM(od.UnitPrice * od.Quantity * (1 - od.Discount)) < 30000 THEN 'Silver'
		   WHEN  SUM(od.UnitPrice * od.Quantity * (1 - od.Discount)) < 49999  THEN 'Gold'
		   ELSE  'Platinum'
	       END AS Tip
FROM Customers c 
JOIN Orders o ON c.CustomerID = o.CustomerID
JOIN [Order Details] od ON o.OrderID = od.OrderID
GROUP BY c.ContactName  `

## 20'den fazla kez sipariş verilmiş ürünler hangileridir?

####   -- Subquery --
`SELECT DISTINCT ProductName
FROM Products
WHERE ProductID IN(SELECT ProductID
                   FROM [Order Details]
				   WHERE Quantity > 20)`
####   -- Join --
`SELECT DISTINCT ProductName
FROM Products p
JOIN [Order Details] od ON p.ProductID = od.ProductID
WHERE od.Quantity > 20`


## Müşteriler arasında en popüler kategori hangisidir?

`SELECT
TOP 1 ca.CategoryID,
       COUNT(ca.CategoryID) as adet
FROM Categories ca 
JOIN Products p ON ca.CategoryID = p.CategoryID
JOIN [Order Details] od ON p.ProductID = od.ProductID
JOIN Orders o ON od.OrderID = o.OrderID
JOIN Customers c ON o.CustomerID = c.CustomerID 
GROUP BY ca.CategoryID	 
ORDER BY adet DESC`


## Haftanın son günü teslim edilen ürünler hangileridir?

`SELECT DISTINCT p.ProductName
FROM Orders o
JOIN [Order Details] od ON o.OrderID = od.OrderID
JOIN Products p ON od.ProductID = p.ProductID
WHERE DATENAME(WEEKDAY,ShippedDate) = 'Friday'`


## Londra'da yaşayan çalışanların ilgilendiği müşterilerden Almanya'da yaşayanlar kimlerdir?

`SELECT DISTINCT c.ContactName AS [İsimler]
FROM Customers c
JOIN Orders o ON c.CustomerID = o.CustomerID
JOIN Employees e ON o.EmployeeID = e.EmployeeID
WHERE e.City = 'London' AND
      c.Country = 'Germany'`


## Her kategorinin en çok satılan ürünü hangisidir?

`SELECT c.CategoryID,
       p.ProductName,
       COUNT(p.ProductName) AS Adet,
	   CASE
	       WHEN COUNT(p.ProductName) = 51 AND c.CategoryID = 1 THEN 'EN ÇOK SATILAN ÜRÜN'
		   WHEN COUNT(p.ProductName) = 38 AND c.CategoryID = 2 THEN 'EN ÇOK SATILAN ÜRÜN'
		   WHEN COUNT(p.ProductName) = 48 AND c.CategoryID = 3 THEN 'EN ÇOK SATILAN ÜRÜN'
		   WHEN COUNT(p.ProductName) = 54 AND c.CategoryID = 4 THEN 'EN ÇOK SATILAN ÜRÜN'
		   WHEN COUNT(p.ProductName) = 50 AND c.CategoryID = 5 THEN 'EN ÇOK SATILAN ÜRÜN'
		   WHEN COUNT(p.ProductName) = 37 AND c.CategoryID = 6 THEN 'EN ÇOK SATILAN ÜRÜN'
		   WHEN COUNT(p.ProductName) = 39 AND c.CategoryID = 7 THEN 'EN ÇOK SATILAN ÜRÜN'
		   WHEN COUNT(p.ProductName) = 47 AND c.CategoryID = 8 THEN 'EN ÇOK SATILAN ÜRÜN'
		   ELSE '-'
	   END AS SONUC
FROM Products p
JOIN Categories c ON p.CategoryID = c.CategoryID
JOIN [Order Details] od ON p.ProductID = od.ProductID
GROUP BY c.CategoryID,p.ProductName
ORDER BY c.CategoryID , Adet`

