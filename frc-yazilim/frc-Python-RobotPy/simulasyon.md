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

    def update_sim(self, now: float, tm_diff: float) -> None:
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

## Motorları ve şasemizi simülasyon objeleri olarak tanımlıyoruz.

```
    def __init__(self, physics_controller: PhysicsInterface, robot: "MyRobot"):
        """
        :param physics_controller: `pyfrc.physics.core.Physics` object
                                   to communicate simulation effects to
        :param robot: your robot object
        """

        self.physics_controller = physics_controller

        self.onSolMotor = wpilib.simulation.PWMSim(robot.onSolMotor.getChannel())
        self.onSagMotor = wpilib.simulation.PWMSim(robot.onSagMotor.getChannel())
        self.arkaSolMotor = wpilib.simulation.PWMSim(robot.arkaSolMotor.getChannel())
        self.arkaSagMotor = wpilib.simulation.PWMSim(robot.arkaSagMotor.getChannel())

        self.drivetrain = pyfrc.physics.drivetrains.FourMotorDrivetrain(
            30 * units.inch,
            1 * units.mps
        )
```

### Motorlar

Motorlarımızı PhysicsEngine class'ının içine PWMSim olarak tanımlıyoruz, bunun için robot.motorismi.getChannel() metodunu kullandım. Direk motorların bağlı olduğu PWM kanallarını PWMSim'e argüman olarak yazabilirsiniz ancak kodun esnekliği açısından bu yolu takip etmenizi şiddetle tavsiye ederim.

### Şase

Şasemizi de 4 motorlu şase olarak tanımladık, bunun için de 2 adet argümanla beraber FourMotorDrivetrain Class'ını kullandık.

1. argüman şasemizin sağ ve sol tekerleri arasındaki mesafe, bu takımınızın şaseyi ne kadar kestiğine göre değişebileceği için örnek olarak ortalama bir açıklık olan 30 inç'i aldım.

2. argüman ise şasemizin m/s cinsinden sürati, bu örnekte 1 m/s olarak aldım ancak siz isterseniz gerçek hayatta bunu hesaplayabilirsiniz. Bu *sallamasyon* değer bu rehberin amaçları için bize yeterli.
