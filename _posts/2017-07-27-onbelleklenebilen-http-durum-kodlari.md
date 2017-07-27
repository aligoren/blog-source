---
layout: post
title: Önbelleklenebilen HTTP Durum Kodları
tags: [Web, HTTP, İnternet]
---

1 aydır yazmıyorum. Bu yazıda HTTP durum kodlarına değineceğim. Bu durum kodları önbelleklenebilir durum kodlarıdır. Çoğu HTTP durum kodunu zaten biliyoruz.
Kimileri buna HTTP hata kodları demeyi tercih ediyor.<!--more-->

![/images/posts/http200.jpg](/images/posts/http200.jpg)

Bu konuyu aslında [@heartsmagic](https://forum.ubuntu-tr.net/index.php?action=profile;u=3) ile başlattığımız bir thread üzerine yazma kararı aldım. Gerçekten faydalı birçok bilgiyi de öğrendim. Konuya girmeden önce konu [RFC 7231](https://tools.ietf.org/html/rfc7231) altında yayınlı tanımlamalara dayanan bilgiler içeriyor.

## Açıklamalar

Bazı HTTP durum kodları aksi belirtilmediği sürece varsayılan olarak önbelleklenmiş yanıtlar döndürürler. Ayrıca bir diğer açıklama ise kaynak sunucu daima önbellek süresinin ne zaman sona ereceğini size söylemez. Açık bir şekilde önbellek sonlanma zamanı belirtilmemişse heuristic(sezgisel) bir şekilde önbelleğe sonlanma zamanı atanıyor.

Önbelleğin ne zaman sona ereceği, [Last Modified](https://tools.ietf.org/html/rfc7232#section-2.2) gibi başlık üst bilgileri kullanılarak tahmin edilebiliyor. Yani eğer bilinçli olarak bir sona eriş tarihi yoksa, sezgisel olarak bi zaman atanıyor. Burada en önemli bilinmesi gereken **ZORUNLULUK** eğer HTTP önbelleklemesi açık bir şekilde tanımlanmamışsa, heuristic sonlanma zamanı atanabilir olmalı. Eğer HTTP önbelleklemesi belirtilmişse heuristic tanımlama olamaz.

## Durum Kodları

![/images/posts/http404.jpg](/images/posts/http404.jpg)

[Bölüm 6.1](https://tools.ietf.org/html/rfc7231#section-6.1) der ki;

  > 200, 203, 204, 206, 300, 301, 404, 405, 410, 414 ve 501

nolu durum kodları aksi belirtilmediği sürece varsayılan olarak önbelleklidir. Diğer tüm durum kodları varsayılan olarak önbelleklenebilir değildir.

### Durum Kodları

[200 OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)

  > Aksi belirtilmediği taktirde varsayılan olarak önbelleklidir [7234 - 4.2.2](https://tools.ietf.org/html/rfc7234#section-4.2.2)

[203 Non-Authoritative Information](https://tools.ietf.org/html/rfc7231#section-6.3.4)

  > Aksi belirtilmediği taktirde varsayılan olarak önbelleklidir [7234 - 4.2.2](https://tools.ietf.org/html/rfc7234#section-4.2.2)

[204 No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)

  > Aksi belirtilmediği taktirde varsayılan olarak önbelleklidir [7234 - 4.2.2](https://tools.ietf.org/html/rfc7234#section-4.2.2)

[206 Partial Content](https://tools.ietf.org/html/rfc7233#section-4.1)

  > Aksi belirtilmediği taktirde varsayılan olarak önbelleklidir [7234 - 4.2.2](https://tools.ietf.org/html/rfc7234#section-4.2.2)

[300 Multiple Choices](https://tools.ietf.org/html/rfc7231#section-6.4.1)

  > Aksi belirtilmediği taktirde varsayılan olarak önbelleklidir [7234 - 4.2.2](https://tools.ietf.org/html/rfc7234#section-4.2.2)

[301 Moved Permanently](https://tools.ietf.org/html/rfc7231#section-6.4.2)

  > Aksi belirtilmediği taktirde varsayılan olarak önbelleklidir [7234 - 4.2.2](https://tools.ietf.org/html/rfc7234#section-4.2.2)

[404 Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)

  > Aksi belirtilmediği taktirde varsayılan olarak önbelleklidir [7234 - 4.2.2](https://tools.ietf.org/html/rfc7234#section-4.2.2)

[405 Method Not Allowed](https://tools.ietf.org/html/rfc7231#section-6.5.5)

  > Aksi belirtilmediği taktirde varsayılan olarak önbelleklidir [7234 - 4.2.2](https://tools.ietf.org/html/rfc7234#section-4.2.2)

[410 Gone](https://tools.ietf.org/html/rfc7231#section-6.5.9)

  > Aksi belirtilmediği taktirde varsayılan olarak önbelleklidir [7234 - 4.2.2](https://tools.ietf.org/html/rfc7234#section-4.2.2)

[414 URI Too Long](https://tools.ietf.org/html/rfc7231#section-6.5.12)

  > Aksi belirtilmediği taktirde varsayılan olarak önbelleklidir [7234 - 4.2.2](https://tools.ietf.org/html/rfc7234#section-4.2.2)

[501 Not Implemented](https://tools.ietf.org/html/rfc7231#section-6.6.2)

  > Aksi belirtilmediği taktirde varsayılan olarak önbelleklidir [7234 - 4.2.2](https://tools.ietf.org/html/rfc7234#section-4.2.2)

### Post İsteğinin Önbelleklenmesi

[POST](https://tools.ietf.org/html/rfc7231#section-4.3.3) isteği açık bir şekilde belirtildiği taktirde önbelleklenebilir. Aslında istek değil, verilen yanıtlar. Bu karmaşayı en sonda ortadan kaldırmak iyi olur diye düşündüm.

Normalde POST isteğinden dönen yanıtın önbelleklenebilirliğinden çok fazla faydalanılmıyor. Bir post isteğine dönen yanıtın, bu istekten sonra gelecek get tarafından tekrar kullanılabileceği durumlarda nadiren de olsa bu tip önbelleklemeler kullanılabiliyor.