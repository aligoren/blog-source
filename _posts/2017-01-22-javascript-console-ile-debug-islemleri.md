---
layout: post
title: JavaScript Console ile Debug İşlemleri
tags: [JavaScript, Programming, Tips]
---

Selamlar. Front-end geliştiricilerin çok iyi bildiği bir konu console’dur. Bütün dillerde yapılabildiği gibi Javascript üzerinde de debug işlemleri yapılabilir.

Aslında debug işlemleri, programlamanın vazgeçilmez bir parçasıdır. Çoğu yazılımcı zamanının bir bölümünü bug ve sorunları düzeltmek amacıyla harcamıştır.
<!--more-->
Bu yazıda JavaScript console nesnesi ile debug işlemlerine dair ufak tefek ipuçları paylaşacağım. Bu basit bi tanıtım olacaktır. Daha derinlemesine bilgi için bahsi geçen yöntemlere göz atmanız daha iyi olacaktır.

## Assert İşlemi

Bu işlem içim `console.assert()` kullanmalısınız. `assert()` fonksiyonu mutlaka boolean türünden bir değer almalıdır. Aşağıdaki örnek tam da buna uygundur:

![/images/posts/assert_js.png](/images/posts/assert_js.png)

## Trace İşlemi

Javascript console’unda log bilgilerini ve bu bilgiler sonucu ortaya çıkan değerleri almak için çokca yöntem mevcuttur. Bunlardan birisi `console.trace()` olmakta.

![/images/posts/trace_js.png](/images/posts/trace_js.png)

Yukarıdaki işlem size aslında bir adet stack trace döndürecektir.

## Tablolar

Evet javascript console’u tablolara da izin vermektedir. Şaşırtıcı gibi ama olmasın :). Bu işlemi `console.table()` ile yaparız. Tabi bu işlem Object ve array türünden değeri bize sunar. Ayrıca bir de hoş bir tabloyu da önümüze getirir.

```javascript
console.table(
	[
		{
			dil: 'PHP', uzanti: '.php' 
		}, 
		{
			dil: 'Javascript', uzanti: '.js'
		}
	]
)
```

![/images/posts/table_js.png](/images/posts/table_js.png)

## Log Tutma

Javascript ile log mesajları oluşturabilirsiniz. Aslında full stack ya da front-end olun fark etmez mutlaka console’da değer döndürmek zorunda kalmışsınızdır. Çünkü bir alert asla ve asla size direkt bilgileri sunmaz.

Bu açıdan bakarsak console’un bize sunduğu loglar bizim için çok değerlidir. Yeri gelmişken kendi hazırladığım [ColorLog.js](https://github.com/aligoren/ColorLog.js) kütüphanesini kullanabilirsiniz. Log mesajlarını renkli olarak alırsınız.

```javascript
console.info("Bilgi Mesajı");
console.warn("Uyarı Mesajı");
console.error("Hata Mesajı");
```
Yukarıdaki log fonksiyonları ile işimizi görecek şekilde mesajlar üretebiliriz.

![/images/posts/log_js.png](/images/posts/log_js.png)

## Timer’lar

Zaman tutmak bazen önemlidir. Örneğin bir for döngüsünün ne kadar sürede tamamlanacağının bilgisini almak gibi basit bir işlem şöyle gerçekleştirilir.

```javascript
console.time("zamanTut");
for(i = 0; i < 100; i++) {
	// işlemler
}
console.timeEnd("zamanTut");
```

![/images/posts/timer_js.png](/images/posts/timer_js.png)

## Monitör Etme

Bazen fonksiyonları izlemek isteyebilirsiniz ya da bazı hareketlerin monitörlenmesini isteyebilirsiniz. O zamanlarda iki tür fonksiyon mevcuttur. İlki;

`monitor()` Fonksiyonu

Bu fonksiyonla bir başka fonksiyonu izlemeniz mümkündür. Örnek;

![/images/posts/monitor_js.png](/images/posts/monitor_js.png)

**monitorEvents() Fonksiyonu**

Bu fonksiyonla event’lar monitör edilebilir. Bu event’ları zaten hepimiz biliyoruz. Örneğin bu event’lar arasında click yer alıyor.

![/images/posts/monitorEvent_js.png](/images/posts/monitorEvent_js.png)

**Alıntı:** Carlos Cuesta [↩](https://carloscuesta.me/blog/debugging-with-the-javascript-console/)