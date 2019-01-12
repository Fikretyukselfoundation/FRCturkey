# Eclipse Kurulumu \(Java ve C++ için\)

### **Eclipse’i Yüklemek \(C++/Java\)** <a id="eclipsei-yueklemek-c-java"></a>

Java ve C++ olan mevcut metin-bazlı programlama dilleri, Eclipse isimli programın güncel sürümünü programlama ortamı olarak kullanır. Seçilen programlama dilinin FRC’ye özel araçları Eclipse programına eklenti olarak yüklenir. Eclipse’in aynı yüklemesine hem Java hem de C++ araçları yükleyebilir, bu sayede aynı araçlar ve arayüzü kullanarak iki dil ile de çalışabilirsiniz.

Eclipse eklentileri Eclipse Luna, Eclipse Mars, Eclipse Neon, ve Eclipse Oxygen sürümleri ile test edilmiştir. 2017 kurulumları halen mevcut olan takımlar yüklemelerini 2018 için güncelleyebilirler. Eğer otomatik güncelleştirme seçeneği aktifse, Eclipse açıldığında güncelleştirme yapmak isteyip istemediğinizi sorar. Bu soruya cevap vererek, veya aşağıdaki eklentileri güncelleme talimatlarını takip ederek güncellemelerinizi yapabilirsiniz. C++ ile çalışan takımlar da yeni araç takımını kurmalıdır \(C++ araç takımının kurulumu\).

WPILib’den CAN Talon SRX çıkarılmıştır. Daha detaylı bilgi ve CTRE araç takımı yükleyicisi için bu adrese bakın. [http://www.ctr-electronics.com/control-system/hro.html\#product\_tabs\_technical\_resources](http://www.ctr-electronics.com/control-system/hro.html#product_tabs_technical_resources)

Not: C++ ve Java araçları ve ortamı Windows, Mac OSX ve Linux için mevcuttur. Fakat Windows sürümü en çok test edilmiştir. Geliştirme yapmak için bu üçünden herhangi birini kullanabilirsiniz. Fakat Sürücü İstasyon yazılımını ve roboRIO imajlama araçları için Windows işletim sistemli bir bilgisayara ihtiyacınız olacağını unutmayın.

Uyarı: Java 9 mevcut FRC araçları tarafından desteklenmemektedir. Java 9 Java’nın eski sürümlerindeki bazı özelliklerini devre dışı bırakmış ve bizim desteklemek durumunda olduğumuz 32-bit sistem desteğini çekmiştir. 2018 yılında Java 9 desteklenmeyecektir.

### Java'yı Edinmek

![x86 veya x64 Eclipse s&#xFC;r&#xFC;m&#xFC;yle e&#x15F;le&#x15F;melidir.](../.gitbook/assets/image%20%2837%29.png)

Eclipse kullanmak için sisteminizde Java 8 JDK bulunmalıdır. Java’yı bu adresten edinebilirsiniz: [http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). Java 9’u seçmeyin!

Sayfayı "Java SE Development Kit" \(Java SE Geliştirme Kiti\) yazan yere kadar aşağıya sürükleyin. Lisans sözleşmesini kabul edin ve bilgisayarınız için Java SDK’yı indirin. Sürüm \(x86 veya x64\) halihazırda yüklediğiniz veya yüklemeyi düşündüğünüz Eclipse sürümüyle eşleşmelidir. Java SE 8u151 sürümü test edilmiştir ancak bundan sonraki sürümler ile de çalışması muhtemeldir.

RoboRIO’ya Java 8 yüklenmiştir ve RoboRIO’nun bütün özelliklerinden faydalanabilmek için sizin de sisteminize Java 8 yüklemeniz tavsiye edilir. Daha eski sürümlerini de kullanabilirsiniz, ancak konsol çıktılarını görüntülemek için kullanılan rioLog Eclipse eklentisi ve bazı diğer WPILib araçları \(gösterge tablosu vs.\) Java 8 gerektirmektedir.

Not: C++ ile geliştirme yapacak olsanız bile geliştirme programı olan Eclipse bir Java yazılımı olduğundan Java yüklemeniz gerekmektedir. Ayrıca yukarıda adresi verilen Oracle web sitesi zamanla değişebilir, dolayısıyla yukarıdaki görsel ile web sitesi birebir aynı olmayabilir.  


### C++ araçlarının kurulumu \(Sadece C++ kullanan takımlar için\)

![!Daha &#xF6;nceki sezonlardan Toolchain y&#xFC;klemesi yapt&#x131;ysan&#x131;z, bunlar&#x131; Y&#xFC;kle/Kald&#x131;r arac&#x131;l&#x131;&#x11F;&#x131;yla kald&#x131;r&#x131;p yenisini y&#xFC;klemelisiniz.](../.gitbook/assets/image%20%2871%29.png)

İşletim sisteminiz için uygun olan C++ Toolchain yükleyicisini bu adresten bulabilirsiniz [http://first.wpi.edu/FRC/roborio/toolchains/](http://first.wpi.edu/FRC/roborio/toolchains/)​

Windows: İndirdiğiniz dosyayı çalıştırmak için çift tıklayın. Güvenlik uyarısı görürseniz Çalıştır \(Run\) düğmesini tıklayın. Lisans sözleşmesini kabul etmek için kutuyu tıklayın, ve Yükle \(Install\) düğmesini tıklayın. Yükleme işlemi tamamlandığında Bitir \(Finish\) düğmesini tıklayın.Mac OSX: İndirdiğiniz dosya arşivini açmak için Finder programında dosyayı bulup çift tıklayın. Finder’da, "FRC ARM Toolchain.pgk" dosyasını sağ tıklayın, klavyedeki option \(seçenek\) düğmesini basın, ve Aç \(Open\) düğmesini tıklayın. Paketi yüklemek için adımları takip edin.

### Eclipse indirin

Linux: C++ araçlarının kurulumu sayfasındaki metin dosyasında yükleme talimatlarını bulabilirsiniz.

**! Windows araç takımları her zaman sistem sürücünün kök klasörüne yüklenecektir.**

**Eclipse’i indirmek**

![](../.gitbook/assets/image%20%283%29.png)

Bu yazının yazılma tarihindeki güncel sürüm olan Oxygen \(4.7\) araçların geliştirilmesinde kullanılmaktadır. Bir sonraki ekranda indirilme yapılacak konumu seçin ve indirme işlemini başlatın. İndirilenler klasörü gibi bir konumu zip arşivi için seçin.

Not: 64 bit Linux işletim sistemlerinde libc kütüphanesinin 32 bit sürümünü yüklemek gerekebilir. Bunu yüklemek için, örneğin Ubuntu Linux sisteminde, aşağıdaki konumu kullanabilirsiniz.

**`sudo apt-get install libc6-i386`**

Bu 32 bit linux için derlenmiş eklentilerin gcc bileşenlerinin çalıştırılabilmesi için gereklidir.

Eclipse artık .exe yükleyiciler de sağlamaktadır. İlerideki birkaç talimat değişikliği haricinde bunlar da hatasız çalışır.  


### Eclipse'i ilk kez çalıştırmak

**Eclipse RAR dosyasını açın ve içeriğini Program Dosyaları’na çıkarın**

![Zip dosyas&#x131;n&#x131;n i&#xE7;eri&#x11F;ini &#xE7;&#x131;karmak i&#xE7;in windows taray&#x131;c&#x131;s&#x131; penceresinde dosyay&#x131; sa&#x11F; t&#x131;klay&#x131;n ve &quot;Hepsini &#xE7;&#x131;kar...&quot; se&#xE7;ene&#x11F;ini se&#xE7;in. &#xC7;&#x131;karma konumu soruldu&#x11F;unda varsay&#x131;lan se&#xE7;ene&#x11F;i se&#xE7;in.](../.gitbook/assets/image%20%2896%29.png)

 **RAR'dan çıkarılmış eclipse klasörünü Program Dosyaları’na taşıyın**  


![](../.gitbook/assets/image%20%28104%29.png)

 Arşivden çıkarılmış klasörü Program Dosyaları, veya kolayca çalıştırmak için daha uygun bir yere taşıyın. Eclipse klasörünün içinde ‘eclipse.exe’ dosyasını göreceksiniz. Bu dosyayı sağ tıklayınca ortaya çıkan ‘Başlangıç menüsüne tuttur’ seçeneği ile programı çalıştırmayı daha kolay bir hale getirebilirsiniz.

 **Eclipse’i ilk kez çalıştırmak**  


![](../.gitbook/assets/image%20%2852%29.png)

 Eclipse’i başlatın \(eğer bir önceki adımda tutturmayı seçtiyseniz başlangıç menünüzde olacaktır.\) Eclipse ilk kez çalıştırıldığında size ‘çalışma alanı’ \(workspace\) için bir konum soracaktır. Çalışma alanı varsayılan olarak projelerin ve dosyaların saklandığı yerdir. Birden fazla çalışma alanınız olabilir, fakat Eclipse ile daha fazla deneyiminiz olana kadar varsayılan konumu seçmenizi öneririz.

### Java Eclipse’ine C++ eklemek

![&#x2018;E&#x11F;er C++ men&#xFC;de yoksa s&#x131;radaki ad&#x131;m&#x131; uygulay&#x131;n. E&#x11F;er varsa s&#x131;radaki ad&#x131;m&#x131; atlayabilirsiniz.&#x2019;](../.gitbook/assets/image%20%2848%29.png)

 Eclipse’in Java sürümünü kullanarak C++ programlaması yapabilmek için C++ Geliştirme Araçları’nı \(C++ Development Tools \(CDT\)\) yüklemeniz gerekmektedir. Bunun halihazırda yüklü olup olmadığını görmek için menüden Pencere’yi \(Window\) ve daha sonra Seçenekler’i \(Preferences\) seçin. Daha sonra sol tarafta C/C++ seçeneğine bakın. Eğer orada değilse yüklemeniz gerekmektedir. Yükleme aşaması bir sonraki aşamada anlatılmıştır.

### Eclipse C++ Geliştirme Araçlarını Yükleyin

![](../.gitbook/assets/image%20%2819%29.png)

Eğer önceki aşamada anlatılan C/C++ menüde yoksa, bu mutlaka yüklenmelidir.

1. Seçenekler \(Preferences\) penceresini eğer açıksa kapatın.

2. Üstteki menü barından Yardım \(Help\) ve daha sonra Yeni Yazılım Yükle \(Install New Software\) seçin.

3. Açılır listeden gösterildiği gibi Neon’u seçin.

4. Programlama Dilleri’ne \(Programming Languages\) gelin ve oku tıklayarak menüyü açın.

5. C++ Geliştirme Araçları’nı \(Eclipse C++ Development Tools\) seçin.

6. Devam \(Next\) düğmesini tıklayın.

7. Diğer seçenekleri olduğu gibi bırakın ve Eclipse’in yeniden başlamasına izin verin.

Bu adımlar tamamlanıp Eclipse yeniden başladığında Seçenekler menüsünde C++ görülebilir olmalı, ve bütün C++ perspektifleri gelmiş olmalıdır.  


### **C++ Eclipse java eklemek**

![E&#x11F;er Java men&#xFC;de yoksa bu ad&#x131;m&#x131; yap&#x131;n, varsa atlay&#x131;n.](../.gitbook/assets/image%20%2832%29.png)

 Eclipse’in C++ sürümünü kullanarak Java ile programlama yapabilmek için Java Geliştirme Araçları’nı \(Java Development Tools, JDT\) yüklemeniz gerekmektedir. Halihazırda yüklü olup olmadığını görmek için yukarıdaki menü bardan Pencere \(Window\) ve daha sonra Seçenekler \(Preferences\) seçeneklerini tıklayın. Sol tarafta Java seçeneği olup olmadığına bakın. Eğer yoksa bunu yüklemelisiniz. Yükleme talimatları bir sonraki aşamada verilmiştir. Eğer bu zaten yüklüyse bir sonraki aşamayı atlayıp Eclipse’te JDK’yı ayarlama aşamasına atlayabilirsiniz.

### Java Geliştirme Araçlarının Yüklenmesi

![](../.gitbook/assets/image%20%2849%29.png)

Önceki adımdaki seçenekler menüsünde Java görünmüyorsa, bu mutlaka yüklenmelidir.

1. Seçenekler penceresini eğer açıksa kapatın.

2. Yukarıdaki menü barından sırasıyla Yardım \(Help\) ve Yeni Yazılım Yükle \(Install New Software\) seçin.

3. Açılır listeden Neon seçeneğini tıklayın.

4. Programlama dillerine gelin ve yanındaki ok işaretini tıklayın.

5. Eclipse Java Geliştirme Araçları’nı \(Eclipse Java Development Tools\) seçin.

6. Devam’ı \(Next\) tıklayın.

7. Diğer seçenekleri olduğu gibi bırakın ve Eclipse’in yeniden başlamasına izin verin.

Bu adımlar tamamlandığında ve Eclipse yeniden başladığında seçenekler penceresinde Java görülebilir olmalı, ve bütün Java perspektifleri aktif hale gelecektir.

### Eclipse JDK ayarlanması \(Sadece Java kullanan takımlar için\)

![](../.gitbook/assets/image%20%2874%29.png)

1. Yukarıdaki menü bardan Pencereler’i \(Windows\) ve daha sonra Seçenekler’i \(Preferences\) seçin.

2. Soldaki menüden önce Java Seçenekleri’ni \(Java Preferences\) ve daha sonra Yüklü JREler’i \(Installed JREs\) seçin.

3. Yüklü JDK’nın resimde gösterildiği gibi seçildiğine emin olun \(İsimler \(name\) aynı olsa da konumda \(location\) jdk 8 veya 1.8 geçtiğine bakın\). Bu sayede eclipse RoboRIO için Java programlar derleyebilecektir. Bu doğru olarak ayarlanmazsa JRE konumuyla ilgili bir hata mesajı görürsünüz.

Eğer bu aşamada hiçbir dosya görmüyorsanız, veya gösterilen tek dosyanın konumunda \(location\) jdk geçmiyorsa bir sonraki aşamayı uygulayın. Aksi halde bir sonraki aşamayı atlayabilirsiniz.

### Eclipse'i yapılandırma

![](../.gitbook/assets/image%20%28106%29.png)

Eclipse için çok sayıda yapılandırma seçeneği vardır. Not etmek için önerilen bir ayar: "oluşturmadan önce otomatik olarak kaydedin."Bu ayar, projeyi oluşturduğunuzda" çalışma alanınızın " tüm değişikliklerinin kaydedilmesine neden olacaktır. Bunu istemiyorsanız, yapılandırmadan önce değişiklikleri kaydetmeyi unutmayın, aksi takdirde yeniden oluşturulmuş program en yeni güncellemelerinizi yansıtmayacaktır.

Bunu ayarlamak için, **Window** -&gt; **Preferences** -&gt; **General** -&gt; **Workspace** -&gt; Check **Save automatically before build** -&gt; **OK**

### Geliştirme eklentilerini yükleme - Seçenek 1: Çevrimiçi Kurulum

![](../.gitbook/assets/image%20%2834%29.png)

Eklentileri aktif bir internet bağlantısı gerektiren ve eklentileri doğrudan Wpılıb sitesinden getiren bu yöntemi kullanarak yüklemeniz önerilir. Bu, Eclipse kullanarak eklentilerin güncellemelerini kontrol etmenizi sağlayacaktır.

Eclipse uzantıları kullanıcı tarafından yüklenmiş eklentileri dayanmaktadır. Wpılıb geliştirme araçlarını almak için diliniz için doğru eklentiyi yüklemeniz gerekir.

Eclipse başladığında:

​

1. Yukarıdan "Help" seçeneğini seçin.
2. "Install new software" seçeneğine tıklayın.
3. Buradan bir yazılım güncelleme sitesi, yani eklentilerin indirileceği yeri eklemeniz gerekir. "add" tıklayın daha sonra " Add Repository" kutusunu doldurun:
4. Name: FRC Plugins
5. Location: [http://first.wpi.edu/FRC/roborio/release/eclipse/​](http://first.wpi.edu/FRC/roborio/release/eclipse/)
6. "OK" butonuna tıklayın.

#### Doğru Eklentileri Seçme <a id="selecting_the_correct_plugins"></a>

![](../.gitbook/assets/image%20%2827%29.png)

Wpilib Robot Development menüsünü genişletmek için ok tuşunu tıklatın.

Kullanmak istediğiniz dile göre Wpilib Robot Development içerisinden dilinizi seçin \(her iki dilde de programlamayı kullanmak isterseniz ikisini de yükleyebilirsiniz\)

Next&gt; tıklatın, sonraki sayfada, lisans sözleşmesini kabul etmek için ok düğmesini tıklatın ve Finish' i tıklatın

Bir güvenlik Uyarısı alırsanız, devam etmek için ok butonuna basın tıklatın.

Bütün bu işlemleri tamamladıktan sonra Eclipse'i yeniden başlatın. Eclipse yeniden başlatıldıktan sonra workspace \(istenirse\) seçtikten sonra Java'yı yükleyen bir iletişim kutusu göreceksiniz. Bu, eklentilerin kurulum ilerlemesini ayrıntıları, devam etmeden önce yüklemenin tamamlanmasını bekleyin. Bu iletişim kutusu yalnızca eklentiler ilk yüklendiğinde veya güncelleştirildiğinde görünmektedir.

Güncellenmiş eklentiler de bir problem olursa, bu işlemi tekrarlayabilirsiniz \(bileşenlerin zaten yüklü olduğunu ve bir yükleme yerine bir yükseltme gerçekleştirileceğini söyleyen bir ek Eclipse penceresi alırsınız\) veya çevrimiçi yükleme bir seçenekse, yukarıdaki çevrimiçi yükleme adımlarını tamamlayabilir, daha sonra Eclipse otomatik güncellemelerini kullanarak gelecekteki güncellemeleri alabilirsiniz \(veya çevrimdışı adımları tekrarlayabilirsiniz.\)

### Geliştirme eklentilerini yükleme - 2. Seçenek: Çevrimdışı kurulum

![](../.gitbook/assets/image%20%2859%29.png)



Eklentileri indirmeniz ve bunları çevrimdışı olarak farklı bir makineye yüklemeniz gerekiyorsa, Eclipse kullanarak güncellemeleri kontrol edemezsiniz. gelen eklentileri içeren zip dosyasını indirin [http://first.wpi.edu/FRC/roborio/release](http://first.wpi.edu/FRC/roborio/release) / zip üzerine sağ tıklayın ve dosyaları çıkart seçeneğini kullanın.

Eclipse başladığında:

1. "help" seçeneğini seçin.
2. "Install new software" seçeneğine tıklayın
3. Buradan bir yazılım güncelleme sitesi, yani eklentilerin indirileceği yeri eklemeniz gerekir. "add" tıklayın daha sonra " Add Repository" kutusunu doldurun:
4. Name: FRC Plugins Offline
5. Local butonuna tıklayın
6. Zip dosyasını çıkardığınız dizinin içindeki "site" dizinini seçin.
7. "ok" butonuna tıklayın.

### Manuel olarak eklentileri güncellemek <a id="manuel-olarak-eklentileri-guencellemek"></a>

![](../.gitbook/assets/image%20%2825%29.png)

Not: eğer eklentileri 1. seçenek çevrimiçi kurulum ile kurduysanız bu adımı yapmanıza gerek yoktur.

Yukarıda açıklandığı gibi otomatik güncelleştirmeleri etkinleştirmemeyi seçerseniz, eklentilerin yeni sürümlerini yüklemek için güncelleştirmeleri el ile denetlemeniz gerekir.

Menü çubuğundan Help'i seçin ve ardından "Check for Updates" seçeneğine tıklayın. Eclipse, yüklü herhangi bir eklentinin daha yeni bir sürümü olup olmadığını kontrol eder ve bir güncelleme bulursa sizi bilgilendirir. Eklentilerin güncellenmesi, geliştirme araçlarının en son sürümü ile güncel olmasını sağlayacaktır.

