---
layout: post
title: Jekyll Github Badge Eklentisi
tags: [jekyll, github, jekyll plugin]
---

Selam bu aslında tam bir eklenti. Değil. Buna daha çok Liquid Tag desek yanlış söylemiş olmayız. Bu eklentinin yaptığı şey kısacası belirli bir repo için <mark>BADGE</mark> oluşturuyor. Tam badge değilse de yapıyor işte. Bunu oluştururken bootstrap 4’ün card özelliğinden faydalandım.
<!--more-->
Eklenti şu anda sadece çağırıldığı anda olan değerleri üretiyor. Yani bugün 10 olan bir beğeni, yarın 15 olsa bile değişmeyecektir. Vakit olursa bir sonraki sürümde onu da değiştirmeyi düşünüyorum.

Kısacası bu eklenti bize şunları sunuyor:

    Proje Adı
    Açıklama
    Proje yazarı / sahibi (Author)
    Programlama dili
    Stargazers (kaç kişi beğenmiş)
    Forks (Çatallama sayısı)
    Download (Projenin master branchteki dosyalarının zip URL’si)

Kullanım için bazı adımlar var. Öncelikle card.css dosyasını aktarmak gerekiyor:

```liquid
{% raw %}{{ "/css/card.css" | prepend: site.baseurl }}{% endraw %}
```

Daha sonra da kullanımı basitçe şöyle oluyor:

```liquid
{% raw %}{% gitrepo KULLANICI_ADINIZ/REPO_ADINIZ %}{% endraw %}
```