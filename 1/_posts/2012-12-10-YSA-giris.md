---
layout: post
Matlab'da Yapay Sinir Ağları ile İki Sayının Çarpımı
---
Çarpma işleminde iki giriş ve bir çıkış vardır. 
Eğitimde 20 adet rastgele sayı seçilmiş ve test aşamasında 
yine 20 adet rastgele sayı seçilerek sistemin performansı ortaya konmuştur.

Çarpma İşlemini yapan kod:

`clear all, close all,clc;`

`p=round(rand(2,20)*10) % Giris verisi 2x20’lik matris, rastgele 0-10`

`t = p(1,:).*p(2,:)     % Cikis verisi p1*p2`

`%Normalizasyon islemi`

`[pn,minp,maxp,tn,mint,maxt] = premnmx(p,t);`

`%YSA’nin tasarımı, eğitimi ve simülasyonu`

`net = newff(minmax(pn),[5 1],{'tansig' 'purelin'},'trainlm');`

`net = train(net,pn,tn);`

`an = sim(net,pn);`

`[a] = postmnmx(an,mint,maxt); %Normalizasyonun tersi`

`%Egitim verilerinin gercek ve YSA cikisinin gosterimi`

`figure(1),plot3(p(1,:),p(2,:),t,'o');`

`hold on,plot3(p(1,:),p(2,:),a,'r*'),grid on;`

`legend('Gerçek deger','YSA cikisi'),xlabel('p1'),ylabel('p2'),zlabel('t'),title('Egitim verisi')`

`%Test verilerinin hazirlanmasi, farkli 20 adet ornek`

`ptest=round(rand(2,20)*10)`

`ttest = ptest(1,:).*ptest(2,:)`

`[ptn,minpt,maxpt,ttn,mintt,maxtt] = premnmx(ptest,ttest);`

`atn = sim(net,ptn); %Simulasyon`

`at] = postmnmx(atn,mintt,maxtt);`

`%Test verilerinin gercek ve YSA cikisinin gosterimi`

`figure(2),plot3(ptest(1,:),ptest(2,:),ttest,'o');`

`hold on,plot3(ptest(1,:),ptest(2,:),at,'r*'),grid on;`

`legend('Gerçek deger','YSA cikisi'),xlabel('p1'),ylabel('p2'),zlabel('t'),title('Test verisi')`

Ekran çıktıları:

![](http://photoload.ru/data/e4/8b/18/e48b185060fb0ff49be6da43e69e624b.jpg)

![](http://s002.youpic.su/pictures/1355950800/7e1dbc62c2888dfc3aec0ecc321ffa7a.jpg)

