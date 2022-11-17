# RobotPy ile simülasyon

Simülasyon, çoğu takım tarafından esgeçilen ancak oldukça faydalı bir WPILib özelliğidir. WPILib'in simülasyon özellikleri sizin daha fiziksel bir robot olmadan algoritmalarınızı test etmenizi, herhangi bir bug'ı robota kod deploy etmeden yakalamanızı, fikirlerinizi hızlıca prototiplemenizi sağlar.

En basitinden bir limit switch ile PWM motor çalıştırmaktan, volan simülasyonuna kadar WPILib size birçok imkan sağlar.

Bu rehberde, RobotPy ile AndyMark(Kitbot) şasesini simüle edip bazı takımların hala yapamadığı taxi görevini otonom etabında tamamlayacağız.

## physics.py dosyası oluşturun
"robot.py" dosyanızla aynı dizinde bir "physics.py" dosyası oluşturun. Simülasyon bu dosya olmadan çalışmaz.
```
from pyfrc.physics.core import PhysicsInterface

import typing

if typing.TYPE_CHECKING:
    from robot import MyRobot


class PhysicsEngine:


    def __init__(self, physics_controller: PhysicsInterface, robot: "MyRobot"):
        """
        :param physics_controller: `pyfrc.physics.core.Physics` object
                                   to communicate simulation effects to
        :param robot: your robot object
        """

        self.physics_controller = physics_controller

    def update_sim(self, now: float, tm_diff: float):
        """
        Called when the simulation parameters for the program need to be
        updated.
        :param now: The current time as a float
        :param tm_diff: The amount of time that has passed since the last
                        time that this function was called
        """
        pass
```
physics.py dosyanıza yukarıda verilen şablonu kopyalayıp yapıştırın. Bu şablonu, verildiği gibi bırakırsanız DIO, analog giriş ile algoritmalarınızı test edebilirsiniz; ancak robotu sanal sahada süremezsiniz ve pathplanning gibi daha kompleks işlemleri simüle edemezsiniz.

## Simülasyon GUI'si

### Simülasyonu çalıştırmak

Windows:

```py -3 robot.py sim```

Linux/OSX:

```python3 robot.py sim```

Yukarıdan ilgili işletim sisteminiz için olan komutu robot.py ile aynı dizindeyken cmd/Terminal'den çalıştırın.

![Sim gui](https://lh5.googleusercontent.com/iy71etFmE9GbTkvepDT7wxfKyVnmebxlsnv1zNARKbUmt1BEjkhqJmuipgJHR_Xoi2c=w2400)

### Simülasyon ayarları

Simülasyonu açtıktan sonra sizi böyle bir pencerenin karşılaması lazım, Şimdilik sol taraftan System Joysticks penceresinden Keyboard 0'ı Joysticks penceresindeki silik Joystick[0] yazısına taşıyın ve üst menünün NetworkTables sekmesinden SmartDashboard'un altındaki Field'ı açın.

![Sim gui field ve joystick](https://lh6.googleusercontent.com/1azqDg3iCgHPCkMs2Cdxk4hBpYDPMkprZd-OHb7BbiO0GerNOvhlt7xQi82C-9n57Ys=w2400)

Böylelikle Klavyemizin sol tarafındaki tuşları 0. joystick olarak tanımladık ve robotumuzu sürebileceğimiz sanal sahamızı açtık.

![decay rate](https://lh5.googleusercontent.com/KleVFEw2V9UqbaFb15H4n7hjjv6Of65KGIose_nrajql9mzmLLTWzw_Fc5FVsYG7rzY=w2400)

System Joysticks'den Keyboard 0'a sağ tıklayıp ayarlar penceresini açın, Axis 2'nin decay rate'ini 0.050 olarak ayarlayın. Bunu yapmazsanız robotu sürerken sanki F310'unuzun sağ joystick modülleri yerinde sıkışmış, siz zorla ters tarafa elinizle thumb'ı çevirmedikçe robot dönmeye devam edecek gibi bir deneyim yaşarsınız. Ayrıca burada klavyenizin hangi tuşunun sanal joystick'inizin hangi tuşuna denk geldiğinizi da görüntüleyebilirsiniz. Simülasyon yaptığınız ayarları otomatik olarak kaydedecektir.

## Andymark(Kitbot) Şase Simülasyonu

Bu simülasyon işini daha somut hale getirmek için bir Andymark şasesinin simülasyonunu yapalım.

### İhtiyacımız olacak ek kütüphaneleri import ediyoruz
```
import wpilib.simulation
import pyfrc.physics.drivetrains
from pyfrc.physics.units import units
```

### Motorları ve şasemizi simülasyon objeleri olarak tanımlıyoruz.

```
    def __init__(self, physics_controller: PhysicsInterface, robot: "MyRobot"):
        """
        :param physics_controller: `pyfrc.physics.core.Physics` object
                                   to communicate simulation effects to
        :param robot: your robot object
        """

        self.physics_controller = physics_controller

        #1
        self.onSolMotor = wpilib.simulation.PWMSim(robot.onSolMotor.getChannel())
        self.onSagMotor = wpilib.simulation.PWMSim(robot.onSagMotor.getChannel())
        self.arkaSolMotor = wpilib.simulation.PWMSim(robot.arkaSolMotor.getChannel())
        self.arkaSagMotor = wpilib.simulation.PWMSim(robot.arkaSagMotor.getChannel())

        #2
        self.drivetrain = pyfrc.physics.drivetrains.FourMotorDrivetrain(
            30 * units.inch,
            1 * units.mps
        )
```

#### 1. Motorlar

Motorlarımızı PhysicsEngine class'ının içine PWMSim olarak tanımlıyoruz, bunun için robot.motorismi.getChannel() metodunu kullandım. Direk motorların bağlı olduğu PWM kanallarını PWMSim'e argüman olarak yazabilirsiniz ancak kodun esnekliği açısından bu yolu takip etmenizi şiddetle tavsiye ederim.

#### 2. Şase

Şasemizi de 4 motorlu şase olarak tanımladık, bunun için de 2 adet argümanla beraber FourMotorDrivetrain Class'ını kullandık.

1. argüman şasemizin sağ ve sol tekerleri arasındaki mesafe, bu takımınızın şaseyi ne kadar kestiğine göre değişebileceği için örnek olarak ortalama bir açıklık olan 30 inç'i aldım.

2. argüman ise şasemizin m/s cinsinden sürati, bu örnekte 1 m/s olarak aldım ancak siz isterseniz gerçek hayatta bunu hesaplayabilirsiniz. Bu *sallamasyon* değer bu rehberin amaçları için bize yeterli.

### Robotu sanal sahada hareket ettirmek

```
    def update_sim(self, now: float, tm_diff: float):
        """
        Called when the simulation parameters for the program need to be
        updated.
        :param now: The current time as a float
        :param tm_diff: The amount of time that has passed since the last
                        time that this function was called
        """
        #1
        onSolMotor = self.onSolMotor.getSpeed()
        onSagMotor = self.onSagMotor.getSpeed()
        arkaSolMotor = self.arkaSolMotor.getSpeed()
        arkaSagMotor = self.arkaSagMotor.getSpeed()

        #2
        saseHizlari = self.drivetrain.calculate(onSolMotor, arkaSolMotor, onSagMotor, arkaSagMotor)
        
        #3
        self.physics_controller.drive(saseHizlari, tm_diff)
```

 #### 1. Motor hızları
 Sırasıyla bütün motorlarımızın PWM hızlarını alıyoruz.

 #### 2. Şasenin hareketi
Drivetrain objemizin .calculate() methodunu kullanarak motorlarımızın PWM hızları ile şasemizin hareketini hesaplıyoruz. Burada motorların hızlarını argüman olarak docstring'e uygun şekilde sıraladığınızdan emin olun!

 #### 3. Pozisyon güncelle
 Fizik kontrolcümüzün .drive() methodu ile sanal sahamızda robotumuzun pozisyonunu güncelliyoruz.
 
 ### Robotumuzu sürüyoruz
 
 Simülatörü açıp a,d ve e,r tuşları ile robotumuzu sanal sahamızda sürüyoruz.
 
 ![robotsurmek](https://lh3.googleusercontent.com/ZFO4P5CjdlQKW8xM00RyF76vecVIoPsCjDvvP3vf57gOCw4-htV9RyTMC5drkM-yhWs=w2400)
 
 eğer dilerseniz axis tuşlarını daha önce decay rate'i ayarladığımız pencereden değiştirebilirsiniz.
 
 ## Otonom Taxi
 
 ### Fonksiyonlar
 
 ```
     def autonomousInit(self):
        self.taxiTimer = wpilib.Timer()
        self.taxiTimer.start()

    def autonomousPeriodic(self):
        if self.taxiTimer.get() < 2:
            self.surus.arcadeDrive(-1,0)
    
    def autonomousExit(self):
        self.taxiTimer.stop()
        del(self.taxiTimer)
 ```

Otonom'da taxi yapmak için bu 3 fonksiyonu kullanıyoruz.

#### autonomousInit

Otonom etabı başladığında 1 kez cağrılacak olan fonksiyon, burayı bir Timer objesi oluşturup saymaya başlamasını ayarlamak için kullanıyoruz.

#### autonomousPeriodic

Otonom etabı boyunca her 50ms'de bir çağırılacak olan fonksiyon, teleopPeriodic gibi.

Burada Timer objemizin .get() fonksiyonunu kullanarak autonomousInit'den beri kaç saniye geçtiğini alıyoruz, eğer 2 saniyeden az ise robotu ileriye sürüyoruz.

#### autonomousExit

Otonom etabı bitince çağırılacak olan fonksiyon, Timer objemizi durdurup robot programımızda siliyoruz; teleop'ta ihtiyacımız olmayacağından dolayı.

### Test edelim

![otonom_etap](https://lh6.googleusercontent.com/Ssj-Xbin_ClgKtjUAKxj-xKHfL6L9s6j_941t-DUDv6KDlXOfNjB4cDulTMb84w24y8=w2400)


**...Ve ittifağımız için 2 puan böylelikle hazır!**
