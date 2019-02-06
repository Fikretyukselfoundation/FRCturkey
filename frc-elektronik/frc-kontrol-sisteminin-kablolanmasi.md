# FRC Kontrol Sisteminin Kablolanması

#### Malzemeleri Topla

![](../.gitbook/assets/image%20%2894%29.png)

Aşağıdaki kontrol sistemi bileşenlerini ve araçlarını bulun



* Power Distribution Panel \(PDP\)
* roboRIO
* Pneumatics Control Module \(PCM\)
* Voltage Regulator Module \(VRM\)
* OpenMesh radio \(Güç kablosu ve ethernet kablosuyla\)
* Robot Signal Light \(RSL\)
* 4x Sparks ya da başka motor sürücüler
* 2x PWM y-cables
* 120A Circuit breaker
* 4x 40A Circuit breaker
* 6 AWG Kırmızı Kablo
* 10 AWG Kırmızı/Siyah kablo
* 18 AWG Kırmızı/Siyah kablo
* 22AWG sarı/yeşil  CAN kablosu
* 16x 10-12 AWG  \(sarı\) halka terminalleri \(entegre kablolu kontrol cihazlarının 8x hızlı bağlantı çiftleri\)
* 2x Andersen SB50 Batarya konnektörleri
* 6AWG Terminal kulpları
* 12V Batarya
* Siyah/Beyaz elektrik bandı
* Dual Lock malzemesi veya bağlantı elemanları
* Zip ties\(Amerikan Kelepçe\)
* 1/4" ya da 1/2" Pleksi

Gerekli Araçlar

* Wago aracı veya küçük düz tornavida
* Çok küçük düz tornavida gözlük tamirlerinde kullanılan boyutunda
* Philips Tornavida
* 5mm Hex anahtarı \(Metrik anahtarınız yoksa 3/16" iş görebilir\)
* 1/16" Hex anahtarı
* Tel kesiciler, sıyırıcılar ve kıvırıcılar
* 7/16” kutu uç anahtarı

#### Çekirdek Kontrol Sistemi Bileşenlerini Düzenleme

![](../.gitbook/assets/image%20%2820%29.png)

Bileşenleri tahtaya yerleştirin. Çalışması gereken bir düzen, yukarıdaki görüntülerde gösterilmektedir.

#### Bileşenleri sabitleyin

![](../.gitbook/assets/image%20%2856%29.png)

Çift taraflı bant veya veya amerikan kelepçe kullanarak, tüm bileşenleri panele sabitleyin. Birçok FRC oyununda robot bağlantısının önemli olabileceğini ve çift taraflı bantın tek başına birçok elektronik bileşeni tutma olasılığının az olduğunu unutmayın.Takımlar, bileşenlerini sabitlemek için somun ve civata tutturucular veya \(yukarıdaki resimde gösterildiği gibi\) amerikan kelepçe kullanmak isteyebilir.

#### Batarya Konektörünü PDP'ye Takın

![](../.gitbook/assets/image%20%2864%29.png)

Dikkat: Batarya Konektörüne bağlı olan kablolar 6AWG olmalıdır.

1. Terminal pabuçları batarya konnektörüne takın.
2. 1/16 "Alyan anahtarı kullanarak, PDP terminal kapağını sabitleyen iki vidayı sökün.
3. 5 mm'lik bir Alyan anahtar kullanarak \(metrik anahtarınız yoksa 3/16 "\), negatif \(-\) cıvatayı ve pulu PDP'den çıkarın ve akü konektörünün negatif terminalini sabitleyin.
4. 7/16 "kutu uçlu anahtar kullanarak, ana kesicinin" Batt "tarafındaki somunu sökün ve akü kondansatörünün artı ucunu sabitleyin.

#### PDP ile şalteri kablolamak

![](../.gitbook/assets/image%20%2869%29.png)

Dikkat olması gereklidir: 6AWG kırmızı tel, 2x 6AWG terminal pabuçları, 5mm Alyan, 7/16 "

Bir terminal pabucunu 6AWG kırmızı telin ucuna sabitleyin. 7/16 "kutu ucunu kullanarak, 120A ana kesicinin" AUX "tarafındaki somunu sökün ve terminali saplamanın üzerine yerleştirin. Somunu gevşek bir şekilde sabitleyin \(kısaca kesmek, bantlamak ve sıkıştırmak için çıkarmak isteyebilirsiniz\) telin diğer ucu\) PDP'nin pozitif terminaline ulaşmak için gereken tel uzunluğunu ölçün.

1. Terminali kırmızı 6AWG kablosunun 2. ucuna kesin, bantlayın ve kıvırın.
2. 7/16 "kutu ucunu kullanarak, kabloyu 120A ana şalterin " AUX "tarafına sabitleyin.
3. 5 mm'yi kullanarak diğer ucunu PDP pozitif terminaline sabitleyin.

#### PDP bağlantılarını yalıtın

![](../.gitbook/assets/image%20%2879%29.png)

 Gerektirir : 1/16" Alyan, Elektrik bandı

Elektrik bandı kullanarak, 120A şaltere iki bağlantıyı yalıtın. Ayrıca kapak değiştirildiği zaman ortaya çıkacak olan PDP terminallerinin herhangi bir parçasını yalıtın. 

#### Wago konektörleri

{% embed url="https://www.youtube.com/watch?v=L3GJGQ7mJqk" %}

Bir sonraki adım PDP'deki Wago konektörlerini kullanmayı içerecektir. Wago konnektörlerini kullanmak için, küçük bir düz tornavidayı dik açıda dikdörtgensel deliğe sokun, ardından kolu açarak terminali açarken bastırmaya devam edin ve tornavidayı yukarıya doğru çevirin.

 PDP'de iki boyutta Wago konektörü bulunur:

Küçük Wago konektörü: 10AWG-24AWG kablo kabul eder.

Büyük Wago konektörü: 6AWG-12AWG kablo kabul eder.

Çekme kuvvetini en üst düzeye çıkarmak ve bağlantı direncini en aza indirmek için Wago konektörüne takmadan önce kalaylı \(ve ideal olarak bükülmemiş\) kablolar kullanılmamalıdır.

#### Motor Sürücü Gücü

![](../.gitbook/assets/image%20%28105%29.png)

![](../.gitbook/assets/image%20%2841%29.png)

Gerektirir: bant, Küçük Düz Tornavida, 10 veya 12 AWG tel, 10 veya 12 AWG çatal / halka terminalleri, tel sıkıştırıcı

Spark Sürücüler için :

1. Kırmızı ve siyah kabloyu 40A \(daha büyük\) Wago terminal çiftlerinden birinden hız kontrol cihazının giriş tarafına \(her uçtaki terminallere takılacak uzunluk için biraz daha fazla\) ulaşmak için uygun uzunlukta kesin.
2. Her bir telin bir ucunu soyun, sonra Wago terminallerine yerleştirin.
3. Her kablonun diğer ucunu soyun ve bir halka veya çatal terminaline sıkıştırın.
4. Terminali motor sürücü giriş terminallerine takın \(kırmızı ila +, siyah - -\)

Pwm kablosu entegreli motor sürücüler için\(2. resim\) :

1. Kırmızı ve siyah güç giriş tellerini kesin ve bantlayın, ardından 40A Wago terminal çiftlerinden birine takın.

#### Weidmuller Konnektörleri

{% embed url="https://www.youtube.com/watch?v=kCcDw3lDYis" %}

Sistemdeki CAN ve güç konektörlerinin bir numarası, bir Weidmuller LSF serisi tel-to-board konnektörü kullanır. En iyi sonuçlar için bu konektörü kullanırken aklınızda bulundurmanız gereken birkaç nokta vardır:  
  


* Kablo, 24AWG'ye 16AWG olmalıdır \(güç kablolaması için gereken göstergeyi doğrulamak için kurallara bakın\)
* Kablo uçları yaklaşık 5/16 " olmalıdır.
* Kabloyu takmak veya çıkarmak için terminali açmak için ilgili "düğmesine" basın.

Bağlantıyı temizledikten sonra temiz ve güvenli olduğundan emin olun:

* Konektörün dışında kısa devreye neden olabilecek çıkıntı teller olmadığından emin olun.
* Tamamen oturduğunu doğrulamak için kabloyu bağlayın. Tel dışarı çıkarsa ve doğru ölçüm aleti varsa, daha fazla takılmalı ve / veya daha fazla soyulmalıdır.

#### roboRIO Gücü

![](../.gitbook/assets/image%20%2835%29.png)

Gerektirir: 10A / 20A mini sigortalar, Kablo soyucu, çok küçük düz tornavida, 18AWG Kırmızı ve Siyah

1. PDP'deki 10A ve 20A mini sigortaları, yukarıdaki resimde gösterilen yerlerde takın.
2. Hem kırmızı hem de siyah 18AWG kablosuna ~ 5/16 "takın ve PDB üzerindeki" Vbat Controller PWR "terminallerine bağlayın.
3. RoboRIO üzerindeki güç girişine ulaşmak için gereken uzunluğu ölçün. Kabloları batarya gibi diğer bileşenlerin etrafına yönlendirmek ve herhangi bir gerilim azaltma veya kablo yönetimine izin vermek için yeterli uzunlukta bırakmaya dikkat edin.
4. Kabloyu kesin ve soyun.
5. Çok küçük bir düz tornavida kullanarak, kabloları roboRIO'nun güç giriş konektörüne bağlayın \(kırmızı  V, siyah  C\). Ayrıca güç konektörünün roboRIO'ya güvenli bir şekilde vidalandığından emin olun.

#### Voltaj Regülatörü Modülü 

![](../.gitbook/assets/image%20%28109%29.png)

Requires: Kablo soyucu, küçük düz uçlu tornavida\(isteğe bağlı\), 18AWG kırmızı ve siyah kablo

1. Kırmızı ve siyah 18 AWG kablosunun ucunu soyun.
2. Kabloyu, PDP üzerindeki "Vbat VRM PCM PWR" etiketli iki terminal çiftinden birine bağlayın.
3. VRM'deki "12Vin" terminallerine ulaşmak için gereken uzunluğu ölçün. Kabloları batarya gibi diğer bileşenlerin etrafına yönlendirmek ve herhangi bir gerilim azaltma veya kablo yönetimine izin vermek için yeterli uzunlukta bırakmaya dikkat edin.
4. Telin ucundan ~ 5/16 "kesin ve soyun.
5. Kabloyu VRM 12Vin terminallerine bağlayın.

#### Pneumatics Control Module  \(İsteğe Bağlı\)

![](../.gitbook/assets/image%20%2895%29.png)



Gerektirir: Kablo soyucu, küçük düz tornavida \(isteğe bağlı\), 18AWG kırmızı ve siyah kablo

1. Kırmızı ve siyah 18 AWG kablosunun ucunu soyun.
2. Kabloyu, PDP üzerindeki "Vbat VRM PCM PWR" etiketli iki terminal çiftinden birine bağlayın.
3. VRM'deki "Vin" terminallerine ulaşmak için gereken uzunluğu ölçün. Kabloları batarya gibi diğer bileşenlerin etrafına yönlendirmek ve herhangi bir gerilim azaltma veya kablo yönetimine izin vermek için yeterli uzunlukta bırakmaya dikkat edin.
4. Telin ucundan ~ 5/16 "kesin ve soyun.
5. Kabloyu VRM 12Vin terminallerine bağlayın.

#### Radyo Gücü ve Ethernet

![](../.gitbook/assets/image%20%2828%29.png)

Gerekenler: Küçük düz tornavida \(isteğe bağlı\), Rev kablosuz PoE kablosu

1. PoE  kablosunun yüksüklerini VRM'nin 12V / 2A bölümündeki ilgili renkli terminallere takın.
2. Kablonun ethernet ucunu, güç konektörüne en yakın  Ethernet portuna \(18-24v POE etiketli\) bağlayın.

#### Roborio'yu radyo ethernetine bağlamak

![](../.gitbook/assets/image%20%2896%29.png)

Gerektirir : Ethernet Kablosu

Rev Passive POE kablosunun dişi RJ45 \(Ethernet\) portundan bir Ethernet kablosunu, roboRIO üzerindeki RJ45 \(Ethernet\) portuna bağlayın.

#### RoboRIO - PCM CAN

![](../.gitbook/assets/image%20%285%29.png)

Not: PCM, robot üzerindeki pnömatik kontrol için kullanılan isteğe bağlı bir bileşendir. PCM'yi kullanmıyorsanız, CAN bağlantısını doğrudan roboRIO'dan \(bu adımda gösterilmiştir\) PDP'ye bağlayın

#### PDP CAN için PCM

![](../.gitbook/assets/image%20%2850%29.png)

#### PWM Kabloları

![](../.gitbook/assets/image%20%2823%29.png)

Seçenek 1 \(Doğrudan bağlantı\):  


1. Her bir Spark’ten gelen PWM kablolarını doğrudan roboRIO’ya bağlayın. Siyah kablo, roboRIO ve Spark'in dışına doğru olmalıdır. En basit programlama deneyimi için sol tarafı PWM 0 ve 1'e ve sağ tarafa PWM 2 ve 3'e bağlamanız önerilir, ancak hangi kanalın hangi kanala gittiğini ve kodu buna göre ayarladığınızı bildiğiniz sürece herhangi bir kanal kullanabilirsiniz.

Seçenek 2 \(Y kablosu\):

1. 1 PWM Y kablosunu, robotun bir tarafını kontrol eden spark için PWM kablolarına bağlayın. Y kablosundaki kahverengi kablo, PWM kablosundaki siyah kabloyla eşleşmelidir.
2. PWM Y kablolarını roboRIO üzerindeki PWM bağlantı noktalarına bağlayın. Kahverengi tel, roboRIO'nun dışına doğru olmalıdır. En basit programlama deneyimi için sol tarafı PWM 0'a ve sağ tarafa PWM 1'e bağlamanız önerilir, ancak hangi kanalın hangi kanala gittiğini ve kodu buna göre ayarladığınızı bildiğiniz sürece herhangi bir kanal kullanabilirsiniz.

#### Robot Sinyal Işığı

![](../.gitbook/assets/image%20%2881%29.png)

Gerektirir: Kablo soyucu, 2 pin kablo, Robot Sinyal Işığı, 18AWG kırmızı kablo, çok küçük düz tornavida

1. Siyah kabloyu merkeze, "N" terminaline takın ve terminali sıkın.
2. 18AWG kırmızı kabloyu soyun ve "La" terminaline takın ve terminali sıkın.
3. "Lb" terminaline takmak için 18AWG kablosunun diğer ucunu kesin ve soyun
4. Kırmızı kabloyu iki pin kablosundan 18AWG kırmızı kablo ile "Lb" terminaline takın ve terminali sıkın.
5. İki pinli konnektörü, roboRIO üzerindeki RSL bağlantı noktasına bağlayın. Siyah kablo, roboRIO'nun dışına en yakın tarafta olmalıdır.

RSL'yi robot inşa edildiğinde daha görünür bir yere taşımanız önerilir

####  Devre kesiciler

![](../.gitbook/assets/image%20%2890%29.png)

40 amperlik Devre Kesicileri, Talonların bağlı olduğu Wago konnektörlerine karşılık gelen PDP üzerindeki konumlara takın. Tüm kesiciler için kesicinin en yakın pozitif \(kırmızı\) terminale denk geldiğine dikkat edin \(yukarıdaki grafiğe bakınız\). Kart üzerindeki tüm negatif terminaller doğrudan dahili olarak bağlanır.

#### Motor gücü

![](../.gitbook/assets/image%20%2831%29.png)

Her bir CIM motoru için:

1. CIM'den gelen kırmızı ve siyah bantların uçlarını bantlayın

spark veya diğer motor sürücüler için:

1. Motor kablolarının her birinde bir halka / çatal terminali sıkın. Kabloları motor kontrol cihazının çıkış tarafına takın \(kırmızı +, siyah  -\)

Entegre tel denetleyiciler\(kendi üzerinde kendiliğinden pwm kablosu olan sürücüler\) için:

1. Victor SP'den gelen beyaz ve yeşil kabloları bantlayın
2. Motor kablolarını Victor SP çıkış kablolarına bağlayın \(kırmızı kabloyu beyaz M + çıkışına bağlamanız önerilir\). 

#### Batarya bağlayın

![](../.gitbook/assets/image%20%28102%29.png)

###  <a id="battery_box"></a>

Aküyü Andersen konektörünün robot tarafına bağlayın. Kolu, 120A ana şalterin üstündeki butonu gövdenin üstündeki sırtın içine hareket ettirerek açın.

