# Co-Processor - Örnekler

## Raspberry Pi

Merhabalar, bu içerikte sizlerle daha önceden geliştirdiğimiz görüntü işleme komutlarını paylaşacağım. Öncelikle kodların tamamına ulaşmak isterseniz : [https://github.com/enisgetmez/FRC-Vision-Processing](https://github.com/enisgetmez/FRC-Vision-Processing) buradan ulaşabilirsiniz.

Bu kısmı buradan daha detaylı bir şekilde izleyebilirsiniz:

{% embed url="https://youtu.be/FReqrby5Its?t=1799" %}



**Co-Processor olarak Raspberry PI kullanacağız.**

### Gerekli Kütüphanelerin Kurulması

Öncelikle görüntü işleme yapacağımız için OpenCV kütüphanelerinden faydalanacağız. OpenCV kütüphanelerinin çalışması için aynı zamanda Numpy kütüphanesini de kurmamız gerekmektedir.

  
Konsola girmemiz gereken komutlar şu şekildedir:  
`sudo apt install python-numpy python-opencv libopencv-dev`

![](../.gitbook/assets/image%20%2813%29.png)

y/n seçeneğine y yanıtını verelim. Bu işlem yaklaşık 5 dakika sürebilir.

Python için pip yani paket yöneticimizi indirmemiz gerekmektedir.

`sudo apt install python python-pip`

y/n seçeneğine y yanıtını verelim. Ardından

`pip install opencv-python`

`pip install pynetworktables`

`pip install imutils`

komutlarını girelim. **Eğer Python 3 kullanıyorsanız**, pip komutlarınız şu şekilde olmalı:

`pip3 install opencv-python`

`pip3 install pynetworktables`

`pip3 install imutils`

Her şey tamamlandıktan sonra bu adımda bitmiş demektir. Bir sonraki adıma geçebiliriz.

### Renk işleme için renk değerlerinin oluşturulması

En başta renk işlememiz için bize tanımlayacağımız rengin alt ve üst sınırları dediğimiz renk kodu gerekmektedir. Bunun için bir converter yazılımı mevcut. Kodlar bu şekildedir:

```python
import sys
import numpy as np
import cv2
 
blue = sys.argv[1]
green = sys.argv[2]
red = sys.argv[3]  
 
color = np.uint8([[[blue, green, red]]])
hsv_color = cv2.cvtColor(color, cv2.COLOR_BGR2HSV)
 
hue = hsv_color[0][0][0]
 
print("Lower bound is :"),
print("[" + str(hue-10) + ", 100, 100]\n")
 
print("Upper bound is :"),
print("[" + str(hue + 10) + ", 255, 255]")
```

 Dilerseniz [Github hesabımdan](https://raw.githubusercontent.com/enisgetmez/BAUROV-Autonomous/master/converter.py) bu kodlara ulaşabilirsiniz. Github üzerinden kolayca wget komutu ile indirebilirsiniz.

`wget` [`https://raw.githubusercontent.com/enisgetmez/BAUROV-Autonomous/master/converter.py`](https://raw.githubusercontent.com/enisgetmez/BAUROV-Autonomous/master/converter.py)\`\`

 Kullanımı ise şu şekildedir :

![](../.gitbook/assets/image%20%28115%29.png)

Algılatmak istediğiniz rengin kırmızı, yeşil, mavi renk değerlerini almalısınız. Ardından şu komutu kullanmalısınız:

`convert.py red green blue`

Lower bound ve upper bound dediğimiz renkleri kopyalayın. İleride kullanacağız.

### Renk İşleme

Kodlar java için şöyledir:

```java
package org.usfirst.frc.team6025.robot;

import edu.wpi.first.wpilibj.networktables.NetworkTable; // network table import
import edu.wpi.first.wpilibj.IterativeRobot;
import edu.wpi.first.wpilibj.Joystick;
import edu.wpi.first.wpilibj.Spark;
import edu.wpi.first.wpilibj.Timer;
import edu.wpi.first.wpilibj.command.Command;
import edu.wpi.first.wpilibj.drive.DifferentialDrive;
import edu.wpi.first.wpilibj.livewindow.LiveWindow;
import edu.wpi.first.wpilibj.smartdashboard.SendableChooser;
import edu.wpi.first.wpilibj.smartdashboard.SmartDashboard;


public class Robot extends IterativeRobot {
	private DifferentialDrive m_robotDrive
			= new DifferentialDrive(new Spark(0), new Spark(1));
	private Joystick m_stick = new Joystick(0);
	private Timer m_timer = new Timer();
	
	public static NetworkTable table1 = NetworkTable.getTable("Vision"); // vision adında table çekilioyr


	@Override
	public void robotInit() {
	}


	@Override
	public void autonomousInit() {
		m_timer.reset();
		m_timer.start();
	}
	
	public static double konumX()
	{
		return table1.getNumber("X", 0.0); //raspberry pi den gelen x kordinatları
	}
	public static double konumY() 
	{
		return table1.getNumber("Y", 0.0); //raspberry pi den gelen y kordinatları
	}


	@Override
	public void autonomousPeriodic() {
		// Drive for 2 seconds
		if (m_timer.get() < 2.0) {
			m_robotDrive.arcadeDrive(0.5, 0.0); // drive forwards half speed
		} else {
			m_robotDrive.stopMotor(); // stop robot
		}
	}

	@Override
	public void teleopInit() {
	}


	@Override
	public void teleopPeriodic() {
		SmartDashboard.putNumber("Nesnenin X konumu: ", konumX()); // smartdashboarda x konumu yazdır
		SmartDashboard.putNumber("Nesnenin Y konumu: ", konumY()); // smartdashboarda y konumunu yazdır

	}


	@Override
	public void testPeriodic() {
	}
}
```

Raspberry PI için python yazılımımız şu şekildedir : 

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
from collections import deque
from networktables import NetworkTables
import numpy as np
import argparse
import cv2
import time
import imutils
x = 0 #programın ileride hata vermemesi için x 0 olarak tanımlıyorum
y = 0 # programın ileride hata vermemesi için y 0 olarak tanımlıyorum
NetworkTables.initialize(server='roborio-6025-frc.local') # Roborio ile iletişim kuruyoruz
table = NetworkTables.getTable("Vision") # table oluşturuyoruz

#sari rengin algilanmasi
colorLower = (24, 100, 100)
colorUpper = (44, 255, 255)
#converter.py ile convert ettiğiniz rengi buraya giriniz
camera = cv2.VideoCapture(0) #  webcamin bagli oldugu port varsayilan 0
while True: #yazılımımız çalıştığı sürece aşağıdaki işlemleri tekrarla


     (grabbed, frame) = camera.read() # grabbed ve frame değişkenini camera.read olarak tanımlıyoruz.

     frame = imutils.resize(frame, width=600) # görüntü genişliğini 600p yapıyoruz
     frame = imutils.rotate(frame, angle=0) # görüntüyü sabitliyoruz

     hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV) # görüntüyü hsv formatına çeviriyoruz

     mask = cv2.inRange(hsv, colorLower, colorUpper) # hsv formatına dönen görüntünün bizim belirttiğimiz renk sınırları içerisinde olanları belirliyor
     mask = cv2.erode(mask, None, iterations=2) # bizim renklerimizi işaretliyor
     mask = cv2.dilate(mask, None, iterations=2) # bizim renklerimizin genişliğini alıyor


     cnts = cv2.findContours(mask.copy(), cv2.RETR_EXTERNAL,
	 cv2.CHAIN_APPROX_SIMPLE)[-2]
     center = None


     if len(cnts) > 0:

		     c = max(cnts, key=cv2.contourArea)
		     ((x, y), radius) = cv2.minEnclosingCircle(c)
		     M = cv2.moments(c)
		     center = (int(M["m10"] / M["m00"]), int(M["m01"] / M["m00"]))


		     if radius > 10: #algılanacak hedefin minumum boyutu
			     cv2.circle(frame, (int(x), int(y)), int(radius),
				 (0, 255, 255), 2)
			     cv2.circle(frame, center, 5, (0, 0, 255), -1)
     else:
        x = 0 ##değerlerin sıfırlanması
        y = 0

     print("x : ")
     print(x) # kameradan gelen görüntüde bizim rengimiz varsa x kordinatı
     print("y : ")
     print(y) # kameradan gelen görüntüde bizim rengimiz varsa y kordinatı

     table.putNumber("X", x) # roborioya değeri göndermek
     table.putNumber("Y", y)
```

Komutların açıklamaları kod içerisinde bulunmaktadır. Eğer Raspberry PI için komutu otomatik olarak indirmek isterseniz wget komutunu kullanabilirsiniz.

`wget` [`https://raw.githubusercontent.com/enisgetmez/FRC-Vision-Processing/master/vision.py`](https://raw.githubusercontent.com/enisgetmez/FRC-Vision-Processing/master/vision.py)\`\`

Artık robotunuzu enable ettiğinizde , Raspberry PI üzerinden yazılımımızı çalıştırdığımızda değerler otomatik olarak smart dashboarda düşmeye başlayacaktır.

Kodunuzu Raspberry PI üzerinden çalıştırmak için:

`python yazilimadi.py`

Şeklinde kullanmanız gerekmektedir. Eğer wget komutunu kullandıysanız yazılım otomatik olarak vision.py olarak gelecektir. Yani çalıştırmak için:

`python vision.py`

komutunu kullanmanız yeterli olacaktır. 

#### Raspberry Pi kod düzenleme\(opsiyonel\)

Raspberry Pi ile gelen kodu düzenlemek isterseniz nano komutunu kullanarak düzenleyebilirsiniz. 

`nano vision.py`

Komutunu girip düzenleyeceğiniz satırı düzenledikten sonra ctrl-x tuşlarıyla çıkış yapabilirsiniz. 

![](../.gitbook/assets/raspi-vision.gif)

![](../.gitbook/assets/image%20%28111%29.png)

### Şekil İşleme

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
import numpy as np
import argparse
import cv2
import cv2 as CV 
from networktables import NetworkTables


NetworkTables.initialize(server='roborio-6025-frc.local') # Roborio ile iletişim kuruyoruz
table = NetworkTables.getTable("Vision") # table oluşturuyoruz

cap = cv2.VideoCapture(0) # webcamin bagli oldugu port


while(True):
	# goruntu yakalama
	ret, frame = cap.read()

	# goruntuyu grilestir
			
	output = frame.copy()
	gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
	
	# goruntuyu blurlastir
	gray = cv2.GaussianBlur(gray,(5,5),0);
	gray = cv2.medianBlur(gray,5)

	gray = cv2.adaptiveThreshold(gray,255,cv2.ADAPTIVE_THRESH_GAUSSIAN_C,\
            cv2.THRESH_BINARY,11,3.5)
	
	kernel = np.ones((5,5),np.uint8)
	gray = cv2.erode(gray,kernel,iterations = 1)

	gray = cv2.dilate(gray,kernel,iterations = 1)
	circles = cv2.HoughCircles(gray, cv2.cv.CV_HOUGH_GRADIENT, 1, 20, param1=50, param2=30, minRadius=0, maxRadius=0) #python2 icin
#	circles = cv2.HoughCircles(gray, cv2.HOUGH_GRADIENT, 1, 20, param1=40, param2=45, minRadius=0, maxRadius=0) # python3 icin 
    # (x−xcenter)2+(y−ycenter)2=r2   (xcenter,ycenter) 
	# kalibre
	# daireyi isle
	
	if circles is not None:

		circles = np.round(circles[0, :]).astype("int")
		
		
		for (x, y, r) in circles:
	
			cv2.circle(output, (x, y), r, (0, 255, 0), 4)
			cv2.rectangle(output, (x - 5, y - 5), (x + 5, y + 5), (0, 128, 255), -1)

			
			print ("X kordinat: ")
			print (x) # x kordinatı
			print ("Y Kordinat: ")
			print (y) # y kordinatı
			print ("Radius: ")
			print (r) # cisimin büyüklüğü
			table.putNumber("X", x) # roborioya değeri göndermek
			table.putNumber("Y", y)
			#cv2.imshow('frame',output) # ekranda göster
			if cv2.waitKey(1) & 0xFF == ord('q'):
				break
cap.release()
cv2.destroyAllWindows()
```

Komutların açıklamaları kod içerisinde bulunmaktadır. Eğer Raspberry PI için komutu otomatik olarak indirmek isterseniz wget komutunu kullanabilirsiniz.

`wget` [`https://raw.githubusercontent.com/enisgetmez/FRC-Vision-Processing/master/circle.py`](https://raw.githubusercontent.com/enisgetmez/FRC-Vision-Processing/master/circle.py)\`\`

Artık robotunuzu enable ettiğinizde , Raspberry PI üzerinden yazılımımızı çalıştırdığımızda değerler otomatik olarak smart dashboarda düşmeye başlayacaktır.

Kodunuzu Raspberry PI üzerinden çalıştırmak için:

`python yazilimadi.py`

Şeklinde kullanmanız gerekmektedir. Eğer wget komutunu kullandıysanız yazılım otomatik olarak vision.py olarak gelecektir. Yani çalıştırmak için:

`python circle.py`

komutunu kullanmanız yeterli olacaktır. 

### 

## Limelight

### Limelight Nedir?

Limelight, _FIRST_ Robotik Yarışması için özel olarak tasarlanmış bir tak-çalıştır akıllı kameradır . Vision Processing tecrübesi gerektirmez. Vision deneyimi olmayan veya yeterli seviyede mentör hocaları olmayan takımlar için Limelight yeterince kolaydır. Vision processing'e ihtiyaç duyan takımlar için bir alternatiftir.

![Limelight Kamera](../.gitbook/assets/image%20%28122%29.png)

![](../.gitbook/assets/image%20%2897%29.png)



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

![](../.gitbook/assets/image%20%28121%29.png)

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

![](../.gitbook/assets/image%20%2838%29.png)

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

## Bilgisayarı Co-processor olarak kullanmak

###  Renk İşlemek

Eğer Raspberry Pi kullanmadan doğrudan bilgisayar aracılığı ile görüntü işlemek için bilgisayarınıza Python ve belirli kütüphaneleri kurmanız gerekmektedir. Gerekli kütüphaneleri şu şekilde kurabilirsiniz:

`pip install pillow`

`pip install opencv-python`

`pip install pynetworktables`

`pip install imutils`

```python
import numpy as np
from PIL import ImageGrab
from collections import deque
from networktables import NetworkTables
import cv2
import time
import imutils
import argparse
import cv2 as CV

x = 0 #programın ileride hata vermemesi için x 0 olarak tanımlıyorum
y = 0 # programın ileride hata vermemesi için y 0 olarak tanımlıyorum

NetworkTables.initialize(server='roborio-6025-frc.local') # Roborio ile iletişim kuruyoruz

table = NetworkTables.getTable("Vision") # table oluşturuyoruz

#sari rengin algilanmasi
colorLower = (24, 100, 100)
colorUpper = (44, 255, 255)
#converter.py ile convert ettiğiniz rengi buraya giriniz

def screen_record():
    x = 0
    y = 0
    r = 0
    last_time = time.time()
    while(True):
        # 800x600 windowed mode
        printscreen =  np.array(ImageGrab.grab(bbox=(0,40,1024,768)))
        print('Tekrarlanma süresi : {} saniye'.format(time.time()-last_time))
        last_time = time.time()

        frame = printscreen
        frame = imutils.resize(frame, width=600)
        frame = imutils.rotate(frame, angle=0)
        hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
        mask = cv2.inRange(hsv, colorLower, colorUpper)
        mask = cv2.erode(mask, None, iterations=2)
        mask = cv2.dilate(mask, None, iterations=2)
        cnts = cv2.findContours(mask.copy(), cv2.RETR_EXTERNAL,
        cv2.CHAIN_APPROX_SIMPLE)[-2]
        center = None
        if len(cnts) > 0:
            c = max(cnts, key=cv2.contourArea)
            ((x, y), radius) = cv2.minEnclosingCircle(c)
            M = cv2.moments(c)
            center = (int(M["m10"] / M["m00"]), int(M["m01"] / M["m00"]))
            if radius > 10:
                cv2.circle(frame, (int(x), int(y)), int(radius),
                (0, 255, 255), 2)
                cv2.circle(frame, center, 5, (0, 0, 255), -1)
        else:
            x = 0
            y = 0

        print("x : ")
        print(x)
        print("y : ")
        print(y)
        table.putNumber("X", x) # roborioya değeri göndermek
        table.putNumber("Y", y) # roborioya değeri göndermek

        cv2.imshow('frame', frame)
        cv2.waitKey(1)
screen_record()
```

Kodunuzu yazıp kaydettikten sonra çalıştırmak için konsoldan şu komutu girmeniz gerekmektedir:

`python yazilimadi.py`

Artık robotunuzu enable ettiğinizde , yazılımımızı çalıştırdığımızda değerler otomatik olarak smart dashboarda düşmeye başlayacaktır.



## Hatalar ve Çözümleri

### Hatalar ve Çözümleri

#### AttributeError: 'NoneType' object has no attribute 'shape' hatası

![](../.gitbook/assets/hata.png)

Bu hatayı alma sebebiniz yazılımımızın sizin kameranızı tanımamasından kaynaklanmaktadır. Kameranızı Raspberry Pi'ye taktıktan sonra konsola

`sudo apt-get update && sudo apt-get upgrade`

komutunu girmeniz gerekmektedir. Bu komut kameranız Raspberry Pi 'ye bağlıyken Raspi'nizi update edip gereklid driverleri indirmesini ve upgrade komutuyla kurmasını sağlayacaktır. Eğer kameranızı yine algılamazsa kodunuzda bulunan 

```python
camera = cv2.VideoCapture(0) #  webcamin bagli oldugu port varsayilan 0
```

satırındaki 0 numarasını 1 ile değiştirebilirsiniz. Raspberry Pi'de 4 port olduğu için 4'e kadar değiştirerek deneyebilirsiniz.

#### No module named 'networktables'

![](../.gitbook/assets/image%20%2840%29.png)

Eğer böyle bir hata ile karşılaşıyorsanız pynetworktables kütüphanesini kurarken sıkıntı yaşamışsınız demektir. 

`pip install pynetworktables`

Komutunu girip tekrar çalıştırmayı deneyin. Eğer aynı hatayı almaya devam ediyorsanız, muhtemelen kodu sudo komutuyla çalıştırmaya çalışıyorsunuzdur. Sudo ve normal işlem esnasında kullandığınız pi farklı kullanıcılardır. Sudo bütün yetkilere sahip olan kullanıcı anlamına gelmektedir. Sudo olarak çalıştırmak istiyorsanız , kütüphaneleri Sudo kullanıcısına tekrardan kurmanız gerekmektedir. Bunun için konsola şu komutları girmelisiniz.

`Sudo su`

Bu komut pi kullanıcısından sudo kullanıcısına geçmenizi sağladı. Şimdi bütün kütüphaneleri tekrardan pip komutu ile kurabilirsiniz.

#### IndentationError: unindent does not match any outer indentation level

![](../.gitbook/assets/image%20%2810%29.png)

Bu hata komutun başında bulunan boşlukları sildiğinizi veya fazladan boşluk koyduğunuz anlamına gelir. Python syntax'ı boşluklarla olduğu için yazdığınız komutu algılamaz. Boşlukları kontrol edip tekrar çalıştırmayı deneyin. Eğer boşluklarla ilgli bir problem göremezseniz herhangi bir Python editörü indirip editör ile kodlarınızı kontrol edebilirsiniz. Editörler bu tarz problemleri kendiliğinden fark edip düzeltirler.

#### Reflektörün beyaz yansıması

![](../.gitbook/assets/image%20%2882%29.png)

Bu problem bir çok farklı sebepten kaynaklanıyor olabilir.

Başlıca sebepleri:

* Kullandığınız ledlerin parlaklığı
* Kullandığınız ledlerin sayısı
* Kamera ve ledlerin konumu
* Kullandığınız reflector

![](../.gitbook/assets/image.png)

Bunun için Wpilib'in yazdığı bir makale bulunmakta. Bu makaleyi okuyarak problemi çözebilirsiniz. 

{% embed url="https://wpilib.screenstepslive.com/s/3120/m/8731/l/90337-target-info-and-retroreflection" %}





