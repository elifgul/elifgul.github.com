---

layout: post

title: Yapay Sinir Ağlarına Giriş 

---

Matlab'da ileri beslemeli bir ağ nesnesi oluşturma komutu:

    net = newff(PR,[S1 S2…SNl],{TF1 TF2…TFNl},BTF,BLF,PF)

* PR  - R elemanlı giriş vektörünün minimum ve maksimum değerlerini içeren Rx2 ‘lik matris.
* Si  - i’nci katmanda bulunan nöron sayısı. 
* TFi - i’nci katmanın transfer fonksiyonu, varsayılan= 'tansig'.
* BTF - Geriye yayılım ağ eğitim fonksiyonu, varsayılan = 'trainlm'.
* BLF - Geriye yayılım ağırlık/bias öğrenme fonksiyonu, varsayılan = 'learngdm'.
* PF  - Performans fonksiyonu, varsayılan = 'mse' dir.

Aşağıdaki kod ile iki katmanlı bir ağ oluştulur.

    net=newff([-1 2; 0 5],[3,1],{'tansig','purelin'},'traingd');
    
Tasarlanan ağın simülasyonu için sim komutu kullanılır. 

Simülasyon ağa bir giriş değeri verip ağın çıkışını hesaplatmaktır.

    p = [1;2];
    a = sim(net,p)
    a =
       -0.1011
