---
layout: post
title: Atıştırmalık MongoDB Yazısı
tags: [MongoDB, NoSQL, Tips]
---

Selam bu yazı MongoDB’ye yeni giriş yapmış olan ben ve benim gibiler için atıştırmalık bir yazı. Bu yazıda basit olarak insert ve select işlemlerinden bahsedeceğim. MongoDB nedir gibi artık klişe hale gelmiş bilgileri paylaşmak yerine direkt olarak nasıl yapılır’ı anlatacağım.

**[NOT]:** Yazı uzun bir yazı olmuş. Şimdiden özür dilerim :)

<!--more-->

![/images/posts/mongo.png](/images/posts/mongo.png)

**Online MongoDB Ortamıyla Tanışın**

Pratiğe giriş yapmadan önce MongoDB bilgisayarınızda kurulu olmalı. MongoShell olmadan bilgisayarınızdan bağlantıyı kurmanız mümkün değil. Online shell kullanırsanız ayrı. Kurulum aşamalarını geçtiniz varsayarak size MongoLaB sitesini önereceğim.

[https://mlab.com](https://mlab.com) Bu adrese girip kaydolarak ücretsiz 500 MB MongoDB hakkı kazanıyorsunuz. Tabii ki online çalışmak zorunda değilsiniz bu bir alternatif.

Kaydolduktan sonra bir veritabanı oluşturmalısınız ve bu veritabanı için bir kullanıcı da oluşturmanız gerekiyor ki buraya kadar olan kısım standart yapıyla aynı. Her şeyi doğru yaptıysanız çalışma alanınızın görüntüsü şöyle olmalı:

![/images/posts/mongolab.png](/images/posts/mongolab.png)

**MongoDB’yi Local Ortamda Kullanmak**

Eğer MongoDB’yi herhangi bir sınırlama olmaksızın (500 MB gibi) local ortamda kullanmak isterseniz öncelikle komut satırına mongod komutunu yazmalısınız. Bu sayede MongoDB sunucusu başlatılır. Varsayılan port olarak 27017 dinlenir. Bunu söyleme sebebim yukarıdaki ilk seçenekte varsayılan port farklı.

![/images/posts/mongod_local.png](/images/posts/mongod_local.png)

Daha sonra ikinci bir komut satırını açarak mongo komutunu verip local mongodb kullanımına başlayabilirsiniz.

![/images/posts/mongo_local.png](/images/posts/mongo_local.png)

**Veritabanını Seçelim**

Veritabanını seçme işlemi, mongoshell üzerinde diğerlerinde olduğu gibi use ile olacaktır. Localde çalışanlar için test veritanı şöyle seçilir:

```sql
use test
```

**Ufaktan Insert Operasyonları**

Veritabanını seçtik. MongoDB kullanırken insert operasyonları, RDBMS’lerden farklı şekilde çalışabilir. Burada yaşayacağınız farklılık syntax anlamında olabilir. Ancak NodeJS kullanıcılarının bu işlemlere karşı yabancı olacağı pek söylenemez :)

MongoDB’de insert operasyonları üç metod altında toplanmıştır. Bunlar sırasıyla;

`insert()`, `insertOne()` ve `insertMany()` metodlarıdır. Bu metodları kullanırken verilerin JSON türünden olduğuna dikkat etmelisiniz. Yani JSON’a da aşina olmanız gerekmektedir. Neyse ki öğrenmesi çok da zor değil.

**Basit Olarak Insert**

Basit olarak bir insert operasyonu şöyle olur.

DBNESNESİ.TABLO.INSERT bu sahte yöntemin gerçeğe dönüşmüş hali şöyledir:

```javascript
db.Tablo.insert({
    'alan': 'Verisi',
    'alan2': 'Baska Veri'
})
```

O zaman bir blog sisteminin yapısından yola çıkarak şöyle olacak diyebiliriz:

```javascript
db.Yazilar.insert({
    'Baslik': 'Blog Başlığı',
    'Icerik': 'Lorem Ipsum Dolor Sit Amet...',
    'Kategori': 'Kişisel'
})
```

Evet klasik bir insert operasyonunu gördük. Bu kodları Mongoshell üzerinde çalıştırınca şu çıktıyı alırsanız her şey doğrudur:

```json
WriteResult({ "nInserted" : 1 })
```

**Nedir Bu insertOne() ?**

Bilinen bir yanlıştan dolayı insert ve insertOne karıştırılmakta. insert sadece tek bir ekleme yapmaz. Eğer sadece insert metodunu kullanıyorsanız birden fazla ekleme de yapabilirsiniz. Örnek:

```javascript
db.Yazilar.insert([
    {"Baslik": "Başka"},
    {"Baslik": "Oteki"}
])

/*
ÇIKTISI:

BulkWriteResult({
        "writeErrors" : [ ],
        "writeConcernErrors" : [ ],
        "nInserted" : 2,
        "nUpserted" : 0,
        "nMatched" : 0,
        "nModified" : 0,
        "nRemoved" : 0,
        "upserted" : [ ]
})

*/
```

Ancak insertOne metodu böyle değil. Sadece ve sadece bir adet ekleme gerçekleştirebilirsiniz. Ayrıca insertOne metodu ile insert edilen içeriğin ID’sini de alabilirsiniz. Kullanımı şöyledir:

```javascript
db.Yazilart.insertOne({
    "Baslik": "Tek Başlık"
})
```

Yani ikinci bir başlık ekleme işlemi yapamazsınız. Evet bu kadar basit.

**Gelelim insertMany() Metoduna**

insert ve insertMany metodları da çoklu ekleme işlemlerini destekler. Ancak insert metodu ile yapılan ekleme işlemlerinde, geriye bir insertedId dönmezdi. Aynı işlemi insertMany ile yaparsak bir ID dönüşü alırız:

```javascript
db.Yazilar.insertMany([
    {"Baslik": "Başka"},
    {"Baslik": "Oteki"}
])

/*
ÇIKTISI:

{
        "acknowledged" : true,
        "insertedIds" : [
                ObjectId("5855b0a5f7e668b0e155210f"),
                ObjectId("5855b0a5f7e668b0e1552110")
        ]
}
*/
```

**Şimdi Bu Verileri Nasıl Çekeriz?**

Veri çekme işlemi MongoDB üzerindeki en basit işlemlerdendir. Örneğin Yazilar tablosundan verileri çekelim. Bunun için find() metodunu kullanırız:

```javascript
db.Yazilar.find()

/*
ÇIKTISI:

{ "_id" : ObjectId("5855aef5f7e668b0e155210c"), "BAŞLIK" : "İçerik" }
{ "_id" : ObjectId("5855aef5f7e668b0e155210d"), "BAŞLIK BAŞKA" : "İçerik 2" }
{ "_id" : ObjectId("5855afd0f7e668b0e155210e"), "ÖTEKİ" : "İçerik 3" }
{ "_id" : ObjectId("5855b0a5f7e668b0e155210f"), "FALAN" : "İçerik 4" }
{ "_id" : ObjectId("5855b0a5f7e668b0e1552110"), "NEYSE" : "İçerik 5" }
*/
```

**Nerede Bu WHERE Clause?**

Benim en çok merak ettiklerimden birisi de bu konuydu. MongoDB üzerinde where aynı mantığa sahip olsa da kullanımı tabii ki farklı. Yine find() metodunu kullanacağız.

```javascript
db.Yazilar.find({ $where: "this.Baslik == 'FALAN'" })
```

Yukarıdaki kullanımla tek satırlık bir değer dönecektir. Ancak aynı değere sahip 2 içerik varsa onları unique yapan diğer değerleri de sorgulamanız iyi olur.

**Hadi Bu İçerikleri Sıralayalım**

ASC ve DESC olaylarını RDBMS üzerinde kullanırken ORDER BY kullanıyorduk. Ancak bunda maalesef ki böyle değil. Fakat daha basit olduğunu söyleyebilirim. Yine find() metodunu olaya dahil ederek şöyle bir işlem yapabiliriz:

```javascript
db.Yazilar.find().sort({_id:1})
```

Yukarıdaki yöntemle birlikte ASCENDING bir sıralama yapabilirsiniz. Bunun tam tersi olarak DESCENDING işlemi ise şöyle yapılır:

```javascript
db.Yazilar.find().sort({_id:-1})
```

**Limit Falan Nasıl Olacak?**

Şimdi bu işlemde de find() metodunu kullanıyoruz. Tabi bu sefer sort() metodu da bize katılıyor. Limit işlemini limit() metodunu kullanarak yaparız. Örneğin kullanımı şöyle:

```javascript
db.Yazilar.find().sort({ID:-1}).limit(1)
```

Yukarıdaki yöntem bize son eklenen verinin içeriğini döndürecektir ve dönen değer tek satırdır. Çünkü limit metodu parametre olarak tek 1 rakamını almıştır. Dilersek dönen değerdeki tek bir alana dair de veri çekebiliriz. Ben bu işlemde kendim ID adında bir alan tanımladım. MongoDB tarafından üretilen _id alanından farklı olduğunu belirteyim. Şimdi tanımlı olan bu alanı çekelim:

```javascript
db.Yazilar.find().sort({ID:-1}).limit(1)[0]["ID"]
```

Buradan anlaşılacağı üzere eklenmiş olan verilerden en sonuncusu getirilip bu satırdaki ID alanında yer alan veriyi göreceğiz. Klasik bir array access işleminden fazlası değil yani.

Evet anlatım bu kadar. Uzun bir yazı dizisi oldu. Eğer CRUD işlemleri hakkında daha fazla bilgi istiyorsanız [MongoDB Manual](https://docs.mongodb.com/manual/crud/) sizin için iyi bir yol gösterici olacaktır.