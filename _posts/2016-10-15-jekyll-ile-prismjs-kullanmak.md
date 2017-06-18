---
layout: post
title: Jekyll ile PrismJS Kullanmak
tags: [jekyll, javascript, prismjs, syntax]
---

Blogu Jekyll’a taşıdıktan sonra uygun bir tema arayışına girmiştim. Bu uygun tema arayışında işimi en çok gören ise Pixyll teması oldu. Yapımcısı footer’da yer almakta. Neyse malumunuz kod içeren makaleler yazınca o makalelerdeki kodların da renklendirilmesi önemli.
<!--more-->
Kullandığımız temalara göre renklendirme özellikleri mevcut elbette. Ancak jekyll ile birlikte [Route](http://rouge.jneen.net/) varsayılan olarak geldiği için pek de arayışa geçmeyiz. Fakat benim gibi görselliği de seviyorsanız mutlaka farklı alternatifleri de düşünürsünüz ki bunlardan birisi de [Pygments](http://pygments.org/) oluyor. Tabi bunu da beğenmiyor olmanız pekala normal karşılanabilir.

Ben özellikle son dönemde en popüler kod renklandiricilerden olan PrismJS kullanmayı uygun gördüm. Bu yazıda ve diğer yazılarda etkisini göreceksiniz. Çok uzatmadan nasıl kurulacağını anlatayım. Şimdi bu işlem 3 adımdan oluşmakta.

    İlk Adım: Rouge’un devre dışı bırakılması
    İkinci Adım PrismJS’in İndirilmesi
    Üçüncü Adım İlgili Kütüphanelerin head ve footer’a eklenmesi

1-) Şimdi öncelikle bize lazım olan şey Rouge’un devre dışı kalması dedik. `_config.yml` dosyasına geçelim. Burada yapmamız gereken şey öncelikle `highlighter: none` demeliyiz.

Daha sonra kramdown için alt seçeneklere bazı eklemeler gerçekleştirmeliyiz. Bu işleme göre syntax highlighter disable olmalı. Kısacası ilgili bölümler aşağıdaki gibi olmalı:

```yaml
highlighter: none
markdown: kramdown
kramdown:
    syntax_highlighter_opts:
        disable: true
```

2-) İlk adımı atlattık şimdi geldi ikinci adıma. İkinci adımda PrismJS’yi [http://prismjs.com/](http://prismjs.com/) adresinden indirelim. Bu indirme ile birlikte iki dosya karşınıza gelecektir.Birisi JS bir diğeri de CSS dosyası.

Ben PrismJS’i özelleştirerek indirmenizi öneriyorum. Syntax highlighter için default tema aslında gayet iyi ama siz dilerseniz farklı temaları da seçebilirsiniz. Blogunuzda hangi dillerde yayın yapıyorsanız (Programlama dilleri) o dilleri seçin öncelikle. Ben hepsini seçmedim ama bazı deneyip bir şeyler aktaracağım dillere dair seçimler gerçekleştirdim.

Dil seçimi tamamlandı. Artık seçmemiz gereken bir diğer şey ise plugin yani eklentiler. Bunlar inanın PrismJS’i daha güzel bir şekilde kullanmanıza da imkan tanıyan eklentiler. Benim önerilerim şunlar:

**Line Numbers:** Satır numaralarını gösterir.

**Autolinker:** Kodlarda yer alan yorum satırlarında link varsa tıklanabilir hale getirir

**Show Language:** Eklenen kodun hangi dilde eklendiğini sağ üst köşede gösterir. Bu yazıdaki YAML örneğinde olduğu gibi

3-) Son adıma geldik. Bu adımda indirilen dosyaları klasörlere atma zamanı. css zaten hazır olduğu için `prism.css` dosyasını o klasöre atın. Eğer js klasörü yoksa oluşturun ve `prism.js` dosyasını da o klasöre atın.

Şimdi gelelim son adımlara. Öncelikle <mark>head.html</mark> dosyasını açalım. Ya da artık header görevini neresi görüyorsa orası. O kısma şu kodu yapıştıralım:

```liquid
<link rel="stylesheet" href="{% raw %}{{ "/css/prism.css" | prepend: site.baseurl }}{% endraw %}">
```

*Not: href içinde süslü parantezler vardır:*

```liquid
{% raw %}{{ "/css/prism.css" | prepend: site.baseurl }}{% endraw %}
```

Bu işlem bittikten sonra da artık son bir işimiz kalıyor. <mark>footer.html</mark> dosyasını açıp şu kodu yapıştıralım:

```liquid
<script src="{% raw %}{{ "/js/prism.js" | prepend: site.baseurl }}{% endraw %}"></script>
```

Yine yukarıdaki uyarı aynı bu kısımda da geçerlidir. Dikkate almanızı öneririm. Evet işlemler bu kadar :) Artık PrismJS hazır durumda.