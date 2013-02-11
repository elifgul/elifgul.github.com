---

layout: post

title: Yapay Sinir Ağları İle Karakter Tanıma

---


Bu yazıda amacımız Matlab'da Yapay Sinir Ağları ile basit bir uygulama yapmak.

Öncelikle ağımız için girdi değerlerimizi tanımlıyoruz.

* Bunlar aynı boyutlarda tek tek harflerden oluşan görüntülerdir.

Anlatım kolaylığı açısından iki harf üzerinden gidelim.

    A = imread('A.png');   
    B = imread('B.png');
    
Eğitimde kullanılan A ve B harfinin görüntüleri:

![](http://img94.imageshack.us/img94/6923/25130601.jpg)

Ağımızın girdi ve çıktı elemamları numerik olmak zorundadır.
Bu nedenle girdilerimize aşağıdaki işlemler uygulanır.

    A = imw2bw(A);
    A = imresize(A,[50 50]);
    A = double(A);
    A = reshape(A,2500,1);

Aynı işlemler B harfinin bulunduğu görüntüye de uygulanır.

Bu işlemlerden sonra A ve B matrislerini 2500x1'lik sutün matrisine
dönüştürdük.

Bu matrisleri ağa girdi olarak sunmak için giriş matrisinde
topluyoruz.

    Giris = zeros(2500,2);
    Giris(:,1) = A;
    Giris(:,2) = B;

Giris matrisinin; 
* 1. sutünu A harfinin sutün değerini
* 2. sutünu B harfinin sutün değerini tutmaktadır.

A ve B 'yi temsil etmek için bir de çıkış matrisine ihtiyacımız var.

    Cikis = eye(2)
    Cikis =

        1     0
        0     1

Ağda girişe uygun Cikis matrisi de iki durumdan oluşmaktadır. 
Bu harf ya A'dır ya da B'dir demenin numarik ifadesidir.

Çıktı ;
* ilk sutüna yakın değerler üretirse verilenin A harfi
* ikinci sutüna yakın değerler alırsa girdinin B harfi olduğunu gösterir.



Matlab'da newff fonksiyonuna gerekli argümanlar verilerek ağ oluşturulur.

İki katmanlı bir ağ:
  
    net=newff(minmax(Giris),[10,2],{'logsig' 'logsig'},'trainscg');

Ağı eğitmek için bazı parametreleri girmemiz gerekiyor.

Performans parametresi (sse = hata karelerinin toplamı):

    net.trainParam.perf='sse';

Döngü sayısı:

    net.trainParam.epochs=500;

Ağ eğtiminin sonlanma noktası:
    
    net.trainParam.goal=1e-5;

Ağın girdi ve çıkış matrisine göre eğitilmesi:

    net=train(net,Giris,Cikis)

Eğitilen ağın ekran çıktısı:

Ağın Eğitim aşaması bittikten sonra test aşamasına geçebiliriz.
Bunun için A görüntüsüne biraz gürültü ekliyoruz.

    A = imread('A.png');
    A = im2bw(A);
    A = imresize(A,[50 50]);
    A = double(A);
    A = A + rand(50,50)*0.8;
    imshow(A)
    A = reshape(A,2500,1);

Gürültü eklenmiş A harfi:

![](http://ib3.keep4u.ru/b/2013/02/11/d5/d533439bb7d9144c51dc4644f0a11203.jpg)

Gürültü eklenen A harfi ile ağın test edilmesi:

    X = sim(net,A);
    X = sim(net,A)

      0.6906
      0.3094

Görüldüğü üzere elde ettiğimiz sonuç matrisinin birinci satırı 1'e yakın ikinci satırı 0' yakındır.
Bu da girdinin A harfi olduğunu göstermektedir.

Hatanın yüksek olmasının nedeni ağ etimi sırasında eğitim setinin 
az olmasıdır. Örnek sayısı artıkça hata oranı ters orantılı olarak azalacaktır.
