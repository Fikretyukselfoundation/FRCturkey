# Limelight ile Vision Processing

### Limelight Nedir?

Limelight, _FIRST_ Robotik Yarışması için özel olarak tasarlanmış bir tak-çalıştır akıllı kameradır . Vision Processing tecrübesi gerektirmez. Vision deneyimi olmayan veya yeterli seviyede mentör hocaları olmayan takımlar için Limelight yeterince kolaydır. Vision processing'e ihtiyaç duyan takımlar için bir alternatiftir.

![Limelight Kamera](../.gitbook/assets/image%20%28118%29.png)

![](../.gitbook/assets/image%20%2893%29.png)



### **Montaj**

Limelight'ınızı monte etmek için nylock somunlarıyla birlikte dört adet 1/4 ”10-32 vida kullanın.

### Kablolama

**Not : Limelight bir 12V giriş alır, ancak 6V'a kadar çalışacak şekilde üretilmiştir. LED'leri 7V'a kadar sabit bir parlaklığa sahiptir.**

* Voltaj Regülatörü Modülü ile kullanmayın!
* Limelight iki kablonnuzu PDP'nizdeki herhangi bir yuvaya geçirin.
* herhangi bir sigortayı \(5A, 10A, 20A, vb.\) PDP'deki Aynı yuvaya ekleyin.
* Limelight'ınızdan bir ethernet kablosunu robot radyonuza bağlayın.

### Image etmek

* Windows7 kullanmayın.
* Güç kaynağınızdan çıkarın.
* En güncel flash aracını ve image [İndirme Sayfası](https://limelightvision.io/pages/downloads)'ndan indirin.
* Flash aracını kurun.
* Dizüstü bilgisayarınızdan bir USB-MicroUSB kablosuyla ışığınızı ayarlayın.
* Limelight'e güç verin.
* Windows arama çubuğundan “Limelight Flash Tool” uygulamasını çalıştırın. Ayrıca, başlangıç menüsü uygulamaları klasörünün altındaki “Limelight” klasörünün altında görünecektir.
* Windows'un kamerayı tanıması 20 saniye kadar sürebilir. Sonraki tüm Windows diyaloglarında “iptal” tuşuna basın.
* İndirilenler klasörünüzdeki en son .zip dosyasını seçin
* “Limelights” menüsünde bir “RPI” cihazı seçin
* “Flash” a tıklayın
* Bir kez yanıp sönme tamamlandığında, güç kaynağınızdaki gücü kaldırın

![](../.gitbook/assets/image%20%28117%29.png)

### Ağ Kurulumu

Güvenilirlik için statik bir IP adresi almamıza rağmen, bazı ekipler dinamik olarak atanan IP adreslerini tercih eder.

Statik IP Adresi \(önerilir\)

* Bonjour'u [https://support.apple.com/kb/dl999?locale=en\_US](https://support.apple.com/kb/dl999?locale=en_US) adresinden yükleyin
* Robotunuza güç verin ve dizüstü bilgisayarınızı robotunuzun ağına bağlayın.
* Limelight'ınızın LED dizisini yaktıktan sonra, http: //limelight.local:5801'e gidin. Burası Limelight yönetim Paneli
* "Networking" sekmesine gidin.
* Takım numaranızı girin.
* “IP Assignment” ınızı “Static” olarak değiştirin.
* Limelight'ın IP adresini “10.TE.AM.11” olarak ayarlayın.
* Ağ Maskesini “255.255.255.0” olarak ayarlayın.
* Ağ Geçidini “10.TE.AM.1” olarak ayarlayın.
* Update butonuna basın.
* Robotunuza güç verin.
* Artık http://10.TE.AM.11:5801 adresindeki yönetim panelinize ve kamera akışınıza http://10.TE.AM.11:5800 adresinden erişebileceksiniz.

### Temel Programlama

Şimdilik, sadece kameradan robotunuza veri almamız gerekiyor. 100hz'de Ağ Tablolarına veri hedefleyen Limelight yayınları. NetworkTable'lar için varsayılan güncelleme hızı 10hz'dir, ancak Limelight daha sık veri aktarımına izin vermek için otomatik olarak üzerine yazar.

Başlamak için, “limelight” Ağ Tablosundan en az 100hz'de dört değer okumayı öneririz. Kod örneklerimiz bunu nasıl yapacağınızı tam olarak gösterir. Hedefinize olan uzaklıklar \(derece olarak\) “tx” ve “ty” olarak gönderilir. Bunlar robotunuzu döndürmek, tareti çevirmek vb. İçin kullanılabilir. Hedef alan, “ta” olarak gönderilir, hedefinize uzak bir mesafe göstergesi olarak kullanılabilir. Alan, 0 ile 100 arasında bir değerdir, burada 0, hedefinizin gövde alanı, toplam görüntü alanının% 0'ı anlamına gelir ve 100, hedefinizin gövdesinin tüm görüntünün dolduğu anlamına gelir. Hedefinizin dönüşü veya “eğilmesi” “ts” olarak döndürülür. Tüm değerler sıfıra eşitse, hedef yoktur.

Ek olarak, NetworkTables'taki değerleri ayarlayarak belirli özellikleri kontrol edebilirsiniz.

“Limelight” tablosundan aşağıdakileri okuyabilirsiniz:

| tv | Limelight'ın geçerli herhangi bir hedefi olup olmadığı \(0 veya 1\) |
| :--- | :--- |
| tx | Crosshair'den Hedefe Yatay Ofset \(-27 derece ila 27 derece\) |
| ty | Crosshair'den Hedefe Dikey Ofset \(-20.5 derece ila 20.5 derece\) |
| ta | Hedef Alan \(görüntünün% 0'ı görüntünün% 100'ü\) |
| ts | yamukluk veya açı \(-90 derece 0 derece\) |
| tl | Gecikme süresi \(ms\) Görüntü yakalama gecikmesi için en az 11ms ekleyin. |

Aşağıdakileri “limelight” tablosuna yazabilirsiniz :



| ledMode | Led Durumunu Ayarlama |
| :--- | :--- |
| 0 | Aç |
| 1 | Kapat |
| 2 | Aç Kapa\(Blink\) |



| camMode | Limelight Çalışma Modunu Ayarlama |
| :--- | :--- |
| 0 | Görüntü işlemeyi etkinleştir |
| 1 | Sürücü kamerası \( Görüntü işlemeyi durdurur\) |

| pipeline | Limelight'in pipeline hattını ayarlar |
| :--- | :--- |
| 0 .. 9 | 0 ile 9 arasında bir seçim yapabilirsiniz. |



#### Java

```java
NetworkTable table = NetworkTableInstance.getDefault().getTable("limelight");
NetworkTableEntry tx = table.getEntry("tx");
NetworkTableEntry ty = table.getEntry("ty");
NetworkTableEntry ta = table.getEntry("ta");
double x = tx.getDouble(0);
double y = ty.getDouble(0);
double area = ta.getDouble(0);
```

Bunları import etmeyi unutmayın : 

```java
import edu.wpi.first.networktables.NetworkTableEntry;
import edu.wpi.first.networktables.NetworkTableInstance;
```

**Labview:**

![](../.gitbook/assets/image%20%2836%29.png)

**C++**

```cpp
std::shared_ptr<NetworkTable> table =   NetworkTable::GetTable("limelight");
float targetOffsetAngle_Horizontal = table->GetNumber("tx");
float targetOffsetAngle_Vertical = table->GetNumber("ty");
float targetArea = table->GetNumber("ta");
float targetSkew = table->GetNumber("ts");
```

**Python**

```python
from networktables import NetworkTables

table = NetworkTables.getTable("limelight")
tx = table.getNumber('tx',None)
ty = table.getNumber('ty',None)
ta = table.getNumber('ta',None)
ts = table.getNumber('ts',None
```

**Java Hedef Hizalama**

```java
if (limelight.angleOffset > 10) { // eğer hedef solda kalıyorsa
    RightMotorMaster.setPower(0.30); // 30% throttle
    LeftMotorMaster.setPower(-0.30); // -30% throttle
} else if (limelight.angleOffset < -10) { // Eğer Hedef Sağda Kaalıyorsa
    RightMotorMaster.setPower(-0.30); // -30% throttle
    LeftMotorMaster.setPower(0.30); // 30% throttle
} else {
    // Robot hizalanmış yavaşça ileri gidin
    RightMotorMaster.setPower(0.30); // 30% throttle
    LeftMotorMaster.setPower(0.30); // 30% throttle
}
```



