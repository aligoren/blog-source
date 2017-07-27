---
layout: post
title: JavaScript Notlarım
tags: [JavaScript, Vue.JS, Flask, Tips]
---

Bir süredir Vue.JS ile uygulamalar geliştiriyordum. Baktım ki uygulamalar artacak, kendi profilim altında toplamaktansa bir organizasyon altında toplamak daha mantıklı olur dedim.
<!--more-->

Şimdilik Vue kullanarak ortaya çıkan projeleri şuraya alıyorum: [https://github.com/vue-project](https://github.com/vue-project). Tabi bu süreç bana birçok şeyi de öğretmiş oldu.

## 1-) fetch API

Aslında videosunu çekmiştim ancak burada basit olarak bahsetmek gerekiyor.

fetch api, es6 ile artık hayatımızda ve browserlar tarafından da destekleniyor. xmlHTTPrequest yerine bunu kullanmak daha kolay geliyor ve tabii promise dönmesi de işimize gelir. XHR olayını anlatmayacağım ancak basit bir fetch api kullanımı şöyle oluyor:

```javascript
fetch('https://site.com').then((resp) => {
  return resp.json() // bazen json fonksiyonunu kullanmamıza gerek olmuyor.
}).then((obj) => {
  // return edilen json değeri burada gönlümüzce kullanırız.
})
```

### Metod Seçimi

Tabi burada metodlar ve post edilen değerler de bir o kadar önemli. İsteklerin sadece `GET` metodundan ibaret olmadığını biliyoruz. Eğer istersek metodu şöyle belirtebiliriz:

```javascript
fetch('https://site.com', {
  method: 'POST',
}).then((resp) => {
  // blah blahlar...
```

### Data Gönderimi

Fakat bu aşamada bile eksik bir şeyler var. Karşı tarafa değer göndermek ve bu değerleri formdata ya da json türünden yollamak. Bunun da yöntemi şöyle. Elimizde bir form olsun `#frmData` olarak select edelim.

```javascript
const frmData = new FormData(document.querySelector("#frmData"));

fetch('https://site.com', {
  method: 'POST',
  body: frmData
}).then((resp) => {
  // blah blahlar...
```

Bu direkt olarak FormData yollamanın bir opsiyonuydu. Dilersek JSON türünden ya da payload olarak data yollayabiliriz. Öncelikle JSON nasılmış ona bakalım:

```javascript
const frmData = new FormData(document.querySelector("#frmData"));

fetch('https://site.com', {
  method: 'POST',
  body: JSON.stringify({
    'username': 'Mahmut'
  })
}).then((resp) => {
  // blah blahlar...
```

Yine bunun payload türünden (jQuery Ajax metodunda bunu kullanıyordu) gönderimi de mevcut.

```javascript
const frmData = new FormData(document.querySelector("#frmData"));

fetch('https://site.com', {
  method: 'POST',
  body: 'username=Mahmut&password=1',
}).then((resp) => {
  // blah blahlar...
```

### Header İşlemleri

Kimi durumlarda headerlar lazım oluyor dediler. fetch api kullanırken Header sınıfı ile headerları da belirtebiliyoruz. Sınıf şart değil, dilersek obje gönderebiliriz.:

```javascript

const frmData = new FormData(document.querySelector("#frmData"));

const headers = new Headers({
  'Content-Type': 'application/json'
})

fetch('https://site.com', {
  method: 'POST',
  body: 'username=Mahmut&password=1',
  headers: headers,
}).then((resp) => {
  // blah blahlar...
```

## 2-) CORS Hataları ile Savaşın

Ben şu sıralar Vue.JS ile çalışıyorum. NPM üzerinden dev sürümünü ayağa kaldırınca sorun yok. Ancak endpoint IP ya da PORT'u farklı olunca bir sıkıntı oluşuyor. Ben bu hatayı alırken, fetch api ile çalışıyordum. Önerim de ona göre olacak. Neyse bununla ilgili hataları arasanız CORS'u görürsünüz. Muhtemeldir ki aldığınız hata şu cümleyi içeriyor:

> If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.

Merak etmeyin çözümü basit fetch api'nıza bir adet mode eklemeniz yeterli olacak. Yukarıdaki örnekten gidersek:

```javascript
fetch('https://site.com', {
  mode: 'no-cors',
  method: 'POST',
}).then((resp) => {
  // blah blahlar...
```

Şimdi sorunsuzca çalışabilirsiniz :)

## 3-) Promise Üzerinde Json Convert Daima Şart Değil

Garip oldu ama açıklamak isterim. Bir endpoint'e istekte bulundunuz. Endpoint size datayı return etti. Gelen datayı direkt olarak `return resp.json()` şeklinde döndürürseniz, JSON syntaxinden kaynaklı hatalar alabilirsiniz. Bu durumda bazen `.json()` gerekmeyebiliyor.

## 4-) Vue.JS Hakkında

Yazının en başında da belirttim. Öğrendiğim bazı şeyler var yok değil.

### Prop ve Data İsimlerine Dikkat

Bir componente prop atayacaksanız, o prop ve data'dan dönen objenin adı aynı olamaz. Olur da console'da kocaman bir warn görürsünüz. Örnek:

*Olmamalı!*

```javascript
export default {
  name: 'mycomponent',
  props: [
    'sample',
  ],
  data() {
    return {
      sample: '',
    }
  }
}
```

### Created ve Mounted Aynı Değil

Vue.JS'de çalışırken benim gibi yenilerin yapacağı bir hatadan bahsetmek isterim. Datayı aldığınız created anında her şeyin olacağını sanmak büyük bir yanılgıdır.

Bizim dataya dair işlemleri DOM üzerinde gerçekleştirmemiz için created değil mounted'da çalışmamız gerekiyor ki bunu [Adem İlter](http://ademilter.com/)'in Bricklayer kütüphanesinde deneyimledim. Örneklersek şöyle:

```javascript
created() {
  this.fetchTrends({
    limit: this.limit,
    offset: this.offset
  });
},
mounted() {
  this.brickLayerInit();
},
```

### Datada Değişimleri İzlemek

En zevkli kısım burası bence. A ve B isimli componentlere sahipsiniz.

A componentinde bir işlem sonucunda X isimli data vs. her neyse, B isimli componente property olarak geçiyor. Çok karıştırmamak adına devamını kodla getireyim.

**A Componenti**

```html
<template>
  <div>
    <b-componenti :sayi="anindatasi"></b-componenti>
    <button v-on:click="tiklaDataGitsin"></button>
  </div>
</template>
```
**Javascript Kodu:**

```javascript
//A componenti
export default {
  name: 'acomp',
  data() {
    return {
      anindatasi: 0,
    }
  },
  methods: {
    tiklaDataGitsin() {
      this.anindatasi += 1;
    }
  }
}
```

Yukarıdaki javascript kodu ve template kısacası şöyle çalışmakta:

Butona tıkla, tıklandığı anda `tiklaDataGitsin` isimli metodu çalıştır. Bu metod `anindatasi` isimli değeri 1 arttırsın. Ardından bu değer `<b-componenti></b-componenti>` içerisine paslansın.

**B Componenti**

O sıralarda B componenti ise şu vaziyetteydi.

```html
<template>
 <div>
  {% raw %}{{bdatasi}}{% endraw %}
 </div>
</template>
```

**Javascript Kodu:**

```javascript
export default {
  name: 'bcomponenti',
  props: [
    'sayi',
  ],
  data() {
    return {
      bdatasi: this.sayi,
    }
  }
}
```

### Çözüm watch Olabilir

Okey buraya kadar hata almadan çalışabiliriz. Fakat A componentindeki olayın, B componentindeki datayı tetiklemesi gerekiyor. Çünkü bu componentler aynı sayfa üzerindeler. Bunun için `watch` kullanabiliriz.

Bu vue.js'in sağladığı büyük bir avantaj. Kısacası bir proptaki değişimi izleyebilirsiniz. Başka noktalarda henüz deneyimlemedim. Ancak `sayi` isimli prop'un değişim yaşadığını görebiliriz. Bir örnekle bakalım:

```javascript
watch: {
  sayi() {
    this.bdatasi += 1;
  }
}
```

Template içerisine yazdırdığımız `{% raw %}{{bdatasi}}{% endraw %}` sonucunda değişiklikler yaşandığını da görebileceğiz demektir bu.