# Sürücü İstasyonu\(Driver Station\) Girişleri ve Geri Bildirim

### Sürücü İstasyonuna Genel Bakış

 FRC Driver Station yazılımı, insan operatörler ve robot arasında bir arayüz görevi görür. Yazılım, bir dizi kaynaktan girdi alır ve robot kodunun kontrol mekanizmaları için hareket edebileceği robotu yönlendirir.

**Giriş tipleri**

![](../.gitbook/assets/image%20%2815%29.png)

Yukarıdaki grafik DS yazılımı tarafından iletilebilecek farklı giriş türlerini göstermektedir. En yaygın giriş, 2009'dan beri Parça Kitinde sağlanan Logitech Attack 3 veya Logitech Extreme 3D Pro joystick gibi bir HID uyumlu joystick veya gamepad'tir. Özel IO'nun kullanılmasına izin veren bir dizi aygıtın mevcut olduğunu unutmayın. TI Launchpad ve Parçalar Kitinizde bulunan 16 Hertz Leonardo ++ gibi standart bir USB HID cihazı olarak ortaya çıkar.

#### Driver Station **Sınıfı**

**C++**  
`DriverStation& ds = DriverStation::GetInstance();  
ds.SomeMethod();  
  
DriverStation::GetInstance().SomeMethod();`  
  
**Java**  
`DriverStation ds = DriverStation.getInstance();  
ds.someMethod();  
  
DriverStation.getInstance.someMethod();`

Driver Station sınıfı, robot modu, akü voltajı, ittifak rengi ve takım numarası gibi bilgilere erişmek için yöntemlere sahiptir. Driver Station sınıfının joystick verilerine erişme yöntemleri olduğu halde, bu verilere çok daha kullanıcı dostu bir arayüz sağlayan başka bir "Joystick" sınıfı olduğunu unutmayın. DriverStation sınıfı, temel sınıf tarafından bir singleton olarak yapılandırılmıştır. Temel sınıf tarafından oluşturulan DriverStation nesnesinin yöntemlerine erişmek için, DriverStation.getInstance \(\) yöntemini çağırın ve ya sonucu bir DriverStation nesnesinde saklayın.

**Robot Modu**

**C++**  
`bool exampleBool;  
exampleBool = IsDisabled();  
exampleBool = IsEnabled();  
exampleBool = IsAutonomous();  
exampleBool = IsOperatorControl();  
exampleBool = IsTest();  
  
while(IsOperatorControl() && IsEnabled())  
{  
}  
  
exampleBool = DriverStation::GetInstance()->IsDisabled();`  
  
**Java**  
`boolean exampleBool;  
exampleBool = isDisabled();  
exampleBool = isEnabled();  
  
exampleBool = isAutonomous();  
exampleBool = isOperatorControl();  
exampleBool = isTest();  
  
while(isOperatorControl() && isEnabled())  
{  
}  
  
exampleBool = DriverStation.getInstance().isDisabled();`

Driver Station sınıfı, robotun mevcut modunu kontrol etmek için bir dizi yöntem sunar, bu yöntemler en çok SampleRobot temel sınıfını kullanırken program akışını kontrol etmek için kullanılır. Geçerli modu, etkin durumu \(etkin / devre dışı\) ve kontrol durumunu \(otonom, operatör kontrolü, test\) tanımlayan iki ayrı bilgi parçası vardır. Bu, ilk gruptan tam olarak bir yöntem ve ikinci gruptan bir yöntemin her zaman doğru olması gerektiği anlamına gelir. Örneğin, Sürücü İstasyonu şu anda Test moduna ayarlanmışsa ve robot devre dışı bırakılmışsa, yöntemler isDisabled \(\) ve isTest \(\) her ikisi de doğru olacaktır. Bu yöntemlerin uygulanması, DriverStation sınıfındayken, \(bu şablonlar genişleyen\) RobotBase sınıfı, bu yöntemlere proxy'ler sağlar, böylece sınıf belirtimi olmadan kullanılabilirler \(yukarıdaki ilk 3 örnek grubunda gösterildiği gibi\). Bu yöntemleri başka bir sınıftan çağırmak için, son örnekte gösterildiği gibi DriverStation örneğini kullanın.

DS bağlantısı, FMS bağlantısı ve Sistem durumu

**C++**  
`bool exampleBool;  
exampleBool = DriverStation::GetInstance().IsDSAttached();  
exampleBool = DriverStation::GetInstance().IsFMSAttached();  
exampleBool = DriverStation::GetInstance().IsSysActive();  
exampleBool = DriverStation::GetInstance().IsSysBrownedOut();`  
  
**Java**  
`boolean exampleBool;  
exampleBool = DriverStation.getInstance().isDSAttached();  
exampleBool = DriverStation.getInstance().isFMSAttached();  
exampleBool = DriverStation.getInstance().isSysActive();  
exampleBool = DriverStation.getInstance().isSysBrownedOut();`

DriverStation sınıfı ayrıca DS'nin robota bağlı olup olmadığını, eğer DS FP çıkışları etkinleştirilmişse \(IsSysActive\) DS'ye FMS sorgulama ile bağlanırsa ve roboRIO'nun yavaşlama korumasında olup olmadığını sorgulamak için yöntemlere sahiptir. FPGA çıkışları aşağıdakiler dahil olmak üzere çeşitli nedenlerden dolayı devre dışı bırakılabilir: DS, Disabled \(Devre Dışı\) veya E-Stop \(Devre Dışı Bırakılıyor\) komutu veriyor, sistem watchdog'u zaman aşımına uğradı \(genellikle DS'nin roboRIO ile iletişim kurmaması nedeniyle\) veya roboRIO'nun blackout koruması var.

**Batarya voltajı**

**C++**  
`double voltage = DriverStation::GetInstance().GetBatteryVoltage();`  
  
**Java**  
`double voltage = DriverStation.getInstance().getBatteryVoltage();`

Uyumluluk amacıyla, akü voltajı, DriverStation sınıfı kullanılarak alınabilir \(şimdi, ControllerPower sınıfından roboRIO giriş voltajı olarak da mevcuttur\). Bu bilgi, voltaj kompanzasyonu yapmak veya akü voltaj düşüşlerini tespit ederek ve kritik olmayan mekanizmaları kapatmak veya sınırlandırmak suretiyle robot güç çekişini aktif olarak yönetmek için DriverStation sınıfından sorgulanabilir.

**İttifak\(Alliance\)**

**C++**  
`DriverStation::Alliance color;  
color = DriverStation::GetInstance().GetAlliance();  
if(color == DriverStation::Alliance::kBlue){  
}`  
  
**Java**  
`DriverStation.Alliance color;  
color = DriverStation.getInstance().getAlliance();  
if(color == DriverStation.Alliance.kBlue){  
}`

DriverStation sınıfı, robotun hangi ittifak rengi olduğuna dair bilgi sağlayabilir. FMS'ye bağlandığında bu alan tarafından DS'ye iletilen ittifak rengi. Bağlı olmadığında, ittifak rengi DS yazılımının İşletim sekmesindeki Ekip İstasyonu açılır kutusu tarafından belirlenir.

**Konum**

**C++**  
`int station;  
station = DriverStation::GetInstance.GetLocation();`  
  
**Java**  
`int station;  
station = DriverStation.getInstance().getLocation();`

Sürücü İstasyonunun getLocation \(\) yöntemi, Sürücü İstasyonunun hangi istasyon numarasını \(1-3\) içerdiğini belirten bir tam sayı döndürür. DS ve kontrollerin yerleştirildiği istasyonun tipik olarak robotun başlangıç pozisyonu ile ilgili olmadığını unutmayın, bu nedenle bu bilgiler sınırlı kullanım olabilir. FMS yazılımına bağlanmadığında, bu durum DS Çalışması sekmesindeki Ekip İstasyonundaki açılır menü tarafından belirlenir.

**Maç Süresi**

**C++**  
`double time;  
time = DriverStation::GetInstance.GetMatchTime();`  
  
**Java**  
`double time;  
time = DriverStation.getInstance().getMatchTime();`

Bu yöntem, geçerli periyotta \(auto, teleop, vb.\) Kalan süreyi saniye cinsinden döndürür. Bu zamanın FMS'den kaynaklandığını unutmayın, ancak çeşitli gecikmeler nedeniyle resmi bir zamanlayıcı değildir. Sürücü İstasyonunun Uygulama Eşleştirme işlevselliği, FMS'ye bağlandığında bu yöntemin davranışına yaklaşacaktır. DS'yi doğrudan Otonom veya Teleop modunda çalıştırmak, bu yönteme göre farklı davranacaktır.

### **Joystickler**

**USB Bağlantısı**

![](../.gitbook/assets/image%20%2859%29.png)

Kumanda Kolu, sürücü istasyonundaki mevcut USB bağlantı noktalarından birine bağlı olmalıdır. Başlatma rutini, joysticklerin pozisyonu ne olursa olsun orta pozisyonda olacak, bu yüzden istasyon açıldığında joysticklerin orta pozisyonlarında olması gerekir. Genel olarak, Sürücü İstasyonu yazılımı, cihazlar arasındaki sıralamayı korumayı deneyecektir, ancak cihazlarınızın hangi sırada olması gerektiğini not etmek iyi bir fikirdir ve Sürücü İstasyonu yazılımını her doğru başlattıklarında kontrol edin. Bu, USB Aygıtları sekmesini seçerek ve sol taraftaki USB Kurulum kutusunda siparişi görüntüleyerek yapılabilir. Bir joystick üzerindeki bir düğmeye basmak, tablodaki girdinin mavi yanmasına ve adından sonra yıldız işaretlerinin görünmesine neden olur. Joystickleri yeniden sıralamak için tıklayıp sürükleyin.

**Joystick Yenilemek**

Sürücü İstasyonu devre dışı modundayken, kumanda kolu cihazlarındaki durum değişikliklerini rutin olarak arar, takılı olmayan aygıtlar listeden çıkarılır ve yeni aygıtlar açılır ve eklenir. FMS'ye bağlanmadığında, bir joystiğin çıkarılması Sürücü Station'ı devre dışı moduna zorlayacaktır. Joystick'i kullanmaya başlamak için joystick'i tekrar yerine takın, doğru noktada görünüp görünmediğini kontrol edin, ardından robotu tekrar etkinleştirin. Sürücü İstasyonu etkin moddayken, yeni bir cihaz taramayacaktır çünkü bu zaman alıcı bir işlemdir ve bağlı cihazlardan gelen sinyallerin zamanında güncellenmesi önceliğe sahiptir.

Robot yarışmada Saha Yönetim Sistemine bağlandığında Sürücü İstasyonu modu FMS tarafından belirlenir. Bu, robotunuzu devre dışı bırakamayacağınız anlamına gelir ve DS, joystick değişikliklerini algılamak için kendini devre dışı bırakamaz. Joysticklerin manuel olarak tamamen yenilenmesi, klavyedeki F1 tuşuna basılarak başlatılabilir. Bunun tüm cihazları kapatıp tekrar açacağını ve tüm cihazların yukarıda belirtildiği gibi orta konumlarında olması gerektiğini unutmayın.

**Bir Joystick Nesnesi Oluşturmak**

\*\*\*\*

```text
C++

Joystick *exampleStick

public:
     Robot(){
     }
     void RobotInit() {
          exampleStick = new Joystick(1);
     }

```

```text
Java

exampleStick = new Joystick(1);
```

Joystick sınıfı için birincil oluşturucu, Joystick'in port numarasını temsil eden tek bir parametre alır. Bu, Driver Station yazılımının Joystick Setup kutusundaki joystick'in yanındaki \(1-6\) rakamıdır \(ilk resimde gösterilir\). Ayrıca, eksenlerin ve düğmelerin sayısının ek parametrelerini alan bir oluşturucu vardır ve belirli aygıtlarla kullanılacak Joystick alt sınıflarını oluşturmak için get ve set axis kanalı yöntemleri ile kullanılabilir.

**Joystick Değerlerine Erişme - Seçenek 1**

\*\*\*\*

```text
C++

double value;
value = exampleStick->GetX();
value = exampleStick->GetY();
value = exampleStick->GetZ();
value = exampleStick->GetThrottle();
value = exampleStick->GetTwist();

boolean buttonValue;
buttonValue = exampleStick->GetTop();
buttonValue = exampleStick->GetTrigger();

```

```text
Java

double value;
value = exampleStick.getX();
value = exampleStick.getY();
value = exampleStick.getZ();
value = exampleStick.getThrottle();
value = exampleStick.getTwist();

boolean buttonValue;
buttonValue = exampleStick.getTop();
buttonValue = exampleStick.getTrigger();
```

Bir joystick nesnesinin geçerli değerlerine erişmenin iki yolu vardır. İlk yol, adlandırılmış erişim yöntemleri veya getAxis yöntemini kullanmaktır. Joystick sınıfı, bu yöntemlerin KOP joystick için joystickin uygun eksenlerine varsayılan haritalamasını içerir. Başka bir cihaz kullanıyorsanız, bu yöntemleri kullanmak için uygun eşlemeleri ayarlamak için Joystick alt sınıfını kullanabilir ve setAxisChannel yöntemini kullanabilirsiniz. Diğer eksenlere veya düğmelere erişmeniz gerekiyorsa, 6 olası eksenden 5'i ve olası on iki düğmeden yalnızca biri için yalnızca adlandırılmış erişimci yöntemleri olduğunu unutmayın,

Joystick eksenleri 1, -1 aralığında bir ölçeklenmiş değer döndürür ve düğmeler tetiklenen durumlarını gösteren bir boole değeri döndürür. Joystick ve "joystick" ler için tipik kuralın, "Joystick" in "ileri", "Joystick" ve "Joystick" 'in sağa doğru itildiğinden dolayı X'in pozitif olması nedeniyle negatif olması gerektiğine dikkat edin. Belirli bir cihaz için bunu kontrol etmek için, "Joystick Mapping'i Belirleme" konusundaki aşağıdaki bölüme bakın.

#### Joystick Değerlerine Erişme - Seçenek 2



```text
C++

double value;
value = exampleStick->GetRawAxis(2);

boolean buttonValue;
buttonValue = exampleStick->GetRawButton(1);

```

```text
Java

double value;
value = exampleStick.getRawAxis(1);

boolean buttonValue;
buttonValue = exampleStick.getRawButton(2);
```

Joystick değerlerine erişmenin ikinci yolu getRawAxis \(\) ve getRawButton \(\) yöntemlerini kullanmaktır. Bu yöntemler, bir parametre olarak ekseni veya düğme numarasını temsil eden bir tam sayı alır ve karşılık gelen değeri döndürür. Cihazınızın fiziksel eksenleri ve düğmeleri ile uygun kanal numarası arasındaki eşleşmeyi belirlemeye yarayan bir yöntem için aşağıdaki "Joystick Eşleştirmesini Belirleme" bölümüne bakın.

**Polar Yöntemler**

\*\*\*\*

```text
C++

double value;
value = exampleStick->GetDirectionDegrees();
value = exampleStick->GetDirectionRadians();
value = exampleStick->GetMagnitude();

```

```text
Java

double value;
value = exampleStick.getDirectionDegrees();
value = exampleStick.getDirectionRadians();
value = exampleStick.getMagnitude();
```

Joystick sınıfı ayrıca joystick girişini bir polar koordinat sistemine dönüştürmek için yardımcı yöntemler içerir. Bu yöntemlerin düzgün çalışması için getX ve getY'nin doğru ekseni döndürmesi gerekir \(gerekirse setChannel \(\) ile yeniden eşleştirin\).

#### Joystick Eşleştirmesini Belirleme

![](../.gitbook/assets/image%20%2870%29.png)

2015 FRC Sürücü İstasyonunda, eksen düğmeleri ve fiziksel joystick özellikleri ile eksen veya düğme numaraları arasındaki eşleşmeyi belirlemek için kullanılabilecek POV değerlerinin göstergeleri bulunur. Seçmek için listede joystick'i tıklamanız yeterlidir ve göstergeler joystick girişine cevap vermeye başlayacaktır.

### DS'de Gösterge - Gösterge Tablosuna Genel Bakış

Çoğu zaman robottan geri sürücülere geri bildirim almak istenir. Robot ve sürücü istasyonu arasındaki iletişim protokolü, programa özel veri göndermek için gerekli hükümleri içerir. Verileri alan sürücü istasyonundaki programa ****dashboard denir.

#### Network Tables - Nedir?

Ağ Tabloları, FRC'deki yazılımlar arasında değişkenleri paylaşmak için kullanılan istemci-sunucu protokolünün adıdır. Robot, Network Tables sunucusu olarak hareket eder ve iletişim kurmak isteyen yazılım istemciler olarak bağlanır. En yaygın Ağ Tabloları istemcisi dashboarddır.

#### Smart Dashboard

Smart Dashboard, ilk olarak 2011'de piyasaya sürülen Java kontrol paneli istemcisine atıfta bulunuldu. Bu istemci, göstergelerin otomatik olarak robot tarafında Ağ Tablolarına girilen verilerle eşleşmesi için Ağ Tabloları protokolünü kullandı. O zamandan bu yana, LabVIEW panosu Ağ Tabloları'nı kullanmaya da dönüştürdüğü için terim biraz bulanıklaştı. SmartDashboard hakkında ek bilgi SmartDashboard Kılavuzunda bulunabilir.

LabVIEW Dashboard hakkında, C ++ veya Java kodlu LabVIEW Dashboard'u kullanma hakkında bir makale de dahil olmak üzere, FRC Driver Station kılavuzunda bulunabilir.

