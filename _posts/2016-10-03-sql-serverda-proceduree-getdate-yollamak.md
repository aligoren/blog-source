---
layout: post
title: SQL Server'da Procedure'e GETDATE() Yollamak
tags: [sql, mssql, sql server]
---

SQL Server tarafında çalışma yaparken bazen oluşturmuş olduğunuz Stored Procedure’lere parametre olarak GETDATE() yollamak isteyebiliriz.
<!--more-->
Kısaca bir örnek verecek olursak kullanıcıların kaydedildiği bir tablo var normalde bu işlemleri bir programlama dili yardımıyla zaten hallediyordunuz ama işiniz SQL Server’a düştü.

Senaryoya göre ekleme işleminiz şöyle gerçekleşebilir:

```sql
EXEC InsertNewUser 'Ali', '123456', GETDATE();
```

Ancak burada bir hata alacaksınız. Bu hatanın içeriğinde nvarchar ve date ile ilgili bir metin yer alacaktır.

Bu hata’yı çözmenin basit bir yolu var. Tarih bilgisini saklayacak alanı nullable olarak tanımlayarak basit bir kontrolle bu işlemi halledebiliriz. Örneğin önceden oluşturduğumuz PROCEDURE şu haldeydi:

```sql
CREATE PROC InsertNewUser
	@UserName nvarchar(30),
	@UserPassword nvarchar(30),
	@RegDate date
AS
    INSERT INTO Users(UserName, UserPassword, RegDate)
    VALUES (@UserName, @UserPassword, @RegDate)
```

Burada aslında bir hata yok PROCEDURE ilgili değeri aldığı sürece doğru çalışıyor. Ama direkt olarak bugünün tarihini almasını istediğimiz durumlarda parametre almıyor. O zaman oluşturduğumuz PROC üzerinde bir değişiklik yaparak başlangıçta nullable bir date değişkeni oluşturup durumunu kontrol edebiliriz. PROCEDURE düzenlenir ve şöyle bir işlem gerçekleştirilir:

```sql
ALTER PROC InsertNewUser
	@UserName nvarchar(30),
	@UserPassword nvarchar(30),
	@RegDate date = NULL
AS

IF @RegDate IS NULL
    SET @RegDate = GETDATE()

    INSERT INTO Users(UserName, UserPassword, RegDate)
    VALUES (@UserName, @UserPassword, @RegDate)
```

Artık bu sayede SQL Server tarafında NULL değer almış olsa dahi bugünün tarihini ekleyecektir. Eğer sizin için bu tip durumlar hayati önem taşıyorsa bu yapıyı kullanabilirsiniz. Basit olarak NULL yazmak, formatlı tarih yollamaktan daha kolay geliyor da olabilir. Unutmadan artık SP şöyle çalışacaktır:

```sql
EXEC InsertNewUser 'Ali', 'GÖREN', NULL
```

Bu çalışmanın SQL Fiddle üzerindeki örneğine şuradan ulaşabilirsiniz:

[http://sqlfiddle.com/#!6/921f6/1](http://sqlfiddle.com/#!6/921f6/1)