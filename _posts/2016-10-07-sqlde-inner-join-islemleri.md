---
layout: post
title: SQL'de Inner Join İşlemleri
tags: [sql, mssql, sql server]
---

SQL’de normalizasyon işlemlerini gerçekleştirdiğimiz zaman tabloları da bölme işlemini gerçekleştirmiş oluruz. Bu sayede hem normal formlarda bir database tasarımı gerçekleştirmiş oluruz hem de performans kazanabiliriz.
<!--more-->
Bu yazıda bölünmüş tabloların birleştirilmesinde faydalanabileceğimiz INNER JOIN kullanımına değineceğim. Yukarıda değinmiş olduğum performans konusu sadece yukarısı için geçerlidir. Her INNER JOIN performanslı olacak diye bir şey yoktur.

İki tablo düşünelim. Bu tablolardan birisi kullanıcıların sadece adlarını tutuyor. Bir diğeri ise kullanıcıların sadece açıklamalarını tutuyor. Bu iki tablonun tek sorguda ortak sonuçlar üretmesi gerekiyor. Aslında sanal bir tablo oluştuğunu düşünün. Tıpkı string birleştirme işlemlerinde olduğu gibi. Hemen bunu tanımlayan iki tablo oluşturalım.

```sql
CREATE TABLE [UserDetail] (
	[ID] int NOT NULL IDENTITY(1,1) ,
	[UserID] int NOT NULL ,
	[UserDesc] nvarchar(50) NOT NULL
);

CREATE TABLE [Users] (
	[ID] int NOT NULL IDENTITY(1,1) ,
	[Name] nvarchar(50) NOT NULL
);
```

Bu tabloları oluşturduktan sonra veri ekleme işlemlerini yapmalıyız. Bu işlemlerden ilkinde sadece kullanıcının adı yer alacakken, ikinci tabloda yani UserDetail tablosunda ise ilgili kullanıcının hem ID’si hem de açıklaması yer alacak. Eklemeleri ona göre gerçekleştirelim.

```sql
INSERT INTO Users(Name) VALUES('Ali');
INSERT INTO Users(Name) VALUES('Ali2');
INSERT INTO Users(Name) VALUES('Ali3');

INSERT INTO UserDetail(UserID, UserDesc) VALUES(1, 'Açıklama');
INSERT INTO UserDetail(UserID, UserDesc) VALUES(2, '2. Açıklama');
INSERT INTO UserDetail(UserID, UserDesc) VALUES(3, '3. Açıklama');
```

Şimdi normalde veri çekme işlemini yapıyor gibi SELECT sorgumuzu yazalım. Burada farklı olan şey ise veriyi seçeceğimiz asıl tablonun hemen yanına INNER JOIN yazmamız. JOIN işleminden sonra asıl tabloya bağlamak istediğimiz tabloyu belirtiyoruz. WHERE işleminin JOIN işlemlerinden sonra kullanıldığına da dikkat edin.

```sql
SELECT u.Name, ud.UserDesc FROM Users u
INNER JOIN UserDetail ud ON  u.ID=ud.UserID WHERE u.ID=2;
```

Yukarıdaki kodda Users tablosunda bulunan Name kolonunu, UserDetail tablosunda bulunan UserDesc kolonunu, her iki tabloda da eşleşen kayda göre getir dedik.

Buradan sahte bir kod yapısıyla şunu anlayabiliriz:

```
if UsersTablosundakiUserID eşittir UserDetaildekiUserID ise
	getir(UsersTablosundakiUserID)

# Yani UserID2=UserDetailID2 ise WHERE ID=2 olanı getir
```

İşlemin nasıl çalıştığını yukarıdaki kodla basitçe anlatmaya çalıştım. Anlayacağımız bir diğer şey ise görüyoruz ki sadece birbirine eşit olan kayıtları getiriyor. Burada bu işlemden sonra çıktının şöyle olduğunu görmekteyiz:

<table class="table table-hover">
	<thead>
		<tr>
			<th>Name</th>
			<th>UserDesc</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>Ali2</td>
			<td>2. Açiklama</td>
		</tr>
	</tbody>
</table>

Dilersek bir WHERE sorgusunu kullanmadan tablodaki eşleşen tüm kayıtları getirebiliriz. Bunun için UserDetail tablosunu düzenleyip UserID bölümünü NULLABLE olarak ayarlayalım ve bu tabloya bir veri eklemesi gerçekleştirelim.

UserDetail tablosuna eklenen 4. verinin UserID kolonuna eklenecek değer NULL olarak gönderilsin.

```sql
CREATE TABLE [UserDetail] (
	[ID] int NOT NULL IDENTITY(1,1) ,
	[UserID] int,
	[UserDesc] nvarchar(50) NOT NULL
);

CREATE TABLE [Users] (
	[ID] int NOT NULL IDENTITY(1,1) ,
	[Name] nvarchar(50) NOT NULL
);

INSERT INTO Users(Name) VALUES('Ali');
INSERT INTO Users(Name) VALUES('Ali2');
INSERT INTO Users(Name) VALUES('Ali3');
INSERT INTO Users(Name) VALUES('Ali4');

INSERT INTO UserDetail(UserID, UserDesc) VALUES(1, 'Açıklama');
INSERT INTO UserDetail(UserID, UserDesc) VALUES(2, '2. Açıklama');
INSERT INTO UserDetail(UserID, UserDesc) VALUES(3, '3. Açıklama');
INSERT INTO UserDetail(UserID, UserDesc) VALUES(NULL, '4. Açıklama');
```

Daha sonra JOIN işlemini WHERE kullanmadan çalıştıralım:

```sql
SELECT u.Name, ud.UserDesc FROM Users u
INNER JOIN UserDetail ud ON  u.ID=ud.UserID;
```

<table class="table table-hover">
	<thead>
		<tr>
			<th>Name</th>
			<th>UserDesc</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>Ali</td>
			<td>Açiklama</td>
		</tr>
		<tr>
			<td>Ali2</td>
			<td>2. Açiklama</td>
		</tr>
		<tr>
			<td>Ali3</td>
			<td>3. Açiklama</td>
		</tr>
	</tbody>
</table>

Fark ettiyseniz çıktıda eklediğimiz 4. veri yok. Çünkü iki tablodaki ortak kabul edilen alanlardan birisi diğerinden farklı. Eşleşme sağlanamıyor. Buradan INNER JOIN için NULL olmayan, eşleşen kayıtları getiren SQL ifadeleridir çıkarımını da gerçekleştirebiliriz.

1- Yöntem Olarak WHERE Sorgusu Kullanmak

Evet eğer aklınızda WHERE sorgusu belirmiş olabilir. İkinci yöntem olarak WHERE sorgusu da kullanabilirsiniz

```sql
SELECT u.Name, ud.UserDesc
FROM Users u, UserDetail ud
WHERE u.ID=ud.UserID

-- ya da

SELECT u.Name, ud.UserDesc
FROM Users u, UserDetail ud
WHERE u.ID=ud.UserID and u.ID=2
```

Yine aynı çıktıyı verecektir. İkisi arasındaki farkları araştırmanız, neyin nerede kullanılacağını bilmeniz sizin için iyi olur.

Örnek SQL Fiddle [Linki](http://sqlfiddle.com/#!6/1e87c/2)