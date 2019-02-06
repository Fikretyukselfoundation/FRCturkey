# Aktüatörler kullanma \(motorlar, servolar ve röleler\)

###  Aktüatöre Genel Bakış

 Bu bölümde speed controller, röleler  veWPILib yöntemleri ile motorların ve pnömatiklerin kontrolü ele alınmaktadır.

**Aktüatör tipleri**

![](../.gitbook/assets/image%20%2816%29.png)

 Yukarıda gösterilen grafik, WPILib aracılığıyla kontrol edilebilen aktüatör tiplerini göstermektedir. Bu bölümdeki makaleler, bu tip aktüatörlerin her birini ve bunları kontrol eden WPILib yöntemleri ve sınıflarını kapsayacaktır.

###  Hız kontrolörlü nesnelere sahip motorlar \(Victors, Talons ve Jaguars\)

**PWM kontrolörleri, kısa çalışma teorisi**

Kısaltma PWM **\(** **Pulse Width Modulation \)**   **Sinyal Genişlik Modülasyonu** anlamına gelir. Victor , Talon ve Jaguar speed controllerleri için  PWM girişlerine başvurabilirsiniz bu yöntem motor hızını kontrol etmek için kullanılır. Motorun hızını kontrol etmek için kontrolör motorun algılanan giriş voltajını değiştirmelidir. Bunu yapmak için kontrolör, kontrol sinyaline dayanarak üzerinde olduğu süreyi değiştirerek, giriş voltajını çok hızlı bir şekilde açıp kapatır. FRC'DE kullanılan motor tiplerinin Mekanik Ve Elektronik zaman sabitleri nedeniyle bu hızlı anahtarlama, sabit bir düşük voltajın uygulanmasıyla eşdeğer bir etki üretir \(%50 anahtarlama, ~6V ile aynı etkiyi üretir\).

Denetleyicilerin  giriş için kullandığı PWM sinyali biraz farklıdır. Sinyal aralığının sınırlarında bile \(maksimum ileri veya maksimum geri\) sinyal asla %0 veya %100'lik bir görev döngüsüne yaklaşmaz. Bunun yerine Denetleyiciler, 5 ms veya 10 ms'lik bir süre ve 1.5 ms'lik bir orta nokta darbe genişliği olan bir sinyal kullanır. Talon ve Victor kontrolörleri, 1 ms'den 2 ms'ye kadar tipik hobi RC kontrol zamanlamasını kullanır ve Jaguar, 7ms ila ~ 2.3 ms genişletilmiş bir zamanlama kullanır. 

**Ham ve İşlenmiş Veri**

Genel olarak, WPILib'deki tüm motor kontrol sınıfları, bir aktüatöre çıkış olarak işlenmiş hali -1.0 ila 1.0 değeri almak üzere ayarlanır. FPGA'DAKİ PWM modülü, 5, 10 veya 20ms dönemleri ile PWM sinyalleri üretme yeteneğine sahiptir ve darbe genişliğini 2000 basamak aralığında değiştirebilir.Her biri orta noktanın etrafında 001ms\(orta noktanın etrafındaki her yönde 1000 adım\). Bu modüle gönderilen ham değerler, düşük sinyal \(devre dışı\) tutan özel bir durum olan  0-2000 aralığındadır. Her motor denetleyicisi için sınıf tipik bağlı değerleri \(min, max ve deadband her iki tarafında\) yanı sıra tipik orta noktası hakkında bilgi içerir. WPILib, işlenmiş değeri motor denetleyicisi için uygun aralığa eşlemek için bu değerleri kullanabilir. Bu kod denetleyicileri farklı türleri arasında sorunsuz geçiş ve belirli sinyal ayrıntıları özetlemelerini sağlar.

**PWM ve Safe PWM Classları**

**PWM**

PWM classı, PWM sinyalleri ile çalışan cihazlar için temel sınıftır ve roborio'daki PWM sinyal oluşturma donanımına bağlantılıdır. Doğrudan bir speed controller cihazı veya servo üzerinde kullanılmak üzere tasarlanmamıştır. PWM classı Victor, Jaguar, Talon ve Servo alt sınıfları  güncelleme oranını ayarlamak için, deadband eliminasyonunu ve çıktısının profil şekillenmesini belirleyen kod çıktıları verir.

**Safe PWM**  


PWM classının bir alt classıdır. Her nesneye detaylı bir izleme ve speed controller nesnesi ekler.

**Bir Speed Controller nesnesinin oluşturulması**  
  


 **C++**

```cpp
frc::Jaguar exampleJaguar{0};
frc::Talon exampleTalon{1};
frc::TalonSRX exampleTalonSRX{2};
frc::Victor exampleVictor{11};
frc::VictorSP exampleVictorSP{12};
```

**Java**

```java
Jaguar exampleJaguar = new Jaguar(0);
Talon exampleTalon = new Talon(1);
TalonSRX exampleTalonSRX = new TalonSRX(2);
Victor exampleVictor = new Victor(11);
VictorSP exampleVictorSP = new VictorSP(12);
```

**Parametreleri Ayarlama**

 **C++**

```cpp
frc::Jaguar exampleJaguar{0};
exampleJaguar.EnableDeadbandElimination(true);
```

**Java**

```java
Jaguar exampleJaguar = new Jaguar(0);
exampleJaguar.enableDeadbandElimination(true);
```



* Deadband Elimination-Ölçekleme algoritmaları denetleyicisi deadband ortadan kaldırmak için true olarak ayarlayın. Denetleyici varsayılan ayarlamak için false olarak ayarlayın.

**Hız Ayarı**

 **C++**

```cpp
frc::Jaguar exampleJaguar{0};
exampleJaguar.Set(0.7);
```

**Java**

```java
Jaguar exampleJaguar = new Jaguar(0);
exampleJaguar.set(0.7);
```

Daha önce belirtildiği gibi, Speed Controller nesneleri -1.0 \(tam ters\) ila +1.0 \(tam ileri\) arasında değişen tek bir hız parametresi alır.

### WPILib sürücü sınıfları: Drivetrain türleri

 WPILib Sürücü sınıfları, her bir aktarma organı tipi için ayrı sınıflar içerir. WPILib sınıfları tarafından desteklenen üç tip drivetrain vardır. Bu makalede üç tür açıklanmaktadır.

**Differential Drive**

![](../.gitbook/assets/image%20%2889%29.png)

Bu drive base genellikle iki veya daha fazla çekiş gücüne sahiptir. Aynı zamanda "skid-steer", "tank drive" veya "West Coast Drive"olarak da bilinir. Omni tekerlekler kullanarak yatay eksende hareket yapabilirsiniz.  Görseldeki robot drivetrain kiti içerisinden çıkan parçalarla yapılmış differential drive örneğidir. Bu drivetrains ileri/geri sürüş yeteneğine sahiptir ve iki tarafı ters yönde sürerek tekerleklerin yanlara doğru kaymasına neden olabilir. Bu drivetrain yan öteleme hareketi yeteneğine sahip değildir.

**Mecanum Drive**

![](../.gitbook/assets/image%20%2825%29.png)

Mecanum drive robotun şasesi ile dönüş yapmadan tekerlekeri kullanarak herhangi bir yönde sürüş yapmanızı sağlayan yöntemdir. Bu yönteme holonomic drive da denir. Tekerlekler 45 derecelik açıda silindirlere sahiptir. 

Üstten bakıldığında bir mecanum robotunun tekerlekleri X desenini oluşturmalıdır. Tekerlekleri farklı yönlerde döndürerek, kuvvet vektörlerinin çeşitli bileşenleri iptal edilir ve istenen robot hareketi ile sonuçlanır. Aşağıda, bu hareketlerin her biri için kuvvet vektörlerinin çizilmesi, bu drivetrainlerin nasıl çalıştığını anlamanızda yardımcı olabilir.



| Hareket Yönü | Sol Ön  | Sağ Ön | Sol Arka | Sağ Arka |
| :--- | :--- | :--- | :--- | :--- |
| İleri | İleri  | İleri  | İleri  | İleri  |
| Geri | Geri  | Geri  | Geri  | Geri  |
| Sağ Yatay | İleri | Geri | Geri | İleri |
| Sol Yatay | Geri | İleri | İleri | Geri |
| Saat Yönünde Dönüş | İleri | Geri | İleri | Geri |
| Saat Yönünün Tersinde | Geri | İleri | Geri | İleri |

**Killough Drive**

Killough drive \(aynı zamanda kiwi drive olarak da bilinir\) birbirinden 120 derece açılı üç omni tekerlek kullanan bir holonomik drivetraindir. Mecanum sürücüye benzer şekilde, tekerlekler istenen  hareketi gerçekleştirmek için farklı hızlarda çalıştırılır. Sağlanan kontrol yöntemleri Mecanum sürücüsü ile aynıdır.



### **Differential Drive kullanarak robotu sürmek**

**Differential Drive Nesnesi Oluşturmak**

 **C++**

```cpp
class Robot 
{
	public:   
		frc::Spark m_left{1};
		frc::Spark m_right{2}; 
		frc::DifferentialDrive m_drive{m_left, m_right};
```

**Java**

```java
public class Robot 
{
	Spark m_left = new Spark(1);
	Spark m_right = new Spark(2);
	DifferentialDrive m_drive = new DifferentialDrive(m_left, m_right);
```

**Multi-Motor Sürüşü**

Birçok FRC drivetrain, her tarafta 1'den fazla motora sahiptir. Bunları DifferentialDrive ile kullanmak için, her iki taraftaki motorlar SpeedControllerGroup sınıfı kullanılarak tek bir SpeedController'a toplanmalıdır. Aşağıdaki örneklerde 4 motor \(her bir tarafta 2\) aktarma organı gösterilmiştir. Daha fazla motora kadar genişletmek için, sadece ek kontrolörleri yaratın ve hepsini SpeedController grubu kontrolörüne aktarın \(bu, keyfi bir giriş sayısı alır, kullandığınız motor sayısına göre ekleme yapabilirsiniz\).

**C++**

```cpp
class Robot 
{
	public:   
		frc::Spark m_frontLeft{1};
		frc::Spark m_rearLeft{2}; 
		frc::SpeedControllerGroup m_left{m_frontLeft, m_rearLeft};

		frc::Spark m_frontRight{3};
		frc::Spark m_rearRight{4}; 
		frc::SpeedControllerGroup m_right{m_frontRight, m_rearRight};

		frc::DifferentialDrive m_drive{m_left, m_right};
```

**Java**

```java
public class Robot 
{
	Spark m_frontLeft = new Spark(1);
	Spark m_rearLeft = new Spark(2);
	SpeedControllerGroup m_left = new SpeedControllerGroup(m_frontLeft, m_rearLeft);

	Spark m_frontRight = new Spark(3);
	Spark m_rearRight = new Spark(4);
	SpeedControllerGroup m_Right = new SpeedControllerGroup(m_frontRight, m_rearRight);
	DifferentialDrive m_drive = new DifferentialDrive(m_left, m_right);
```

**Sürüş Modları**

DifferentialDrive sınıfı 3 sürücü modu içerir:



* Tank Drive - Bu mod, aktarma organlarının her iki tarafını kontrol etmek için birer değer kullanır.
* Arcade Drive - Bu mod, aktarma organının gaz kelebeğini \(X ekseni boyunca hız\) ve bir rotasyon oranını kontrol etmek için bir değer kullanır.
* Curvature Drive - "Cheesy Drive" olarak da bilinir; Rotasyon argümanı, rotanın değişim oranından ziyade robotun yolunun eğriliğini kontrol eder. Bu, robotu yüksek hızlarda daha kontrol edilebilir hale getirir. Ayrıca, robotun hızlı dönüş fonksiyonunu ele alır - "hızlı dönüş", yerinde manevralar için sabit eğrilik dönüşünü geçersiz kılar.

**Tank Drive**

Tank Drive modu, aktarma organlarının her iki tarafını bağımsız olarak kontrol etmek için kullanılır \(genellikle her 2 taraf için 2 farklı joystick ile kullanılır.\). Bu örnek, aktarma organını Tank modunda çalıştırmak için iki ayrı kumanda kolunun Y ekseninin nasıl kullanılacağını gösterir.

**C++**

```cpp
class Robot: public frc::TimedRobot
{
	//Object construction

	void TeleopPeriodic() override {		
		myDrive.TankDrive(leftStick.GetY(), rightStick.GetY());
	}	
}
```

**Java**

```java
public class RobotTemplate extends TimedRobot 
{
	//Object construction

	public void teleopPeriodic() {    			
		myDrive.tankDrive(leftStick.getY(),  rightStick.getY());   
	}
}
```

**Arcade Drive**

Arcade Drive modu, aktarma organını hız / gaz\(throttle\) ve dönüş oranını kullanarak kontrol etmek için kullanılır. Bu genellikle tek bir kumanda kolundan iki eksenle ya da bir çubuktan gelen gaz kelebeği ve diğerinden dönüş ile birlikte \(genellikle tek bir joystick üzerinde\) joystick'lere bölünür. Bu örnek, Arcade modu ile tek bir joystick'in nasıl kullanılacağını gösterir.



 **C++**

```cpp
class Robot: public frc::TimedRobot
{
	//Object construction

	void TeleopPeriodic() override {		
		myDrive.ArcadeDrive(driveStick.GetY(), driveStick.GetX());
	}	
}
```

**Java**

```java
public class RobotTemplate extends TimedRobot 
{
	//Object construction

	public void teleopPeriodic() {    			
		myDrive.arcadeDrive(driveStick.getY(),  driveStick.getX());   
	}
}
```

**Curvature Drive**

Arcade Drive gibi, Curvature Drive modu hız / gaz ve dönüş oranını kullanarak aktarma organlarını kontrol etmek için kullanılır. Fark, rotasyon kontrolünün, başlık değişiminin hızı yerine eğrilik yarıçapını kontrol etmeye çalışmasıdır. Bu mod ayrıca, yerinde dönmeyi sağlayan bir alt modu devreye sokmak için kullanılan hızlı dönüş parametresine de sahiptir. Bu örnek Curvature moduyla tek bir joystick'in nasıl kullanılacağını gösterir.

 **C++**

```cpp
class Robot: public frc::TimedRobot
{
	//Object construction

	void TeleopPeriodic() override {		
		myDrive.CurvatureDrive(driveStick.GetY(), driveStick.GetX(), driveStick.GetButton(1));
	}	
}
```

**Java**

```java
public class RobotTemplate extends TimedRobot 
{
	//Object construction

	public void teleopPeriodic() {    			
		myDrive.curvatureDrive(driveStick.getY(),  driveStick.getX(), driveStick.GetButton(1));   
	}
}
```

### Mecanum Drive kullanarak robotu sürmek

![](../.gitbook/assets/image%20%281%29.png)

Bu robotta gösterilen tekerlekler, sürücülerin, klasik bir sürüş durumunda olduğu gibi, düz ileriye doğru 45 derecelik bir açıda uygulanmasına neden olan silindirlere sahiptir. Tekerleklerin hızını değiştirmenin herhangi bir yönde hareket etmesini sağladığını tahmin edebilirsiniz. Mecanum jantların internetteki çeşitli web sitelerinde nasıl çalıştığını araştırabilirsiniz.

**Mecanum Kontrolü : Cartesian vs Polar**

MecanumDrive sınıfı aktarma organlarını kontrol etmek için iki yol içerir:

* Kartezyen: Bu yöntem X, Y , Döndürme parametrelerini alır ve joystickleri mecanum sürüş hareketine eşleştirirken yaygın olarak kullanılır. Elde edilen robot dönüşü istenen X ve Y hareketinin birleşimidir. 
* Polar: Bu yöntem Büyüklük, Açı ,Döndürme parametrelerini alır ve robotu bağımsız olarak kontrol ederken yaygın olarak kullanılır. Açı, Z ekseni etrafında derece olarak belirtilmelidir \(-180 ve 180 arasında\).

**Mecanum tekerlekler için teleop sürüş kodu**

Tek bir joystick ile mecanum tekerlekleri kullanarak sürmek için minimum kodu gösteren örnek bir program. Joystick XY konumu, robotun takip etmesi gereken bir robot yön vektörünü temsil eder. Joystick üzerindeki twist \(Z\) ekseni, sürüş sırasında robot için dönme hızını temsil eder.

 **C++**

```cpp
#include "WPILib.h"
/**
 * Simplest program to drive a robot with mecanum drive using a single Logitech
 * Extreme 3D Pro joystick and 4 drive motors connected as follows:
 *     - PWM 0 - Connected to front left drive motor
 *     - PWM 1 - Connected to rear left drive motor
 *     - PWM 2 - Connected to front right drive motor
 *     - PWM 3 - Connected to rear right drive motor
 */
class MecanumDefaultCode : public frc::TimedRobot
{
	
	frc::Spark m_frontLeft{0};		
	frc::Spark m_rearLeft{1}; 
	frc::Spark m_frontRight{2};
	frc::Spark m_rearRight{3};
	frc::MecanumDrive m_drive{m_frontLeft, m_rearLeft, m_frontRight, m_rearRight};
	frc::Joystick m_driveStick{1};

	/**
	 * Gets called once for each new packet from the DS.
	 */
	void TeleopPeriodic override (void)
	{
		m_robotDrive.MecanumDrive_Cartesian(m_driveStick.GetX(), m_driveStick.GetY(), m_driveStick.GetZ());
	}

};
START_ROBOT_CLASS(MecanumDefaultCode);
```

**Java**

```java
import edu.wpi.first.wpilibj.Joystick;
import edu.wpi.first.wpilibj.RobotDrive;
import edu.wpi.first.wpilibj.TimedRobot;

/*
 * Simplest program to drive a robot with mecanum drive using a single Logitech
 * Extreme 3D Pro joystick and 4 drive motors connected as follows:
 *     - PWM 0 - Connected to front left drive motor
 *     - PWM 1 - Connected to rear left drive motor
 *     - PWM 2 - Connected to front right drive motor
 *     - PWM 3 - Connected to rear right drive motor
 */

public class MecanumDefaultCode extends TimedRobot {
     //Create a robot drive object using PWMs 0, 1, 2 and 3
     Spark m_frontLeft = new Spark(1);	
		Spark m_rearLeft = new Spark(2);
		Spark m_frontRight = new Spark(3);	
		Spark m_rearRight = new Spark(4);
     //Define joystick being used at USB port 1 on the Driver Station
     Joystick m_driveStick = new Joystick(1);

     public void teleopPeriodic() 
		{
          m_robotDrive.mecanumDrive_Cartesian(m_driveStick.getX(), m_driveStick.getY(), m_driveStick.getZ());
     }
}
```

**Alan odaklı sürüş için programın güncellenmesi**

bir Gyro sensöründen döndürülen açı olan MecanumDrive\_Cartesian\(\) yönteminde isteğe bağlı  4.  parametre vardır. Joystick'ten verilen X/Y değerleri robotu alana göre ayarlayacaktır. Bu yöntem mecanum sürücüsü için çok yararlıdır. çünkü sürüş için robotun gerçekten ön, arka veya yanları yoktur. Her yöne gidebilir. Bir Gyro sensöründen derece açısını ekleyerek robotun yönlerini daha doğru bir şekilde kullanabilirsiniz.

Alan odaklı sürüş kullanımı genellikle robot sürücüleri karşı karşıya olduğunda kontrolleri tersine çevrilir.

MecanumDrive\_Cartesian \(\) her çağrıldığında gyro açısını almayı unutmayın.

 **C++**

```cpp
m_robotDrive.MecanumDrive_Cartesian(m_driveStick.GetX(), m_driveStick.GetY(), m_driveStick.GetZ(), m_gyro.GetAngle());
```

**Java**

```java
m_robotDrive.mecanumDrive_Cartesian(m_driveStick.getX(), m_driveStick.getY(), m_driveStick.getZ(), m_gyro.getAngle());
```

### WPILib ile Servo Kontrolü

**Bir Servo nesnesi oluşturmak**

```cpp
C++
Servo *exampleServo = new Servo(1);
```

```java
Java
Servo exampleServo = new Servo(1);

```

**Servo Değerlerini Ayarlamak**

```cpp
C++

exampleServo->Set(.5);
exampleServo->SetAngle(75);

```

```java
Java

exampleServo.set(.5);
exampleServo.setAngle(75);
```

WPILib'de servo değerlerini ayarlamak için iki yöntem vardır:

* Scaled Değer - 0 ile 1,0 arasında bir ölçek değeri kullanarak servo konumunu ayarlar. 0, servonun bir ucuna karşılık gelir ve 1.0, diğerine karşılık gelir**.**
* Angle - Açıyı, derece cinsinden belirterek servo konumunu ayarlayın. Bu yöntem, Hitec HS-322HD servo \(0 ila 170 derece\) ile aynı aralıktaki servolar için çalışacaktır.

### On/Off kontrol mekanizmaları , motorlar, diğer mekanizmalar

Motorlar, solenoidler, ışıklar veya diğer özel devreler gibi diğer mekanizmaların On / Off kontrolü için, WPILib, VEX Robotics'ten Spike H-Bridge Relay'e arabirim oluşturmak için tasarlanmış röle çıkışları için destek oluşturmuştur. Bu cihazlar, bir H-Köprü konfigürasyonunda bağlanan iki rölenin durumunu bağımsız olarak kontrol etmek için 3 pinli bir çıkış \(GND, Forward, Reverse\) kullanır. Bu, rölenin her iki kutuptaki çıkışlara güç sağlamasına veya her iki çıkışı aynı anda açmasına izin verir.

**Röle yönlerini ayarlamak**  
WPILib röleleri içinde kBothDirections \(geri dönüşlü motor veya iki yönlü solenoid\), kForwardOnly \(sadece ileri pimi kullanır\) veya kReverseOnly \(yalnızca geri pini kullanır\) olarak ayarlanabilir. Yön için bir değer girilmezse, varsayılan olarak kBothDirections olarak ayarlanır. Bu, Relay sınıfındaki hangi yöntemlerin belirli bir örnekle kullanılabileceğini belirler.



```cpp
C++

Relay *exampleRelay = new Relay(1);
Relay *exampleRelay = new Relay(1, Relay::Value::kForward)

exampleRelay->Set(Relay::Value::kOn);
exampleRelay->Set(Relay::Value::kForward);

```

```java
Java

exampleRelay = new Relay(1);
exampleRelay = new Relay(1, Relay.Value.kForward);

exampleRelay.set(Relay.Value.kOn);
exampleRelay.set(Relay.Value.kForward);

```

Röle durumu set\(\) yöntemi kullanılarak ayarlanır. Yöntem, aşağıdaki değerlerle bir numaralandırma parametre olarak alır:

* kOff - Her iki röle çıkışını da kapatır 
* kForward - Röleyi ileriye doğru ayarlar \(M + @ 12V, M- @ GND\) 
* kReverse - Röleyi tersine çevirir \(M + @ GND, M- @ 12V\) 
* KOn - Her iki röle çıkışını da ayarlar \(M + @ 12V, M- @ 12V\).

Röle yönü, yalnızca ileri veya geri pimler etkinleştirilecek şekilde ayarlanmışsa, bu yöntemin kForward veya kReverse'ye eşdeğer olacağını unutmayın. kOn kullanılması önerilmez.

### Pnömatik için kompresör kullanmak

 **Kompresörün Uygulanması, Başlatılması ve Durdurulması**

 **C++**

```cpp
Compressor *c = new Compressor(0);

c->SetClosedLoopControl(true);
c->SetClosedLoopControl(false);

```

  
**Java**

```java
Compressor c = new Compressor(0);

c.setClosedLoopControl(true);
c.setClosedLoopControl(false);
```

**Kompresör durumu**

 **C++**

```cpp
bool enabled = c->Enabled();
bool pressureSwitch = c->GetPressureSwitchValue();
double current = c->GetCompressorCurrent();

```

  
**Java**

```java
boolean enabled = c.enabled();
boolean pressureSwitch = c.getPressureSwitchValue();
double current = c.getCompressorCurrent();
```

 kompresör nesnesi oluşturmanın diğer nedeni, kompresörün durumunu sorgulamaktır. Halihazırda olan veya olmayan durum, pressure switch durumu ve compressorcurrent , compressor nesnesinden sorgulanabilir.

###  Pnömatik silindirleri - Solenoidler'i kullanmak

**Solenoid'e genel bakış**

FRC'de kullanılan pnömatik solenoid valfler dahili pilotlu vanalardır. Dahili pilotlu solenoid valflerin çalışması hakkında daha fazla bilgi için, [bu Wikipedia makalesine bakın](http://en.wikipedia.org/wiki/Solenoid_valve). Valfın harekete geçmesi için gereken minimum bir giriş basıncı olmalıdır. FRC takımları tarafından yaygın olarak kullanılan vanaların çoğu için bu 20 ila 30 psi arasındadır. 

PCM'nin kendisindeki LED'lere bakmak, elektrik veya hava basıncı giriş sorunlarını ortadan kaldırmak için kodun beklediğiniz gibi davrandığını doğrulamanın en iyi yoludur.

Single solenoidler, tek bir çıkış portundan basınç uygular veya havalandırır. Tipik olarak, ya harici bir kuvvet, silindirin \(yay, yerçekimi, ayrı mekanizma\) geri dönüş hareketini sağlayacağı ya da double solenoid olarak hareket edecek çiftler halinde kullanılır. double solenoid, iki çıkış portu arasındaki hava akışını değiştirir \(aynı zamanda çıkışların ne şekilde havalandırıldığı veya girişe bağlı olmadığı bir orta konuma da sahiptir\). Çift silindirli valfler, hava basıncını kullanarak bir silindirin hem uzatılması hem de geri çekilme hareketlerini kontrol etmek istediğinizde yaygın olarak kullanılır. Double solenoid valfler, solenoid koparmada iki ayrı kanala bağlanan iki elektrik girişine sahiptir.

**Single Solenoid kullanımı**

 **C++**

```cpp
frc::Solenoid exampleSolenoid {1};

exampleSolenoid.Set(true);
exampleSolenoid.Set(false);

```

  
**Java**

```java
Solenoid exampleSolenoid = new Solenoid(1);

exampleSolenoid.set(true);
exampleSolenoid.set(false);
```

WPILib'deki single solenoidler Solenoid sınıfı kullanılarak kontrol edilir. Bir Solenoid nesnesi oluşturmak için, istenen port numarasını \( veya Node ID\) ve port numarasını yazmanız yeterlidir. Solenoid çıkışını etkinleştirmek için set\(true\) değerini ayarlamak veya  pasifleştirmek için  set\(false\) ayarlamak gerekir.

**Double Solenoid kullanımı**

 **C++**

```cpp
frc::DoubleSolenoid exampleDouble {1, 2};

exampleDouble.Set(frc::DoubleSolenoid::Value::kOff);
exampleDouble.Set(frc::DoubleSolenoid::Value::kForward);
exampleDouble.Set(frc::DoubleSolenoid::Value::kReverse);

```

  
**Java**

```java
DoubleSolenoid exampleDouble = new DoubleSolenoid(1, 2);

exampleDouble.set(DoubleSolenoid.Value.kOff);
exampleDouble.set(DoubleSolenoid.Value.kForward);
exampleDouble.set(DoubleSolenoid.Value.kReverse);
```

Double solenoidler WPILib'deki DoubleSolenoid sınıfı tarafından kontrol edilir. Bunlar, single solenoide benzer şekilde yapılandırılmıştır, ancak  iki port numarası bulunmaktadır. Valfin durumu daha sonra kOff \(çıkış etkin değildir\), kForward \(ileri kanal etkin\) veya kReverse \(ters kanal etkin\) olarak ayarlanabilir.

