---
layout: post
Matlab'ta Görüntü İşlemeye Giriş
---

Akla ilk gelen sorular:

 * Görüntüleri koda nasıl aktarırız?
 * Görüntülere sayısal işlemleri nasıl uygularız?

Öncelikle görüntünün tanımını yapmakla başlayalım.

Görüntü piksellerden oluşan bir matristir ve programlama ile uğraşan hemen hemen herkes matrislerle 
sayısal işlemler yapmıştır.

Matlab'ta görüntü türleri

 * İkilik Görüntü 
 * Gri seviyeli görüntüler
 * Renkli resimler 

İkilik Görüntü

Görüntüde her piksel siyah ya da beyazdır. Siyahın renk değeri 0 ile ifade edilirken beyazın renk değeri
1 ile ifade edilir.

Şimdi sıra ikilik formattaki görüntümüzü Matlab'a tanıtmada.
 * Okutacağımız görüntü ile çalışacağımız konumun aynı olmasına dikkat edelim.
 * imread fonksiyonuna, Matlab'a okutacağımız görüntünün adını veririz. Erişim kolaylığı için atama işlemi yapılır.
 * imshow fonksiyonu ile okuttuğumuz görüntüyü ekranda görüntüleriz.
 
 ' bw = imread('organlar.jpg'); '

 ' imshow(bw) '

![](http://i011.radikal.ru/1212/19/81cb8224d5f0.jpg)   

Görüldüğü üzere resim siyahve beyaz renklerinden oluşmaktadır.

