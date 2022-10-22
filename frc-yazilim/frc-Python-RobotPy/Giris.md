
# RobotPy ile FRC'de Python Kullanmak
## Nedir Bu RobotPy?
[RobotPy](https://github.com/robotpy), FRC'de kullanmak üzere Python3 için WPILib paketleri geliştirmeyi hedefleyen açık kaynak kodlu bir projedir. 
## RobotPy Artıları-Eksileri
RobotPy, her ne kadar 2010'dan beri yürürlükte olan bir proje olmasına rağmen F.I.R.S.T. tarafından resmi olarak desteklenmemektedir. Ancak bu durum RobotPy'ı sadece offseason etkinlikleri için kullanabileceğiniz anlama gelmez, 2010 senesinden beri birçok takım RobotPy ile robotlarını programlayıp resmi yarışmalarda yer almaktadır.

Başlıca Avantajlar:
 - Python'un basit ve dinamik bir dil olması
 - Türkiye'de bazı ortaöğretim kurumlarında Python'un ders olarak gösterilmesi ile yeni öğrencilerin FRC takımlarına katılmaya teşvik edilmesi
 -  Proje üzerinde uzun zamandır çalışan destekçiler ile hızlı destek
 
 Başlıca Dezavantajlar:
 
 - F.I.R.S.T. tarafından resmi olarak desteklenmemek
 - WPILibJ ve WPILibC'ye göre daha az örnek kodun ve öğretici kaynağın bulunması

## RobotPy Kurulumu (Bilgisayarlar İçin)
**Bu Adım, Roborio'ya RobotPy kurmak ve Python kodu deploy etmek için zorunludur.**

**Windows**

     py -3 -m pip install robotpy
**Linux/OSX**

    pip3 install robotpy
İlgili işletim sistemi için üst kısımdaki komutlar cmd'ye veya Terminal'de çalıştırıldığında Python3 ve PIP kurulu olması takdirde RobotPy'ın ihtiyacınız olacağı çoğu paket sisteminize otomatik olarak inip kurulacaktır. Özellikle Linux kullanıcılarının PIP versiyonlarının en üst sürüm olmasına dikkat etmeleri gerekir.

**Önemli Not**
Commands gibi opsiyonel/rev-ctre gibi üretici kütüphaneleri de kurmak isteyen takımlar robotpy'a bitişik olarak köşeli parantez içerisinde istedikleri kütüphanenin ismini girip aynı süreci uygulayabilirler. Devlet lisesi takımları FATİH ağının bu kütüphanelerinin hosting'ini yasakladığı için mobil veriden indirmelilerdir.

Örnek:
*Windows*

    py -3 -m pip install robotpy[ctre]

## RobotPy Kurulumu (RoboRIO İçin)
**Windows**

     py -3 -m robotpy_installer download-python
**Linux/OSX**

    robotpy-installer download-python
   Üstteki ilgili cmd/Terminal komutları internetten RoboRIO için gerekli Python sürümünü indirir ve sisteminizde cache'ler.
   
**Windows**

     py -3 -m robotpy_installer install-python
**Linux/OSX**

    robotpy-installer install-python
   
   Üstteki ilgili cmd/Terminal komutları sisteminizde cache'lenmiş python sürümünü bir RoboRIO ağındayken RoboRIO'da kurar. Bu adım önemlidir, elinizde bir Python3 intreperter'ı yok ise hiçbir pip paketi işinize yaramaz. 😊

**RoboRIO üzerinde Python3 Kurduğumuza göre RobotPy Paketlerini yükleme adımına geçebiliriz.**

**Windows**

     py -3 -m robotpy_installer download robotpy
**Linux/OSX**

    robotpy-installer download robotpy
   Üstteki ilgili cmd/Terminal komutları internetten RoboRIO için gerekli RobotPy paketlerini indirir ve sisteminizde cache'ler.
   
**Windows**

     py -3 -m robotpy_installer install robotpy
**Linux/OSX**

    robotpy-installer install robotpy
   Üstteki ilgili cmd/Terminal komutları sisteminizde cache'lenmiş RoboRIO için gerekli RobotPy sürümünü bir RoboRIO ağındayken RoboRIO'da kurar.


**Önemli Not**
Commands gibi opsiyonel/rev-ctre gibi üretici kütüphaneleri de kurmak isteyen takımlar robotpy'a bitişik olarak köşeli parantez içerisinde istedikleri kütüphanenin ismini girip aynı süreci uygulayabilirler. Devlet lisesi takımları FATİH ağının bu kütüphanelerinin hosting'ini yasakladığı için mobil veriden indirmelilerdir.

Örnek:
*Windows*

    py -3 -m robotpy_installer download robotpy[ctre]

    py -3 -m robotpy_installer install robotpy[ctre]
