# Command Based Programlama

### Command Based programlama nedir?

WPILib, "Command Based programlama" adlı programlar yazma yöntemini destekler. Command Based programlama, robot programlarınızı düzenlemenize yardımcı olacak bir tasarım modelidir. Diğer masaüstü programlarından farklı olabilecek bazı robot programlarının özellikleri şunlardır:

* Faaliyetler zamanla gerçekleşir, örneğin bir Frizbi çekmek veya bir asansör yükseltmek ve bir hedefe bir tüp yerleştirmek için bir dizi adım.
* Bu aktiviteler eşzamanlı olarak gerçekleşir, bu da robot performansını arttırmak için bir asansör, bilek ve kavrayıcının hepsi aynı zamanda bir alma pozisyonuna hareket etmesi istenebilir.
* Robot mekanizmalarını ve aktivitelerini, robotunuzu hata ayıklamaya yardımcı olmak için ayrı ayrı test etmek istenebilir.
* Çoğu zaman programın son dakikada belki de yarışmalarda ek otonom programlarla güçlendirilmesi gerekir, bu nedenle kolayca genişletilebilir kod önemlidir.

Command based programlama, robot programını daha az yapılandırılmış bir teknik kullanmaktan çok daha basit hale getirmek için tüm bu hedefleri kolayca destekler.

#### Komutlar ve alt sistemler

![](../.gitbook/assets/image%20%2842%29.png)

WPILib kütüphanesine dayalı programlar iki temel kavram etrafında düzenlenmiştir: Alt Sistemler ve Komutlar.

Alt sistemler - robotun her bir parçasının yeteneklerini tanımlar ve Alt Sistemin alt sınıflarıdır.

Komutlar - alt sistemlerde tanımlanan yetenekleri içeren robotun çalışmasını tanımlar. Komutlar Komut veya KomutGrup alt sınıflarıdır. Komutlar, zamanlanmış veya SmartDashboard'dan basılan düğmelere veya sanal düğmelere yanıt olarak çalışır.

#### Komutlar nasıl çalışır

![](../.gitbook/assets/image%20%28116%29.png)

Komutlar, robotu küçük parçalar halinde çalıştırmanın görevlerini çözmenizi sağlar. Her komutun, bazı çalışma yapan bir execute \(\) yöntemi ve tamamlandığını söyleyen isFinished \(\) yöntemi vardır. Bu, sürücü istasyonundan veya her 20 ms'den bir güncellemede gerçekleşir. Komutlar birlikte gruplandırılabilir ve sıralı olarak gerçekleştirilebilir, bir önceki grupta olduğu gibi grupta bir sonraki başlar.

####  Eşzamanlılık

![](../.gitbook/assets/image%20%2884%29.png)

Bazen eşzamanlı olarak gerçekleşen çeşitli operasyonların olması arzu edilir. Önceki örnekte, asansör hareket ederken bileğerin konumunu ayarlamak isteyebilirsiniz. Bu durumda bir komut grubu çalışan bir paralel komut \(veya komut grubu\) başlatabilir.

**Nasıl Çalışır - Komutları Zamanlama**

![](../.gitbook/assets/image%20%2858%29.png)

Komutların planlandığı üç ana yol vardır:



1. Elle, komut üzerindeki start \(\) yöntemini çağırarak \(otonom olarak kullanılır\)
2. Programlayıcı tarafından otomatik olarak kodda belirtilen düğme / tetikleme eylemlerine dayanır \(genellikle OI sınıfında tanımlanır ancak Zamanlayıcı tarafından kontrol edilir\).
3. Bir önceki komut tamamlandığında otomatik olarak \(varsayılan komutlar ve komut grupları\).

Sürücü istasyonu yeni veriler aldığında, robot programınızın periyodik yöntemi denir. Herhangi bir komutun planlanması veya iptal edilmesi gerekip gerekmediğini görmek için tetikleyici koşullarını kontrol eden bir Zamanlayıcı çalıştırır.

Bir komut zamanlandığında, Zamanlayıcı, başka hiçbir komutun, yeni komutun gerektirdiği aynı alt sistemleri kullanmadığından emin olmayı denetler. Alt sistemlerden biri veya daha fazlası şu anda kullanılıyorsa ve geçerli komut kesilirse, kesintiye uğrayacak ve yeni komut zamanlanacaktır. Geçerli komut kesilmezse, yeni komut programlanamaz.

#### Nasıl Çalışır - Komutları Çalıştırma

![](../.gitbook/assets/image%20%2894%29.png)

Yeni komutları kontrol ettikten sonra, programlayıcı aktif komutlar listesinden geçer ve her komutta execute \(\) ve isFinished \(\) yöntemlerini çağırır. Görünür eşzamanlı yürütmenin, programa karmaşıklık katacak iş parçacıkları veya görevler kullanılmadan gerçekleştirildiğine dikkat edin. Her bir komutun basitçe, hedefine doğru ilerlemek için bir kod \(yürütme yöntemi\) ve komutun hedefe ulaşıp ulaşmadığını belirleyen bir yöntem \(isFinished\) vardır. Execute ve isFinished yöntemleri tekrar tekrar denir.

#### Komut grupları

![](../.gitbook/assets/image%20%2857%29.png)

Daha basit komutlardan daha karmaşık komutlar oluşturulabilir. Örneğin, bir diski çekmek, birbiri ardına yürütülen uzun bir komut dizisi olabilir. Belki sıradaki bu komutların bazıları eşzamanlı olarak yürütülebilir. Komut grupları komutlardır, ancak bir Tamamlanmamış ve yürütme yöntemine sahip olmak yerine, yürütmek için başka komutların bir listesi vardır. Bu, daha basit işlemlerden daha karmaşık işlemlerin oluşturulmasına olanak tanır, programlamada temel bir ilkedir. Bireysel küçük komutların her biri önce kolayca test edilebilir, daha sonra grup test edilebilir. Komut grupları hakkında daha fazla bilgi, Komut grubu oluşturma komutları makalesinde bulunabilir.

### Bir robot projesi oluşturma

Eclipse eklentileri ile sağlanan şablon projelerinden birini kullanarak bir komut tabanlı robot projesi oluşturun.  
  
**Proje Oluştur**

![](../.gitbook/assets/image%20%282%29.png)

Proje gezgini penceresinde boş bir alanda sağ tıklayın. "New" i ve ardından "Project ..." i seçin.

#### Proje tipinin seçilmesi

![](../.gitbook/assets/image%20%2839%29.png)

Gerekirse WPILib Robot C ++ veya Java Geliştirme klasörlerini genişletin ve uygun proje türünü seçin, Robot C ++ veya Java Project. Sadece yüklediğiniz eklentinin seçeneklerini göreceksiniz.

#### Örnek Bir Proje Seçmek

![](../.gitbook/assets/image%20%2815%29.png)

Proje isminizi kutuya yazıp Command-Based Robot için radyo düğmesini seçin.

#### Proje Gezgini penceresindeki örnek projeye bakın

![](../.gitbook/assets/image%20%28102%29.png)

 CommandBasedRobotTemplate projesinin Project Explorer penceresinde bulunmuş olabilecek diğer projelere eklendiğine dikkat edin. Komutlar için bir klasör ve Alt sistemler için başka bir klasör var.

### Projeye Komut ve Alt Sistemlerin Eklenmesi

Komutlar ve Altsistemler her biri sınıf olarak oluşturulur. Eklenti, programınıza eklemenizi kolaylaştırmak için hem Komutlar hem de Alt Sistemler için yerleşik şablonlara sahiptir.

#### Projeye alt sistemler ekleme

![](../.gitbook/assets/image%20%2822%29.png)

Bir alt sistem eklemek için, proje adına sağ tıklayın ve açılan menüden "new" ve "Subsystem" i seçin.

#### Alt sistemi adlandırma

![](../.gitbook/assets/image%20%28100%29.png)

Alt sistem için bir ad doldurun. Bu alt sistem için sonuçta ortaya çıkan sınıf adı olacaktır, bu nedenle adınız sizin diliniz için geçerli bir sınıf adı olmalıdır.

#### Projede oluşturulan alt sistem

![](../.gitbook/assets/image%20%2810%29.png)

Projedeki Subsystems klasöründe oluşturulan yeni alt sistemi görebilirsiniz. Alt sistemler oluşturma hakkında daha fazla bilgi için Basit alt sistemler makalesine bakın.

#### Projeye bir komut eklemek

![](../.gitbook/assets/image%20%2835%29.png)

Bir alt sistem oluşturmaya benzer adımlar kullanarak proje için bir komut oluşturulabilir. Proje Gezgini'nde proje adına ilk olarak sağ tıklayın ve new -&gt; Command seçin.  


#### Komut adını ayarla

![](../.gitbook/assets/image%20%28103%29.png)

Komut adını iletişim kutusundaki "Class Name" alanına girin. Bu, Komuta için sınıf adı olacaktır, bu nedenle diliniz için geçerli bir sınıf adı olmalıdır.

#### Komut projede oluşturuldu

![](../.gitbook/assets/image%20%2887%29.png)

Komutun Proje Gezgini penceresinde projedeki Komutlar klasöründe oluşturulduğunu görebilirsiniz. Komut oluşturma hakkında daha fazla bilgi için Basit Komutlar Oluşturma makalesine bakın.

### Basit Alt Sistemler

#### Bir alt sistem oluşturma

```java
Java

import edu.wpi.first.wpilibj.*;
import edu.wpi.first.wpilibj.command.Subsystem;
import org.usfirst.frc.team1.robot.RobotMap;

public class Claw extends Subsystem {

	Victor motor = RobotMap.clawMotor;

    public void initDefaultCommand() {
    }

    public void open() {
    	motor.set(1);
    }

    public void close() {
    	motor.set(-1);
    }

    public void stop() {
    	motor.set(0);
    }
}

```

Bu bir robot üzerinde bir pençe çalışan oldukça basit bir alt sisteme bir örnektir. Pençe mekanizması, pençeyi açmak ve kapatmak için tek bir motora sahiptir ve sensörler yoktur \(pratikte mutlaka iyi bir fikir değildir, ancak örnek için çalışır\). Buradaki fikir, açık ve kapalı işlemlerin basitçe zamanlanmasıdır. Pençe motorunu çalıştıran üç yöntem vardır, open \(\), close \(\) ve stop \(\). Çenenin açılıp kapanmadığını kontrol eden belirli bir kodun bulunmadığına dikkat edin. Açık yöntem, tırnağı açık yönde hareket ettirir ve yakın yöntem, pençenin yakın yönde hareket etmesini sağlar. Pençenin belirli bir süre boyunca açılıp kapanmasını sağlamak için bu işlemin zamanlamasını kontrol etmek için bir komut kullanın.

#### Pençeyi bir komutla çalıştırma

```java

Java

package org.usfirst.frc.team1.robot.commands;

import edu.wpi.first.wpilibj.command.Command;
import org.usfirst.frc.team1.robot.Robot;
/**
 *
 */
public class OpenClaw extends Command {

    public OpenClaw() {
        requires(Robot.claw);
        setTimeout(.9);
    }

    protected void initialize() {
    	Robot.claw.open()
    }

    protected void execute() {
    }

    protected boolean isFinished() {
        return isTimedOut();
    }

    protected void end() {
    	Robot.claw.stop();
    }

    protected void interrupted() {
    	end();
    }
}

```

Komutlar, alt sistemlerin işlemlerinin zamanlamasını sağlar. Her komut alt sistemle farklı bir işlem yapar. Komutlar, açma veya kapama için zamanlamayı sağlar. İşte pençenin açılmasını kontrol eden basit bir Komutanlık örneği. Bu komut için bir zaman aşımı ayarlandığına dikkat edin \(0.9 saniye\), pençenin açılmasını ve isFinished \(\) yöntemindeki süreyi kontrol etmeyi zamanlamak için. Komutları kullanma hakkında makalede daha fazla ayrıntı bulabilirsiniz.

### Yerleşik PID kontrolü için PIDSubsystems

#### Bir bilek ekleminin açısını kontrol etmek için bir PIDSubsystem

Bu örnekte, bilek eklemi için bir PIDSubsystem'in temel öğelerini görebilirsiniz:



```java
Java

package org.usfirst.frc.team1.robot.subsystems;
import edu.wpi.first.wpilibj.*;
import edu.wpi.first.wpilibj.command.PIDSubsystem;
import org.usfirst.frc.team1.robot.RobotMap;


public class Wrist extends PIDSubsystem { // This system extends PIDSubsystem

	Victor motor = RobotMap.wristMotor;
	AnalogInput pot = RobotMap.wristPot();

	public Wrist() {
		super("Wrist", 2.0, 0.0, 0.0);// The constructor passes a name for the subsystem and the P, I and D constants that are used when computing the motor output
		setAbsoluteTolerance(0.05);
		getPIDController().setContinuous(false);
	}
	
    public void initDefaultCommand() {
    }

    protected double returnPIDInput() {
    	return pot.getAverageVoltage(); // returns the sensor value that is providing the feedback for the system
    }

    protected void usePIDOutput(double output) {
    	motor.pidWrite(output); // this is where the computed output value fromthe PIDController is applied to the motor
    }
}

```

### Basit Komutlar Oluşturma

Bu makalede, bir Komutun temel biçimi açıklanmakta ve robotunuzu Joystick'lerle sürmek için bir komut oluşturma örneği sunulmaktadır.

#### Temel Komut Biçimi

Bir komutu uygulamak için, WPILib Command sınıfından bir takım yöntemler geçersiz kılınır. Yöntemlerin çoğu, kazan plakasıdır ve çoğu zaman göz ardı edilebilir, ancak ihtiyacınız olduğunda maksimum esneklik için vardır. Bu temel komut sınıfında birkaç parça var:



```cpp
C++

#include "MyCommandName.h"

/*
	 * 1.	Constructor - Might have parameters for this command such as target positions of devices. Should also set the name of the command for debugging purposes.
	 *  This will be used if the status is viewed in the dashboard. And the command should require (reserve) any devices is might use.
	 */
MyCommandName::MyCommandName() : CommandBase("MyCommandName") 
{
	Requires(Elevator);
}

// initialize() - This method sets up the command and is called immediately before the command is executed for the first time and 
// every subsequent time it is started . Any initialization code should be here.
void MyCommandName::Initialize()
{

}

/*
 *	execute() - This method is called periodically (about every 20ms) and does the work of the command. Sometimes, if there is a position a
 *  subsystem is moving to, the command might set the target position for the subsystem in initialize() and have an empty execute() method.
 */
void MyCommandName::Execute()
{

}

bool MyCommandName::IsFinished()
{
	return false;
}

void MyCommandName::End()
{

}

// Make this return true when this Command no longer needs to run execute()
void MyCommandName::Interrupted()
{

}

```

```java
Java

public class MyCommandName extends Command {

	/*
	 * 1.	Constructor - Might have parameters for this command such as target positions of devices. Should also set the name of the command for debugging purposes.
	 *  This will be used if the status is viewed in the dashboard. And the command should require (reserve) any devices is might use.
	 */
    public MyCommandName() {
    	super("MyCommandName");
        requires(elevator);
    }

    // 	initialize() - This method sets up the command and is called immediately before the command is executed for the first time and every subsequent time it is started .
    //  Any initialization code should be here. 
    protected void initialize() {
    }

    /*
     *	execute() - This method is called periodically (about every 20ms) and does the work of the command. Sometimes, if there is a position a
     *  subsystem is moving to, the command might set the target position for the subsystem in initialize() and have an empty execute() method.
     */
    protected void execute() {
    }

    // Make this return true when this Command no longer needs to run execute()
    protected boolean isFinished() {
        return false;
    }
}

```

#### Basit Komut Örneği

Bu örnek, robotun tank tahriklerini kullanarak joysticklerin sağladığı değerlerle sürecek basit bir komutu göstermektedir.  
  


```cpp
C++

#include "DriveWithJoysticks.h"
#include "RobotMap.h"

DriveWithJoysticks::DriveWithJoysticks() : CommandBase("DriveWithJoysticks")
{
	Requires(Robot::drivetrain); // Drivetrain is our instance of the drive system
}

// Called just before this Command runs the first time
void DriveWithJoysticks::Initialize()
{

}

    /*
     * execute() - In our execute method we call a tankDrive method we have created in our subsystem. This method takes two speeds as a parameter which we get from methods in the OI class.
     * These methods abstract the joystick objects so that if we want to change how we get the speed later we can do so without modifying our commands
     * (for example, if we want the joysticks to be less sensitive, we can multiply them by .5 in the getLeftSpeed method and leave our command the same).
     */
void DriveWithJoysticks::Execute()
{
	Robot::drivetrain->Drive(Robot::oi->GetJoystick());
}

    /*
     * isFinished - Our isFinished method always returns false meaning this command never completes on it's own. The reason we do this is that this command will be set as the default command for the subsystem. This means that whenever the subsystem is not running another command, it will run this command. If any other command is scheduled it will interrupt this command, then return to this command when the other command completes.
     */
bool DriveWithJoysticks::IsFinished()
{
	return false;
}

void DriveWithJoysticks::End()
{
	Robot::drivetrain->Drive(0, 0);
}

// Called when another command which requires one or more of the same
// subsystems is scheduled to run
void DriveWithJoysticks::Interrupted()
{
	End();
}

```

```java
Java

public class DriveWithJoysticks extends Command {

    public DriveWithJoysticks() {
    	requires(drivetrain);// drivetrain is an instance of our Drivetrain subsystem
    }

    protected void initialize() {
    }

    /*
     * execute() - In our execute method we call a tankDrive method we have created in our subsystem. This method takes two speeds as a parameter which we get from methods in the OI class.
     * These methods abstract the joystick objects so that if we want to change how we get the speed later we can do so without modifying our commands
     * (for example, if we want the joysticks to be less sensitive, we can multiply them by .5 in the getLeftSpeed method and leave our command the same).
     */
    protected void execute() {
    	drivetrain.tankDrive(oi.getLeftSpeed(), oi.getRightSpeed());
    }

    /*
     * isFinished - Our isFinished method always returns false meaning this command never completes on it's own. The reason we do this is that this command will be set as the default command for the subsystem. This means that whenever the subsystem is not running another command, it will run this command. If any other command is scheduled it will interrupt this command, then return to this command when the other command completes.
     */
    protected boolean isFinished() {
        return false;
    }

    protected void end() {
    }

    protected void interrupted() {
    }
}

```

### Komut grupları oluşturma

#### Karmaşık bir işlem yapmak için bir komut oluşturma



```cpp
C++

#include "PlaceSoda.h"

PlaceSoda::PlaceSoda()
{
	AddSequential(new SetElevatorSetpoint(TABLE_HEIGHT));
	AddSequential(new SetWristSetpoint(PICKUP));
	AddSequential(new OpenClaw());
}


```

```java
Java

public class PlaceSoda extends CommandGroup {

    public  PlaceSoda() {
    	addSequential(new SetElevatorSetpoint(Elevator.TABLE_HEIGHT));
    	addSequential(new SetWristSetpoint(Wrist.PICKUP));
    	addSequential(new OpenClaw());
    }
}

```

Bu, bir sodayı masaya yerleştiren bir komut grubunun örneğidir. Bunu başarmak için, \(1\) robot elevatörü "TABLE\_HEIGHT" a geçmeli, \(2\) bilek açısını ayarlamalı, ardından \(3\) tırnağı açmalıdır. Tüm bu görevler, sodanın düşemeyeceğinden emin olmak için sırayla çalıştırılmalıdır. AddSequential \(\) yöntemi, bir komutu \(veya komut grubunu\) bir parametre olarak alır ve bu komut programlandığında bunları birbiri ardına yürütür.

#### Komutları paralel olarak çalıştırma



```cpp
C++

#include "PrepareToGrab.h"

PrepareToGrab::PrepareToGrab()
{
	AddParallel(new SetWristSetpoint(PICKUP));
	AddParallel(new SetElevatorSetpoint(BOTTOM));
	AddParallel(new OpenClaw());
}

```

```java
Java

public class PrepareToGrab extends CommandGroup {

    public  PrepareToGrab() {
    	addParallel(new SetWristSetpoint(Wrist.PICKUP));
    	addParallel(new SetElevatorSetpoint(Elevator.BOTTOM));
    	addParallel(new OpenClaw());
    }
}
```

Programı daha verimli hale getirmek için, çoğu kez aynı anda birden fazla komutun çalıştırılması istenir. Bu örnekte, robot bir soda alabilir. Robot hiçbir şey tutmadığından, tüm eklemler bir şey düşürmekten endişe etmeden aynı anda hareket edebilir. Burada tüm komutlar paralel olarak çalışır, böylece tüm motorlar aynı anda çalışır ve her isFinished \(\) yöntemi çağrıldığında tamamlanır. Komutlar emredilemez. Adımlar şunlardır: \(1\) bileği başlatma ayar noktasına getirin, daha sonra \(2\) asansörü yerden teslim konumuna getirin ve \(3\) tırnağı açın.

#### Paralel ve sıralı komutları karıştırma



```cpp
C++

#include "Grab.h"

Grab::Grab()
{
	AddSequential(new CloseClaw());
	AddParallel(new SetElevatorSetpoint(STOW));
	AddSequential(new SetWristSetpoint(STOW));
}

```

```java
Java

public class Grab extends CommandGroup {

    public  Grab() {
    	addSequential(new CloseClaw());
    	addParallel(new SetElevatorSetpoint(Elevator.STOW));
    	addSequential(new SetWristSetpoint(Wrist.STOW));
    }
}
```

Genellikle, diğer parçaların çalıştırılmasından önce tamamlanması gereken bir komut grubunun bazı bölümleri vardır. Bu örnekte, bir soda kabı tutulur, daha sonra asansör ve bilek istiflenmiş konumlarına hareket edebilir. Bu durumda, el bileği ve asansörü, kap kapana kadar beklemek zorundadır, sonra bağımsız olarak çalışabilirler.

### Joystick inputları ile komutları çalıştırma

Düğmeye basılı tutulduğunda kumanda düğmelerine basıldığında, serbest bırakıldığında veya sürekli olarak komutların çalışmasına neden olabilirsiniz. Bu sadece birkaç satırlık kod gerektiren yapmak için son derece kolaydır.

#### OI sınıfı

![](../.gitbook/assets/image%20%2882%29.png)

Komut tabanlı şablon, Operatör Arayüzü davranışlarının tipik olarak tanımlandığı OI.java'da bulunan OI adında bir sınıf içerir. Eğer RobotBuilder kullanıyorsanız bu dosya org.usfirst.frc \#\#\#\#. NAME paketinde bulunabilir.  


#### Joystick nesnesini ve JoystickButton nesnelerini oluşturun



```cpp
C++

OI::OI()
{
	joy = new Joystick(1);

	JoystickButton* button1 = new JoystickButton(joy, 1),
			button2 = new JoystickButton(joy, 2),
			button3 = new JoystickButton(joy, 3),
			button4 = new JoystickButton(joy, 4),
			button5 = new JoystickButton(joy, 5),
			button6 = new JoystickButton(joy, 6),
			button7 = new JoystickButton(joy, 7),
			button8 = new JoystickButton(joy, 8);
	
}

```

```java
Java

public class OI {
    // Create the joystick and the 8 buttons on it
	Joystick leftJoy = new Joystick(1);
	Button button1 = new JoystickButton(leftJoy, 1),
			button2 = new JoystickButton(leftJoy, 2),
			button3 = new JoystickButton(leftJoy, 3),
			button4 = new JoystickButton(leftJoy, 4),
			button5 = new JoystickButton(leftJoy, 5),
			button6 = new JoystickButton(leftJoy, 6),
			button7 = new JoystickButton(leftJoy, 7),
			button8 = new JoystickButton(leftJoy, 8);
	
}
```

Bu örnekte Joystick 1 olarak bağlı bir Joystick nesnesi vardır. Daha sonra robotun çeşitli yönlerini kontrol etmek için bu joystick üzerinde 8 düğme tanımlanmıştır. Bu, test için özellikle yararlıdır, ancak SmartDashboard üzerindeki butonların üretilmesi komutların test edilmesi için başka bir alternatiftir.

#### Düğmeleri komutlarla ilişkilendirin



```cpp
C++

	button1->WhenPressed(new PrepareToGrab());
	button2->WhenPressed(new Grab());
	button3->WhenPressed(new DriveToDistance(0.11));
	button4->WhenPressed(new PlaceSoda());
	button6->WhenPressed(new DriveToDistance(0.2));
	button8->WhenPressed(new Stow());
	
	button7->WhenPressed(new SodaDelivery());

```

```java
Java

	public OI() {
		button1.whenPressed(new PrepareToGrab());
		button2.whenPressed(new Grab());
		button3.whenPressed(new DriveToDistance(0.11));
		button4.whenPressed(new PlaceSoda());
		button6.whenPressed(new DriveToDistance(0.2));
		button8.whenPressed(new Stow());
		
		button7.whenPressed(new SodaDelivery());
	}

```

Bu örnekte, önceki kod parçasından gelen joystick düğmelerinin çoğu, komutlarla ilişkilendirilmiştir. İlgili tuşa basıldığında komut çalıştırılır. Bu, özel eylemler yapmak için düğmeleri olan bir teleop programı oluşturmanın mükemmel bir yoludur.

#### Diğer seçenekler

 Yukarıda gösterilen "whenPressed \(\)" koşuluna ek olarak, düğmeleri komutlara bağlamak için kullanabileceğiniz birkaç başka koşul vardır:

* Komutlar, bir düğme WhenPold \(\) yerine whenReleased \(\) kullanılarak serbest bırakıldığında çalışabilir.
* Komutlar, whileHeld \(\) öğesini çağırmak suretiyle tuşa basarken sürekli olarak çalışabilir.
* ToggleWhenPressed \(\) kullanılarak bir tuşa basıldığında komutlar değiştirilebilir.
* CancelWhenPressed \(\) kullanılarak bir tuşa basıldığında bir komut iptal edilebilir.

Ayrıca, komutlar, Düğme yerine Trigger sınıfını kullanarak seçiminizin keyfi koşulları tarafından tetiklenebilir. Tetikleyiciler \(ve Düğmeler\) genellikle her 20 ms'de veya zamanlayıcı çağrıldığında her zaman sorgulanır.

### Otonom periyodu boyunca komutları çalıştırma

#### Otonom Periyodunda kullanılacak bir komut oluşturma

```cpp
C++

	button1->WhenPressed(new PrepareToGrab());
	button2->WhenPressed(new Grab());
	button3->WhenPressed(new DriveToDistance(0.11));
	button4->WhenPressed(new PlaceSoda());
	button6->WhenPressed(new DriveToDistance(0.2));
	button8->WhenPressed(new Stow());
	
	button7->WhenPressed(new SodaDelivery());

```

```java
Java

	public OI() {
		button1.whenPressed(new PrepareToGrab());
		button2.whenPressed(new Grab());
		button3.whenPressed(new DriveToDistance(0.11));
		button4.whenPressed(new PlaceSoda());
		button6.whenPressed(new DriveToDistance(0.2));
		button8.whenPressed(new Stow());
		
		button7.whenPressed(new SodaDelivery());
	}

```

Robotumuz, otonom dönemde aşağıdaki görevleri yerine getirmelidir: yerden bir soda alabilir, daha sonra bir masadan belirli bir mesafeyi sürdürebilir ve oradaki tenekeyi teslim edebilir. Süreci oluşur:

1. Kapmak için hazırlayın \(asansör, bilek ve kavrayıcıyı yerine hareket ettirin\)
2. Sodayı tutabilirsiniz.
3. Ultrasonik telemetre ile gösterilen tablodan bir mesafeye kadar sürün
4. Sodayı bırakın
5. Uzaklık mesafesinden bir mesafeye geri dönüş
6. Tutucuyu yeniden yerleştirmek

Bu görevleri yapmak için, bu örnekte gösterildiği gibi sırayla yürütülen 6 komut grubu vardır.

#### Otonom çalışacak şekilde bu komutu ayarlama

```text
C++

Command* autonomousCommand;

class Robot: public IterativeRobot {

    /**
     * This function is run when the robot is first started up and should be
     * used for any initialization code.
     */
	void RobotInit()
	{
        // instantiate the command used for the autonomous period
		autonomousCommand = new SodaDelivery();
		oi = new OI();

	}
	

	void AutonomousInit()
	{
        // schedule the autonomous command (example)
		if(autonomousCommand != NULL) autonomousCommand->Start();
	}
	/*
	 * This function is called periodically during autonomous
	 */
	void AutonomousPeriodic()
	{
		Scheduler::GetInstance()->Run();
	}

```

```text
Java

public class Robot extends IterativeRobot {



    Command autonomousCommand;

    /**
     * This function is run when the robot is first started up and should be
     * used for any initialization code.
     */
    public void robotInit() {
		oi = new OI();
        // instantiate the command used for the autonomous period
        autonomousCommand = new SodaDelivery();
    }

    public void autonomousInit() {
        // schedule the autonomous command (example)
        if (autonomousCommand != null) autonomousCommand.start();
    }

    /**
     * This function is called periodically during autonomous
     */
    public void autonomousPeriodic() {
        Scheduler.getInstance().run();
    }
```

SodaDelivery komutunu otonom program olarak çalıştırmak için

1. Bunu RobotInit \(\) yönteminde örneklendirin. RobotInit \(\), robot başladığında sadece bir kez çağrılır, böylece komut örneğini oluşturmak için iyi bir zaman olur.
2. AutonomousInit \(\) yöntemi sırasında başlatın. AutonomousInit \(\), otonom dönemin başlangıcında bir kez çağrılır, böylece oradaki komutu programlanır.
3. Programlayıcıya AutonomousPeriodic \(\) yöntemi sırasında tekrar tekrar denir. AutonomousPeriodic \(\) her 20 ms'de \(nominal olarak\) çağrılır, böylece programlanmış olan tüm komutlar boyunca geçiş yapan zamanlayıcıyı çalıştırmak için iyi bir zamandır.

### Basit Otonom Bir Programı Command-Based Otonom Programa Dönüştürme

#### Döngüler ile ilk otonom kod



```text
C++

// Aim shooter
SetTargetAngle(); // Initialization: prepares for the action to be performed
while (!AtRightAngle()) { // Condition: keeps the loop going while it is satisfied
	CorrectAngle(); // Execution: repeatedly updates the code to try to make the condition false
	delay(); // Delay to prevent maxing CPU
}
HoldAngle(); // End: performs any cleanup and final task before moving on to the next action

// Spin up to Speed
SetTargetSpeed(); // Initialization: prepares for the action to be performed
while (!FastEnough()) { // Condition: keeps the loop going while it is satisfied
	SpeedUp(); // Execution: repeatedly updates the code to try to make the condition false
	delay(); // Delay to prevent maxing CPU
}
HoldSpeed();

// Shoot Frisbee
Shoot(); // End: performs any cleanup and final task before moving on to the next action
}

```

```text
Java

// Aim shooter
SetTargetAngle(); // Initialization: prepares for the action to be performed
while (!AtRightAngle()) { // Condition: keeps the loop going while it is satisfied
	CorrectAngle(); // Execution: repeatedly updates the code to try to make the condition false
	delay(); // Delay to prevent maxing CPU
}
HoldAngle(); // End: performs any cleanup and final task before moving on to the next action

// Spin up to Speed
SetTargetSpeed(); // Initialization: prepares for the action to be performed
while (!FastEnough()) { // Condition: keeps the loop going while it is satisfied
	SpeedUp(); // Execution: repeatedly updates the code to try to make the condition false
	delay(); // Delay to prevent maxing CPU
}
HoldSpeed();

// Shoot Frisbee
Shoot(); // End: performs any cleanup and final task before moving on to the next action
}
```

Yukarıdaki kod bir atıcıyı hedefler, sonra bir tekerleği döndürür ve son olarak, tekerlek istenen hızda çalıştığında frizbi vurur. Kod üç farklı eylemden oluşur: amaç, hızlanmak ve Frizbi'yi vurmak. İlk iki eylem, dört bölümden oluşan bir komut desenini takip eder:

1. Başlatma: eylemin gerçekleştirilmesini hazırlar.
2. Durum: her şey yolunda iken döngü devam ediyor.
3. Yürütme: koşulu yanlış yapmak için kodu tekrar tekrar günceller.
4. Son: Bir sonraki eyleme geçmeden önce herhangi bir temizleme ve son görevi gerçekleştirir.

Son eylem sadece açık bir başlatma özelliğine sahiptir, ancak nasıl okunduğunuza bağlı olarak, bir dizi koşul altında örtülü olarak sona erebilir. 

#### Komut olarak yeniden yazma



```cpp
C++

#include "AutonomousCommand.h"

AutonomousCommand::AutonomousCommand()
{
     AddSequential(new Aim());
     AddSequential(new SpinUpShooter());
     AddSequential(new Shoot());
}

```

```java
Java

public class AutonomousCommand extends CommandGroup {

    public  AutonomousCommand() {
    	addSequential(new Aim());
    	addSequential(new SpinUpShooter());
    	addSequential(new Shoot());
    }
}

```

Aynı kod, her eylemin kendi komutu olarak yazıldığı üç eylemi gruplandıran bir KomutGrup olarak yeniden yazılabilir. Öncelikle, komut grubu yazılacak, daha sonra üç eylemi gerçekleştirmek için komutlar yazılacaktır. Bu kod oldukça basittir. Ardı ardına üç eylem yapar, bu birbiri ardına gerçekleşir. Çizgi 3 robotu hedefliyor, sonra 4 numaralı çizgi shooterup'ı çeviriyor ve son olarak 5. hat aslında frizbiyi vuruyor. AddSequential \(\) yöntemi, bu komutların birbiri ardına çalışması için ayarlar.

#### Aim Komutu

```cpp
C++

#include "Aim.h"

Aim::Aim()
{
     Requires(Robot::turret);
}

// Called just before this Command runs the first time
void Aim::Initialize()
{
     SetTargetAngle();
}

// Called repeatedly when this Command is scheduled to run
void Aim:Execute()
{
 \    CorrectAngle();
}

// Make this return true when this Command no longer needs to run execute()
bool Aim:IsFinished()
{
     return AtRightAngle();
}

// Called once after isFinished returns true
void Aim::End()
{
     HoldAngle();
}
// Called when another command which requires one or more of the same
// subsystems is scheduled to run
void Aim:Interrupted()
{
     End();
}

```

```java
Java

public class Aim extends Command {

    public Aim() {
    	requires(Robot.turret);
    }

    // Called just before this Command runs the first time
    protected void initialize() {
    	SetTargetAngle();
    }

    // Called repeatedly when this Command is scheduled to run
    protected void execute() {
    	CorrectAngle();
 \   }

    // Make this return true when this Command no longer needs to run execute()
    protected boolean isFinished() {
        return AtRightAngle();
 \   }

    // Called once after isFinished returns true
    protected void end() {
    	HoldAngle();
    }

    // Called when another command which requires one or more of the same
    // subsystems is scheduled to run
    protected void interrupted() {
    	end();
    }
}

```

Gördüğünüz gibi, komut daha önce tartıştığımız eylemin dört bölümünü yansıtıyor. Ayrıca aşağıda tartışılacak olan interrupted \(\) yöntemine de sahiptir. Diğer önemli fark, isFinished \(\) öğesindeki koşulun while döngüsünün koşulu olarak ne koyacağınızın tersi olması, eğer yürütme yöntemini false yerine çalıştırmayı durdurmak istediğinizde true olarak geri dönmesidir. Başlatma, yürütme ve sonlandırma tamamen aynıdır, sadece yaptıkları şeyi belirtmek için kendi yöntemlerine girerler.

#### SpinUpShooter komutu



```cpp
C++

#include "SpinUpShooter.h"

SpinUpShooter::SpinUpShooter()
{
     Requires(Robot::shooter)
}

// Called just before this Command runs the first time
void SpinUpShooter::Initialize()
{
 \    SetTargetSpeed();
}

// Called repeatedly when this Command is scheduled to run
void SpinUpShooter::Execute()
{
     SpeedUp();
}

// Make this return true when this Command no longer needs to run execute()
bool SpinUpShooter::IsFinished()
{
 \    return FastEnough();
}

// Called once after isFinished returns true
void SpinUpShooter::End()
{
     HoldSpeed();
}

// Called when another command which requires one or more of the same
// subsystems is scheduled to run
void SpinUpShooter::Interrupted()
{
     End();
}

```

```java
Java

public class SpinUpShooter extends Command {

    public SpinUpShooter() {
        requires(Robot.shooter);
 \   }

    // Called just before this Command runs the first time
    protected void initialize() {
    	SetTargetSpeed();
    }

    // Called repeatedly when this Command is scheduled to run
    protected void execute() {
    	SpeedUp();
 \   }

    // Make this return true when this Command no longer needs to run execute()
    protected boolean isFinished() {
        return FastEnough();
 \   }

    // Called once after isFinished returns true
    protected void end() {
    	HoldSpeed();
    }

    // Called when another command which requires one or more of the same
    // subsystems is scheduled to run
    protected void interrupted() {
    	end();
    }
}

```

Spin up shooter komutu, Aim komutuna çok benziyor, aynı temel fikir.

#### Shoot Komutu



```cpp
C++

#include "Shoot.h"

Shoot::Shoot()
{
     Requires(Robot.shooter);
}

// Called just before this Command runs the first time
void Shoot::Initialize()
{
 \    Shoot();
}

// Called repeatedly when this Command is scheduled to run
void Shoot::Execute()
{
}

// Make this return true when this Command no longer needs to run execute()
bool Shoot::IsFinished()
{
     return true;
}

// Called once after isFinished returns true
void Shoot::End()
{
}

// Called when another command which requires one or more of the same
// subsystems is scheduled to run
void Shoot::Interrupted()
{
     End();
}

```

```java
Java

public class Shoot extends Command {

    public Shoot() {
        requires(shooter);
    }

    // Called just before this Command runs the first time
    protected void initialize() {
    	Shoot();
    }

    // Called repeatedly when this Command is scheduled to run
    protected void execute() {
    }

    // Make this return true when this Command no longer needs to run execute()
    protected boolean isFinished() {
        return true;
    }

    // Called once after isFinished returns true
    protected void end() {
    }

    // Called when another command which requires one or more of the same
    // subsystems is scheduled to run
 \   protected void interrupted() {
    	end();
    }
}
```

Shoot komutu yine aynı temel dönüşümdür, ancak hemen sona erecek şekilde ayarlanmıştır. CommandBased programlamada, fırlatma eylemi bittiğinde, Bitmiş yöntemin doğru olması için daha iyidir, ancak bu, orijinal kodun daha doğrudan bir çevirisidir.

### Varsayılan Komutlar

Bazı durumlarda, ne olursa olsun her zaman bir komut çalıştırmak istediğiniz bir alt sisteminiz olabilir. Peki, şu anda çalıştırmakta olduğunuz komut ne zaman bitiyor? Burada varsayılan komutlar gelir.

#### Varsayılan komut nedir?

Her bir alt sistem, alt sistem boşta olduğunda programlanmış olan varsayılan bir komut içerebilir, ancak bunun için gerekli değildir \(sistem şu anda tamamlanması gereken komut\). Varsayılan komutun en yaygın örneği, normal joystick kontrolünü uygulayan aktarma organları için bir komuttur. Bu komut, belirli manevralar \("hassas mod", otomatik hizalama / hedefleme, vb.\) İçin başka komutlar tarafından kesintiye uğrayabilir, ancak aktarma organlarının tamamlanmasını gerektiren herhangi bir komutun ardından joystick komutu tekrar programlanır.

#### Varsayılan komutu ayarlama

```text
C++

#include "ExampleSubsystem.h"

ExampleSubsystem::ExampleSubsystem()
{
     // Put methods for controlling this subsystem
     // here. Call these from Commands.
}

ExampleSubsystem::InitDefaultCommand()
{
     // Set the default command for a subsystem here.
     SetDefaultCommand(new MyDefaultCommand());
}

```

```text
Java

public class ExampleSubsystem extends Subsystem {

    // Put methods for controlling this subsystem
    // here. Call these from Commands.

    public void initDefaultCommand() {
        // Set the default command for a subsystem here.
        setDefaultCommand(new MyDefaultCommand());
    }
}
```

Tüm alt sistemler, istenirse varsayılan komutu ayarlayacağınız initDefaultCommand \(\) adında bir yöntem içermelidir. Varsayılan bir komut kullanmak istemiyorsanız, bu yöntemi boş bırakın. Varsayılan bir komut ayarlamak isterseniz, setDefaultCommand'ı bu yöntem içinden çağırarak, varsayılan olarak ayarlanacak komutu iletin.

#### İki komutu senkronize etme

Komutlar daha karmaşık komutlar oluşturmak için komut gruplarının içine yerleştirilebilir. Daha basit komutlar, sıralı olarak \(her komut bir sonraki başlatmadan önce bitirilir\) veya paralel olarak \(komut zamanlanır ve bir sonraki komut da hemen programlanır\) komut gruplarına eklenebilir. Bazen, bir sonraki komuta geçmeden önce iki paralel komutun tamamlandığından emin olmak istediğiniz zamanlar vardır. Bu makalede, bunun nasıl yapılacağı anlatılmaktadır.

#### Sıralı ve paralel komutlarla bir komut grubu oluşturma

```cpp
C++

#include "CoopBridgeAutonomous.h"

CoopBridgeAutonomous::CoopBridgeAutonomous()
{
     // SmartDashboard->PutDouble("Camera Time", 5.0);
     AddSequential(new SetTipperState(READY_STATE);
     AddParallel(new SetVirtualSetpoint(HYBRID_LOCATION);
     AddSequential(new DriveToBridge());
     AddParallel(new ContinuousCollect());
     AddSequential(new SetTipperState(DOWN_STATE));

     // addParallel(new WaitThenShoot());

     AddSequential(new TurnToTargetLowPassFilterHybrid(4.0));
     AddSequential(new FireSequence());
     AddSequential(new MoveBallToShooter(true));
}

```

```java
Java

public class CoopBridgeAutonomous extends CommandGroup {

    public  CoopBridgeAutonomous() {
    	//SmartDashboard.putDouble("Camera Time", 5.0);
    	addSequential(new SetTipperState(BridgeTipper.READY_STATE)); // 1
    	addParallel(new SetVirtualSetpoint(SetVirtualSetpoint.HYBRID_LOCATION)); // 2
    	addSequential(new DriveToBridge()); // 3
    	addParallel(new ContinuousCollect());
    	addSequential(new SetTipperState(BridgeTipper.DOWN_STATE));
    	
    	// addParallel(new WaitThenShoot());
    	
    	addSequential(new TurnToTargetLowPassFilterHybrid(4.0));
    	addSequential(new FireSequence());
    	addSequential(new MoveBallToShooter(true));
    }
}
```

Bu örnekte, bazı komutlar paralel olarak eklenir ve diğerleri CommandGroup CoopBridgeAutonomous \(1\) öğesine ardışık olarak eklenir. SetVirtualSetpoint komutu başlamadan önce ilk komut "SetTipperState" eklenir ve tamamlanır \(2\). SetVirtualSetpoint komutu tamamlanmadan önce, SetVirtualSetpoint paralel olarak eklendiğinden hemen DriveToBridge komutuna programlanır \(3\). Bu örnek, komutların nasıl planlandığı konusunda size bir fikir verebilir.

#### Örnek Akış Şeması

![](../.gitbook/assets/image%20%2868%29.png)

Yukarıda gösterilen kod bir akış şeması olarak gösterilmiştir. "Add Parallel" kullanılarak planlanan komutlardan herhangi bir bağımlılık olmadığına dikkat edin, MoveBallToShooter komutuna ulaşıldığında bu komutların ikisi de veya ikisi de çalışıyor olabilir. Bir paralel komut tarafından kullanılan bir alt sistemi gerektiren ana dizideki \(burada sağdaki sıra\) herhangi bir komut, paralel komutun iptal edilmesine neden olur. Örneğin, FireSequence, SetVirtualSetpoint tarafından kullanılan bir alt sistem gerektiriyorsa, FireSequence zamanlandığında SetVirtualSetpoint komutu iptal edilir.

