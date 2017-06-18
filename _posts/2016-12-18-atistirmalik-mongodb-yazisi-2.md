---
layout: post
title: Atıştırmalık MongoDB Yazısı - 2
tags: [MongoDB, NoSQL, Tips]
---

Selam bu yazı bir önceki [Atıştırmalık MongoDB Yazısı](/atistirmalik-mongodb-yazisi)‘nın devamı niteliğinde bir yazı. Bu yazıda MongoDB ile Update ve Delete işlemlerini anlatacağım. Klasik SQL yöntemlerine göre farklılıklar içerdiğini önceki yazımda da belirtmiştim.

<!--more-->

![/images/posts/mongo.png](/images/posts/mongo.png)


**MongoDB’yi Dosyadan Okuyalım**

MongoDB için shell güzel bir araç. Ancak uzun Mongo sorgularında shell üzerinde girintiler ve diğer işlemler programlama anlamında pek de kolaylık sağlamıyor. MongoDB’yi geliştiren ekip böyle durumları tahmin ederek bir kolaylık sağlamış. Örneğin şöyle bir sorguyu shell ile yazdığınızı düşünün:

```javascript
use deneme;

db.x.insert({
	"kolon": "içerik",
	"kolon2": "içerik2",
	"kolon3": "içerik3",
	"kolon4": "içerik4",
	"kolon5": "içerik5",
	"kolon6": "içerik6",
	"kolon7": "içerik7",
	"kolon8": "içerik8",
	"kolon9": "içerik9",
});
```

Yukarıdaki sorgulama işlemlerini shell’de yapmak inanın gerçekten zahmetli bir işlem. Bunun için yukarıdaki kodları js uzantılı bir dosyaya kaydedin (Farklı uzantıları denemedim) ve komut satırından şu komutu verin: mongo < dosyaniz.js

Evet bu kadar :)

**Nasıl Yapılıyor Bu Update?**

Yukarıdaki X collection’ında yer alan içeriklerden "kolon4" e ait datayı değiştirelim:

```javascript
db.x.update(
	{
		"kolon4": "Yenilendi"
	}
)
```

İstersek şöyle bir sorgu ile kolon4’e ait yeni değeri görüntüleyebiliriz:

```javascript
db.x.find()[1]["kolon4"]
```

Tabii bu kesin bir yöntem olmayabiliyor. Bunu anlamanın en kolay olu koleksiyonu yazdırmanızdan geçiyor. Hem eski değer hem de yeni değer görülebiliyor. Tabii yaptığınız işlem sonucunda nInserted yerine nModified içinde bir rakam göreceksiniz.

Yine aynı şekilde updateMany ve updateOne metodlarını kullanarak çoklu ya da tekil güncelleme işlemlerini de yapabilirsiniz. Bu iki metoda değinmiyorum çünkü Many ve One ne işe yarıyor bunları önceki yazıda anlatmıştım. Kullanımdan kaynaklı farklılılar var sadece.

Bunlar haricinde yeni bir metodla karşılaşıyoruz. Bu metodun adı replaceOne() evet bu yeni metod, koşulda eşleşme gerçekleşirse gerçekleşen ilk eşleşmeye işlem yapar. Örneği bir SQL cümlesinden verelim:

```sql
SELECT * FROM Kullanicilar WHERE Adi='Ali'
```

Yukarıdaki satır birden fazla Ali kullanıcısını döndürebilir. Ancak replaceOne() metodu dönen bu değerlerden ilk olanını bulur ve güncelleme işlemini yapar. Örnek kullanımı şöyledir:

```javascript
db.x.replaceOne(
	{
		kolon: "icerik"
	},
	{
		kolon: "Yeni icerik"
	}
)

/*
ÇIKTISI:

{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
*/
```

İsterseniz db.x.find() diyerek çıkan sonucu görüntüleyebilirsiniz:

```json
{ "_id" : ObjectId("5856e7347db38a7560ec56ae"), "kolon" : "Yeni icerik" }
```

Güncelleme işlemleri sadece bu kadarla sınırlığı değil. Bu işlemleri siz verdiğim linklerden daha geniş bir şekilde öğrenebilirsiniz.

**Nasıl Sileceğiz?**

RDBMS kullananlar olarak önceki alışkanlıklarımızdan kopamıyoruz. MongoDB’de bu alışkanlıklardan kopabilmiş değil. Evet tahmin edildiği üzere silme işlemi destekliyor MongoDB. Ancak biraz farklı. Bunun için şu metodları bize sunuyor:

- remove()
- deleteOne()
- deleteMany()

**remove Metodu**

Bunlardan ilki olan remove() metoduyla tek seferde tüm koleksiyonu silebildiğimiz gibi koşul kullanarak koleksiyonda toplu silme de yapabiliriz. Burada bilmemiz gereken toplu silme yaptığımızdır.

Örnek herhangi bir koşula bağlı olmaksızın tüm x koleksiyonunu silen sorgu kodunu verelim:

```javascript
db.x.remove( {} )
```

Eğer bu silme işleminin bütün tablo geneline değil de sadece belirli değerlere yansımasını istiyorsak şöyle yapmalıyız:

```javascript
db.x.remove({ 
    "kolon": "Bir İçerik"
})
```

Artık bu sayede içinde Bir İçerik geçen yani tam eşleşme sağlayan tüm değerler x tablosundan silinecektir. Tüm dokümana buradan bakabilirsiniz.

**deleteOne() Metodu**

Yine geldik One’lı Many’li metodlara. Mantığı artık söylememe gerek yok. Sadece nasıl çalıştıklarını belirtip bu yazıyı noktalayacağım. Tamamını anlatmıyorum çünkü artık siz de ben de ufak bilgilere sahibiz ve MongoDB dokümanlarına erişebilir durumdayız :)

```javascript
db.x.deleteOne( { "_id" : ObjectId("5856e7347db38a7560ec56ae") } );

/*
ÇIKTISI:

{ "acknowledged" : true, "deletedCount" : 1 }
*/
```

Yukarıdaki işlemin klasik SQL sorgusu şöyle:

```sql
DELETE FROM Kullanicilar WHERE ID=2
```

**deleteMany() Metodu**

İstersek çoklu silme işlemlerini de yapabiliriz. Fakat buradan şunu anlamayalım. Örneğin ID’si 1,2,3,4,5,6 olanları tek tek yazacağız gibi bir şey anlamayalım. Burada aslında remove() metodundaki toplu silmenin bir benzerini yapabiliriz. Örnek:

```javascript
db.x.deleteMany( { "kolon" : "A Değeri" } );

/*
ÇIKTISI:

{ "acknowledged" : true, "deletedCount" : 1 }
*/
```

Yukarıda A Değeri sadece 1 adet olduğu için silindiğinde de deletedCount 1 değerini döndürmüştür. Eğer 10 adet aynı değere sahip kayıt olsaydı o değer 10 olacaktı.

## Kaynaklar

[Update İşlemleri - MongoDB Resmi Dokümanı](https://docs.mongodb.com/manual/tutorial/update-documents/#update)

[Delete İşlemleri - MongoDB Resmi Dokümanı](https://docs.mongodb.com/manual/tutorial/remove-documents/#delete-methods)