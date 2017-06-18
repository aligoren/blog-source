---
layout: post
title: PHP ile Base URL Almak
tags: [PHP, Tips, Programming]
---

Selamlar. Ufak tefek PHP projeleri ile mevcut bilgimi daha yukarı taşımayı hedefliyorum. Tabi her projede yaşanabilmesi muhtemel bazı sorunları ben de yaşadım. Bu sorunlardan birisi de base url sorunu.

Soruna dair cevabı Stackoverflow üzerinde arıyordum. Bir başlığa denk [geldikten](https://stackoverflow.com/questions/2820723/how-to-get-base-url-with-php/41407858) sonra oturup kodunu kendim yazmalıyım dedim.
<!--more-->
Sorun şu normalde `http://localhost` adresi bizim `base_url` adresimiz fakat proje bir alt klasörde yer alıyorsa o zaman sıkıntılar çıkabiliyor.

Çünkü `http://localhost/proje` böyle bir adrese sahip olabilirsiniz ve dahası bu proje yolunda çalışırken htaccess kullanarak diğer sayfaları sanki bir klasörmüş gibi gösterebilirsiniz. Bu nedenden dolayı da mutlak url’yi yakalamakta sorun yaşayabiliyorsunuz. Bunun birden fazla çözüm yöntemi var. Öncelikle en basit olanını yapalım:

### Çözüm 1:

Bu yöntemde, base_url methodu tanımlanır ve o yönteme statik url verilir:

```php
<?php
	function base_url() {
		return 'http://localhost/proje';
	}
?>
```

### Çözüm 2:

Bu yöntemde biraz programlama beceriniz önemli aslında. Ben trait kullandım, çünkü bu kod şu anki projemin bi parçası. Multiple olarak extends işlemi yapamadığım için 5.4 ile gelen traitleri kullanmaya karar verdim.

**Gereksinimler:**

- Gezindiğimiz alan adı: `$_SERVER['SERVER_NAME'];`
- Şu anda bulunduğumuz sayfa: `$_SERVER['REQUEST_URI'];`

Bu işlemde mevcut bulunduğumuz sayfayı PHP’nin `str_replace()` fonksiyonunu kullanarak temizleyeceğiz. Zaten düz root ise hiçbir şey görünmeyecek. Yok farklı URL’lerde isek o zaman kalan kısım görülecektir. Örneğin şöyle:

```php
/*
http://localhost => return ""
http://localhost/proje => return "/proje"
*/
```

Yukarıdaki gibi bir temizleme işlemi sonrası yapılması gereken PHP’nin `explode()` fonksiyonunu slash ile kullanarak slashtan sonra gelen ilk değeri almak. Yani şöyle düşünelim:

```php
/*
/proje
/proje/abc/
/proje/a/b/c/d/e/f/g
*/
```

İlk satırdaki proje değerini almak zaten sıkıntılı değil ama bulunduğunuz sayfa 3. satırdaki gibi ise bir problemimiz var demektir. Bu yüzden explode ile böldüklerimizin ilkini alıyoruz. Bu genelde projenin root dizinidir.

Dizinin altındaki dizinlere maalesef bir çözümüm yok, o durumlarda artık manuel olarak ilk yöntemi denemelisiniz. Neyse bu işlem sonrasında ise dönen array türü değerin 1. indexini çağırıyoruz.

Bu projemizin root olarak görünen klasörüdür. Yani `/proje/a/b/c/d/e/f/g` gibi bir adresten sadece proje değerini alıyoruz. Ve ilk değerin başına tekrar bir slash ekliyoruz Son olarak dönen değer ”/” ya da “/proje_kok_dizini” şeklinde oluyor. Aslında senaryo olarak size bu işlemin nasıl yapacağını söylediğime göre kodlaması da basit anlamda kafanızda canlandı. Yine de kodlarını paylaşayım:

**url.php**

```php
<?php
trait URL {
    private $url = '';
    private $current_url = '';
    public $get = '';

    function __construct()
    {
        $this->url = $_SERVER['SERVER_NAME'];
        $this->current_url = $_SERVER['REQUEST_URI'];

        $clean_server = str_replace('', $this->url, $this->current_url);
        $clean_server = explode('/', $clean_server);

        $this->get = array('base_url' => "/".$clean_server[1]);
    }
}
?>
```

**index.php**

```php
<?php
require_once 'url.php'

class Home
{
    use URL;
}

$h = new Home();

// Bu satır HTML kodları içermektedir.
// O yüzden üst kısımda PHP taglerini kapatmalısınız.
?>
<a href="<?=$h->get['base_url']?>">Base</a>
```

Bu yazıyı Stackoverflow’da verdiğim [şu](https://stackoverflow.com/a/41407858/3821823) cevaptan esinlenerek oluşturdum