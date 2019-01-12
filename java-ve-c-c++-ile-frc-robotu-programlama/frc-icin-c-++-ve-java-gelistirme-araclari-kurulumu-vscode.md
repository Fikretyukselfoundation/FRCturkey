# FRC için C ++ ve Java Geliştirme Araçları Kurulumu \(vscode\)

[GitHub'dan ](https://github.com/wpilibsuite/allwpilib/releases)Windows kurulumunuz için uygun yükleyiciyi \(32 bit veya 64 bit\) indirin.

Çalıştırmak için yükleyiciye çift tıklayın. Herhangi bir Güvenlik uyarısı görürseniz, Çalıştır  veya Daha Fazla Bilgi-&gt; Yine de Çalıştır'ı tıklayın.

#### Kurulum Tipi

![](../.gitbook/assets/image%20%2828%29.png)

Makinede Tüm Kullanıcılar için mi yoksa Geçerli Kullanıcı için mi yükleneceğini seçin. "All Users" seçeneği Yönetici ayrıcalıkları gerektirir, ancak tüm kullanıcı hesaplarının erişebileceği bir şekilde kurulur, Geçerli Kullanıcı yüklemesine yalnızca kurulduğu hesaptan erişilebilir.

Tüm Kullanıcılar'ı seçerseniz, görünen güvenlik istemini kabul etmeniz gerekir.

#### VSCode İndirin <a id="download-vscode"></a>

Lisanslama nedeniyle, yükleyici paketlenmiş VSCode yükleyicisini içeremez. VSCode yükleyicisini indir veya önceden indirilmiş bir kopya seçmek için VSCode Seç / İndir'i tıklatın. İnternet bağlantısı olmayan diğer makinelere kurmak istiyorsanız, indirme tamamlandıktan sonra, Çevrimdışı Yükleyici ile birlikte kopyalamak üzere dosya sistemindeki zip dosyasına götürmek için İndirilen Dosyayı Aç seçeneğine tıklayabilirsiniz.

![](../.gitbook/assets/image%20%2860%29.png)

#### Kurulumu çalıştırın

Tüm onay kutularının işaretli olduğundan emin olun \(bu makineye 2019 WPILib yazılımı önceden yüklediyseniz ve yazılım otomatik olarak işaretini kaldırmadıysa\), sonra Install seçeneğine tıklayın.  
  


![](../.gitbook/assets/image%20%2841%29.png)

#### Ne Yüklendi?



Çevrimdışı Yükleyici aşağıdaki bileşenleri yükler:

* Visual Studio Code - 2019 robot kodu geliştirme için desteklenen IDE. Çevrimdışı yükleyici, makinenizde VSCode olsa bile, WPILib geliştirme için ayrı bir VSCode kopyası hazırlar. Bu, WPILib kurulumunu çalıştıran ayarların bazılarının VSCode'u diğer projeler için kullanıyorsanız mevcut iş akışlarını bozabileceği için yapılır.
* C ++ Compiler - roboRIO için C ++ kodu oluşturma araçları
* Gradle - C ++ veya Java robot kodu oluşturmak / dağıtmak için kullanılan Gradle sürümü
* Java JDK / JRE - Java robot kodunu oluşturmak ve Java tabanlı Araçlardan herhangi birini Çalıştırmak için kullanılan, Java JDK / JRE'nin belirli bir sürümü. Bu, mevcut herhangi bir JDK kurulumuyla yan yana var ve JAVA\_HOME değişkeninin üzerine yazmıyor.
* WPILib Araçları - SmartDashboard, Shuffleboard, Robot Oluşturucu, Anahat Görüntüleyici, Pathweaver
* WPILib Bağımlılıkları - OpenCV, vs.
* VSCode Uzantıları - VSCode'da robot kodu gelişimi için WPILib uzantıları

#### Yüklenenler - Devam

Çevrimdışı Yükleyici, VSCode’un WPILib kopyasına bir Masaüstü Kısayolu da kurar ve bir komut kısayolu kurar, böylece VSCode’nin bu kopyası "frccode2019" komutunu kullanarak komut satırından açılabilir.

Bunların her ikisi de WPIlib C ++  Java araçları olarak belirli yıla referans veriyor, şimdi farklı mevsimlerden birden fazla ortamın yan yana kurulmasını destekliyor.

![](../.gitbook/assets/image%20%2824%29.png)

#### Bitirelim

Yükleyici tamamlandığında, şimdi VSCode'un WPILib sürümünü açabilecek ve kullanabileceksiniz. Herhangi bir 3. parti kütüphaneyi kullanıyorsanız, bunları robot kodunda kullanmadan önce bunları ayrı ayrı kurmanız gerekecektir.

![](../.gitbook/assets/image%20%2883%29.png)

