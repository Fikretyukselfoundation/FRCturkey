# FRC için farklı IDE kullanmak

IDE (Integrated Developement Environment): Bilgisayar programcılarının yazılım geliştirmesi için uygun şartlar sağlayan, içindeki araçlar ile yazılımınızın hızlıca gelişmesine yardımcı olan programlardır.

Şu anda FIRST'ün FRC için desteklediği IDE Visual Studio Code. Peki neden başka IDE kullanmak isteyelim? Bunun birden fazla nedeni olabilir.
* Performans sıkıntıları yaşıyor olabilirsiniz.
* Diğer IDE'lerin bulundurduğu araç Visual Studio Code'da bulunmayabilir.
* Başka bir IDE kullanmaya alışmış olabilirsiniz.

### **Peki biz IDE'mizi değiştirmek için ne yapmalıyız?**

**Eğer var olan bir projeniz yoksa**

Projeniz yoksa ilk işiniz proje temel dosyalarını indirmek olacaktır. [GradleRIO](https://github.com/wpilibsuite/GradleRIO/releases) adresine giderek buradan en üstteki sürümden `java.zip`'i indiriyoruz. 
![gradlerio](https://i.hizliresim.com/JZ3dGB.png)
Ardından indirdiğimiz sıkıştırılmış klasördeki tüm dosyaları yeni açtığımız proje klasörüne çıkartıyoruz. Çıkarttıktan sonra proje dosyalarından **build.gradle**'da ufak ayarlar yapmamız gerekiyor. `plugins {}` bloğunda bulunan `id "edu.wpi.first.GradleRIO" version "xxxx.xx.xx"` bölümündeki `xxxx.xx.xx` ile belirtilen versiyonu [Gradle - GradleRIO Plugin](https://plugins.gradle.org/plugin/edu.wpi.first.GradleRIO) sitesinden bakarak en son sürüm ile güncelleyebilirsiniz.
![Gradle - GradleRIO Plugin](https://i.hizliresim.com/9aGvPk.png)
En sonunda böyle bir satır elde etmeniz gerekiyor.
```
id "edu.wpi.first.GradleRIO" version "2019.1.1"
```

Eğer kodunuzda sınıfınızın paket ismini değiştirecekseniz `def ROBOT_MAIN_CLASS = "frc.team0000.robot.Main"` satındaki sınıfı kendinize göre düzenleyebilirsiniz. Daha sonra proje klasöründen **.wpilib/wpilib_prefences.json** dosyasını düzenliyoruz.
```
{
  "enableCppIntellisense": false,
  "currentLanguage": "java",
  "projectYear": "2019", //yarışma senesi
  "teamNumber": 6874 //takım numarası
}
```

En sonunda ise projenin ana klasöründe komut istemcisi açıp `gradlew` komutunu çalıştırabiliriz.


**Eğer var olan bir Visual Studio Code projeniz varsa**

O zaman sizin için proje dosyaları Visual Studio Code tarafından oluşturulmuştur.

### **Peki projemizi başka IDE'lere nasıl taşıyacağız?**

**Projemizi Eclipse ya da IntelliJ IDEA'da kullanmak için** proje klasörümüzün içindeki **build.gradle** dosyasını açarak `plugins {}` bloğunu istediğiniz IDE'yi ekleyerek güncellemelisiniz.
```
plugins {
    //...
    id 'idea' //Eğer IntelliJ IDEA kullanacaksanız
    id 'eclipse' //Eğer Eclipse kullanacaksanız
}
```

Daha sonra proje klasörümüzde komut istemcisini açıp eğer IntelliJ IDEA kullanacaksak `gradlew idea`, eğer Eclipse kullanacaksak `gradlew eclipse` yazarak klasörünüzün içinde IDE Proje dosyalarınızın oluştuğunu göreceksiniz. Eğer IntelliJ IDEA ya da Eclipse'den başka IDE kullanmak istiyorsanız ne yazık ki onların Gradle için plugin desteği bulunmamakta, fakat bahsedeceğim komutlar ile kendi IDE'nizi de kullanabilirsiniz!

### **GradleRIO Komutları**

Bu komutları proje ana klasörünüzde komut istemcisi ile çalıştırırsanız aşağıdaki sonuçları elde edersiniz.

Genel Komutlar

* `gradlew build` komutu robotunuzun kodunu derler.
* `gradlew deploy` komutu robotunuzun kodunu derleyip o kodu robota yükler.
* `gradlew riolog` komutu RoboRIO konsolunuzun komut ekranında gösterilmesini sağlar.

**Yarışmada mısınız? İnternete bağlı değil misiniz?** Komutu `--offline` parametresi ile çalıştırın. örn. `gradlew deploy --offline`

### **3. Parti kütüphaneleri kurmak**
Proje ana klasörümüz içinde **vendordeps** adlı bir klasör açıyoruz. İçine kurmak istediğimiz 3. Parti kütüphanenin .json dosyasını kaydediyoruz.

Örnek olarak CTR-E Phoenix: **C:\Users\Public\frc2019\vendordeps** içinde Phoenix.json olarak bulunur.

### **WPILib kütüphanesini güncellemek**

Güncelleme yapmak için en başta Gradle sürümünü güncellemeniz gerekiyor. **build.gradle** dosyasının aşağısındaki gradleVersion değerini Gradle sürümüne göre değiştirebilirsiniz. Değiştirdikten sonra proje ana klasöründe `gradlew wrapper` komutunu çalıştırın.
```
task wrapper(type: Wrapper) {
    gradleVersion = '5.0'
}
```

Daha sonrasında `plugins {}` bloğunda bulunan `id "edu.wpi.first.GradleRIO" version "xxxx.xx.xx"` bölümündeki `xxxx.xx.xx` ile belirtilen versiyonu [Gradle - GradleRIO Plugin](https://plugins.gradle.org/plugin/edu.wpi.first.GradleRIO) sitesinden bakarak en son sürüm ile güncellemeniz gerekiyor.
![Gradle - GradleRIO Plugin](https://i.hizliresim.com/9aGvPk.png)
En sonunda güncellediğiniz sürüme göre bu şekilde bir satır elde etmeniz gerekiyor.
```
id "edu.wpi.first.GradleRIO" version "2019.1.1"
```
Daha sonrasında rahatça yazılımınızı güncel bir şekilde kullanabilirsiniz.
