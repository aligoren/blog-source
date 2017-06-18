---
layout: post
title: OpenSUSE Güncellemesini Zypper ile Yapma
tags: opensuse linux
---

Öncelikle güncelleme depolarını tanımlayalım ve aktif edelim:

`sudo zypper repos --uri`

Daha sonra zypper ile depoları yenileyelim. Ve sonrasında update edelim:
<!--more-->
```bash
sudo zypper refresh

sudo zypper update
```

Güncelleme işlemi artık başlamıştır.

Repoların tanımlı olduğu yol ise burada:

`/etc/zypp/repos.d`

Kolay gelsin.