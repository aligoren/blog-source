---
layout: post
title: PHP'de Anonim Fonksiyonlar
tags: [PHP, Programming, Tips]
---

Selamlar. Bu yazıda size closure yani anonim fonksiyonları göstereceğim. Öğrenme aşamasından önce yani sadece ilk bakışta kafamı karıştırmıştı. Ama zor değilmiş.

Şimdi elimizde bir nesne var diyelim. Bu nesnenin bir instance’ı yani örneğini oluşturdunuz ve bu oluşturulan instance’lara ait methodları kullanarak yeni bir instance oluşturabileceğinizi düşünün daha önce düşündünüz mü bilmiyorum.

## Yalnız Değilsiniz!

<!--more-->

Eğer hayır diyorsanız yalnız olmadığınıza inancım tam. Size reflection ve closure kavramlarının nasıl kullanılacağını göstermek için bu yazıyı yazdım.

Öncelikle closure ya da anonym functions olarak karşımıza çıkar bu kavram. Onu bilelim. PHP konu olduğu için belirtmek gerekiyor ki 5.3 sürümünden itibaren closure’ları kullanabiliriz. Yanılmıyorsam günümüzde desteklemeyen pek az yer vardır.

Zaten bu closure kavramını araştırsanız internette bir sürü kaynak bulabilirsiniz. Ben bu yazıyı yazmadan önce baya baya yazanlar olmuştur gerek Türkçe gerekse İngilizce dilinde mevcuttur yani. Çok uzatmayayım.

## İsimsiz

![/images/posts/bourne.jpg](/images/posts/bourne.jpg)

İsimsiz denilince aklıma Jason Bourne gelir. Bilmiyorum Bourne serisini izlediniz mi ama kendisi kimliksiz aslında adı olmayan kafasının içinde kaybolan bir abimiz. Ama merak etmeyin bu anonim fonksiyonlar öyle aksiyon dolu bir şey değil.

Anonim fonksiyonlar dediğime göre bu fonksiyonların nasıl fonksiyonlar olacağını adlarından anlamış olmalısınız. Anonim fonksiyonlar. Eğer isme sahip olmayan fonksiyon tanımı aklınıza geliyorsa doğru düşünmüşsünüz. Basit bir örneği şöyle size gösterebilirim.

```php
<?php
$isimsiz = function() {
	echo 'Merhaba';
};

$isimsiz(); // Çıktısı Merhaba;
?>
```

Kodu dikkatli incelersiniz fonksiyona herhangi bir isim verilmedi ancak oluşturulduğu anda `$isimsiz` isimli bir değişkene atamasını yaptık. Yani normal bir değişken oluşturma işlemini yaptınız sonuna noktalı virgülünü de koydunuz. Tek farkı değişkene atanan şey direkt bir value yani değer değil. Bir fonksiyon. Bu kısmı da uzun uzun yazmayayım. Sadece bu fonksiyonları çağırırken başlarına dollar sign yani $ koymanız gerektiğini unutmayın.

Bu fonksiyonların bir başka metoda parameter olarak da geçilebileceğini unutmayın. Basit bir başka örnek vereyim bu da fonksiyona, anonim bir fonksiyon eklenebilmesine dair olsun:

```php
<?php
function benim_fonksiyonum($anonim_fonksiyon) {
	return $anonim_fonksiyon();
}

benim_fonksiyonum(function() {
	echo 'Fonksiyon içinde fonksiyon mu olur? Oluyor işte';
});
?>
```

Yukarıdaki yönteme eğer frameworklere aşinaysanız ya da onları incelediyseniz denk gelmişsinizdir. Kısacası böyle de kullanabilirsiniz. Ben bu yöntemi kullanarak şöyle basit bir repo olışturdum:

[https://github.com/aligoren/Request](https://github.com/aligoren/Request)

Readme dosyasını incelerseniz aslında anlattığım şeyin ne gibi noktalarda kullanılabileceğini de anlamış olursunuz. Yazıyı çok uzatmak istemiyorum ancak bu konuda [ReflectionMethod](https://php.net/manual/tr/class.reflectionmethod.php) sınıfını iyi incelemenizi öneririm. Bu sayede direkt olarak closure kullanımını da deneyimlemiş olursunuz.

Umarım faydalı olmuştur.