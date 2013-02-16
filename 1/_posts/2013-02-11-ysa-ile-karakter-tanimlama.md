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

* Birinci sutünu A harfinin sutün değerini
* İkinci sutünu  B harfinin sutün değerini tutmaktadır.

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

![](http://s018.radikal.ru/i510/1302/46/98934c0973ee.jpg)

Gürültü eklenen A harfi ile ağın test edilmesi:

    X = sim(net,A);
    X = sim(net,A)

      0.6906
      0.3094

Görüldüğü üzere elde ettiğimiz sonuç matrisinin birinci satırı 1'e yakın ikinci satırı 0' yakındır.
Bu da girdinin A harfi olduğunu göstermektedir.

Hatanın yüksek olmasının nedeni ağ etimi sırasında eğitim setinin 
az olmasıdır. Örnek sayısı artıkça hata oranı ters orantılı olarak azalacaktır.

---

Aynı uygulamayı Matlab'ın bize sunduğu nntool toolbox'ı ile yapalım.
nntool komutu ile toolbox'ı açıyoruz.

    nntool 

Açılan arayüzde oluşturacağımız ağın girdi ve beklenen çıktı değerlerini 
import ediyoruz.

New butonuyla yeni bir ağ tanımlayoruz.

Görüntüde olduğu gibi ağ parametrelerini seçtikten sonra Create tuşuyla 
ağı oluşturuyoruz.

![](http://images.vfl.ru/ii/1361009053/beec937e/1767039.jpg)

Oluşan yapay sinir ağı :

![](http://images.vfl.ru/ii/1361008723/660355b4/1766980.jpg)

Arayüzde train menüsüyle ağın eğitmi için şekildeki parametreler seçilir.

![](http://s2.ipicture.ru/uploads/20130216/VUUse935.jpg)
![](http://images.vfl.ru/ii/1361008878/79106477/1767003.jpg)

Tüm sonuçların programdan alınabilmesi için arayüzdeki export seçeneği kullanılabilir. 

![](http://s002.youpic.su/pictures/1360962000/401872b8e06ea72bc5637e724d4d8784.jpg)

Oluşturdumuz ağı ve elemanlarını seçerek Save tuşuyla .mat uzantılı olarak kaydedelim.

Kaydedilen ağın çağrılması:

    load karTan
    
Ağın gürültü eklenmiş A harfi ile test edilmesi:

    X = sim(karTan, A);
    
    0.9987
    0.5000
