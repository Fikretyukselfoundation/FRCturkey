---
description: >-
  Raspberry Pi İle Pixy Kullanımı içeriğinde sağladığı kaynaklar için #6025
  Adroit Anroids takımına teşekkürler!
---

# Co-Processor - Cmucam5 Pixy

### Pixy Nedir?

Pixy, nesneleri en hızlı şekilde tanıtarak onları bulmak için tasarlanmış bütünleşik bir kamera modülü diyebiliriz.  Üzerinde NXP Firmasının LPC4330 Çift çekirdekli mikrodenetleyici, 204Mhz de çalışan 32bit ARM Cortex-M4 ün yanında ayrıca 204Mhz de çalışan 32bit ARM Cortex-M0 işlemcisi bulunmakta.Nesneleri tanıtmak için yapmanız gereken tek şey, nesneyi kameraya gösterip üzerinde bulunan butona basmak. Modül, turuncu, sarı, kırmızı,  mavi, açık mavi, yeşil ve mor olmak üzere 7 rengi tanıyabiliyor. **\(kaynak :** [**https://www.robimek.com/nesnelerin-rengini-ve-konumunu-algilayan-pixy-kamera-modul/**](https://www.robimek.com/nesnelerin-rengini-ve-konumunu-algilayan-pixy-kamera-modul/)**\)**  


![](../../.gitbook/assets/image%20%2895%29.png)

#### **Pixy Teknik Özellikleri**

**İşlemci:** NXP LPC4330, 204 MHz, dual core  
**Görüntü Sensörü:** Omnivision OV9715, 1/4″, 1280×800  
**Lens Görüş Alanı:** 75 degrees horizontal, 47 degrees vertical  
**Lens Tipi:** standard M12 \(several different types available\)  
**Güç Tüketimi:** 140 mA typical  
**Güç Girişi:** USB input \(5V\) or unregulated input \(6V to 10V\)  
**RAM:** 264K bytes  
**Flaş:** 1M bytes  
**Kullanılabilir Veri Çıktıları:** UART serial, SPI, I2C, USB, digital, analog  
**Boyutlar:** 2.1″ x 1.75″ x 1.4″

Bir megapixel çözünürlüğe sahip sensör,  60FPS’de 640×400\(WXGA\), 30FPS de ise 1280×800\(WXGA\) çözünürlükte resim alabiliyor.

## FRC Pixy

**Pixy Modülün Kullanımı ve Ayarları**

Pixy wiki sayfasını açın &gt;&gt;  [http://cmucam.org/projects/cmucam5/wiki/Latest\_release](http://cmucam.org/projects/cmucam5/wiki/Latest_release)

Wiki sayfasında PixyMon adlı programı işletim sisteminize uyumlu linkten indirin.

### **Pixy Modüle Nesne Tanıtma**

 Pixy ‘ye nesne tanıtma işlemini iki şekilde yapabilirsiniz. Birincisi PixyMon programı ile ekrandan fare ile nesneyi seçerek, ikincisi ise modül üzerinde bulunan butona basarak nesneyi tanıtabilirsiniz.

**1.Adım:** Şimdi öğretmek istediğiniz nesneyi Pixy’nin önünde tutun ve açılan menüden **Action-&gt; Set signature 1**‘i seçin.

[![](https://www.robimek.com/wp-content/uploads/pixymon-action.jpg)](https://www.robimek.com/wp-content/uploads/pixymon-action.jpg)

**2.Adım:** Fareyi kullanarak, Pixy’nin nesneyi öğrenmek için kullanmasını istediğiniz bölgeyi seçmek için tıklatın ve sürükleyin.

[![](https://www.robimek.com/wp-content/uploads/pxymon-nesne-se%C3%A7imi.jpg)](https://www.robimek.com/wp-content/uploads/pxymon-nesne-se%C3%A7imi.jpg)

**3.Adım:** Bölgeyi seçtikten sonra, Pixy, nesnenizi öğrenir ve otomatik olarak video moduna girer, böylece renk imzanızın ne kadar iyi çalıştığını doğrulayabilirsiniz.

[![](https://www.robimek.com/wp-content/uploads/pixymon-nesne-ogrenme.jpg)](https://www.robimek.com/wp-content/uploads/pixymon-nesne-ogrenme.jpg)

#### **Renk İmzası Ayarı**

Pixy, nesneyi daha net hatlarıyla tanıması için birkaç ayar yapılmalıdır. Dişli çark simgesini tıklayın ve Pixy Parameters altındaki Signature Tuning bölmesini seçin.

[![](https://www.robimek.com/wp-content/uploads/pixymon-ayar.jpg)](https://www.robimek.com/wp-content/uploads/pixymon-ayar.jpg)

Signature 1 kısmından uygun görünümü yakalayana kadar değerleri değiştirin.[![](https://www.robimek.com/wp-content/uploads/pixymon-signature.jpg)](https://www.robimek.com/wp-content/uploads/pixymon-signature.jpg)

Tüm imza için algılama doğruluğunu en üst düzeye çıkarmak için yedi renk imzasını bu şekilde ayarlayabilirsiniz. Kaydırıcı aralıklarını kaydetmek için Uygula’ya veya Tamam’a basmayı unutmayın! Ayarlamış olduğunuz değerler, İptal düğmesine basarsanız veya iletişim kutusunu kapattığınızda kaydedilmeyecektir.

### Buton İle Nesne Tanıtma

Pixy ‘nin en avantajlı yönlerinden biri üzerinde bulunan butona basarak nesneyi tanıtabiliyoruz. Modüle enerji verin. Önünde bulunan rgb led beyaz yandıktan sonra sönecektir. Söndükten sonra tanıtmak istediğimiz nesneyi kameraya gösterin ve butonu yaklaşık 1.5 saniye basılı tutun. Önünde bulunan ledin beyaz yandığını  göreceksiniz. Bu durum modülün öğrenme modunu aktif ettiği anlamına gelir. Basılı tuttuğunuz sürece ledin rengi kırmızı, turuncu, sarı, açık mavi, mavi, mor şeklinde yanmaya devam eder ve tekrar beyaz renge dönerek tekrarlar. Bu renkler pxymoon programındaki **Set Signature 1, Set Signature 2, Set Signature 3, Set Signature 4, Set Signature 5, Set Signature 6, Set Signature 7** adet öğrenme hafızasını temsil eder. Şimdi led renk değiştirirken butonu bıraktığınız anda hangi renk yanıyorsa o rengin karşılığı olan öğrenme hafızasına kaydeder. Örnek verecek olursak, turuncu renk yandığında butonu bırakırsanız ledin rengi kamera önüne koyduğunuz cismin rengine döner ve butona tekrar basıp bıraktığınızda led parlak bir şekilde turuncu flaş yaparak 2. hafıza bölgesine kayıt olur. Bu şekilde diğer hafıza bölgelerine de cisim kaydedebilirsiniz.

### Genel Bilgi

Pixy doğrudan Roborio'ya bağlanamaz. Çünkü Pixy kütüphaneleri Roborio'yu desteklememektedir. FRC'de Pixy kullanabilmek için 2 yol vardır.

* Pixy Raspberry pi ile çalıştırıp verileri networktables ile Roborio 'ya aktarmak..
* Pixy Arduino ile çalıştırıp verileri I2c portu veya SPI portu ile Roborio 'ya aktarmak.

Yani Pixy donanımını tek başına kullanamazsınız. Eğer Pixy kullanmak istiyorsanız 1 adet Raspberry Pi veya Arduino 'ya ihtiyacınız vardır.

### **Raspberry Pi ile Pixy Kullanımı** 

#### Kurulum

Python SWIG modülü oluşturmak için bağımlılıkları kurun

Bir uçbirim penceresine şunu yazın:  
`sudo apt-get install swig`

Libusb-1.0-0-dev'i kurun.

`sudo apt-get install libusb-1.0-0-dev`

G ++ \(derleyici\) yükleyin.

`sudo apt-get install g++`

Libboost'u yükleyin.

`sudo apt-get install libboost-all-dev`

Pixy kaynak kodunu indirin.

`git clone https://github.com/charmedlabs/pixy.git`

Python modülünü oluşturun.

 `cd pixy/scripts`  
`./build_libpixyusb_swig.sh`

Kodun olduğu dizine gidin.

 `cd ../build/libpixyusb_swig`

Ardından wget komutu ile Pixy kodlarımızı indirelim.

```text
wget https://raw.githubusercontent.com/enisgetmez/FRC-Vision-Processing/master/Pixy/pixy.py
```

Artık kodlarınız hazır. Kodlarınızı python pixy.py komutuyla çalıştırabilirsiniz. 

![Raspberry Pi &#x130;le Pixy Kullan&#x131;m&#x131; i&#xE7;eri&#x11F;inde sa&#x11F;lad&#x131;&#x11F;&#x131; kaynaklar i&#xE7;in \#6025 Adroit Anroids tak&#x131;m&#x131;na te&#x15F;ekk&#xFC;rler!](../../.gitbook/assets/12717164_240673006268960_5793277403987910316_n.jpg)

### Javadan Değerleri Çekin!

Kütüphaneleri import edin

```java
import edu.wpi.first.wpilibj.networktables.NetworkTable; // Networktables kütüphanesi
import edu.wpi.first.wpilibj.smartdashboard.SendableChooser; // smartdashboardan verileri görmek için
import edu.wpi.first.wpilibj.smartdashboard.SmartDashboard;
```

Class'ınızın altına networktables'i tanımlayın

```java
	public static NetworkTable table1 = NetworkTable.getTable("Vision"); // vision adında table çekiliyor
```

Değerleri okumak için 2 tane void oluşturalım.

```java
public static double konumX()
	{
		return table1.getNumber("X", 0.0); //raspberry pi den gelen x kordinatları
	}
	public static double konumY() 
	{
		return table1.getNumber("Y", 0.0); //raspberry pi den gelen y kordinatları
	}
```

Değerleri SmartDashboard'a yazdıralım.

```java
	public void teleopPeriodic() {
		SmartDashboard.putNumber("Nesnenin X konumu: ", konumX()); // smartdashboarda x konumu yazdır
		SmartDashboard.putNumber("Nesnenin Y konumu: ", konumY()); // smartdashboarda y konumunu yazdır

	}
```

Motorlarımız Değerlere göre haraket ettirelim!

```java
public void autonomousPeriodic() {
	    if(konumX() == 0)
	{
	sagmotor1.set(0);
	sagmotor2.set(0);
	solmotor1.set(0);
	solmotor2.set(0);
	}
		else if(konumX() < 285) // degerler 285'ten kucukse saga don
		{
		sagmotor1.set(0.5); // sag motorları calistir
		sagmotor2.set(0.5);
		}
		else if (konumX() > 295) // degerler 295'ten buyukse sola don
		{
		solmotor1.set(0.5); //sol motorlari calistir
		solmotor2.set(0.5);
		}

	}
```

Kodların tamamına buradan ulaşabilirsiniz : 

{% embed url="https://github.com/enisgetmez/FRC-Vision-Processing/blob/master/Java/robot.java" %}

Artık Pixy yazılımınızı Raspberry Pi 'den çalıştırdığınızda robotunuz otomatik olarak roborio ile haberleşmeye başlayacaktır.

