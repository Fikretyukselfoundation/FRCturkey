---
description: İçerik Hazırlanıyor..
---

# Görüntü İşleme Kaynakları

### Vision Processing Stratejileri

Vision Processing kullanmak, robotunuzun alandaki öğelere duyarlı olmasını ve daha fazla özerk olmasını sağlamanın harika bir yoludur. Genellikle FRC oyunlarında, topların veya diğer oyun parçalarının otonom olarak hedeflenmesi veya sahadaki yerlere gitmesi için bonus puanlar vardır. Vision Processing, bu sorunların çoğunu çözmenin harika bir yoludur.Eğer meydan okuyan özerk bir kodunuz varsa, o zaman insan sürücülerine yardım etmek için teleop döneminde de kullanılabilir.

Görme işlemi için ve vizyon programının çalışması gereken bileşenleri seçmek için birçok seçenek vardır. WPILib ve ilgili araçlar bir dizi seçeneği destekler ve ekiplere ne yapacağına karar vermek için çok fazla esneklik sağlar. Bu makalede, mevcut olan birçok seçenek ile ilgili bazı bilgiler verilecektir.

![](../.gitbook/assets/image%20%2859%29.png)

#### OpenCV Görüntü İşleme Kütüphanesi

OpenCV, akademi ve endüstri genelinde yaygın olarak kullanılan açık kaynaklı bir görüntü işleme kütüphanesidir. GPU hızlandırılmış işlem sağlayan bir desteğe sahip, C ++, Java ve Python da dahil olmak üzere birçok dil için uygun kütüphanelere sahiptir. Ayrıca birçok web sitesi, kitap, video ve eğitim kursları ile belgelenmiştir, böylece nasıl kullanılacağını öğrenmenize yardımcı olacak birçok kaynak bulunmaktadır. Bu nedenlerle ve daha fazlası için WPILib'in C ++ ve Java sürümleriyle kullanım için OpenCV'yi kabul ettik. 2017'den başlayarak WPILib ile birlikte gelen OpenCV kütüphaneleri, video yakalama, işleme ve vision için kütüphanede destek ve vision algoritmalarınızı oluşturmanıza yardımcı olacak araçlar hazırladık. OpenCV hakkında daha fazla bilgi için bkz. [Http://opencv.org.](Http://opencv.org)

**RoboRIO üzerinde vision kodu**

![](../.gitbook/assets/image%20%2864%29.png)

Vision programı, genel robot programının sadece bir parçası olduğu için programlama oldukça basittir. Program elle veya GRIP tarafından C ++ veya Java ile yazılabilir. Dezavantajı, robot programı ile aynı işlemci üzerinde çalışan vision kodunun performans sorunları yaşayabilmesidir. Bu, robot ve vision programınızın gereksinimlerine bağlı olarak değerlendirmeniz gereken bir şeydir.

Vision kodu, robot kodunun kullandığı sonuçları üretir. Bir görüş dizisinden değer alan robot kodu yazarken senkronizasyon sorunlarına dikkat edin. GRIP tarafından üretilen kod ve WPILib'deki VisionRunner sınıfı bunu daha kolay hale getirecektir.

Video akışı kamera operatörü arayüzünü kullanarak SmartDashboard'a gönderilebilir, böylece operatörler kameranın gördüklerini görebilir. Buna ek olarak, OpenCV komutlarını kullanarak görüntülere ek açıklamalar ekleyebilirsiniz, böylece kontrol panelinde hedefler veya diğer ilginç nesneler tanımlanabilir.

**DS bilgisayarında vision kodu**

![](../.gitbook/assets/image%20%2832%29.png)

Görüntü, işlem yapmak için Driver Station dizüstü bilgisayarına geri akışa alınmıştır. Classmate dizüstü bilgisayarlar vision işlemede roboRIO'dan daha hızlıdır fakat üzerinde çalışan gerçek zamanlı programlara sahip değildir. GRIP, dizüstü bilgisayarında doğrudan NetworkTables kullanarak robota gönderilen sonuçlarla birlikte çalıştırılabilir. Alternatif olarak, kendi vision programınızı seçtiğiniz bir dili kullanarak yazabilirsiniz. Yerel bir NetworkTables uygulaması ve OpenCV bağlantısı çok iyi olduğundan Python iyi bir seçimdir.

Görüntüler işlendikten sonra, hedef konum, mesafe veya ihtiyacınız olan herhangi bir şey gibi anahtar değerler, NetworkTables ile robota geri gönderilebilir. Gecikme sizler için bir sorun olabilir çünkü dizüstü bilgisayardaki görüntülerde gecikmeler ve sonuçların geri gelmesi biraz zaman alabilir.

Görüntü akışı SmartDashboard veya GRIP'de görüntülenebilir.

**Bir işlemci üzerinde vision kodu**

![](../.gitbook/assets/image%20%2834%29.png)

Raspberry PI veya Kangaroo gibi iş süreçleri, görüntü kodunun desteklenmesi için idealdir. Avantajı, tam hız çalıştırabilmeleri ve robot programına müdahale etmemeleridir. Bu durumda, kamera büyük olasılıkla işlemciye\(Raspberry pi vb.\) veya \(ethernet kamera kullanıyorsanız\) robotun üzerindeki bir ethernet girişine bağlanır. Program, herhangi bir dilde yazılabilir, ancak OpenCV ve NetworkTable'lar için basit bağlantılardan dolayı Python iyi bir seçimdir. Bazı takımlar ve deneyimsiz programcılar için çok karmaşık olabilse de, Nvidia Jetson gibi işlemciler en yüksek hız ve en yüksek çözünürlük için yüksek performans sağlayabilmesi için kullanılabilir.

Veri, ağ işlemcisi üzerindeki vision programından, NetworkTables  veya seri bağlantı üzerinden özel bir protokol kullanılarak robota gönderilebilir.

#### Kamera seçenekleri

WPILib tarafından desteklenen bir dizi kamera seçeneği vardır. Kameralar, nasıl çalıştığını belirleyebilecek bir dizi parametreye sahiptir. Örneğin, kare hızı ve görüntü çözünürlüğü, alınan görüntülerin kalitesini etkiler, ancak çok yüksek etkili işlem süresi , robot programınızda ciddi etkiye sahip olabilecek bant genişliğinin oluşmasını sağlar.

2017'de yeni olan, C ++ ve Java'da mevcut olan ve robota bağlı kameralarla arayüz sağlayan bir CameraServer sınıfı var. Bir Kaynak nesnesi aracılığıyla yerel işlem için görüntüler alır ve orada görüntülemek veya işlem yapmak için akışınızı sürücü istasyonunuza gönderir. Video akışı CameraServer tarafından işlenir.

WPILib ile kameraların kullanımı ile ilgili detaylar bir sonraki bölümde detaylandırılmıştır.

### Görüntü alın ve işleyin: CameraServer sınıfı

**Kavramlar**

Genellikle FRC'de kullanılan kameralar \(Axis kamera gibi USB ve Ethernet kameraları\) nispeten sınırlı çalışma modları sunar. Genel olarak, tek bir çözünürlük ve kare hızında yalnızca tek bir görüntü çıktısı \(tipik olarak JPG gibi sıkıştırılmış biçimde bir RGB görüntüsü \) sağlarlar. USB kameralar, sadece birden fazla uygulama kameranıza aynı anda erişemeyeceği için sınırlıdır.

2017 CameraServer çoklu kameraları destekler. Bir kamera bağlantısı kesildiğinde otomatik olarak yeniden bağlanma gibi ayrıntıları ele alır ve aynı zamanda kameranın sunduğu görüntüleri çoklu "istemciler" haline getirir \(örn. Hem robot kodunuz hem de gösterge panosu kameranıza aynı anda bağlanabilir\).

#### Kamera isimleri

CameraServer'daki her kamera benzersiz bir şekilde adlandırılmalıdır. Bu ayrıca, Panodaki kamera için görünen addır. CameraServer startAutomaticCapture \(\) ve addAxisCamera \(\) işlevlerinin bazı türevleri otomatik olarak kamerayı \(ör. "USB Camera 0" veya "Axis Camera"\) adlandırır veya kameraya daha açıklayıcı bir ad verebilirsiniz \(örn. "Intake Cam"\). Tek gereksinim, her kameranın kendine özgü bir adı olmasıdır.

#### USB kamera ile ilgili notlar

#### CPU kullanımı

CameraServer ile CPU Kullanımı, gerektiğinde sadece sıkıştırma ve açma işlemlerini gerçekleştirerek , herhangi bir istemci bağlı olmadığında akışı otomatik olarak devre dışı bırakarak CPU kullanımını en aza indirecek şekilde tasarlanmıştır.

CPU kullanımını en aza indirmek için, gösterge paneli çözünürlüğü kamerayla aynı çözünürlüğe ayarlanmalıdır; Bu, CameraServer'ın görüntüyü sıkıştırmasını ve yeniden sıkıştırmasını sağlamaz, bunun yerine resim gösterge panelinden alınan JPEG görüntüsünü doğrudan panoya iletebilir. Kontrol panelindeki çözünürlüğü değiştirmenin \* kamera çözünürlüğünü değiştirmediğini unutmayın. kamera çözünürlüğünü değiştirmek, NetworkTables üzerinden veya robot kodunuzda \(kamera nesnesinde setResolution öğesini çağırarak\) yapılabilir.

#### USB Bant Genişliği

Roborio, USB arabirimleri üzerinden tek seferde çok fazla veri iletebilir ve alabilir. Kamera görüntüleri çok fazla veri gerektirebilir ve bu yüzden bant genişliği limitini aşmak nispeten kolaydır. Bir USB bant genişliği hatasının en yaygın nedeni, özellikle birden çok kamera bağlı olduğunda, JPEG olmayan bir video modunun seçilmesi veya çok yüksek çözünürlükte çalışmasıdır.

#### Mimari

2017 için CameraServer iki katmandan, yüksek seviyeli WPILib CameraServer sınıfından ve düşük seviyeli cscore kitaplığından oluşmaktadır.

#### CameraServer sınıfı

CameraServer sınıfı \(WPILib'in bir parçası\) robot kodunuza kamera eklemek için yüksek düzeyde bir arabirim sağlar. Ayrıca kameralar ve kamera sunucuları hakkındaki bilgileri NetworkTable'lara yayınlamaktan sorumludur, böylece LabView Dashboard ve SmartDashboard gibi Sürücü İstasyonu kontrol panelleri kameraları listeleyebilir ve akışlarının nerede bulunduğunu belirleyebilir. Oluşturulan tüm kameraların ve sunucuların bir veritabanını korumak için tek bir desen kullanır.

CameraServer'daki bazı temel işlevler şunlardır:



* startAutomaticCapture \(\): Bir USB kamera \(ör. Microsoft LifeCam\) ekleyin ve bunun için bir sunucu başlatır, böylece panodan görüntülenebilir.
* addAxisCamera \(\): Bir axis kamerası eklemek için kullanılır. Robot kodunuzda Axis kameranın görüntülerini işlemiyor olsanız bile, bu işlevi Axis kamerasının Pano'nun açılır menü listesinde görünmesi için kullanabilirsiniz. Aynı zamanda bir sunucu başlatır, böylece sürücü istasyonunuz USB üzerinden roboRio'ya bağlandığında axis akışı hala görüntülenebilir \(hem Axis kameranız hem de iki robot radyo Ethernet portuna bağlı roboRio varsa yarışmada kullanışlıdır\).
* getVideo \(\): Bir kameraya OpenCV erişimi edinin. Bu, roboRio'da \(robot kodunuzda\) görüntü işleme için kameradan görüntüler almanızı sağlar. 
* putVideo \(\): OpenCV görüntülerini besleyebileceğiniz bir sunucu başlatın. Bu, özel işlenmiş ve / veya açıklamalı görüntüleri panoya aktarmanıza olanak tanır.

#### Cscore Kütüphanesi

Cscore kütüphanesi alt seviye uygulamasına aşağıdakileri sağlar:

* USB ve HTTP \(örneğin, axis kamera\) kameralarından görüntüler alın
* Kamera ayarlarını değiştir \(ör. Kontrast ve parlaklık\)
* Kamera video modlarını değiştir \(piksel formatı, çözünürlük ve kare hızı\)
* Bir web sunucusu olarak hareket edin ve görüntüleri standart bir M-JPEG akışı olarak sunun
* Görüntü işleme için görüntüleri OpenCV "Mat" nesnelerine dönüştürün

**Kaynaklar ve Havuz**

Cscore kütüphanesinin temel mimarisi, kaynaklarla havuzlar arasında bölünmüş işlevselliğe sahip MJPGStreamer'inkine benzer. Aynı anda oluşturulmuş ve çalışan birden çok kaynak ve çoklu havuz olabilir.

Görüntü üreten bir nesne bir kaynaktır ve görüntüleri kabul eden / tüketen bir nesne bir havuzdur. Üretmek / tüketmek kütüphanenin perspektifinden. Böylece kameralar kaynaklardır \(görüntü üretirler\). MJPEG web sunucusu bir havuzdur çünkü program içindeki görüntüleri kabul eder \(bu görüntüleri bir web tarayıcısına veya kontrol paneline yönlendiriyor olabilir\). Kaynaklar birden fazla havuza bağlanabilir, ancak havuzlar tek bir kaynağa bağlanabilir. Bir kaynağa bağlandığında, cscore kütüphanesi her bir görüntüyü kaynağından havuza geçirmekten sorumludur.

* **Kaynaklar** çerçeveler elde eder \(örneğin USB kamerasından görüntü alır\) ve yeni bir çerçeve mevcut olduğunda herhangi bir olayı tetikler. Hiçbir havuz belirli bir kaynağı dinlemiyorsa, işlemciyi ve G / Ç kaynaklarını kaydetmek için  bir işlemi duraklatabilir veya bağlantısını kesebilir. Kütüphane, olayların tetiklenmesini ve durdurulmasını sağlayarak kamera bağlantılarını / yeniden bağlanmalarını özerk olarak yönetir.
* **Havuzlar** belirli bir kaynağın olayını dinler, en son görüntüyü yakalar ve hedefine uygun biçimde iletir. Kaynaklara benzer şekilde, belirli bir havuz devre dışı ise \(ör. İstemci HTTP sunucusu üzerinden yapılandırılmış bir M-JPEG'e bağlı değildir\), kütüphane, işlemci kaynaklarını kaydetmek için işlemin bazı bölümlerini devre dışı bırakabilir.

Kullanıcı kodu \(bir FRC robot programında kullanılanlar gibi\), OpenCV kaynağı ve dağıtma nesneleri aracılığıyla bir kaynak \(bir fotoğraf makinesiymiş gibi işlenmiş çerçeveler sağlar\) veya bir havuz \(işleme için bir çerçeve alır\) olarak işlev görebilir. Böylece, bir kameradan görüntü alan ve işlenen görüntüleri dışarıdan görüntüleyen bir görüntü işleme hattı aşağıdaki gibi görünür:

UsbCamera \(VideoSource\) --&gt; \(cscore internal\) --&gt; CvSink \(VideoSink\)

 --&gt; \(your OpenCV processing code\) --&gt; CvSource \(VideoSource\)

 --&gt; \(cscore internal\) --&gt; MjpegServer \(VideoSink\)

Kaynaklar bağlı birden fazla havuza sahip olduğundan, bu hat daha fazla alt işleme ayrılabilir. Örneğin, orijinal kamera görüntüsü, UsbCamera kaynağının CvSink'e ek olarak ikinci bir MjpegServer havuzuna bağlanmasıyla da sunulabilir, sonuçta aşağıdaki grafik elde edilir:

UsbCamera --&gt; CvSink --&gt; \(your code\) --&gt; CvSource --&gt; MjpegServer \[2\]

          \-&gt; MjpegServer \[1\]

Kamera tarafından yeni bir görüntü yakalandığında, hem CvSink hem de MjpegServer \[1\] bunu alır.

Yukarıdaki grafik, aşağıdaki CameraServer snippet'inin yarattığı grafiktir:

// UsbCamera ve MjpegServer \[1\] oluşturur ve bunları bağlar

`CameraServer.getInstance().startAutomaticCapture()`

// CvSink'i oluşturur ve UsbCamera CvSink'e bağlar. CvSink =`CameraServer.getInstance().getVideo()`

// CvSource ve MjpegServer \[2\] 'yi oluşturur ve bunları CvSource outputStream = ile bağlar. `CameraServer.getInstance().putVideo("Blur", 640, 480);` 

CameraServer uygulaması, cscore seviyesinde aşağıdakileri etkin bir şekilde yapar \(açıklama amacıyla\). CameraServer, tüm cscore nesnelerine benzersiz isimler oluşturma ve otomatik olarak port numaralarını seçme gibi birçok ayrıntıya dikkat çeker. Ayrıca CameraServer oluşturulmuş nesnelerin tekil kayıtlarını tutar, böylece kapsam dışına çıkarlarsa yok olmazlar.

`UsbCamera usbCamera = new UsbCamera("USB Camera 0", 0);`

`MjpegServer mjpegServer1 = new MjpegServer("serve_USB Camera 0", 1181);`

`mjpegServer1.setSource(usbCamera); CvSink cvSink = new CvSink("opencv_USB Camera 0");`

`cvSink.setSource(usbCamera);`

`CvSource outputStream = new CvSource("Blur", PixelFormat.kMJPEG, 640, 480, 30);`

`MjpegServer mjpegServer2 = new MjpegServer("serve_Blur", 1182);`

`mjpegServer2.setSource(outputStream);`

**Referans sayımı**

Tüm cscore nesneleri dahili olarak referans sayılır. Bir havuzun kaynağa bağlanması kaynağın referans sayısını artırır, bu nedenle havuzu kapsam dahilinde tutmak kesinlikle gereklidir. CameraServer sınıfı, CameraServer işlevleriyle oluşturulmuş tüm nesnelerin bir kaydını tutar, bu şekilde oluşturulan kaynaklar ve havuzlar etkin bir şekilde kapsam dışında kalmaz \(açıkça kaldırılmadıkça\).

### 2017 Vision Örnekleri

#### LabVIEW

2017 LabVIEW Vizyon Örneği, diğer LabVIEW örnekleriyle birlikte sunulmuştur. Açılış ekranından, Destek -&gt; FRC Örneklerini Bul'u veya başka herhangi bir LabVIEW penceresini tıklayın,  Help-&gt;Find Examples  tıklayın ve 2017 Vision Örneğini bulmak için Vision klasörünü bulun. Örnek görüntüler örnekle birlikte paketlenmiştir.

**C++/Java**

Daha eksiksiz bir örnek yakında geliyor.

Şimdilik, bir GRIP projesini ve aşağıdaki açıklamayı ve[ TeamForge'da bulunabilecek bir ZIP'e eklenmiş örnek görüntüleri sağladık.](https://usfirst.collab.net/sf/frs/do/viewRelease/projects.wpilib/frs.sample_programs.2017_c_java_vision_sample?_message=1483834990405)

GRIP tarafından oluşturulan kodun robot programınıza entegre edilmesiyle ilgili ayrıntılar burada:  [Using Generated Code in a Robot Program](https://wpilib.screenstepslive.com/s/currentCS/m/24194/l/674733-using-generated-code-in-a-robot-program)

GRIP projesi tarafından oluşturulan kod, bu ZIP'in Görsel Görüntüler klasöründe bulunanlar gibi resimlerdeki yeşil parçacıklar için OpenCV konturlarını bulacaktır. Burada, hedef olup olmadığını değerlendirmek için bu konturları daha fazla işlemek isteyebilirsiniz. Bunu yapmak için:

1. Sınırların etrafında sınırlayıcı dikdörtgenler çizmek için boundingRect yöntemini kullanın
2. LabVIEW örnek kodu, hedef için 5 ayrı oran hesaplar. Bu oranların her biri, nominal olarak 1.0'a eşit olmalıdır. Bunu yapmak için, konturları boyutlarına göre sıralar, sonra en büyük ile başlayarak, bu değerler, hedef olabilecek her bir çift kontür için hesaplar ve bir hedef bulduğunda veya bulduğu en iyi çifti döndürürse durur.

Aşağıdaki formüllerde, bir harfin ardından bir harf, birinci konturun sınırlayıcı rektininin koordinatını ifade eder, ki bu, ikiden daha büyük olan \(örneğin, 1L = 1. kontur sol kenar\) ve bir harfle 2, 2 konturdur. \(H = Yükseklik, L = Sol, T = Üst, B = Alt, W = Genişlik\)



* Grup Yüksekliği = 1H / \(\(2B - 1T\) \* .4\) Üst yükseklik toplam yüksekliğin% 40'ı \(4 inç / 10 inç\) olmalıdır.
* dTop = \(2T - 1T\) / \(\(2B - 1T\) \* .6\) Üstteki şeridin  tepesine kadar toplam yüksekliğinin% 60'ı \(6 inç / 10 inç\) olmalıdır.
* LEdge = \(\(1L - 2L\) / 1W\) + 1 Kontürün \(1\) sol kenarı ile konturun \(2\) sol kenarı arasındaki mesafe, 1. konturun genişliğine göre küçük olmalıdır**.**o zaman, oranın ortalanmasını sağlamak için 1 ekliyoruz.
* Genişlik oranı = 1W / 2W Her iki konturun genişliği de aynı olmalıdır.
* Yükseklik oranı = 1H / \(2H \* 2\) Daha büyük şerit, daha küçük olandan iki kat daha uzun olmalıdır.

Bu oranların her biri daha sonra hesaplanarak 0-100 arasında bir değere dönüştürülür: 100 - \(100 \* abs \(1 - Val\)\)

Mesafeyi belirlemek için, üst sınırlayıcı kutunun üstünden pikselleri alt sınırlayıcı kutunun altına ölçün

distance = Hedef yükseklik ft. \(10/12\)  _YRes / \(2_  PixelHeight \* tan \(kamera görünümü\)\)

LabVIEW örneği, yuvarlak hedefin kenarlarının algılamadaki gürültüye en fazla maruz kalması nedeniyle yüksekliği kullanır \(kameradan daha fazla olan açı noktaları daha az yeşil gözüktüğü için\). Bunun dezavantajı, görüntüdeki hedefin piksel yüksekliğinin kamera açısının perspektif bozulmasından etkilenmesidir. Olası düzeltmeler şunları içerir:

1. Bunun yerine instead kullanmayı deneyin
2. Çeşitli mesafelerde  ölçüm yapın ve bir lookup tablosu veya regresyon fonksiyonu oluşturun
3. Kamerayı bir servoya monte edin, görüntüde dikey olarak hedefi ortalayın ve mesafe hesaplaması için servo açısını kullanın \(kendinize uygun bir şekilde çalışmanız veya bir matematik öğretmeni bulmanız gerekir!\)
4. OpenCV kullanarak perspektif bozulmasını düzeltin. Bunu yapmak için kameranızı OpenCV ile burada açıklandığı şekilde kalibre etmeniz gerekecektir: [http://docs.opencv.org/2.4/doc/tutorials/calib3d/camera\_calibration/camera\_calibration.html](http://docs.opencv.org/2.4/doc/tutorials/calib3d/camera_calibration/camera_calibration.html) Bu, distorsiyon matrisine ve kamera matrisine neden olacaktır. Bu iki matrisi alıp, mesafe hesaplaması için ölçmek istediğiniz noktaları "doğru" koordinatlarla eşleştirmek için undistortPoints fonksiyonu  kullanmalısınız \(bu, tüm görüntüyü bozmadan çok daha az CPU kullanımınıa sahip\).

