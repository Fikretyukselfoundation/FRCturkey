# Başlangıç için örnek kodlar

### **Robotunuz için örnek değişkenlerinizin tanımlanması.**

```java
//Programın ihtiyac duydugu diger dosyalari ice aktarir
import edu.wpi.first.wpilibj.IterativeRobot;
import edu.wpi.first.wpilibj.Joystick;
import edu.wpi.first.wpilibj.RobotDrive;
import edu.wpi.first.wpilibj.livewindow.LiveWindow;

public class Robot extends IterativeRobot {

//Robot classimiz icin degiskenler tanimlar
     RobotDrive myRobot;
     Joystick stick;
     Timer timer;

//Robot init yönetimindeki degiskenleri cagirir robot basladiginda ilk bu komutlar calisir
     public void robotInit() {
          myRobot = new RobotDrive(0,1); //myRobot adinda robotumuzu haraket ettirecek kutuphaneyi cagirdik 0,1 robotumuzun tekerleklerinin bagli oldugu pwm numaralaridir 4 teker kullanıyorsanız 0,1,2,3 seklinde degistirebilirsiniz
          stick = new Joystick(1); //stick adinda joystick veya xbox kullanmamiza olanak saglayacak kutuphaneyi cagiriyoruz 1 kullandiginiz kolun takili oldugu usb portudur driver station uzerinden gorebilir ve ayarlayabilirsiniz
          timer = new Timer(); // timer adinda bir zamanlayici kullanmamiza olanak saglayacak kutuphaneyi cagiriyoruz
     }
}
```

### **Robotunuz için örnek otonom kodu**

```java
public void autonomousInit() { //Bu metod her zaman robot otonom moda girdiginde baslar
     timer.reset(); // zamanlayiciyi resetle
     timer.start(); // saymaya başla
}

public void autonomousPeriodic() { //Bu metod robot otonom modun enable olduguna dair bir paket aldiginda calismaya baslar
     // 2 saniye boyunca robotu sur
     if (timer.get() < 2.0) {
          myRobot.drive(-0.5, 0.0); // robotu yarim hizda ileri sur
     } else {
          myRobot.drive(0.0, 0.0); // Robotu durdur
     }
}
```

### Teleoperation için kolay robot sürme kodu <a id="easy-arcade-drive-for-teleoperation"></a>

```java
public void teleopInit() {// teleopInit metodu her zaman teleop moduna girdiginizde cagrilir
}

public void teleopPeriodic() { // teleperiodic metodu robotun enable olduguna dair bir paket aldiginda calismaya baslar
     myRobot.arcadeDrive(stick); //Bu kod robota tanimladiginiz joystick ile motorlari surmenizi saglar
}

```

### **Test Modu**

```java
public void testPeriodic() {
     LiveWindow.run();
}
//Test Modu, robot işlevselliğini test etmek için kullanılır.Programımızın test modu LiveWindow'u çalıştırır. LiveWindow, Robot Test Modundayken, robot üzerindeki kontrol panelindeki girişleri ve kontrol çıkışlarını görmenizi sağlayan SmartDashboard'un bir parçasıdır.SmartDashboard bölümünde LiveWindow hakkında daha fazla bilgi edinebilirsiniz.
```



