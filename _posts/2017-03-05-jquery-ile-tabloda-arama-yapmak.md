---
layout: post
title: jQuery ile Tabloda Arama Yapmak
tags: [JavaScript, jQuery, Tips]
---

Selamlar. Bu yazıda sizlere çoğunuzun kullanmış olabileceği bir eklentinin daha basit halini anlatacağım. [jQuery Datatables](https://datatables.net/) eklentisi muazzam iş gören bir eklenti.
<!--more-->
![/images/posts/ridvan.jpg](/images/posts/ridvan.jpg)
*Çizerken baya zorlandım*

Ancak bazı durumlarda gerekmeyebilir. Bu gibi durumlarda gereksiz eklenti kullanımı performansa da etki ediyor haliyle. Elimizde basit olarak bir tablo yapısının olduğunu varsayalım. Tabi arama kutumuz da o basit yapının içinde olsun :)

```html
<input type="text" class="search" placeholder="Ürün Arama...">
<span class="counter"></span>
<table class="results" cellspacing="0" width="100%">
    <thead>
        <tr class="active">
            <th>Ürün Adı</th>
            <th>Miktar</th>
            <th>Birim Fiyat</th>
            <th>Tutar</th>
            <th>Personel</th>
            <th>İşlemler</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Patates</td>
            <td>10</td>
            <td>15</td>
            <td>150</td>
            <td>Ali</td>
            <td>Sat</td>
        </tr>
        <!-- Buralar eskiden TR'likti -->
    </tbody>
    <tfoot>
        <tr class="warning no-result" style="display:none;">
            <td colspan="8"><i class="fa fa-warning"></i> Bulunamadı</td>
        </tr>
    </tfoot>
</table>
```

Senaryo bize der ki kardeşim, ben inputu her çağırdığımda sen tablodaki satırlar arasında arama gerçekleştir. Spesifik bir cell de belirtebilirdik ancak tüm cell’leri gezmesini düşünüyoruz. DataTables öyle çalışıyor çünkü.

Yukarıda `search` class’ına sahip inputu yakalayıp, keyup event’ini kontrol etmemiz gerekiyor. Bu kontrol gerçekleşirken de inputtaki değeri de almalıyız. Tabi bulunanların sayısını tutacağımız bir değişkene de ihtiyacımız var.

```javascript
$(".search").keyup(function() {
    var searchTerm = $(".search").val();
    var bulunan = 0
    //...
})
```

Buraya kadar her şey basit bir select işlemi. Bundan sonrasında ise tabloda kontrolü sağlamalıyız.

Eğer tablonuz herhangi bir şekilde `thead` ya da `tfoot` elementlerine sahip değilse direkt tablodaki tr elementlerini yakalayabilirsiniz.

Ama benim gibi tablonun gerçek kısmı olan tbody kısmıyla çalışıyorsanız mutlaka bunu belirtmelisiniz.

Tabloda kontrolü her tr elemanını tbody altında arayacak şekilde gerçekleştirmeliyiz. Yani tbody altındaki her tr’yi gezeceğiz. Ayrıca tabi tablonun aramasını yaptığımız kısmını da bir değişkene atayalım ki her defasında $(this) yazmaktan kurtulalım :)

```javascript
$('.results tbody tr').each(function(e) {
    var table = $(this)
    //...
})
```

Tamam şimdi kalıyor tek bir kısım. Bu son kısımda gezilen satırın text’ini alıyoruz ve `toLowerCase()` metodunu kullanarak satırdaki tüm textleri küçültürüyoruz.

Ardından da **ES6** ile gelen `includes()` metodunu kullanarak searchTerm’deki değeri içerip içermediğini kontrol ediyoruz. Burada searchTerm ise bizim inputtan gelen değer.

```javascript
if (table.text().toLowerCase().includes(searchTerm.toLowerCase())) {
    bulunan += 1
    table.show()
    $(".counter").text(bulunan + " kayıt bulundu")
    $(".no-result").css('display', 'none')
} else {
    table.hide()
    $(".counter").text(bulunan + " kayıt bulundu")
    if (bulunan == 0) {
        $(".no-result").css('display', '')
    }
}
```

Yukarıda eğer gezilen satırda, aranan değere dair bir şeyler yer alıyorsa gezilen satırı aktif et diyoruz. Bu cümle bize, diğer satırların kaybolacağı anlamını veriyor ki öyle de olacak.

Tabi `bulunan` isimli değişkenin değerini 1 arttırıp `counter` class’ına sahip span etiketinin textine ekliyoruz. Böylece kaç sonuç bulunmuş kullanıcıya sunabiliriz.

Son olarak da eğer `no-result` class’ı aktif durumdaysa onu kapatalım ki çünkü sonuç bulunmuş durumda.

Diğer şart ise sonuç bulunamazsa yani bulunan değişkenindeki değer 0’a eşitse no-result’ı aktif eder. Onun haricindeki durumlarda tablodaki tüm satırları sayar ki o da kaç satır varsa onu verir.

## Çalışan Örnek:

<script async src="//jsfiddle.net/aligoren/od3d139L/embed/"></script>