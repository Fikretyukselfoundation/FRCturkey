# CAN cihazlarının kullanımı

### CAN alt sistemini RoboRIO ile kullanma

RoboRIO ile CAN kullanılması, robot kontrol cihazı ve çevre cihazları arasında önceki bağlantı yöntemlerine göre birçok avantaj sunar.

CAN bağlantıları, tüm cihazlar arasında papatya şeklinde zincirlenmiş tek bir telden geçmektedir, bu nedenle ev kablolaması gerekli değildir. Bu protokol tabanlı sinyalizasyon olduğundan cihazlar akıllı olabilir ve başlatma, durdurma ve hızı ayarlama dışında daha yüksek seviyeli komutları kabul edebilir. Cihazlar durumu robot kontrol cihazına rapor edebilir ve CAN kullanan cihazları kullanarak çok daha iyi kontrol algoritmalarına sahip olabilir. FRC kontrol sisteminde desteklenen çeşitli CAN cihazları vardır:

1. Jaguar hız kontrolörleri 
2. CAN-Talon hız kontrolörleri 
3. Güç Dağıtım Panosu \(PDP\) 
4. Pnömatik Kontrol Modülü \(PCM\)

Cihazlar tipik olarak, bükülmüş çift kablo kullanarak \(modüler telefon tarzı konektörler kullanan Jaguar hariç\) RoboRIO CAN veriyoluna bağlanır.

### Jaguar nesnesi oluşturma

Her fiziksel Jaguar, C ++ veya Java'da karşılık gelen bir Jaguar nesnesine sahiptir. Jaguar nesneleri, aşağıdaki gibi her iki dilde yeni operatör kullanılarak oluşturulur:

```java
m_jaguar = new CANJaguar(CANJaguarIDValue);
```

CANJaguarIDValue değeri, gerçeklenmekte olan cihaz için RoboRIO'nun web arayüzünde programlanmış CAN ID'dir. En son ürün yazılımı sürümünün Jaguar'da yüklü olduğundan emin olun. Jaguar örneğini oluşturduktan sonra çalışma modu ayarlanmalı ve çalışma modunu gösterildiği gibi ayarlamak için enableControl \(\) yöntemi çağrılmalıdır:

```cpp
m_jaguar->SetPercentMode(CANJaguar::QuadEncoder, 360);
	m_jaguar->EnableControl();
	m_jaguar->Set(0.0f);
```

Kontrol modu, programa çalışma moduna özgü diğer parametreleri ayarlama şansı vermek için EnableControl \(\) çağrılana kadar ayarlanmaz. Bu örnekte Jaguar referans sensörü olarak bir kuadratür kodlayıcı ile Yüzde Moduna ayarlanmıştır. SetPercentMode \(\) yöntemindeki enkoderin belirlenmesi, programın gösterilen enkoderler ile ilgili konum hakkında geri bildirim almasına izin verecektir:

```cpp
	double initialPosition = m_jaguar->GetPosition();
```

Sınırları kullanma Jaguar ile kullanılabilecek iki tür sınır vardır:



1. Yumuşak limitler - referans cihaz değerleri Jaguar'ın minimum veya maksimum sayıda enkoder dönüşü gibi her iki yönde de geçmeyeceği değerlerdir. 
2. Limit anahtarları - bir mekanizmanın maksimum hareketini kontrol eden anahtarlar

Yumuşak sınırlar etkinleştirilip devre dışı bırakılabilir, limit anahtarı kontrolü daima tüm Jaguar modlarında \(PWM dahil\) çalışır. Limit anahtarları kullanılmazsa, Jaguar'ın düzgün bir şekilde çalışmasını sağlamak için jumperların takılması gerekir. Jumperların kullanımı hakkında ayrıntılı bilgi için Jaguar belgelerine bakın.

#### Jaguar'da kapalı döngü kontrolünü kullanma

Kapalı çevrim kontrolü, Jaguar'ın sensör girişlerine bakması, bir hata değeri \(sensör değeri ve ayar noktası arasındaki fark\) hesaplaması, bazı aktüatörleri \(motorları\) ve döngüleri çalıştırması anlamına gelir. Jaguarlar, hız, Pozisyon ve Akım çalışma modları için cihaz içinde bu algoritmayı gerçekleştirebilir. Kapalı döngü geri bildirimini kullanmak için aşağıdakileri yapmanız gerekir:

1. SetMode \(\) yöntemini kullanarak istenen modu ayarlayın
2. Doğru ayar modu yönteminde P, I ve D sabitlerini sağlayın
3. Operasyon için uygun bir sensör sağlayın
4. ve denetleyicinin çalışmasını başlatmak için EnableControl \(\) öğesini çağırın.

İlk üç işlem tipik olarak SetMode \(\) yöntemi ile yapılır - sensör ve P, I ve D sabitleriyle birlikte verilir.

#### Voltaj kontrolü kullanma

Çıkış voltajı, sistem bara voltajının yüzdesi veya yukarıda açıklandığı gibi mutlak bir voltaj olabilir. Jaguar'ı Gerilim modunda ayarlamak için aşağıdaki yöntemi kullanın:



```cpp
		m_jaguar->SetVoltageMode();
```

ve Yüzde Voltaj moduna ayarlamak için bu yöntemi kullanın:

```cpp
m_jaguar->SetPercentMode();
```

Her iki durumda da burada gösterildiği gibi bir referans cihaz belirtilebilir:

```cpp
	m_jaguar->SetPercentMode(CANJaguar::QuadEncoder, 360);
```

Bu durumda, bir CPI \(counts per revolution\)\(devir başına sayım\) değeri ile referans cihaz olarak bir kuadratür enkoder seçilir. Enkoderin değerini okuyabilir, fakat cihazı kontrol etmek için kullanılmaz - sadece bağlı bir sensör olarak ele alınır. 

#### Pozisyon kontrolünü kullanma

Pozisyon kontrolü, motoru hassas konumlara getirmek için PID sabitleri ile birlikte bir potansiyometre veya kareleme enkoder kullanabilir. Konum modunu ayarlarken, sensörü \(bu durumda bir kuadratür enkoder\), devir başına sayım sayısını ve dahili PID geri besleme döngüsü için P, I ve D sabitlerini belirtmeniz gerekir.

```cpp
m_jaguar->SetPositionMode(CANJaguar::QuadEncoder, 360, 10.0f, 0.4f, 0.2f);
	m_jaguar->EnableControl();
	double setpoint = m_jaguar->GetPosition() + 10.0f;
	m_jaguar->Set(setpoint);
```

Bu program Jaguar'ı konum moduna getirir ve motoru enkoder 10'a yönlendirir. Unutmayın ki, bu kodlayıcı bir transmisyonun bir ara dişli üzerine monte edilebildiğinden, motor mili rotasyonları zorunlu değildir.

#### Hız kontrolünü kullanma

Jaguar'ı gösterildiği gibi SetMode yöntemini çağırarak hız moduna geçirin:

```cpp
setSpeedMode(EncoderTag tag, int codesPerRev, double p, double i, double d)
```

EncoderTag şunlardan biridir: kEncoder, kQuadEncoder veya kPotentiometer. CodesPerRev argümanı, kodlayıcının kullandığı devir başına sayım sayısıdır.

#### Jaguar dan durumunu alma

Burada gösterildiği gibi çeşitli durum değerleri Jaguarlardan okunabilir:

```cpp
m_jaguar->GetBusVoltage();
	m_jaguar->GetOutputVoltage();
	m_jaguar->GetOutputCurrent();
	m_jaguar->GetTemperature();
	
	m_jaguar->GetFaults();
```

Bu yöntemler Jaguar'dan çeşitli çalışma değerlerini döndürür. Son yöntem, GetFaults \(\), çeşitli türlerde hatalar olup olmadığını açıklayan bireysel bitlerle bir bit alanı döndürür. Hataların ve diğer durum yöntemlerinin tam açıklaması için, üstbilgi dosyasına veya seçtiğiniz dile ait JavaDocs'a başvurunuz.

Jaguar çalışırken, bu servisi talep etmek zorunda kalmadan, her 20 ms'de bir durum değerlerini RoboRIO'ya geri göndermeye ayarlanır. Bu, bu yöntemlerden okuduğunuz durumun, son iletiler kümesinin ne zaman alındığına bağlı olarak 20 ms veya daha eski olabileceği anlamına gelir.

### Pnömatik Kontrol Modülü

#### Kompresörü Kontrol Etmek

Basınç şalteri ve kompresör doğru şekilde bağlandığında, PCM kompresörün kapalı döngü kontrolünü dahili olarak idare eder. Bu kontrolü etkinleştirmek için, gereken tek şey bir solenoid nesnesi ve robotun Etkinleştirilmesidir. Daha fazla bilgi için, [bkz. Pnömatik için kompresör çalıştırma.](http://wpilib.screenstepslive.com/s/currentCS/m/cpp/l/241865-operating-a-compressor-for-pneumatics)

#### Solenoidlerin Kullanımı

RoboRIO için WPILib Solenoid sınıfı artık PCM kullanılarak selenoidleri uygulayan biri ile değiştirilmiştir. Aynı yöntemler şimdi PCM'deki Solenoid portlarına takılan pnömatikleri kontrol edecektir, böylece kodunuz çoğunlukla değişmeden çalışmalıdır. Daha fazla bilgi için, [bkz. Çalışma pnömatik silindirleri - Solenoidler](http://wpilib.screenstepslive.com/s/currentCS/m/cpp/l/241866-operating-pneumatic-cylinders-solenoids)



### Power Distribution Panel

2015 için Güç Dağıtım Panosu \(PDP\), devre korumalı 12V çıkışlardan herhangi birine bağlı olan her bir cihaza akımı ölçebilmeyi sağlar. Bu kabiliyete sahip olmak, ek donanım gerektirmeden motorlar tarafından geliştirilen torkun algılanmasını gerektiren bir dizi algoritma kullanma imkanı sunar. PDP, CAN veriyolu üzerinden RoboRIO'ya bağlanır ve kütüphaneler iletişimi yönetmeye özen gösterir.

Kullanmak için PowerDistributionPanel nesnesinin bir örneğini oluşturun:

PowerDistributionPanel pdp = yeni PowerDistributionPanel \(\);

Not: Değerleri okumanız gerekmedikçe bir PowerDistributionPanel nesnesi oluşturmak gerekli değildir. Nesne hiç oluşturulmasa bile kart tüm kanallarda çalışır ve güç sağlar.

#### PDP CAN ID

C ++ ve Java WPILib'in mevcut sürümleriyle çalışmak için PDP için CAN ID 0 olmalıdır.

#### PDP voltajını ve sıcaklığını okumak

PDP'ye gelen voltajı ve PDP'deki bileşenlerin sıcaklığını okuyabilirsiniz. Motorların yüksek bir tork ayarında çalışıp, sistem akü voltajının düşmesine neden olduğunda, voltajın ölçülmesi önemli olabilir.

#### PDP'deki kanal başına akımı okumak

PowerDistributionPanel nesnesini kullanarak PDP'nin bireysel kanallarındaki akımı okuyabilirsiniz. Kanal 1'deki akımı okumak için getCurrent yöntemini kullanın:

`double current = pdp.getCurrent(1);`

Bu, amper cinsinden geçerli değeri döndürür.

Not: Kanal numaraları 0 tabanlıdır.

