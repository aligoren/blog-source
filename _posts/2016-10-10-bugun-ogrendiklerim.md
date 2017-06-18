---
layout: post
title: Bugün Öğrendiklerim - Git
tags: [git, svn, github]
---

Bugün enteresan bir şekilde güzel şeyler öğrendim. Aslında dün de öğrendim. Github’ı sadece ve sadece master’a dosya push etmek adına kullanan birisi olarak şimdilerde branch ne kadar da gerekli der oldum. Bu yazıda özellikle branchleri anlatmak isterim. Çünkü master ve branchler gerçekten çok önemli şeyler.
<!--more-->

**Yeni Bir Git Reposu:**

Bulunduğunuz dizini içereceğini bilerek `git init` komutunu vermeniz yeterlidir.

**Uzak Git Reposunu Clonelamak**

Bu en çok ihtiyaç duyduğumuz şeylerden birisi olabilir. Farklı amaçlarla bunu yaparız. Çoğu zaman dosyaları direkt indirebiliriz. Ancak neden tarayıcıyı açalım ki? Tabii bir de projelere katkıda bulunmak da var. Neyse bu işlem şöyle oluyor:

`git clone https://uzak-git-deposu`

Şaka değil bu kadar basit.

**Dosya Ekleme**

Yapılan çalışmaların artık ortak çalışma alanına aktarılması gerekmektedir. Öncelikle dosyaların eklenmesi şarttır. İki yöntem var.

**Tamamını Ekleme:**

`git add *`

**Spesifik Dosyalar Ekleme:**

`git add main.rb lib.rb file.rb`

Yöntemlerden dilediğinizi seçebilirsiniz ki bu size kalmış bir şey.

**Eklenen Dosyalara Mesaj Eklemek**

Bu yöntemle hangi dosyalar çalışmada etkilenmiş yani değişikliğe uğramışsa o dosyalar bu mesajla repoya gönderilir. Örneğin şöyle düşünelim, info.py dosyasında bir değişiklik yaptınız. Bu değişiklik Person adında yeni bir class’ın eklenmesi olabilir. O zaman commit’e şu işlemi yapabilirsiniz:

`git commit -am "Add person class"`

**Değişiklikleri Yayına Almak**

En önemli mevzu bu bence. Herkesin katkı yaptığı bir projeye localden katkı yapmanız bir şeyi değiştirmez. Önemli olan bu katkının asıl repoya & sizin kendi reponuza aktarılmasıdır. Eğer reponuz yeni ise varsayılan olarak master isimli branch’e sahipsinizdir. Bu arada branch dal ya da dallanmak demekmiş. Neyse değişiklikleri yayına almak yani uzak repoda göstermek için şu komutlar işimizi görebilir:

`git push origin master`

Mümkün olduğunca master branch rahatsız edilmemeli. Oraya publish edilen yapı aktarılmalı. Geliştirme aşamalarında kullanılan kalabalık dosyalar başka branchlerde yer almalı.

**Hangi Branch’e Bağlıyım?**

Bu da önemlidir. Olur da farklı bir branch’te çalışırsınız aman diyim. Bunun için `git branch` komutu yeterlidir. Bu komutla şu anda hangi branch’te çalıştığınızı görebilirsiniz.

**Yeni Branch Oluşturma**

Bu işlem sanırım en zevkli işlem. Basit bir komutla dev isimli yeni branch oluşturabilirsiniz:

`git checkout -b dev`

**Branchler Arasında Dolaşma**

Eğer yapacağınız değişiklikler belirli bir branchte yayınlanacaksa o branch’e geçiş yapmanız gerekiyor. Bunun için de işlem yine çok basit. Örneğin şu anda master branch’tesiniz ve dev isimli branch’e geçeceksiniz:

`git checkout dev`

**Oluşturulan Branch’i Yayınlamak**

Eğer branch, uzak repoya gönderilmediyse o branch sizden başkası için görülebilir olmayacaktır. Branch’i yayınlama işlemini yukarıda oluşturduğumuz dev isimli branch’i baz alarak gerçekleştireceğiz.

`git push origin dev`

Artık branch’imiz yayında olacaktır.

**Güncelleme**

Siz bi uyudunuz falan sabah oldu işe gittiniz geldiniz o da ne bir sürü değişimler oldu. Ama bunlar bilgisayarınızda yok. Meraklanmayın. Basit bir komut işinizi görüyor:

`git fetch & git pull origin master`

**İşime Yarayan Komutlar:**

Yapılan işlemlere dair logları `git log` yazarak alabiliyorum. Eğer yapılan değişikliklere dair bir durum alacaksam da `git status` yaparak eklenmemiş olanları görebilirim.

Bugün daha çok şeyler öğrendim bunda @hwclass hocama teşekkür etmem gerekiyor. Onlar da başka bir yazının konusu olsun.