---
layout: post
title: jQuery ile Dinamik İşlemler
tags: [JavaScript, jQuery, Programming]
---

Selam. Bu yazının içeriğini Tayfun Erbilen’in soru-cevap platformu olarak açtığı UzmanCevap üzerinde yer alan bir [sorudan](http://uzmancevap.org/soru/7644/jquery-kosulu-renklendirme) çıkarıyorum.
<!--more-->
Sorun şu. Bir arkadaşımız HTML tablo oluşturmuş. Muhtemelen endüstriyel bir sistem var. Bu sistemde kapasitelerin ölçümünü yapıyor. Kapasitesi belli bir alanın üzerinde olana da renk vermek istiyor. Ama burada işlemin jQuery ile nasıl yapılacağı ile ilgileniyor. Normalde bunu back-end kısımda sorguyu çekerken de halledebileceğinizi söylemem gerekiyor.

Sorunun özüne dönersek ilk satırda bir adet robot bulunmakta. Bu robota dair üç adet kapasite birimi bulunmakta. Arkadaşımız diyor ki kapasitesi 12’yi geçen hücrenin arkaplanı kırmızı renk olsun. Aslında kendisi zaten bir adet hücreyi yapmış. Fakat o tüm hücrelerin hesaplanmasını ve aslında tüm satırların da bu hesaba dahil olmasını istiyor.

Öncelikle basit olarak tablomuzu şöyle size aktarayım kodları da yer alsın:

```html
<table>
  <thead>
    <th>Ad</th>
    <th>Kapasite</th>
    <th>Kapasite 2</th>
    <th>Kapasite 3</th>
  </thead>
  <tbody>
    <tr>
      <td>Robot1</td>
      <td class='kapasite'>13</td>
      <td class='kapasite'>12</td>
      <td class='kapasite'>11</td>
    </tr>
    <tr>
      <td>Robot2</td>
      <td class='kapasite'>13</td>
      <td class='kapasite'>13</td>
      <td class='kapasite'>13</td>
    </tr>
    <tr>
      <td>Robot3</td>
      <td class='kapasite'>4</td>
      <td class='kapasite'>18</td>
      <td class='kapasite'>21</td>
    </tr>
    <tr>
      <td>Robot4</td>
      <td class='kapasite'>5</td>
      <td class='kapasite'>21</td>
      <td class='kapasite'>11</td>
    </tr>
  </tbody>
</table>
```

**Tablo Örnek**

![/images/posts/robot_table.png](/images/posts/robot_table.png)

Neyse bu tabloda belli başlı alanlar kurallı olarak renklendirilmeli. Arkadaşımızın kuralı 12’den büyük olanların arkaplanı kırmızı olsun şeklinde. Ben tüm hücrelere class vererek tek bir noktanın kontrolünün sağlanmasını istedim. Her bir satırda bulunan her bir hücrenin kontrolünü de sağlayarak bu işlemi tamamlayalım.

**jQuery Kodu**

```javascript
$("table").ready(function() {
  	$(this).find("tr td.kapasite").each(function() {
    	if(parseInt($(this).text()) > 12) {
      	$(this).css("background-color", "red")
      }
    })
    
  })
```

Yukarıdaki kodla tablo hazır olduğu anda hazır olan tabloda dolaşıyoruz. Öncelikle o tabloda tr yani satırı yakalıyoruz. Ardından o satırda yer alan hücreyi dolaşıyoruz tabi bunu yaparken ana şartıız ise class’ı kapasite olanı dolaşıyoruz. Tabloda 50 satır varsa 50 satırı ve hücrelerini dolaşacak kısacası. Bu işlemi `each()` fonksiyonu ile yapıyoruz.

Daha sonra ilk dolaşılan satırdaki ilk hücrede yer alan değerin text’ini int türüne parse edip arkadaşımızın istediği koşuldan büyük mü kontrol ediyoruz. Zaten sonrası ise o hücreye yeni bir CSS atamak. Aynı işlemi buton için de yapabiliriz. Yani butona tıklayınca bu işlem gerçekleşmeli. Tabii bu işlemde bu sefer değer inputtan gelecek.

```javascript
$("button").on("click", function() {
      $("table").ready(function() {
      var kapasite = $(this).find("tr td.kapasite").each(function() {
      	$(this).css("background-color", "")
        if(parseInt($(this).text()) > parseInt($("[name='txtSayi']").val())) {
          $(this).css("background-color", "red")
        }
      })

    })
  })
```

## JSFiddle Üzerinde Örnek

<script async src="//jsfiddle.net/cc6ded5w/1/embed/"></script>

Evet işlem bu kadar.