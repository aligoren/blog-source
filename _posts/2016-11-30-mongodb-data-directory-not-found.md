---
layout: post
title: MongoDB Data Directory Not Found Hatası
tags: [MongoDB, NoSQL, Tips]
---

Selam. MongoDB öğrenmeye başladım. Ufak bi kaç adımla başlayacaktım. Hedefte o vardı. Ancak daha henüz ilk kurulumda şöyle bir hata ile karşılaştım:
<!--more-->
```
"Data directory C:\data\db\ not found., terminating"
```

Eğer böyle bir hata alıyorsanız MongoDB kurulduktan sonra C: dizininde ya da bulunduğunuz sistemin belirtilen dizininde data klasörü ve onun içerisine db klasörü oluşturulamamış demektir.

Bu durumda yapılacak iki şey vardır. Birincisi oluşturulamayan data path’i siz oluşturacaksınız.

C: dizini altına kendiniz bir data klasörü ve içine de db klasörü oluşturun.

Ardından mongod komutunu vererek mongodb’yi çalıştırın. Bir diğer yöntem ise data path flag’ini kullanarak data yolu belirtebilirsin.

Ben mongodb 3.4 kullanarak şöyle yapıyorum

```
mongod --dbpath "C:\Program Files\MongoDB\Server\3.4\data\db"
```

Artık mongodb başarılı bir şekilde çalışacaktır.