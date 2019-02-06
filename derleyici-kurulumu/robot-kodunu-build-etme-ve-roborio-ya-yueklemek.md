# Robot kodunu build etme ve RoboRIO 'ya yüklemek

![Robot ba&#x15F;lang&#x131;c&#x131;nda &#xE7;al&#x131;&#x15F;mas&#x131; i&#xE7;in program y&#xFC;kleniyor](../.gitbook/assets/image%20%28119%29.png)

Programınızın roboRIO önyüklemesinin her defasında otomatik olarak çalışmasını sağlamak için, kodunuz roboRIO flash belleğe yüklenmelidir. Böylece robot yeniden başlatıldığında ve maç oynandığında kodunuz sürekli orada olacaktır. Bunu yapmak için:

1. Projenizin adına sağ tıklayın
2. "Run As" tıklayın
3. Ardından "WPILib Java Deploy" tıklayın

Ardından deployement  süreci başlayacak ve Konsol durum bilgisinin çıktısını almaya başlayacaktır \(genellikle pencerenin alt orta veya sağ alt köşesinde bulunur\). **Program deploy edildikten hemen sonra çalışacaktır , roboRIO'yu yeniden başlatmaya gerek yoktur.**

### **Programı yükleme süreci**

"WPILib Java Deploy" butonuna bastıktan sonra deploy süreci bir kaç adımdan geçecektir.

![Temizleme](../.gitbook/assets/image%20%2875%29.png)

Deploy sürecinizin temiz bir şekilde çalışması için , herhangi bir bug ile karşılaşmamanız için gereklidir.

![Derleme ve Jar](../.gitbook/assets/image%20%2885%29.png)

Ardından kod bir jar dosyasında derlenir ve paketlenir.

![Hedef IP adresini bulmak](../.gitbook/assets/image%20%2865%29.png)

 Bu adımda, eklenti roboRIO'nun IP'sini belirler. Eklenti, aşağıdaki yöntemlerle roboRIO'ya ulaşmaya çalışacaktır:

1. mDNS \(roboRIO-TEAM-FRC.local  TEAM sizin takım numaranızdır\)
2. DNS \(roboRIO-TEAM-FRC.lan where TEAM sizin takım numaranızdır\)
3. USB static IP \(172.22.11.2\)
4. Ethernet static IP \(10.TE.AM.2\)

RoboRIO bu yöntemlerden herhangi biri aracılığıyla erişilebilir ise, Deploy işlemi hedef olarak bu adresi kullanarak devam edecektir. RoboRIO bu yöntemlerden herhangi birini kullanarak bulunamazsa, Deploy işlemi iptal olur. Aşağıdaki sorun giderme bölümünde bağlantı hatalarına bakın.



![Ba&#x11F;&#x131;ml&#x131;l&#x131;klar](../.gitbook/assets/image%20%28120%29.png)

Bağımlılıklar bölümü roboRİO işlem için gerekli  bağımlılıkları denetler. Şu anda Java için, bu iki bileşenden oluşur:



1. RoboRIO image sürümünü kontrol edin. Doğrulama başarılı olursa, " version was validated and the deploy will proceed" diye bir mesaj alırsınız. Doğrulanma başarısız olursa "indicating the allowed image version for this version of the plugins, see [Image Check Failure under Troubleshooting](https://wpilib.screenstepslive.com/s/currentCS/m/cpp/l/145320-building-and-downloading-a-robot-project-to-the-roborio#) below for more details" şeklinde bir mesaj görürsünüz. 
2. JRE roboRIO yüklü olduğunu kontrol edin. Eğer Değilse, eklentiler tarafından otomatik olarak deploy edilir.

![Kodu kopyalama ve ba&#x15F;latma](../.gitbook/assets/image%20%2854%29.png)

Deploy işleminin kalan kısımları aşağıdaki gibi devam eder:

1. Kullanıcı programını ve robotCommand dosyası kopyalanır. RobotCommand dosyası, başlatılarak kodu başlatan çerçeveyi anlatır \(C ++ ile Java ve Debug vs deploy\).
2. Kodu başlat. Eklenti, mevcut kullanıcı programını silen ve daha sonra LabVIEW RT işlemini yeniden başlatan bir komut dosyası dosyasını çağırır. Bu, kodu, roboRIO başlatıldığında otomatik oarak başlatılacağı şekilde çalıştırır.

### **Kodun Deploy edilmesi**

Robot çalışmaya başladığı andan itibaren kod otomatik olarak içerisinde bulunacaktır. Robotu her açıp kapattığınızda kodu deploy etmenize gerek yoktur. 

### **Sonraki Adımlar**

Radionun configure edilmesi [Programming your radio for home use](https://wpilib.screenstepslive.com/s/currentCS/m/getting_started/l/144986-programming-your-radio) 

###  <a id="troubleshooting"></a>

