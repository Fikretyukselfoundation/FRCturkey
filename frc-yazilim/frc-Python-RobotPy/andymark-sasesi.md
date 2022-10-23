# RobotPy ile Andymark(Kitbot) Şasesi Kullanma
Bu sayfada kickoff kiti ile Drive base opt-out yapmadığınız takdirde her takıma gönderilen standart 6 tekerli drop center şaseyi TankDrive olarak kodlayacağız.
Bu örnek, [RobotPy'ın örnekler kütüphanesi](https://github.com/robotpy/examples/blob/main/tank-drive/robot.py)'nden alınmıştır. Birçok farklı örnek kodu orada bulabilirsiniz.

## robot.py İsimli bir Python dosyası oluşturuyoruz.
Burada açıklanacak pek bir şey yok. Dilediğiniz IDE'den robot.py isimli bir Python dosyası açın. Nerede olduğunu bilmeniz önemli, RobotPy kullanırken WPILib kurulumu ile gelen robot projesi oluşturma gibi VSCode uzantılarını kullanmıyoruz. Çoğu şeyi yapabilmek için cmd/Terminal kullanacağız.

## WPILib'i ve DifferentialDrive'ı import ediyoruz.

```
import wpilib
from wpilib.drive import DifferentialDrive
```

## 
