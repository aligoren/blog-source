---
layout: post
title: MSSQL'de Stored Procedure Kullanımı
tags: [sql, mssql, sql server]
---

Daha önceleri çok fazla veritabanı kullanan işlemler yapmadım. O yüzden şu sıralar pek yeniyim. Öğrenme aşamaları hızlıca gelişiyor.

Stored Procedure dediğimiz şey ise veritabanında tutulan ve ilk çalışma derlenen daha sonraları tekrar derlenmeye ihtiyaç duyulmayan SQL ifadeleridir. Yani bir Stored Procedure oluşturursanız tekrar aynı isimde Stored Procedure’ü aynı veritabanı için oluşturamıyorsunuz.
<!--more-->
Bu gibi durumlarda oluşturduğunuz Stored Procedure’ü Modify etmeniz gerekiyor ki bu da ALTER komutu ile birlikte oluyor. Stored Procedure’lerin belli başlı avantajları vardır.

Bu avantajlardan en önemlisi kuşkusuz hızlı çalışmalarıdır. Bunun sebebi ise oluşturulan Stored Procedure bir kere derlendikten sonra tekrar tekrar derlemenize gerek kalmaz. O zaman buradan Stored Procedure’lerin veritabanı tarafında saklandığını anlayabiliriz.

Eğer uygulama katmanında çalışıyor olsaydık tek bir sorgu tekrar tekrar derlenmelidir ki bu da büyük sistemlerde büyük bir sorun haline gelebilir. Tabii ki zaman kaybı anlamına da geliyor. Yanlış anlaşılmasın C# tarafında SQL kodu yazıp, INSERT ettirmekle burada yazıp ettirmek mantık olarak pek farklı değil.

Ama olur da uygulama katmanında kullanmanız gerekiyordur size daha rahat bir geliştirme şansı sunar ki şöyle düşünün 20 satırlık SQL sorgunuz var ve siz bunu Entity Framework kullanarak geliştirmiyorsunuz. Her sorgunun 20 satır sürdüğünü hayal edersek sanırım Database Provider baya bir şişecektir. Stored Procedure kullanmak bunun da önüne geçebilir.

Bir diğer avantaj ise benim geçenlerde kullandığım özellikleri kullandırması yani T-SQL komutlarını kullanabiliyorsunuz ve if-else gibi while gibi işlemleri de gerçekleştirebiliyorsunuz.

Eğer birisi veritabanınıza erişim imkanına sahipse yapacak bir şey yok ama veritabanı erişimine sahip olmayıp bir şekilde uygulama katmanındaki kodlara erişimi olan kişiler sadece Stored Procedure ve o Procedure’ün aldığı parametreleri göreceği için güvenlik anlamında da avantaj sağlar.

Peki nasıl kullanabiliriz dediğinizi biliyorum. Aslında bir Stored Procedure çok zor da değil. Parametre alan ya da almayan olarak türleri vardır ve RETURN komutunu da kullanabilirsiniz. Basit olarak parametre alamayan bir Stored Procedure şöyle oluşturulur:

CREATE PROC ProcedureAdi şeklinde ya da CREATE PROCEDURE ProcedureAdi şeklinde olabilir. Ben genellikle PROC kullanıyorum. Şimdi parametre almayan ve sadece SELECT sorgusunu döndüren bir PROC yazalım:

```sql
CREATE PROC GetUsers
AS
    SELECT * FROM Users
```

Evet parametre almayan basit olan bir Procedure yazdık bile. Bu Procedure bize kullanıcıları listelemede yardımcı olacaktır. Hmm nasıl çağırıldığını yazmadık:/

EXEC komutunu kullanarak çalıştırma işlemini yaparız. Eğer Procedure parametre almıyorsa basit şöyledir:

```sql
EXEC GetUsers
```

Peki tamam parametreli olanlar nasıl? Herhangi bir programlama dilinde method oluşturduğunuzu düşünün ve bu metodların parametre aldığını düşünün. Aynı şekilde Procedure’ler de parametreleri böyle alırlar. Tek farkı ise parantezler içerisinde parametreler almak yerine normal şekilde almak:

```sql
EXEC GetUsers 'Ali'
```

Buradaki senaryo şöyle:

```sql
SELECT * FROM Users WHERE Name='Ali'
```

Bunu Stored Procedure ile şöyle yapabiliriz.

```sql
CREATE PROC GetSpesificUser
	@UserName nvarchar(30)
AS
    SELECT * FROM Users WHERE Name=@UserName

EXEC GetSpesificUser 'Ali'
```

Eğer birden fazla parametre alacaksa da virgülle belirtilmeli.

```sql
CREATE PROC GetSpesificUserWithID
	@ID int,
	@UserName nvarchar(30)
AS
    SELECT * FROM Users WHERE ID=@ID and Name=@UserName
```

Tabii bu Stored Procedure çalıştırılırken de alacağı parametre sırasına göre parametreleri, aldığı türe göre Stored Procedure’e yollamalıyız. Örnek:

```sql
EXEC GetSpesificUserWithID 1, 'Ali
```