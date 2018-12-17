# FRC Java Temelleri

### **Java Metodları , Nesneler ve Değişkenler**

 **Java'da roboRIO'ya bağlı nesneler oluşturma**

```java
Gyro headingGyro = new AnalogGyro(1);
double heading = headingGyro.getAngle();
```

Genellikle roboRİO breakout panolarından\(Türkiye'de bilinen adıyla roborio üzerindeki pin girişleri\) birine bağlanan WPILib'deki tüm nesneler, bağlandığı kanal veya bağlantı noktası numarasını belirttiğiniz yerde oluşturulduğunda oluşturucuda bir argümana sahiptir. Yukarıdaki örnek Java için WPILib kullanılan kuralları göstermektedir.



1. Analog kanal 1'e bağlı bir AnalogGyro nesnesi oluşturur ve adresini "headingGyro" içine kaydeder.
2. Geçerli pozisyonu AnalogGyro'dan derece olarak alır ve değişken "heading" içinde saklar.

### **Java'da operatör arayüzü nesneleri oluşturma**  

![](../.gitbook/assets/image%20%281%29.png)

Genel olarak Driver Station PC'sine USB üzerinden bağlanan nesneler, bağlandıkları USB portunu belirten tek bir argüman alırlar. FRC Driver Station ile çalışan herhangi bir joystick veya gamepad ile arabirim kurmak için gereken işlevselliği sağlayan tek bir Joystick sınıfı sağlanmıştır.

1. DS'deki USB bağlantı noktasına 1'e bağlı bir Joystick nesnesi oluşturur.
2. Joystick'in geçerli X ekseni değerini alır ve "speed" değişkeninde saklar.

### **Sınıf, yöntem ve değişken adlandırma**

![](../.gitbook/assets/image%20%2835%29.png)

### MXP IO Numarlandırılması

![](../.gitbook/assets/image%20%2891%29.png)

### Java'da multithreading

 Eşzamanlılık ve threads hakkında daha fazla bilgi için Oracle tarafından yayınlanan [bu makaleye](http://docs.oracle.com/javase/tutorial/essential/concurrency/) bakın. Aşağıda açıklanamayan hatalara neden olacak WPILib'de yazılan robot programları için önemli bilgiler bulacaksınız.

### **Thread**

Aşağıdaki kod asla döngüden çıkamayacaktır! Tüm JVM durduğunda duracaktır. Thread tek başına çıkmadığı sürece  thread'ı durdurmak için hiçbir yol yoktur. Bu problemi yaşayan takımlar genelde otonom içerisinde bu hatayı yapmaktadırlar. Bu da maç esnasında otonom bittikten sonra teleop kodlarını asla kullanamayacağınız anlamına gelmektedir.

**Kötü Örnek:**

```java
public class Robot extends IterativeRobot {
    public void robotInit() {
        Thread t = new Thread(() -> {
            while (true) {
					// Burada takıldık!
            }
        });
        t.start();
    }
}
```

Bir flag ayarlayarak bu sorunu çözebiliriz. Java'da her thread  için tasarlanmış bir flag vardır. Bu flag'ı kontrol etmek için kodumuzu değiştirmemiz gerek. Bu örneğe bir göz atın:

```java
public class Robot extends IterativeRobot {
    public void robotInit() {
        Thread t = new Thread(() -> {
            while (!Thread.interrupted()) {
					// Bu sefer takılmadık!
            }
        });
        t.start();
    }
}
```

### Base Class Seçmek

| Base class | Application |
| :--- | :--- |
| SampleRobot | SampleRobot base class'ı kulağa hoş geliyor, küçük örnek programlar yazmak için iyi, özellikle fikirleri denemek için. Bir yarışma programı oluşturmak için kullanılabilir olsa da, ek yetenekler eklendikçe genişletilmesi çok zor olduğu için tavsiye edilmez. Bunun yerine, aşağıda açıklanan diğer şablonlardan birini seçebilirsiniz. |
| IterativeRobot | IterativeRobot base class, yeni veriler sürücü istasyonundan geldiğinde her zaman periyodik olarak çağrılan yöntemlere sahiptir. Fikir, robotun \(özerk, teleop veya test\) çalıştığı her mod için, programın az miktarda iş yaptığı uygun periyodik yöntem denir. Döngüler veya gecikmeler gibi periyodik yöntemlerde uzun süre çalışan bir kod bulundurmamak önemlidir. Bunu yapmak, robot performansını olumsuz yönde etkileyebilecek eksik sürücü istasyonu güncellemelerine neden olabilir. Her periyot yaklaşık olarak 20 milisaniye olup, roboRİO, Driver Station bilgisayarı veya ağ trafiği üzerindeki CPU yüküne bağlı olarak değişebilir. Eğer hassas zamanlama gerekiyorsa, örneğin robot kontrol algoritmaları uygulamak için tavsiye edilmez. bunun yerine TimedRobot \(aşağıda\) kullanabilirsiniz. |
| TimedRobot | TimedRobot, periyodik yöntemlerin öngörülebilir bir zaman aralığında çağrıldığını garanti etmek için bir timer \(Notifier\) kullanması dışında IterativeRobot ile aynıdır. Joystick değerleri gibi sürücü istasyonu verilerini alırken, zaman aralığı 20 milisaniye veri dağıtımı ile gecikme yaşamayacağı için en iyi performans sağlanacaktır. Bu, çoğu robot programı için önerilen temel sınıftır. Tıpkı IterativeRobot gibi, periyodik yöntemlerde uzun süre çalışan kod veya döngülere sahip olmak çok önemlidir. |
| Command based robot | TimedRobot temel sınıfına dayanırken, çoğu takım için Command bassed robot programlama stili önerilir. Program üzerinde bir kolun bir yere yükseltilmesi, bir mesafe sürmesi vb. gibi bazı robot davranışlarını uygulayan komutlara ayırmayı kolaylaştırır. Ayrıca programı kolayca genişletilebilir ve test edilebilir hale getirir. RobotBuilder programı \(eclipse eklentileri ile birlikte\) programı organize etmek için kolay bir yol sağlar. Dashboard'lar \(SmartDashboard ve Shuffleboard\) kolayca hata ayıklama ve komut tabanlı programları test etmek için izin verir. |



### IterativeRobot <a id="iterativerobot"></a>

**C++**  


```cpp
RobotTemplate::RobotTemplate()
{
}

void RobotTemplate::RobotInit()
{
}

void RobotTemplate::AutonomousInit()
{
}

void RobotTemplate::AutonomousPeriodic()
{
}

```

  
**Java**  


```java
public class RobotTemplate extends IterativeRobot {

     public void robotInit() {

     }

     public void autonomousInit() {

     }

     public void autonomousPeriodic() {

     }
}
```

 Iterative Robot, en yaygın kod yapısına, robot kodunun yerine taban sınıfında durum geçişlerini ve döngülerini ele alarak destekler.

### Timedrobot

TimedRobot temel sınıfı, belirtilen zaman aralığını kullanarak periyodik işlevleri çağırması dışında IterativeRobot \(yukarıdaki\) ile aynıdır. Her bir çağrı için uygun periyodik fonksiyona varsayılan zaman aralığı 0.02 saniyedir \(20 milisaniye\). Varsayılan süre dahili setPeriod \(java\) veya SetPeriod \(C ++\) değeri saniye cinsinden çift değer olarak çağrılarak geçersiz kılınabilir. Dahili olarak bir Notifier, aralığı ayarlamak için kullanılır.

### SampleRobot <a id="samplerobot"></a>



 **C++**  


```cpp
RobotTemplate::RobotTemplate()
{
}

//This function is called once each time the robot enters autonomous mode.
void RobotTemplate::Autonomous()
{
     while (IsAutonomous() && IsEnable()) 
     {
          // Put code here
          Wait(0.05);
     }
}

// This function is called once each time the robot enters teleop mode.
void RobotTemplate::OperatorControl() {
     while (isOperatorControl() && isEnabled())
     {
          // Put code here
          Wait(0.05);
     }
}

```

  
**Java**  


```java
public class RobotTemplate extends SampleRobot {

     //This function is called once each time the robot enters autonomous mode.
     public void autonomous() {
          // Put code here
          Timer.delay(0.05);
     }

     // This function is called once each time the robot enters teleop mode.
     public void operatorControl() {
         while(isOperatorControl() && isEnabled()) {
               //Put code here
               //Timer.delay(0.05);
         }
     }
}

```

SampleRobot , durum akışının çoğunun doğrudan programınızda göründüğü ve WPILib kodunda gizlenmediği en basit şablondur. Olumsuz yanı, bu durum akışının yanlış uygulanması, programlarınızda karmaşıklığa neden olabilir.  Yeni başlayanların Iterative Template veya Command Based robotunu seçmeleri önerilir. 

### Command-Based Robot <a id="command-based-robot"></a>

Kesinlikle bir temel sınıf olmasa da, Command-Based robot modeli, daha kolay genişletilebilen daha büyük programları oluşturmak için bir yöntemdir. Robotunuzu tasarlamayı, alt sistemleri oluşturma ve robot ile operatör arayüzü arasındaki etkileşimleri kontrol etmeyi kolaylaştıran bir dizi sınıfla desteklenmiştir. Ayrıca, otonom programlar yazmak için basit bir mekanizma sağlar.

