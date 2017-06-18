---
layout: post
title: SQL Server'da Filtreli Arama Gerceklestirme
tags: [sql, mssql, sql server]
---

Merhaba. Karmaşık bir arama işleminiz olabilir. Örneğin basit bir uygulama ile olaya bakalım:

Varsayalım ki InsanKaynaklari adında bir tablonuz bulunmakta. Bu tablo üzerinde bazı aramalar gerçekleştirmek istiyorsunuz. Örneğin bu aramalar
<!--more-->
Doğum Tarihi, Doğum Yeri, Cinsiyet, Medeni Durum, Maaş, Eğitim Düzeyi, Mesleki Belge ya da Meslek şeklinde olabilir. Normalde bir aramada tek şekilde şöyle bakabilirdik

```sql
SELECT * FROM InsanKaynaklari as Ik
WHERE Ik.MedeniDurum='Bekar' AND Ik.Cinsiyet='Erkek'
```

Bu sorgu ilk başlangıç olarak işimizi görüyor olabilir. Ancak bazı durumlarda sorgularımız sadece iki tane ile sınırlı kalmayabilir ve işin ilginç noktası bu sorguları örneğin C# tarafında gerçekleştiriyor olabiliriz. 6-7 adet textBox içinden sadece 3 tanesinin dolu olduğunu, diğerlerinin NULL gönderildiğini düşünün.

Bu gibi durumlar için bir Stored Procedure oluşturabiliriz. Bir Stored Procedure oluşturma aşamasını bildiğimizi varsayarak basit olarak bir SP oluşturalım. Örneğin:

```sql
CREATE PROCEDURE PersonelAra
	@dtarih date,
	@dyeri nvarchar(50),
	@cinsiyet nvarchar(5),
	@mdurum nvarchar(15),
	@maas nvarchar(50), -- bilinçli money yapmadım
	@eduzey nvarchar(50),
	@mbelge nvarchar(50),
	@meslek nvarchar(50)
AS
BEGIN

END
```

şeklinde bir hayali SP oluşturdum. Bu işlemde WHERE harici belki bize yabancı gelen bir method göreceğiz. COALESCE methodu. Bu metoda bakarsak eğer sınırsız sayıda parametre alabiliyor. Yaptığı işlemse ilk olarak NULL olmayan değeri geri döndürmektir. Eğer değer NULL ise zaten bu değeri geri döndürecektir.

Çalışma mantığına bakacak olursak öyle işlemektedir. Varsayalım ki WHERE kontrolü gerçekleştiriyoruz:

```sql
WHERE kolonAdi=COALESCE(@spDegiskeni, kolonAdi)
```

şeklinde bir kontrol ile null ya da null olmayan tüm değerleri listeletebiliriz. Bu bilgilere bakarak o zaman ben diyebilirim ki gelen her değişkeni COALESCE ile işletip null olmayan bir değer döndürebilirim. Eğer NULL ise yine gelecek diyebilirim.

Örneğin yukarıda oluşturduğumuz SP içinden devam edelim:

```sql
SELECT isim, soyisim, FROM InsanKanaklari as Ik
    WHERE (
        ik.dogumtarihi=COALESCE(@dtarih, ik.dogumtarihi)
        AND ik.dogumyeri=COALESCE(@dyeri, ik.dogumyeri)
        AND ik.cinsiyet=COALESCE(@cinsiyet, ik.cinsiyet)
        AND ik.medenidurum=COALESCE(@mdurum, ik.medenidurum)
        AND ik.maas=COALESCE(@maas, ik.maas)
        AND ik.egitimduzeyi=COALESCE(@eduzey, ik.egitimduzeyi)
        AND ik.meslekibelge=COALESCE(@mbelge, ik.meslekibelge)
        AND ik.meslek=COALESCE(@meslek, ik.meslek)
)
```

Burada öncelikle WHERE şartını gerçekleştirdik. Daha sonra ise WHERE şartında kolonun değerini, COALESCE işlemi ile karşılaştırıp değerleri getirebiliyoruz. COALESCE metodu ilk parametresinde sorgulanacak değeri, ikinci parametresinde de tablo kolonunu aldı. Bunu görerek filtreli sorgulama gerçekleştirebiliriz.

Son adımda SP şöyle oluyor:

```sql
CREATE PROCEDURE PersonelAra
	@dtarih date,
	@dyeri nvarchar(50),
	@cinsiyet nvarchar(5),
	@mdurum nvarchar(15),
	@maas nvarchar(50), -- bilinçli money yapmadım
	@eduzey nvarchar(50),
	@mbelge nvarchar(50),
	@meslek nvarchar(50)
AS
BEGIN
    SELECT isim, soyisim, FROM InsanKanaklari as Ik
        WHERE (
            ik.dogumtarihi=COALESCE(@dtarih, ik.dogumtarihi)
            AND ik.dogumyeri=COALESCE(@dyeri, ik.dogumyeri)
            AND ik.cinsiyet=COALESCE(@cinsiyet, ik.cinsiyet)
            AND ik.medenidurum=COALESCE(@mdurum, ik.medenidurum)
            AND ik.maas=COALESCE(@maas, ik.maas)
            AND ik.egitimduzeyi=COALESCE(@eduzey, ik.egitimduzeyi)
            AND ik.meslekibelge=COALESCE(@mbelge, ik.meslekibelge)
            AND ik.meslek=COALESCE(@meslek, ik.meslek)
    )
END
```