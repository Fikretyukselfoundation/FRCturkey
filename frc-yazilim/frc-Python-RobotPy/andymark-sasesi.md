# RobotPy ile Andymark(Kitbot) Şasesi Kullanma
Bu sayfada kickoff kiti ile Drive base opt-out yapmadığınız takdirde her takıma gönderilen standart 6 tekerli drop center şaseyi TankDrive olarak kodlayacağız.
Bu örnek, [RobotPy'ın örnekler kütüphanesi](https://github.com/robotpy/examples/blob/main/arcade-drive/robot.py)'nden alınmıştır. Birçok farklı örnek kodu orada bulabilirsiniz.

## robot.py İsimli bir Python dosyası oluşturuyoruz.
Burada açıklanacak pek bir şey yok. Dilediğiniz IDE'den robot.py isimli bir Python dosyası açın. Nerede olduğunu bilmeniz önemli, RobotPy kullanırken WPILib kurulumu ile gelen robot projesi oluşturma gibi VSCode uzantılarını kullanmıyoruz. Çoğu şeyi yapabilmek için cmd/Terminal kullanacağız.

## WPILib'i import ediyoruz.

```
import wpilib
import wpilib.drive
```

## Robot Class'ımızı oluşturuyoruz
```
class MyRobot(wpilib.TimedRobot):

    def robotInit(self):
        pass

    def teleopInit(self):
        pass

    def teleopPeriodic(self):
        pass


if __name__ == "__main__":
    wpilib.run(MyRobot)
```

Burada WPILib'in TimedRobot base class'ını kullandım. [buradan](../frc-java-temelleri/frc-java-temelleri.md) farklı base class'lara ve ne yaptıklarına ulaşabilirsiniz. robotInit, teleopInit, teleopPeriodic fonksiyonlarını yazıp içine şimdilik sadece pass yazıyoruz. Bunlar üzerideki base class'ların anlatıldığı dokümanda anlatılıyor. Size en basitinden Arduino'ya benzer olarak anlatmamı isterseniz; robotInit setup(), teleopInit() her teleop modu aktif edildiğinde çağırılan ayrı bir setup(), teleopPeriodic ise teleop modundayken 50ms arayla çağırılan periodic(). Fonsiyonlarımızın argümanlarına self'i eklemeyi ve son 2 satırı unutmayın.

## Motor sürücülerini tanımlama ve gruplama

    def robotInit(self):
        self.onSolMotor = wpilib.PWMSparkMax(0)
        self.onSagMotor = wpilib.PWMSparkMax(1)
        self.arkaSolMotor = wpilib.PWMSparkMax(2)
        self.arkaSagMotor = wpilib.PWMSparkMax(3)
Artık KOP ile SparkMax motor kontrolcüleri gönderildiği için burada PWM üzerinden SparkMax kullandım. wpilib içinde bütün PWM özellikli motor kontrolcüleri için bir PWM class'ı bulundurur, yapmanız gereken tek şey PWM kablosu ile motor kontrolcüyü 1. parametrede belirlediğiniz numaralı PWM pinlerine bağlamaktır.
```
    def robotInit(self):
        self.onSolMotor = wpilib.PWMSparkMax(0)
        self.onSagMotor = wpilib.PWMSparkMax(1)
        self.arkaSolMotor = wpilib.PWMSparkMax(2)
        self.arkaSagMotor = wpilib.PWMSparkMax(3)
        
        self.solMotorGrup = wpilib.MotorControllerGroup(self.onSolMotor, self.arkaSolMotor)
        self.sagMotorGrup = wpilib.MotorControllerGroup(self.onSagMotor, self.arkaSagMotor)
        
        self.solMotorGrup.setInverted(True)
        
        self.surus = wpilib.drive.DifferentialDrive(self.solMotorGrup, self.sagMotorGrup)
```
Burada Motorlarımızı grupladık, sol motorun yönlerini ters çevirdik ve sürüş class'ımızı oluşturduk. 4 adet motorumuz KOP şasesinde ikişer ikişer toplam 2 adet ToughBox Mini'ye bağlandığından aynı anda aynı miktar dönecekleri için onları "grupluyoruz", Sol motorların yönünü değiştiriyoruz çünkü onlar sağ motorlara göre ters yönde takılıyor ve robotun ileriye doğru gitmesi için ters yönde dönmeleri gerekiyor ve sürüş class'ımız içindeki arcadeDrive() fonksiyonu ile motorları kontrol etmemizi kolaylaştırıyor.

## Joystick tanımlama
    def robotInit(self):
        self.onSolMotor = wpilib.PWMSparkMax(0)
        self.onSagMotor = wpilib.PWMSparkMax(1)
        self.arkaSolMotor = wpilib.PWMSparkMax(2)
        self.arkaSagMotor = wpilib.PWMSparkMax(3)
        
        self.solMotorGrup = wpilib.MotorControllerGroup(self.onSolMotor, self.arkaSolMotor)
        self.sagMotorGrup = wpilib.MotorControllerGroup(self.onSagMotor, self.arkaSagMotor)
        
        self.solMotorGrup.setInverted(True)
        
        self.surus = wpilib.drive.DifferentialDrive(self.solMotorGrup, self.sagMotorGrup)
        
        self.stick = wpilib.Joystick(0)
Rookie takımlara gönderildiği için burada Logitech F310 üzerinden ilerleyeceğiz. Diğer flight stick gibi joystickleri aynı şekilde bağlayıp DriverStation'dan kaçıncı joystick olduğunu, kaçıncı tuşun kaçıncı olduğuna, hangi axis'in kaçıncı axis olduğunu tespit edebilirsiniz.

## Robotu sürmek (sonunda!)
    def teleopPeriodic(self):
        self.surus.arcadeDrive(
        -self.stick.getRawAxis(0), self.stick.getRawAxis(2), False
        )


Bir joystickle robotu kontrol ettirebilmemiz için robotun teleop modunda olması lazım, o yüzden sürüş kodumunuzu teleopPeriodic()'in içine yazıyoruz.

![A test image](https://imgyukle.com/f/2022/10/24/n0f2jA.png)

Kumandamınızın axisleri böyle çalıştığı için sırasıyla 0 ve 2. axisleri argüman olarak verip 0. axis'in yönünü - ile çarparak değiştiriyoruz. Bunu yapmazsak robot sol axis'i ileriye ittirdiğimizde geriye gider.

## Kodu deploy etme
```
import wpilib
import wpilib.drive

class MyRobot(wpilib.TimedRobot):

    def robotInit(self):
        self.onSolMotor = wpilib.PWMSparkMax(0)
        self.onSagMotor = wpilib.PWMSparkMax(1)
        self.arkaSolMotor = wpilib.PWMSparkMax(2)
        self.arkaSagMotor = wpilib.PWMSparkMax(3)
        
        self.solMotorGrup = wpilib.MotorControllerGroup(self.onSolMotor, self.arkaSolMotor)
        self.sagMotorGrup = wpilib.MotorControllerGroup(self.onSagMotor, self.arkaSagMotor)
        
        self.solMotorGrup.setInverted(True)
        
        self.surus = wpilib.drive.DifferentialDrive(self.solMotorGrup, self.sagMotorGrup)
        
        self.stick = wpilib.Joystick(0)

    def teleopInit(self):
        pass

    def teleopPeriodic(self):
        self.surus.arcadeDrive(
        -self.stick.getRawAxis(0), self.stick.getRawAxis(2), False
        )


if __name__ == "__main__":
    wpilib.run(MyRobot)

```

Kodumuz hazır, şimdi deploy etmek için robotun ağına bağlanalım.

Windows
```
py -3 robot.py deploy
```
Linux/OSX
```
python3 robot.py deploy
```

cmd/Terminal'inizin robot.py'ın bulunduğu directory'de olmasına dikkat edin, ilk kez deploy ederken ayrıca sizden takım numaranızı isteyebilir.


# Ve işte bu kadar
### herhangi bir Java/C++ kodu yüklenmiş bir robotu kullanacağınız şekilde DriverStation'ı açın ve RobotPy'ı kendiniz için deneyin!
