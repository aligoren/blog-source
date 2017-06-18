---
layout: post
title: Nginx 413 Request Entity Too Large Hatası ve Çözümü
tags: nginx tips server
---

Muhtemelen bu hatayı nginx’de siz de aldınız. Bugün localde çalışırken başıma gelen bir uyarı. Bu hatanın sebebi ya `client_max_body_size` tanımlı değildir ya da tanımlı ama boyut değeri çok düşüktür.
<!--more-->
Bende bu alan tanımlı değildi onun için bu alanı tanımlatmam gerekiyordu. Ubuntu için konuşacak olursak (Diğerlerinde de aynıdır yol. Denemedim):

`sudo nano /etc/nginx/nginx.conf`

komutunu verdim öncelikle. Daha sonra ise nginx’de http bloğu en üstte olduğundan oraya ekleyeceğiz:

```nginx
http {

        ##
        # Basic Settings
        ##
        ..
        ..
}
```

Aradığımız kısım burası. Burada şu şekilde bir değişim olacak:

```nginx
http {

        ##
        # Basic Settings
        ##
        client_max_body_size 2M;
        ...
        ...
}
```

Ben orada 2M olarak belirttim. Çünkü wordpress’in xml dosyasını yükleyecektim. 1.5 MB civarıydı boyutu. Ama siz orayı isterseniz daha da arttırabilirsiniz. Bu işlemden sonra şu komutu veriyoruz:

`sudo service nginx restart`

Bu komuttan sonra eğer hata almadıysak tekrar deneyebiliriz.