# WPILib sensörleri

### WPILib Sensör Genel Bakış

WPI Robotik Kütüphanesi, parçaların FRC kitinde bulunan sensörleri ve endüstriyel ve hobi robotik tedarikçileri aracılığıyla FIRST takımlarına yaygın olarak kullanılan birçok sensörü destekler.

#### Desteklenen sensör türleri

![](../.gitbook/assets/image%20%2856%29.png)

roboRIO günü, FPGA olursa olsun birçok sensörler ve motorlar robota nasıl bağlandığını hassas ölçümler sağlamak adanmış donanım aracılığıyla tüm yüksek hızlı ölçümleri uygular. Bu, karmaşık gerçek zamanlı yazılım rutinlerini gerektiren önceki sistemlere göre bir gelişmedir. Kütüphane, aşağıda gösterilen kategorilerde algılayıcıları kendiliğinden destekler:



* Tekerlek / motor pozisyonu ölçümü - Gear-Toth sensörleri, enkoderler, analog enkoderler ve potansiyometreler
* Robot oryantasyonu - Pusula, jiroskop, ivmeölçer, ultrasonik rangefinder
* Genel - Darbe çıkışı Sayıcılar, analog, I2C, SPI, Serial, Dijital giriş

WPI Robotics Kütüphanesinde, önceden yazılmış sınıflara sahip olmayan sensörleri kolayca uygulamanızı sağlayan birçok özellik vardır. Örneğin, genel amaçlı sayaçlar periyodu ölçebilir ve çıkış darbeleri üreten herhangi bir cihazdan sayılabilir.



### Anahtarlar - Limitleyici Anahtarların Kullanımı

Limit anahtarları genellikle robotlardaki mekanizmaları kontrol etmek için kullanılır. Limit şalterlerinin kullanımı basit olmakla birlikte, sadece hareketli bir parçanın tek bir pozisyonunu algılayabilirler. Bu, hareketin bir sınırı aşmadığından emin olmak için idealdir, ancak hareketin hızını sınırlara yaklaştıkça kontrol etmede o kadar da iyi değildir. Örneğin, bir robot kolu üzerindeki rotasyonel bir omuz eklemi, bir potansiyometre veya mutlak enkoder kullanılarak en iyi şekilde kontrol edilir, limit şalteri, potansiyometrenin arızalanması durumunda, limit anahtarın robotun çok ileri gitmesini ve hasara neden olmasını engelleyebilir. 

####  Limit anahtarıyla hangi değerler sağlanır?

![](../.gitbook/assets/image%20%2851%29.png)

Limit anahtarları "normalde açılmış" veya "normalde kapalı" çıkışlara sahip olabilir. Düğmeyi kablolamanın olağan yolu, dijital giriş sinyali bağlantısı ile toprak arasındadır. Dijital giriş, şalter açıkken girişi yüksek \(1 değer\) yapacak olan çekme dirençlerine sahiptir, fakat anahtar kapandığında, değer şuan toprağa bağlı olduğu için 0'a gider. Burada gösterilen anahtar hem normalde açık ve normalde kapalı çıkışlara sahiptir.

#### Anahtardan kapalı olduğunda değer almak

```cpp
C++

#include "WPILib.h"


class Robot: public SampleRobot
{
	DigitalInput limitSwitch;
	
public:
	Robot() {
		
	}

	void RobotInit()
	{
		limitSwitch = new DigitalInput(1);
	}

	void OperatorControl() {
		// more code here
		while (limitSwitch.Get()) {
			Wait(10);
		}
		// more code here
	}

```

```java
Java

package org.usfirst.frc.team1.robot;

import edu.wpi.first.wpilibj.DigitalInput;
import edu.wpi.first.wpilibj.SampleRobot;
import edu.wpi.first.wpilibj.Timer;

public class RobotTemplate extends SampleRobot {

	DigitalInput limitSwitch;

 \   public void robotInit() {
    	limitSwitch = new DigitalInput(1);
    }

 \   public void operatorControl() {
    	// more code here
    	while (limitSwitch.get()) {
    		Timer.delay(10);
    	}
        // more code here
    }

```

Limit anahtarını okuyan ve tekrar tekrar okuyan çok basit bir kod parçası yazabilir, böylece değeri 1'den \(açık\) 0'a \(kapalı\) geçişini algılayana kadar bekleyebilirsiniz. 

#### Sınır anahtarına kadar çalışacak Command Based program



```java
package edu.wpi.first.wpilibj.templates.commands;

public class ArmUp extends CommandBase {
    public ArmUp() {
    }

    protected void initialize() {
        arm.armUp();
    }

    protected void execute() {
    }

    protected boolean isFinished() {
        return arm.isSwitchSet();
    }

    protected void end() {
        arm.armStop();
    }

    protected void interrupted() {
        end();
    }
}
```

Komutlar, execute \(\) ve isFinished \(\) yöntemlerini saniyede 50 kez veya her 20 ms hızında söyler. Limit anahtar kapanıncaya kadar motoru çalıştıracak bir komut, isFinished \(\) yöntemindeki dijital giriş değerini okuyabilir ve anahtar doğru duruma dönüştüğünde doğru döner. Daha sonra komut motoru durdurabilir.

**Unutmayın, mekanizmanın \(bu durumda bir Kol\) biraz eylemsizliği vardır bundan dolayı hemen durmayabilir, kol yavaşlarken hiçbir şeyin kırılmadığından emin olmanız önemlidir.**

####  Anahtarın kapanmasını algılamak için bir sayaç kullanma



```java
package edu.wpi.first.wpilibj.templates.subsystems;
import edu.wpi.first.wpilibj.Counter;
import edu.wpi.first.wpilibj.DigitalInput;
import edu.wpi.first.wpilibj.SpeedController;
import edu.wpi.first.wpilibj.Victor;
import edu.wpi.first.wpilibj.command.Subsystem;
public class Arm extends Subsystem {

    DigitalInput limitSwitch = new DigitalInput(1);
    SpeedController armMotor = new Victor(1);
    Counter counter = new Counter(limitSwitch);

    public boolean isSwitchSet() {
        return counter.get() > 0;
    }

    public void initializeCounter() {
        counter.reset();
    }

    public void armUp() {
        armMotor.set(0.5);
    }

    public void armDown() {
        armMotor.set(-0.5);
    }

    public void armStop() {
        armMotor.set(0.0);
    }
    protected void initDefaultCommand() {
    }
}
```

Bir limit anahtarının kapanabileceği, ardından bir mekanizmanın anahtardan geçerken tekrar açılabileceği mümkündür. Kapak yeterince hızlıysa, program anahtarın kapalı olduğunu fark etmeyebilir. Anahtar kapamayı yakalamak için alternatif bir yöntem, bir Counter nesnesi kullanmaktır. Sayaçlar donanımda uygulandığından, en hızlı anahtarların kapanmasını yakalayabilir ve sayımını artırabilir. Daha sonra program, sayımın arttığını ve operasyonu yapmak için gerekli olan adımları attığını fark edebilir.

Yukarıda, limit anahtarını izlemek ve değerin değişmesini beklemek için bir sayaç kullanan bir alt sistemdir. Bu olduğunda, sayaç artar ve bu bir komut izlenebilir.

#### Anahtar kapamayı algılamak için sayacı kullanan bir komut oluşturun.

```java
package edu.wpi.first.wpilibj.templates.commands;

public class ArmUp extends CommandBase {

    public ArmUp() {
    }

    protected void initialize() {
        arm.initializeCounter();
        arm.armUp();
    }

    protected void execute() {
    }

    protected boolean isFinished() {
        return arm.isSwitchSet();
    }

    protected void end() {
        arm.armStop();
    }

    protected void interrupted() {
        end();
    }
}
```

Bu komut, yukarıdaki alt sistemdeki sayacı başlatır ve ardından motoru hareket ettirmeye başlar. Ardından, limit anahtarı değişimini saymak için bekleyen isFinished \(\) yöntemindeki sayaç değerini test eder. Ne zaman, kol durdurulur. Bir donanım sayacı kullanarak, çok hızlı açılıp kapanabilen bir anahtar program tarafından hala yakalanabilir.

###  İvmeölçerler - hızlanma ve eğimi ölçme

İvmeölçerler bir veya daha fazla eksende ivmeyi ölçer. Tipik bir kullanım robot hızlanmasını ölçmektir. Diğer bir yaygın kullanım robot eğimini ölçmektir, bu durumda yerçekimine bağlı ivmeyi ölçer.

 **İki eksenli analog ivmeölçer**

![](../.gitbook/assets/image%20%2881%29.png)

Yaygın olarak kullanılan bir kısım \(yukarıdaki resimde gösterildiği gibi\) iki eksenli bir ivmeölçerdir. Bu cihaz, devre kartına göre X ve Y eksenlerinde hızlandırma verileri sağlayabilir. WPI Robotik Kütüphanesi'ni, biri X ekseni, diğeri Y ekseni için olmak üzere iki ayrı cihaz olarak değerlendirirsiniz. İvmeölçer, yerçekimi ivmesini ölçerek bir eğim sensörü olarak kullanılabilir. Bu durumda, cihazı yana çevirmek 1000 miliG veya bir tane G gösterir. Gösterilen, robot üzerindeki iki analog girişe bağlı 2 eksenli bir ivmeölçer kartıdır. 

Örnek Kod:



```cpp
class AccelerometerSample: public SampleRobot {
	AnalogAccelerometer *accel;
	double acceleration;
AccelerometerSample()
{
    accel = new AnalogAccelerometer(0); //create accelerometer on analog input 0
    accel->SetSensitivity(.018); // Set sensitivity to 18mV/g (ADXL193)
    accel->SetZero(2.5); //Set zero to 2.5V (actual value should be determined experimentally)
}

public void OperatorControl() {
    while(IsOperatorControl() && IsEnabled())
    {
        acceleration = accel->GetAcceleration();
    }
}
}
```

public class AccelerometerSample extends SampleRobot { AnalogAccelerometer accel; double acceleration;

```java
AccelerometerSample()
{
    accel = new AnalogAccelerometer(0); //create accelerometer on analog input 0
    accel.setSensitivity(.018); // Set sensitivity to 18mV/g (ADXL193)
    accel.setZero(2.5); //Set zero to 2.5V (actual value should be determined experimentally)
}

public void operatorControl() {
    while(isOperatorControl() && isEnabled())
    {
        acceleration = accel.getAcceleration();
    }
}
}
```

Analog kanal 1'e bağlı bir analog ivmeölçer nasıl ayarlandığını gösteren yukarıda bir kısa kod örneği gösterilmektedir. Hassasiyet ve sıfır voltajları veri sayfasına göre ayarlanmıştır \(varsayılan bölüm ADXL193, sıfır gerilimi ideal olarak ayarlanmıştır.\)

**İvmeölçer arayüzü**

```cpp
C++
Accelerometer *accel;
accel = new BuiltInAccelerometer(Accelerometer:kRange_4G); 
double xVal = accel->GetX();
double yVal = accel->GetY();
double zVal = accel->GetZ();
```

```java
Java
Accelerometer accel;
accel = new BuiltInAccelerometer(); 
accel = new BuiltInAccelerometer(Accelerometer.Range.k4G); 
double xVal = accel.getX();
double yVal = accel.getY();
double zVal = accel.getZ();
```

ADXL345 için her iki sınıf ve Dahili İvmeölçer sınıfının tümü ortak bir İvmeölçer arabirimini devralır / uygular. Gelecekte plan, AnalogAccelerometer sınıfının bu arayüzden türetilmesini sağlamaya çalışmaktır. Bu sensörlerden birini kullanmayı planlıyorsanız, kodunuzu genel arayüze karşı yazmanız önerilir. Bu şekilde, altta yatan sınıflar arasında, eğer istenirse, kodunuzda en az değişiklikle değiştirebilirsiniz. Ayrıca, bu yeteneğin gelişmeye devam ettiği için kodunuzu simülasyonla daha uyumlu hale getirmeye yardımcı olacaktır.

#### ADXL345 İvmeölçer

![](../.gitbook/assets/image%20%2872%29.png)

ADXL345, 2012-2014 KOP'taki sensör kartının bir parçası olarak sağlanan üç eksenli bir ivmeölçerdir. ADXL345, +/- 16g'ye kadar olan hızları ölçebilir ve I2C veya SPI üzerinden iletişim kurabilir. Protokol için bağlantı yönergeleri FRC bileşen veri sayfasında bulunabilir. Ek bilgi Analog Cihazlar ADXL345 veri sayfasında bulunabilir. WPILib, her bir protokol için, veri yolunun ayarlanması ve sensörün etkinleştirilmesinin ayrıntılarını ele alan ayrı bir sınıf sağlar.

ADXL345 Örnek Kod:



```cpp
C++
class AccelerometerSample: public SampleRobot {
	Accelerometer *accel;
	double accelerationX;
	double accelerationY;
	double accelerationZ;

	AccelerometerSample()
	{
		accel = new ADXL345_I2C(I2C::Port::kOnboard, Accelerometer::Range::kRange_4G);
	}

	public void OperatorControl() {
		while(IsOperatorControl() && IsEnabled())
		{
			accelerationX = accel->GetX();
			accelerationY = accel->GetY();
			accelerationZ = accel->GetZ();
		}
	}
}

```



```java
Java
public class AccelerometerSample extends SampleRobot {
	Accelerometer accel;
	double accelerationX;
	double accelerationY;
	double accelerationZ;

	AccelerometerSample()
	{
		accel = new ADXL345_I2C(I2C.Port.kOnboard, Accelerometer.Range.k4G);
	}

	public void operatorControl() {
		while(isOperatorControl() && isEnabled())
		{
			accelerationX = accel.getX();
			accelerationY = accel.getY();
			accelerationZ = accel.getZ();
		}
	}
}
```

Yukarıda yerleşik I2C veriyoluna bağlı ADXL345'in kullanımını gösteren kısa bir kod örneği gösterilmiştir. İvmeölçer +/- 2g modunda çalışacak şekilde ayarlanmıştır. Örnek, hem Accelerometer arabirimini kullanarak, sensör değerlerini elde etmenin sadece tek eksenli yöntemini göstermektedir. 3 eksende senkronize okumaya ihtiyacınız varsa, arabirimi terk etmeniz ve doğrudan GetAccelerations \(\) yöntemine erişmek için ADXL345 sınıfını kullanmanız gerekir. SPI işlemi benzerdir, SPI üzerinden sensör kullanımı hakkında ek ayrıntılar için ADXL345\_SPI sınıfı için Javadoc / Doxygen'ye başvurun.

 **Yerleşik İvmeölçer**

RoboRIO, +/- 8 g, 12 bit çözünürlük ve 800 Örnek / sn örnekleme hızına sahip dahili 3 eksenli bir ivmeölçer içerir. Bu ivmeölçeri kullanmak için, BuiltInAccelerometer sınıfını kullanın. Genel İvmeölçer arabirimini kullanarak +/- 4g modunda çalışan bu ivme ölçer kullanımını gösteren kod için yukarıdaki İvmeölçer Arayüzü bölümüne bakın \(bu arabirimi, dahili ivmeölçerin +/- 16g modunu desteklemediğini unutmayın\).

### Jiroskop\(gyro\) - Dönüş yönü ve robotun sürüş yönünü kontrol etme

Parçaların FIRST kitinde tipik olarak Gyros, Analog Devices tarafından sağlanır ve aslında açısal oran sensörleridir. Çıkış gerilimi, jiro çipinin üst paket yüzeyine dik eksen ekseninin dönüş hızı ile orantılıdır. Değer mV / ° / saniye olarak ifade edilir \(derece / saniye veya bir voltaj olarak ifade edilen dönüş\). Zaman içinde hız çıkışını entegre ederek \(toplayarak\), sistem robotun nispi yönünü türetebilir.

Gyro için bir başka önemli özellik, tam ölçekli aralığıdır. Yüksek ölçekli tam aralıklara sahip Gyroslar, çıkışı "sabitlemeksizin" hızlı dönüşü ölçebilir. Ölçek çok daha büyüktür, bu nedenle daha hızlı dönme oranları okunabilir, ancak aynı sayıda dijital analog girişe yayılan çok daha büyük değerler aralığı nedeniyle daha az çözünürlük vardır. Bir jiro seçerken, robotunuzun yaşayacağı en hızlı dönüş oranına uyan tam ölçekli bir menzile sahip olanı seçmeniz gerekir. Bu, robotun bu aralığı asla aşmaması koşuluyla mümkün olan en yüksek doğruluğu sağlayacaktır.

**AnalogGyro sınıfını kullanma**  
Gyro nesnesi, RobotBase türetilmiş nesnenin yapıcısında oluşturulmalıdır. AnalogGyro nesnesi kullanıldığında, robotun sürüklenmeyi en aza indirmek için dinlenirken, hız çıkışının ofsetini ölçmek için bir kalibrasyon periyodundan geçecektir. Bu, robotun sabit ve jiroskopun kalibrasyon tamamlanana kadar kullanılamaz olmasını gerektirir.

Başladıktan sonra, Gyro nesnesinin GetAngle \(\) \(veya Java'daki getAngle \(\)\) yöntemi, kalibrasyon süresi boyunca robotun konumuna göre pozitif veya negatif bir sayı olarak dönme derecelerinin sayısını döndürecektir. Sıfır başlığı, herhangi bir zamanda, AnalogGyro nesnesindeki  **Reset\(\)**  yöntemini çağırarak sıfırlanabilir.

AnalogGyro nesnelerini kullanma fikri için aşağıdaki kod örneklerine bakın.

#### Robotu düz sürmek için gyro kullanmak

Aşağıdaki örnek programlar, robotun, GRO sensörünü RobotDrive sınıfı ile birlikte kullanarak düz bir çizgide sürmesine neden olur. RobotDrive.Drive yöntemi, hızı ve dönüş oranını argüman olarak alır; her ikisi de -1.0 ila 1.0 arasında değişir. Jiro, robotun ilk başlığından saptığı derecenin pozitif veya negatif sayısını gösteren bir değer döndürür. Robot düz gitmeye devam ettiği sürece, başlık sıfır olacaktır. Bu örnek, Drive yönteminin dönüş parametresini değiştirerek robotu devam ettirmek için gyro'yu kullanır.

Açı, robot sürücünün hızı için ölçeklendirmek üzere orantılı bir ölçekleme sabiti \(Kp\) ile çarpılır. Bu faktöre oransal sabit veya döngü kazancı denir. Artan Kp robotun daha hızlı düzeltmesine neden olur \(ancak çok yüksek ve osilasyon yapar\). Değerin azaltılması, robotun daha yavaş bir şekilde düzeltilmesine neden olur \(muhtemelen istenen pozisyona asla ulaşmaz\). Bu, oransal kontrol olarak bilinir ve ileri programlama bölümünün PID kontrol bölümünde daha ayrıntılı tartışılır.

```cpp
C++

class GyroSample : public SampleRobot
{
	RobotDrive myRobot; // robot drive system
	AnalogGyro gyro;
	static const float kP = 0.03;
	
public:
	GyroSample():
		myRobot(1, 2), // initialize the sensors in initilization list
		gyro(1)
	{
		myRobot.SetExpiration(0.1);
	}
	
	void Autonomous()
	{
		gyro.Reset();
		while (IsAutonomous())
		{
			float angle = gyro.GetAngle(); // get heading
			myRobot.Drive(-1.0, -angle * kP); // turn to correct heading
			Wait(0.004);
		}
		myRobot.Drive(0.0, 0.0); // stop robot
	}
};

```



```java
Java

package edu.wpi.first.wpilibj.templates;
import edu.wpi.first.wpilibj.AnalogGyro;
import edu.wpi.first.wpilibj.RobotDrive;
import edu.wpi.first.wpilibj.SampleRobot;
import edu.wpi.first.wpilibj.Timer;
public class GyroSample extends SampleRobot {

 \   private RobotDrive myRobot; // robot drive system
    private Gyro gyro;

 \   double Kp = 0.03;

    public GyroSample() {
        gyro = new AnalogGyro(1); \            // Gyro on Analog Channel 1
        myRobot = new RobotDrive(1,2); \ // Drive train jaguars on PWM 1 and 2
        myRobot.setExpiration(0.1);
 \   }

    public void autonomous() {
        gyro.reset();
        while (isAutonomous()) {
            double angle = gyro.getAngle(); // get current heading
            myRobot.drive(-1.0, -angle*Kp); // drive towards heading 0
            Timer.delay(0.004);
        }
        myRobot.drive(0.0, 0.0);
 \   }
}

```

### Ultrasonik Sensörler 

Ultrasonik Rangefinder, algılama konisi içindeki en yakın nesneye olan mesafeyi belirlemek için ultrasonik darbenin hareket süresini kullanır. Çeşitli ultrasonik sensörlerin aşağıdakileri içeren ölçüm sonuçlarını iletmesi için çeşitli farklı yollar vardır:

* Ping-Response \(ex. [Devantech SRF04](http://www.acroname.com/robotics/parts/R93-SRF04.html), [VEX Ultrasonic Rangefinder](http://www.vexrobotics.com/276-2155.html)\)
* Analog \(ex. [Maxbotix LV-MaxSonar-EZ1](http://www.maxbotix.com/Ultrasonic_Sensors/MB1010.htm)\)
* I2C \(ex. [Maxbotix I2CXL-MaxSonar-EZ2](http://www.maxbotix.com/Ultrasonic_Sensors/MB1222.htm)\)

#### Ping-Response Ultrasonik sensörler

Yukarıda görülen Devantech SRF04 gibi Ping-Response Ultrasonik sensörlerin kullanımına yardımcı olmak için WPILib bir Ultrasonik sınıf içerir. Bu tip sensörde iki transdüser, ultrasonik ses patlaması yapan bir hoparlör ve sesin yakındaki bir nesneden yansıyacağını dinleyen bir mikrofon bulunur. Bu, roboRIO'ya, ping'i başlatan diğerine ses geldiğinde söyleyen iki bağlantı gerektirir. Ultrasonik nesne, aktarım ile yankı alımı arasındaki süreyi ölçer.

#### Ultrasonik bir nesne oluşturmak ve mesafeyi okumak 



```cpp
C++

class ultrasonicSample : public SampleRobot
{
	Ultrasonic *ultra; // creates the ultra object

public:
	ultrasonicSample()
	{
		ultra = new Ultrasonic(1, 1); // assigns ultra to be an ultrasonic sensor which uses DigitalOutput 1 for the echo pulse and DigitalInput 1 for the trigger pulse
		ultra->SetAutomaticMode(true); // turns on automatic mode
	}

	void Teleop()
	{
		int range = ultra->GetRangeInches(); // reads the range on the ultrasonic sensor
	}
};

```

```java
Java

import edu.wpi.first.wpilibj.SampleRobot;
import edu.wpi.first.wpilibj.Ultrasonic;

public class RobotTemplate extends SampleRobot {

	Ultrasonic ultra = new Ultrasonic(1,1); // creates the ultra object andassigns ultra to be an ultrasonic sensor which uses DigitalOutput 1 for 
        // the echo pulse and DigitalInput 1 for the trigger pulse
    public void robotInit() {
    	ultra.setAutomaticMode(true); // turns on automatic mode
    }

    public void ultrasonicSample() {
    	double range = ultra.getRangeInches(); // reads the range on the ultrasonic sensor
    }
```

Yankı Darbe Çıkışı ve Tetik Darbe Girişinin her ikisi de bir Dijital Bölme üzerindeki dijital G / Ç bağlantı noktalarına bağlanmalıdır. Ultrasonik nesneyi oluştururken, yukarıdaki örneklerde gösterildiği gibi kurucuya hangi kanalları bağlı olduğunu belirtin. Bu durumda, ULTRASONIC\_ECHO\_PULSE\_OUTPUT ve ULTRASONIC\_TRIGGER\_PULSE\_INPUT, dijital G / Ç bağlantı noktası numaraları olarak tanımlanan iki sabittir. Bu bağlantılara sahip olmayan ultrasonik rangefinder için ultrasonik sınıfı kullanmayın. Bunun yerine, analog voltaj olarak aralığı döndüren bir ultrasonik sensör için bir AnalogChannel nesnesi gibi sensör için uygun sınıfı kullanın.

#### Analog Rangefinders

Birçok ultrasonik Rangefinder, menzili analog voltaj olarak geri döndürür. Mesafeyi elde etmek için analog voltajı hassasiyet veya ölçek faktörü ile çarpın \(tipik olarak inç / V veya inç / mV cinsinden\). Bu tip bir sensörü WPILib ile kullanmak için, bunu bir Analog Kanal olarak oluşturabilir ve doğrudan robot kodunuzda ölçeklendirmeyi yapabilir ya da bir ölçüm isteğinde bulunduğunuzda ölçeklendirmeyi yapacak bir sınıf yazabilirsiniz.

#### I2C ve diğer Dijital Rangefinders

I2C, SPI veya Serial üzerinden dijital olarak haberleşen Rangefinder de roboRIO ile birlikte kullanılabilir, ancak bu cihazlar için WPILib aracılığıyla belirli sınıflar sağlanmaz. Kullanılan veriyoluna bağlı olarak uygun iletişim sınıfını kullanın ve cihazı hangi veri veya isteklerin gönderileceğini ve alınan verilerin hangi formatta yer alacağını belirlemek için bölümün veri sayfasına bakın.

### Counters -  Dönüş rotasyonu, sayma darbeleri ve daha fazlası

Counter nesneler, bir dijital giriş sinyali veya bir analog tetikleyiciden gelen girdileri sayabilen son derece esnek öğelerdir.

#### Counter genel bakış

![](../.gitbook/assets/image%20%289%29.png)

FPGA'da bulunan ve her biri giriş sinyalinin türüne bağlı olarak birkaç modda çalışabilen 8 adet Yukarı / Aşağı Sayıcı birimi vardır:



* Gear-Tooth/ Darbe Genişliği modu - Giriş darbesinin genişliğine bağlı olarak yukarı / aşağı sayımını etkinleştirir. Bu, GearTooth sensör sınıfını yön algılama ile uygulamak için kullanılır.
* Semi-Period modu - Giriş sinyalinin bir kısmının periyodunu sayar. Bu mod, eko pulsunun uçuş süresini ölçmek için Ultrasonik sınıf tarafından kullanılır. 
* Harici Yön modu - Bir girişin sinyallerini, bir girişe ikinci giriş tarafından belirlenen yön \(yukarı / aşağı\) ile sayar. 
* "Normal mod" / İki Darbe modu - 2 bağımsız kaynaktan kenarları sayabilir \(1 yukarı, 1 aşağı\)

**Gear-Tooth Modu ve GearTooth sensörleri**

![](../.gitbook/assets/image%20%2883%29.png)

GearTooth sensörleri, demirli dişli**ye** veya zincir dişi dişlerine bitişik olarak monte edilecek ve bir dişin ne zaman geçtiğini algılayacak şekilde tasarlanmıştır.  Gear-Tooth sensörü, geçmekte olan dişlerin neden olduğu tarladaki değişimleri ölçebilen bir mıknatıs ve katı-hal cihazı kullanan bir Hall-Effect cihazdır. Yukarıdaki resim, metal dişli rotasyonunu ölçmek için monte edilmiş GearTooth sensörünü göstermektedir. Plastik dişliye bir metal dişli bağlandığına dikkat edin. Gear Tooth sensörünün dönüşü tespit etmek için onun tarafından geçen bir demir materyaline ihtiyacı vardır.

FPGA sayacının Gear-Tooth modu, 2006 FRC'de verilen ATS651 gibi her dişin geçtiği gibi yaydığı nabzın uzunluğunu değiştirerek dönme yönünü gösteren Gear Tooth sensörleri ile çalışmak üzere tasarlanmıştır. KOP.

#### Semi-Period modu

C++

  
`Counter *exampleCounterHi = new Counter(0);  
Counter *exampleCounterLow = new Counter(3);  
exampleCounterHi->SetSemiPeriodMode(true);  
exampleCounterLow->SetSemiPeriodMode(false);  
double highPulse = exampleCounterHi->GetPeriod();  
double lowPulse = exampleCounterLow->GetPeriod();`

Java  
  
  
`Counter exampleCounterHi = new Counter(0);  
Counter exampleCounterLow = new Counter(3);  
exampleCounterHi.setSemiPeriodMode(true);  
exampleCounterLow.setSemiPeriodMode(false);  
double highPulse = exampleCounterHi.getPeriod();  
double lowPulse = exampleCounterLow.getPeriod();`

Sayacın Semi-Period modu, tek bir kaynaktan \(Yukarı Kaynak\) yüksek bir darbenin \(düşen kenarın düşme kenarına kadar\) ya da düşük bir darbenin \(yükselen kenara düşen kenar\) darbe genişliğini ölçecektir. Yüksek darbeleri ölçmek için setSemiPeriodMode \(true\) ve düşük atımları ölçmek için setSemiPeriodMode \(yanlış\) çağrısı yapın. Her iki durumda da, son ölçülen nabzın uzunluğunu \(saniye cinsinden\) elde etmek için getPeriod \(\) öğesini çağırın.

**Dış yön modu**

Sayacın harici yön modu, bir kaynaktan \(Yukarı Kaynak\) kenarları sayar ve yönü belirlemek için diğer kaynağı \(Aşağı Kaynak\) kullanır. Bu modun en yaygın kullanımı 1x ve 2x modunda dördün kod çözme işlemidir. Bu kullanım durumu, bir dahili Sayaç nesnesini oluşturan Enkoder sınıfı tarafından ele alınmakta ve bir sonraki eşyada Enkoder - Bir tekerleğin ya da başka bir milin dönmesinin ölçülmesiyle kapsanmaktadır.

#### Normal mod

**C++**  
`Counter *normalCounter = new Counter();  
normalCounter->SetUpSource(1);  
normalCounter->SetUpDownCounterMode();`  
  
**Java**  
`Counter normalCounter = new Counter();  
normalCounter.setUpSource(1);  
normalCounter.setUpDownCounterMode();`

Yukarı / Aşağı modu veya İki Darbe modu olarak da bilinen sayacın "normal modu", iki ayrı kaynağa kadar meydana gelen darbeleri, Yukarı için bir kaynak ve Aşağı için bir kaynak sayar. Bu modun ortak kullanım durumu, tek yönlü bir kodlayıcı olarak yansıtıcı bir sensör veya hall ****effect sensörü ile tek bir kaynak \(Up Source\) kullanmaktadır. Yukarıdaki kod örneği, Sayaç kaynaklarını ayarlamak için alternatif bir yöntem göstermektedir, bu yöntem, modlardan herhangi biri için geçerlidir. Semi Period modu örneğinde gösterilen yöntem, Normal Mod dahil olmak üzere sayacın tüm modları için de geçerlidir.

#### Counter Ayarları

**C++**  
`Counter *normalCounter = new Counter(1);  
normalCounter->SetMaxPeriod(.1);  
normalCounter->SetUpdateWhenEmpty(true);  
normalCounter->SetReverseDirection(false);  
normalCounter->SetSamplesToAverage(10);  
normalCounter->SetDistancePerPulse(12);`  
  
**Java**  
`Counter normalCounter = new Counter(1);  
normalCounter.setMaxPeriod(.1);  
normalCounter.setUpdateWhenEmpty(true);  
normalCounter.setReverseDirection(false);  
normalCounter.setSamplesToAverage(10);  
normalCounter.setDistancePerPulse(12);`

Sayaç davranışının çeşitli yönlerini kontrol etmek için ayarlanabilen birkaç farklı parametre vardır:



* Max Period - Cihazın hala hareket ettiği düşünülen maksimum süre \(saniye cinsinden\). Bu değer, getStopped \(\) yönteminin durumunu belirlemek ve getPeriod \(\) ve getRate \(\) yöntemlerinin çıkışını etkilemek için kullanılır.
* Update When Empty - Sahte olarak ayarlanması, sayaç durduğunda \(yukarıda tanımlanan Maks. Periyot'a göre\) en son süreyi sayaçta saklar. Bu parametrenin True olarak ayarlanması, 0 durmuş sayacın periyodu olarak geri döner.
* Reverse Direction - Sadece harici yön modunda geçerlidir. Bu parametrenin true olarak ayarlanması, sayacın harici yön modunun sayma yönünü tersine çevirir.
* Samples to Average - Dönemi belirlerken numune sayısını ortalamaya ayarlar. Ortalama alma, mekanik kusurları \(örneğin, bir yansıtıcı sensörü bir kodlayıcı olarak kullanırken eşit olmayan aralıklı reflektörler gibi\) veya çözünürlüğü arttırmak için aşırı örneklemeyi hesaba katmak istenebilir. Geçerli değerler 1 ila 127 örnektir.
* Distance Per Pulse - getDistance \(\) yöntemini kullanırken saymadan uzaklığı belirlemek için kullanılan çarpanı ayarlar.

#### Counter'i sıfırlama

**C++**  
`Counter *normalCounter = new Counter(1);  
normalCounter->Reset();`  
  
**Java**  
`Counter normalCounter = new Counter(1);  
normalCounter.reset();`

**Counter değerlerini almak**

**C++**  
`Counter *normalCounter = new Counter(1);  
int count = normalCounter->Get();  
double distance = normalCounter->GetDistance();  
double period = normalCounter->GetPeriod();  
double rate = normalCounter->GetRate();  
bool direction = normalCounter->GetDirection();  
bool stopped = normalCounter->GetStopped();`  
  
**Java**  
`Counter normalCounter = new Counter(1);  
int count = normalCounter.get();  
double distance = normalCounter.getDistance();  
double period = normalCounter.getPeriod();  
double rate = normalCounter.getRate();  
boolean direction = normalCounter.getDirection();  
boolean stopped = normalCounter.getStopped();`

Aşağıdaki değerler Counter'den alınabilir:

* Count - Mevcut sayım. reset\(\) çağrılarak sıfırlanabilir 
* Distance - Sayaçtan güncel uzaklık okuması. Bu Sayım Başına Ölçek Faktörü ile çarpılan sayıdır.
* Period - Sayacın saniye cinsinden geçerli periyodu. Sayaç durdurulursa, Boş Değer Güncelleme parametresinin ayarına bağlı olarak bu değer 0'a dönebilir.
* Rate - Sayacın birim / saniye cinsinden geçerli oranı. DistancePerPulse kullanılarak döneme bölünerek hesaplanır. Sayaç durdurulursa, bu değer dile bağlı olarak Inf veya NaN'ye dönüşebilir.
* Direction - Son değer değişiminin yönü \(Yukarı doğru, Aşağı doğru yanlış\)
* Stopped - Sayaç şu anda durdurulduysa \(süre Max Dönemini aşmıştır\)

### Enkoderler - Bir tekerleğin veya diğer milin dönüşünün ölçülmesi

Enkoderler, bir eğirme milinin dönüşünü ölçmek için cihazlardır. Enkoderler tipik olarak, bir çarkın döndüğü mesafeyi ölçmek için kullanılır; bu, robotun gittiği mesafeye tercüme edilebilir. Ölçülen bir süre boyunca kat edilen mesafe, robotun hızını temsil eder ve enkoderler için başka bir yaygın kullanımdır. Enkoderler, darbeler arasındaki süreyi belirleyerek de dönüş oranını doğrudan ölçebilir. Bu makalede, dörtlü enkoderlerin \(aşağıda tanımlanmıştır\) kullanımı ele alınmıştır. Kuadratür olmayan artımlı enkoderler için, sayaçlarla ilgili makaleye bakınız. Mutlak enkoderler için uygun makale giriş tipine \(en yaygın olarak analog, I2C veya SPI\) bağlı olacaktır.

**Quadrature Enkoder Genel Bakış**

![](../.gitbook/assets/image%20%2823%29.png)

Dörtlü bir enkoder, 90 derece faz dışı iki algılama elemanından oluşan şaft dönüşünü ölçen bir cihazdır. FRC'de tipik olarak kullanılan en yaygın ****enkoder türü, şeritli veya yarık kod tekerleğine ve bir tanesi iki ayrı 90 derece aralıklı bir veya daha fazla ışık kaynağı \(LED\) kullanan bir optik enkoderdir \(bunlar, iletimi algılamak için LED'in karşısında bulunabilirler\) yansımayı ölçmek için LED ile aynı tarafta\). Sinyaller arasındaki faz farkı, hangi sinyalin diğerini "yönlendirdiğini" belirleyerek dönme yönünü tespit etmek için kullanılabilir.

Enkoderler vs Counters  
  


![](../.gitbook/assets/image%20%2839%29.png)

FRC FPGA, 2 kanallı bir dörtlü enkoder sinyalin 4x kod çözme işlemini yapabilen 8 adet Quadrature dekoder modülüne sahiptir. Bu, modülün, her bir kanalın her birindeki her bir darbenin yükselen ve düşen kenarlarını, kod çarkındaki her şerit için 4 kenara sahip olacak şekilde sayması anlamına gelir. Quadrature dekoder modülü ayrıca, her bir devir başına bir puls üreten bazı enkoderlerde  özellik olan bir endeks kanalını idare edebilir. Counter FPGA modülleri, bir kanalın yükselen veya yükselen ve düşen kenarlarının sayıldığı ve ikinci kanalın yönü belirlemek için kullanıldığı 1x veya 2x kod çözme için kullanılır. Her iki durumda da, tüm kareleme encoderler için Encoder sınıfının kullanılması tavsiye edilir; sınıf, seçtiğiniz kodlama türüne göre uygun FPGA modülünü atayacaktır.

**Bir Enkoder nesnesinin oluşturulması**

**C++**  
`Encoder *enc;  
enc = new Encoder(0, 1, false, Encoder::EncodingType::k4X);`  
  
**Java**  
`Encoder enc;  
enc = new Encoder(0, 1, false, Encoder.EncodingType.k4X);`

Enkoder oluşturmak için kullanabileceğiniz birçok kurucu vardır, ancak en yaygın olarak yukarıda gösterilmiştir. Örnekte, 0 ve 1, iki dijital giriş için port numaralarıdır ve false, Enkoderin sayma yönünü tersine çevirmemesini söyler. Algılanan yön, enkoderin ölçülen şafta göre nasıl monte edildiğine bağlı olabilir. K4X, FPGA'dan bir enkoder modülünün kullanılmasını ve 4X doğruluğunun elde edilmesini sağlar.

**Enkoder Parametrelerini Ayarlama**

**C++**  
`Encoder *sampleEncoder = new Encoder(0, 1, false, Encoder::EncodingType::k4X);  
sampleEncoder->SetMaxPeriod(.1);  
sampleEncoder->SetMinRate(10);  
sampleEncoder->SetDistancePerPulse(5);  
sampleEncoder->SetReverseDirection(true);  
sampleEncoder->SetSamplesToAverage(7);`  
  
**Java**  
`Encoder sampleEncoder = new Encoder(0, 1, false, Encoder.EncodingType.k4X);  
sampleEncoder.setMaxPeriod(.1);  
sampleEncoder.setMinRate(10);  
sampleEncoder.setDistancePerPulse(5);  
sampleEncoder.setReverseDirection(true);  
sampleEncoder.setSamplesToAverage(7);`

Enkoder sınıfının aşağıdaki parametreleri kod aracılığıyla ayarlanabilir:



* Max Period - cihazın hala hareket ettiği düşünülen maksimum süre \(saniye cinsinden\). Bu değer, getStopped \(\) yönteminin durumunu belirlemek ve getPeriod \(\) ve getRate \(\) yöntemlerinin çıkışını etkilemek için kullanılır. Bu, bireysel kanaldaki atımlar arasındaki zamandır \(ölçek faktörü hesaba katılır\). Min Hız parametresinin, darbe başına düşen mesafeyi hesaba katan, bunun yerine mühendislik ünitelerinde hızı ayarlamanıza izin vermesi önerilir.
* Min Rate - Cihaz durdurulan kabul edilmeden önce asgari hızını ayarlar. Bu, hem darbe faktörü hem de darbe başına uzaklığı telafi eder ve bu nedenle mühendislik ünitelerine \(RPM, RPS, Derece / sn, In / s, vb.\) Girilmelidir.
* Distance Per Pulse - Puls ve mesafe arasındaki ölçek faktörünü ayarlar. Kütüphane, kod çözme ölçeği faktörünü \(1x, 2x, 4x\) ayrı ayrı hesaba katar,
* Reverse Direction - Enkoder montajı varsayılan sayma yönünü anlamsız hale getiriyorsa yönü çevirmek için kullanılan enkoder sayımlarını belirler.
* Samples to Average - Dönemi belirlerken numune sayısını ortalamaya ayarlar. Ortalama alma, mekanik kusurları \(örneğin, bir yansıtıcı sensörü bir kodlayıcı olarak kullanırken eşit olmayan aralıklı reflektörler gibi\) veya çözünürlüğü arttırmak için aşırı örneklemeyi hesaba katmak istenebilir. Geçerli değerler 1 ila 127 örnektir.

 **Enkoderleri Başlatmak, Durdurmak ve Sıfırlamak**

**C++**  
`Encoder *sampleEncoder = new Encoder(0, 1, false, Encoder::EncodingType::k4X);  
sampleEncoder->Reset();`  
  
**Java**  
`Encoder sampleEncoder = new Encoder(0, 1, false, Encoder.EncodingType.k4X);  
sampleEncoder.reset();`

Kodlayıcı, oluşturulduğu anda saymaya başlayacaktır. Enkoder değerini sıfırlamak için **reset**\(\) komutunu kullanın.

#### Enkoder Değerlerini Almak

**C++**  
`Encoder *sampleEncoder = new Encoder(0, 1, false, Encoder::EncodingType::k4X);  
int count = sampleEncoder->Get();  
double distance = sampleEncoder->GetRaw();  
double distance = sampleEncoder->GetDistance();  
double period = sampleEncoder->GetPeriod();  
double rate = sampleEncoder->GetRate();  
boolean direction = sampleEncoder->GetDirection();  
boolean stopped = sampleEncoder->GetStopped();`  
**Java**  
`Encoder sampleEncoder = new Encoder(0, 1, false, Encoder.EncodingType.k4X);  
int count = sampleEncoder.get();  
double distance = sampleEncoder.getRaw();  
double distance = sampleEncoder.getDistance();  
double period = sampleEncoder.getPeriod();  
double rate = sampleEncoder.getRate();  
boolean direction = sampleEncoder.getDirection();  
boolean stopped = sampleEncoder.getStopped();`

Aşağıdaki değerler Enkoderden alınabilir:



* Count - Mevcut sayım. reset\(\) ile sıfırlanabilir.
* Raw Count - Kod çözme faktörü için telafisiz sayım.
* Distance - Counterden güncel uzaklık okuması. Bu Sayım Başına Ölçek Faktörü ile çarpılan sayıdır.
* Period - Counterin saniye cinsinden geçerli periyodu. Sayaç durdurulursa bu değer 0'a dönebilir. Bu kullanımdan kaldırılır, bunun yerine oranın kullanılması önerilir.
* Rate - Sayacın birim / saniye cinsinden geçerli oranı. DistancePerPulse kullanılarak döneme bölünerek hesaplanır. Sayaç durdurulursa, bu değer dile bağlı olarak Inf veya NaN'ye dönüşebilir.
* Direction - Son değer değişiminin yönü \(Yukarı True ,Aşağı False\)
* Stopped - sayaç şu anda durdurulursa \(periyot Max Dönemi aşmıştır\)

### Analog Girişler

RoboRIO Analog to Digital modülünün daha basit kontrol cihazlarında bulunmayan bazı özellikleri vardır. Analog kanalları, yuvarlak bir robin tarzında otomatik olarak örnekleyerek, 500 ks / s \(500.000 örnek / saniye\) bir kombine örnek oranı sağlar. Bu kanallar, program tarafından kullanılan değeri sağlamak için isteğe bağlı olarak aşırı örneklenebilir ve ortalaması alınabilir. Ortalama değerlere ek olarak, ham tamsayı ve kayan noktalı voltaj çıkışı vardır. Aşağıdaki şema bu süreci özetlemektedir.

![Analog System Diagram](../.gitbook/assets/image%20%2879%29.png)

Sistem birkaç örnek ortalama olduğunda, bölünme, tamsayı değerli sonucun üretilmesinde kaybedilen cevabın kesirli bir kısmıyla sonuçlanır. Aşırı örnekleme, fazladan örneklerin toplandığı ancak ortalama üretmek için bölünmediği bir tekniktir. Sistemin 16 kez fazla örnekleme olduğunu varsayın - bu, döndürülen değerlerin aslında ortalama 16 kat olduğu anlamına gelir. Aşırı örneklenmiş değeri kullanmak, döndürülen değerde ek bir hassasiyet sağlar.

**Bir Analog Giriş Oluşturmak**

**C++**  
`AnalogInput *ai;  
ai = new AnalogInput(0);`  
  
**Java**  
`AnalogInput ai;  
ai = new AnalogInput(0);`

 Bir AnalogInput nesnesini oluşturmak için, istenen girişin kanal numarasını girmeniz yeterlidir.

#### Aşırı Örnekleme ve Ortalama Alma 

![](../.gitbook/assets/image%20%2865%29.png)

Ortalama ve aşırı örneklenmiş değerlerin sayısı her zaman ikidir \(aşırı örnekleme / ortalama alma sayısı\). Bu nedenle, aşırı örneklenmiş veya ortalama değerler iki bittir; burada "bitler", yöntemlere geçirilir: SetOversampleBits \(bitler\) ve SetAverageBits \(bitler\). Değerlerin analog giriş kanalından üretildiği gerçek oran, ortalama ve fazla örneklenmiş değerler ile azaltılır. Örneğin, aşırı örneklenmiş bitlerin sayısını 4'e ve ortalama bitleri 2'ye ayarlamak, teslim edilen örneklerin sayısını 16x ve 4x veya toplam 64x azaltacaktır.

Örnek Kodlar

**C++**  
`AnalogInput *exampleAnalog = new AnalogInput(0);  
int bits;  
exampleAnalog->SetOversampleBits(4);  
bits = exampleAnalog->GetOversampleBits();  
exampleAnalog->SetAverageBits(2);  
bits = exampleAnalog->GetAverageBits();`  
  
**Java**  
`AnalogInput exampleAnalog = new AnalogInput(0);  
int bits;  
exampleAnalog.setOversampleBits(4);  
bits = exampleAnalog.getOversampleBits();  
exampleAnalog.setAverageBits(2);  
bits = exampleAnalog.getAverageBits();`

 Yukarıdaki kod, bir analog kanaldaki fazla örnek bitlerinin ve ortalama bitlerin sayısını alma ve ayarlamanın bir örneğini gösterir.

#### Sample Rate

**C++**  
`AnalogInput::SetSampleRate(62500);`  
  
**Java**  
`AnalogInput.setGlobalSampleRate(62500);`

Sample Rate analog I / O modülü başına sabitlenmiştir, böylece belirli bir modüldeki tüm kanallar aynı hızda örneklenmelidir. Bununla birlikte, her bir kanal için ortalama ve sample rate değiştirilebilmektedir. Bazı sensörlerin \(şu anda sadece Gyro\) kullanımı, smaple rate bağlı olduğu modül için belirli bir değere ayarlayacaktır. Yukarıdaki örnek, bir modül için sample ratenin, saniyede kanal başına 62.500 örnek varsayılan değerine \(toplam 500kS / s\) ayarlandığını göstermektedir.

**Analog Değerleri Okuma**

**C++**  
`AnalogInput *exampleAnalog = new AnalogInput(0);  
int raw = exampleAnalog->GetValue();  
double volts = exampleAnalog->GetVoltage();  
int averageRaw = exampleAnalog->GetAverageValue();  
double averageVolts = exampleAnalog->GetAverageVoltage();`  
  
**Java**  
`AnalogInput exampleAnalog = new AnalogInput(0);  
int raw = exampleAnalog.getValue();  
double volts = exampleAnalog.getVoltage();  
int averageRaw = exampleAnalog.getAverageValue();  
double averageVolts = exampleAnalog.getAverageVoltage();`

Bir analog kanaldan Analog giriş değerlerini okumak için bir dizi seçenek vardır:  
  


1. Raw value - ADC'nin 0-5V aralığını temsil eden anlık ham 12 bit \(0-4096\) değeri. Bu yöntemin, modülde depolanan kalibrasyon bilgilerini dikkate almadığını unutmayın.
2. Voltage - Kanalın anlık voltaj değeri. Bu yöntem, ham değeri bir voltaja dönüştürmek için modülde depolanan kalibrasyon bilgilerini dikkate alır.
3. Average Raw value - Aşırı örnekleme ve ortalama motorun ham, ölçeklendirilmemiş değer çıkışı. Aşırı örnekleme ve ortalama alma ve her biri için bit sayısını nasıl ayarlayacağınız hakkında bilgi için yukarıya bakın.
4. Average Voltage - ****Aşırı örnekleme ve ortalama motordan ölçülen voltaj değeri çıkışı. Bu yöntem, ham ortalama değerini bir voltaja dönüştürmek için saklanan kalibrasyon bilgisini kullanır.

 **Akümülatör**

Analog akümülatör, FPGA'nın analog sinyaller için bir integratör olarak görev yapan ve zaman içindeki değeri toplayan bir parçasıdır. Bu davranış, istenen yere ait genel örnek bir gyro içindir. Bir gyro, dönme hızına karşılık gelen bir analog sinyal çıkarır, ancak yaygın olarak istenen ölçüm, yönlendirme veya toplam rotasyonel yer değiştirmedir. Orandan çıkmak için bir entegrasyon gerçekleştirirsiniz. Bu işlemi donanım seviyesinde gerçekleştirerek, robot kodunda uygulamayı denemenizden çok daha hızlı gerçekleşebilir. Akümülatör ayrıca akümülatöre eklemeden önce analog değere bir denge de uygulayabilir. Gyro örneğine dönersek, çoğu jiroskop dönmediğinde tam skalanın 1 / 2'lik bir voltajını verir ve dönüş yönünü belirtmek için bu referansın üstünde ve altında voltajı değiştirir.

  
**Akümülatör kurmak**

**C++**  
`AnalogInput *exampleAnalog = new AnalogInput(0);  
exampleAnalog->SetAccumulatorInitialValue(0);  
exampleAnalog->SetAccumulatorCenter(2048);  
exampleAnalog->SetAccumulatorDeadband(10);  
exampleAnalog->ResetAccumulator();`  
  
**Java**  
`AnalogInput exampleAnalog = new AnalogInput(0);  
exampleAnalog.setAccumulatorInitialValue(0);  
exampleAnalog.setAccumulatorCenter(2048);  
exampleAnalog.setAccumulatorDeadband(10);  
exampleAnalog.resetAccumulator();`

FPGA'da, 0 ve 1 kanallarına bağlı iki adet akümülatör vardır. Analog akümülatör ile kullanmak istediğiniz herhangi bir cihaz, bu iki kanaldan birine bağlanmalıdır. Akümülatörü kullanmak için ayarlanması gereken zorunlu parametreler yoktur, ancak cihaza bağlı olarak aşağıdakilerden bazılarını veya tümünü ayarlamak isteyebilirsiniz:



1. Accumulator Initial Value -Bu, akümülatörün sıfırlanırken geri döndürdüğü ham değerdir. Değer koda geri gönderilmeden önce donanım akümülatörünün çıkışına eklenir.
2. Accumulator Center - sample akümülatöre uygulanmadan önce her bir sample bu ham değer çıkarılır. Akümülatörün boru hattındaki fazla örnek ve ortalama motordan sonra olduğuna dikkat edin, bu nedenle aşırı örnekleme bu parametre için uygun değeri etkileyecektir.
3. Accumulator Deadband - Akümülatörün sampleyi 0 olarak ele alacağı merkez noktası çevresindeki ölü değer.
4. Accumulator Reset - Akümülatörün değerini Başlangıç Değerine sıfırlar \(varsayılan olarak 0'dır\).

#### Akümülatörden Değer Okuma

**C++**  
`AnalogInput *exampleAnalog = new AnalogInput(0);  
long count = exampleAnalog->GetAccumulatorCount();  
long value = exampleAnalog->GetAccumulatorValue();  
AccumulatorResult *result = new AccumulatorResult();  
exampleAnalog->GetAccumulatorOutput(result);  
count = result->count;  
value = result->value;`  
  
**Java**  
`AnalogInput exampleAnalog = new AnalogInput(0);  
long count = exampleAnalog.getAccumulatorCount();  
long value = exampleAnalog.getAccumulatorValue();  
AccumulatorResult result = new AccumulatorResult();  
exampleAnalog.getAccumulatorOutput(result);  
count = result.count;  
value = result.value;`

### Potansiyometre - Ortak açı veya doğrusal hareket ölçme

Potansiyometreler, bir mekanizmanın mutlak açısal dönüşünü veya doğrusal hareketini ölçmek için kullanılan ortak bir analog sensördür. Potansiyometre, değişken direnç bölücüden hareketli bir kontak kullanan üç terminalli bir cihazdır. Dış kontaklar 5V ve toprağa bağlandığında ve değişken kontak bir analog girişe bağlandığında, analog giriş potansiyometre çevrildiğinde değişen bir analog voltaj görülecektir.

#### Potansiyometre Koniği\(incelmesi\)

Potansiyometrenin incelmesi, pozisyon ve direnç arasındaki ilişkiyi açıklar. İki ortak incelici doğrusal ve logaritmiktir. Doğrusal bir konik potansiyometre, direnci milin dönüşüne orantılı olarak değiştirecektir; Örneğin, şaft, dönme noktasının orta noktasında direnç değerinin% 50'sini ölçecektir. Logaritmik bir konik potansiyometre, milin dönüşüyle logaritmik olarak direnci değiştirecektir. Logaritmik potansiyometreler, ses seviyesinde insan algısı da logaritmik olduğundan, ses kontrollerinde yaygın olarak kullanılmaktadır.

Potansiyometreler FRC için  lineer potansiyometreleri kullanılmalıdır, böylece açı doğrudan voltajdan çıkarılabilir.

#### WPILib ile Potansiyometre kullanma

Potansiyometreler AnalogInput sınıfı ile okunabilir veya Potansiyometre arayüzünü uygulayan AnalogPotentiometer sınıfı ile birlikte okunabilir. AnalogPotentiometer sınıfı, sensörü orantısal olarak okuyacaktır \(Analog besleme voltajını dengeleyecektir\) ve anlamlı birimleri döndürmek için gerilimi ölçekleyecek ve dengeleyecektir.

#### Potansiyometre Oluşturmak

**C++**  
`Potentiometer *pot;  
pot = new AnalogPotentiometer(0, 360, 30);  
AnalogInput *ai = new AnalogInput(1);  
pot = new AnalogPotentiometer(ai, 360, 30);`  
  
**Java**  
`Potentiometer pot;  
pot = new AnalogPotentiometer(0, 360, 30);  
AnalogInput ai = new AnalogInput(1);  
pot = new AnalogPotentiometer(ai, 360, 30);`

Potansiyometre kurucusu 3 parametre alır: analog giriş için bir kanal numarası, faydalı birimleri döndürmek için 0-1 oranmetrik değerini çarpmak için bir ölçek faktörü ve ölçeklemeden sonra eklenecek bir ofset. Genel olarak, en kullanışlı ölçek faktörü, potansiyometrenin açısal veya doğrusal tam ölçeği olacaktır. Örneğin, bir robot koluna bağlı ideal bir tek dönüşlü doğrusal potansiyometreye sahip olduğunuzu varsayalım. Bu pot, 0V-5V aralığında 360 derece dönecek, bu yüzden ölçek faktörü için, çıktılar derece cinsinden olacak. Potansiyometrenin hizalamada küçük kayma nedeniyle kırılmasını önlemek için, kolun "sıfır noktası" ile potansiyometrelerin aralığına çok az bir şekilde monte edilebilir, yukarıdaki örnek başlangıç ​​değerine sahip olan potansiyometreyi gösterir. Uzaklık kullanılarak reddedilen 30 derece, böylece mekanizmanın "sıfır noktası" nda çıktı 0 olur.

**Hesaplamalar**

Potansiyometre çıkışı aşağıdaki formül kullanılarak hesaplanır: \(Analog Giriş Voltajı / Analog Besleme Gerilimi\) \* FullScale + Offset. Gördüğünüz gibi, hesaplamanın ilk kısmının sonucu 0 ila 1 aralığında birimsizdir. Bu, çıkış birimlerinin ölçek faktörünün birimleri ile aynı olduğu anlamına gelir. Ofset, ölçeklendirilmiş miktara eklenir, böylece ölçek faktörü ile aynı birimlere sahip olmalıdır.

**Çıktının Okunması**

**C++**  
`Potentiometer *pot = new AnalogPotentiometer(0, 360, 30);  
double degrees = pot->Get();`  
  
**Java**  
`Potentiometer pot = new AnalogPotentiometer(0, 360, 30);  
double degrees = pot.get()`

### Analog tetikleyiciler

Analog bir tetikleyici, bir analog sinyali FPGA'ya yerleşik kaynakları kullanarak dijital sinyale dönüştürmenin bir yoludur. Elde edilen dijital sinyal daha sonra doğrudan kullanılabilir veya sayaç veya enkoder modülleri gibi FPGA'nın diğer dijital bileşenlerine beslenebilir. Analog tetikleme modülü, analog sinyalleri kod tarafından belirlenen bir voltaj aralığına göre karşılaştırarak çalışır. Spesifik dönüş tipleri ve anlamları, kullanımdaki analog tetikleme moduna bağlıdır.

#### Bir Analog Tetik**leyici** Oluşturulması

**C++**  
`AnalogTrigger *trigger0 = new AnalogTrigger(0);  
AnalogInput *ai1 = new AnalogInput(1);  
AnalogTrigger *trigger1 = new AnalogTrigger(ai1);`  
  
**Java**  
`AnalogTrigger trigger0 = new AnalogTrigger(0);  
AnalogInput ai1 = new AnalogInput(1);  
AnalogTrigger trigger1 = new AnalogTrigger(ai1);`

Bir analog tetikleyici oluşturmak, bir kanal numarasının veya oluşturulmuş bir Analog Kanal nesnesinin geçirilmesini gerektirir.

#### Analog Tetik Voltaj Aralığı Ayarı

**C++**  
`AnalogTrigger *trigger = new AnalogTrigger(0);  
trigger->SetLimitsRaw(2048, 3200);  
trigger->SetLimitsVoltage(0, 3.4);`  
  
**Java**  
`AnalogTrigger trigger0 = new AnalogTrigger(0);  
trigger.setLimitsRaw(2048, 3200);  
trigger.setLimitsVoltage(0, 3.4);`

Analog tetikleyicinin voltaj aralığı, ham birimlerde \(0V ila 5V'yi temsil eden 0 ila 4096\) veya voltajlarda ayarlanabilir. Her iki durumda da, yüksek hızda örnekleme kullanılıyorsa, değer kümesi dikkate alınmaz, kullanıcı kodu ayarlanmadan önce tetik penceresinin uygun telafisini gerçekleştirmelidir.

#### Filtreleme ve Ortalama Alma

**C++**  
`AnalogTrigger *trigger = new AnalogTrigger(0);  
trigger->SetAveraged(true);  
trigger->SetAveraged(false);  
trigger->SetFiltered(true);`  
  
**Java**  
`AnalogTrigger trigger0 = new AnalogTrigger(0);  
trigger.setAveraged(true);  
trigger.setAveraged(false);  
trigger.setFiltered(true);`

Analog tetikleyici, isteğe bağlı olarak, ya ortalama değerin \(ortalama ve aşırı örnek motorun çıkışı\) ya da ham analog kanal değeri yerine filtrelenmiş bir değeri kullanacak şekilde ayarlanabilir. Bu seçeneklerden en fazla biri bir seferde seçilebilir, filtre, ortalama sinyalin üstüne uygulanamaz.

Analog tetikleyicinin filtreleme seçeneği 3 noktalı ortalama reddetme filtresi kullanır. Bu filtre, son üç veri noktasının dairesel bir arabelleğini kullanır ve çıktı olarak medyana en yakın nokta seçer. Bu filtrenin birincil kullanımı, analog tetikleyicinin Yükselen Kenarı ve Düşen Kenar işlevini kullanarak geçişleri algıladığında pencerenin içinde \(ortalamaya veya örneklemeye bağlı olarak\) ortaya çıkan veri noktalarını reddetmektir \(aşağıya bakınız\).

#### Analog Tetikleyiciden Doğrudan Çıkışlar

**C++**  
`AnalogTrigger *trigger = new AnalogTrigger(0);  
bool value;  
value = trigger->GetInWindow();  
value = trigger->GetTriggerState();`  
  
**Java**  
`AnalogTrigger trigger0 = new AnalogTrigger(0);  
boolean value;  
value = trigger.getInWindow();  
value = trigger.getTriggerState();`

Analog tetikleme sınıfının iki doğrudan çıkışı vardır:



* In Window - Değer menzil dahilinde ise true ve eğer dışındaysa \(yukarıda veya aşağıda\) false döndürür
* Trigger State - Değer üst limitin üzerindeyse, eğer alt limitin altında ise false değerini döndürür ve eğer varsa, önceki durumu korur.  

