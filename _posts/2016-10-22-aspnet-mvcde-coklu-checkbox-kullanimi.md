---
layout: post
title: Asp.Net MVC'De Çoklu Checkbox Kullanımı
tags: [asp.net, asp.net mvc, tips]
---

Merhaba. Asp.NET ve özellikle MVC konusunda henüz yeniyim. Aslında MVC’den önce Asp.NET hiç kullanmadım. Ancak iş olunca konu direkt MVC’ye daldım. Zorluk çektiğim pek söylenemez. Model kavramına aşina olduktan sonra pek sıkıntı yok.
<!--more-->
Sıkıntı şu. Veritabanından gelen bazı değerler var. Bu değerlerin de ID değerleri var. Örnekte şöyle olsun:

```html
<input type="checkbox" name="degerler" value="1" />
<input type="checkbox" name="degerler" value="2" />
<input type="checkbox" name="degerler" value="3" />
<input type="checkbox" name="degerler" value="4" />
<input type="checkbox" name="degerler" value="5" />
<input type="checkbox" name="degerler" value="6" />
```

Bunların bazıları seçili olabilir bazıları olmayabilir. Şimdi ben gelen değerleri veritabanına ekletmek istiyorum. Belki başka yöntemleri de vardır bilemem ancak bunun MVC’de basit bir yolu var. Şimdi bu formun basit olarak şöyle post edildiğini düşünelim:

```html
<form action="/Home/DegerAl" method="post">
    <input type="checkbox" name="degerler" value="1" />
    <input type="checkbox" name="degerler" value="2" />
    <input type="checkbox" name="degerler" value="3" />
    <input type="checkbox" name="degerler" value="4" />
    <input type="checkbox" name="degerler" value="5" />
    <input type="checkbox" name="degerler" value="6" />
    <input type="submit" value="Ekle" />
</form>
```

Yukarıdaki yapıda name karşılığı <mark>degerler</mark> olan bir input grup görüyoruz. Bu name karşılıklarının aslında bir name array oluşturduğunu söyleyebiliriz. Bu array’in adı da <mark>degerler</mark> array’i olsun. O zaman C# tarafında bu değerleri hangi türden alacağımızı biliyoruz.

`[HttpPost]` attribute’unu çağıran method’a gelen argüman tipi array olacak. Basit olarak array tipli değer alan methodların nasıl oluştuğunu hatırlayalım:

```csharp
public void DegerAlacak(string[] degerler)
{
    //...
}
```

Şimdi bu bilgilere göre basit anlamda bir checkbox list’ten gelen verinin nasıl alınacağını görelim:

```csharp
[HttpPost]
public ActionResult DegerAl(string[] degerler)
{
    for (int i = 0; i < degerler.Length; i++)
    {
        ViewData["Degerler"] += degerler[i];
    }
    return View();
}
```

Ben bunları `DegerAl` sayfasında yazdırdığım için ViewData içerisine atadım. Dilerseniz parametre alınan noktaya bir breakpoint koyarak gelen değerleri görüntüleyebilirsiniz.

![/images/posts/mvc_check.png](/images/posts/mvc_check.png)