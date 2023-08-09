## Ülkesi A harfi ile bitmeyen müşterileri listeleyin

`select *
from Customers 
where Country not like '%A'`

## 04.07.1996 ile 31.12.1996 tarihleri arasında verilen siparişler hangileridir?

`select *
from Orders
where OrderDate between '1996.07.04' and '1996.12.31'`


## Ürün adlarını ve fiyatlarını, her birine %18 olarak uygulanmak üzere KDV bilgisiyle beraber listeleyin. KDV bilgisi ayrı bir sütun olarak gelmeli.

`select ProductName,
       UnitPrice,
	   ((UnitPrice *18)/100) as 'KDV Miktari',
	   ((UnitPrice *18)/100)+UnitPrice [KDV'li Fiyat]
from Products`


## Siparişlerin tarihlerini ve nereye gönderileceklerini raporlayın.

`select OrderDate,
      ShipAddress
from Orders`


## Her bir müşterinin şirket adı ve açık adreslerini arasında '/' olacak şekilde tek bir kolonda ve yetkili kişiyle birlikte raporlayın.

`select CompanyName+' '+'/'+' '+Address+' '+'-'+' '+ContactName [SirketAdi / Adres - YetkiliIsim]
from Customers`


## Adında ve soyadında 'e' harfi geçmeyen çalışanlar kimlerdir?

`select *
from Employees
where FirstName not like '%e%'
      and
	  LastName not like '%e%'`


## Fiyatı küsüratlı ürünleri bulun.

`select *
from Products
where UnitPrice not like '%.00'`

## Stoğu olmasına rağmen artık satışı yapılmayan ürünler hangileridir?

`select * 
from Products
where UnitsInStock > 0 and Discontinued = 1`


## Teslimatı Amerika'ya geç kalan siparişler hangileridir?

`select *
from Orders
where ShipCountry = 'USA' and ShippedDate-RequiredDate>0`


## 1 doların altında kargo ücreti olan siparişler hangileridir?

`select * 
from Orders
where Freight < 1`


## Sistemde kayıtlı her ürün için bir barkod numarası oluşturulacaktır. Barkod numarası ID'nin 3 kere tekrarlanıp ardına da isminin ilk 2 harfi ile son 2 harfinin eklendiği şekildedir.

`select cast(ProductID as varchar)+cast(ProductID as varchar)+cast(ProductID as varchar)+SUBSTRING(ProductName,1,2)+SUBSTRING(ProductName,len(ProductName)-1,2),ProductID,ProductName
from Products`


## Siparişlerin gönderildiği ülkelerin ilk iki harfini küçük, gerisini büyük harf olacak şekilde listeleyin.

`select concat(lower(SUBSTRING(ShipCountry,1,2)),upper(SUBSTRING(ShipCountry,3,len(ShipCountry)))) as 'Ülkeler'
from Orders`


## En yüksek kargo ücreti ödenmiş 5 sipariş hangi ülkelere gönderilmiş?

`select top 5 Freight as 'Ücret',
          ShipCountry [Ülke]
from Orders 
order by Freight desc`


## Her çalışana kurumsal bir email adresi oluşturulacak. Çalışanın adının ilk harfi.soyadı ve sonunda @northwind.com olacak şekilde. 
## Nancy Davolio için n.davolio@northwind.com gibi. Mail adreslerini listeleyin. Firstname | Lastname | Mail

`select lower(SUBSTRING(FirstName,1,1)) +'.'+lower(LastName)+'@northwind.com' as [Mail Adress]
from Employees`


## Stok miktarı kritik seviyeye veya altına düşmesine rağmen hala siparişi verilmeyen ürünler hangileridir? ProductName

`select ProductName
from Products
where UnitsInStock <= UnitsOnOrder
     and Discontinued = 0`



