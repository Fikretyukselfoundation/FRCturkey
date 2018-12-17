# Yapacağınız iş için doğru sensörü seçmek

Bunu takip eden makaleler WPILib ile çeşitli sensörlerin kullanımı ve kullanımı hakkında ayrıntılı bilgi vermektedir, ancak belirli bir görev için hangi sensörün kullanılacağını nasıl öğrenebilirsiniz? Bu makale, çeşitli ortak FRC görevleri için olası sensör seçeneklerini açıklamaya çalışır.

###  Bir mekanizmanın bir veya iki konumunu tespit etme

Motorlu bir mekanizma için bir veya iki konumun tespit edilmesi çok yaygın bir FRC görevidir. En sık rastlanan olay, bir mekanizmanın her iki uçta da bir limite ulaştığında tespit edilmesidir, ancak istenen pozisyonu veya ev konumunu saptamak da temelde aynı görevdir.

**Limit Anahtarları**  
Mekanik limit anahtarları bu senaryo için en yaygın çözümlerden biridir. Anahtarlar gerçekten mekanizmanın sınırlarını belirliyorsa, anahtarların mekanizma tarafından gözden kaçamayacakları ve mekanizma tarafından hasar görmeyecekleri bir konumda ayarlandığından emin olunuz. Limit şalterler kullanışlıdır çünkü çok basittirler, oldukça ucuzdurlar ve çok çeşitli durumlarda kullanılabilirler.

### Bir mekanizmanın konumunu birçok farklı noktada veya limit olmayan noktalarda tespit etmek

Bazen asansörün ne kadar yüksek olduğunu bilmeniz gerekir, bu yükseklik asansörünüzün en yüksek noktası olmaksızın veya bir kolun başlangıç konumundan ne kadar yüksek olduğu veya atıcı kafanızın hangi açı ile işaret edildiğini bilmeniz gerekir. Bu problemler, bazı zorlu limit switch yerleşimleri ve onlar için yakalanan akıllı bir ekip tarafından çözülebilirdi, ancak bu iş için tasarlanan başka sensörler de var.

**Ultrasonik Sensörler**

Ultrasonik sensörler gibi mesafe sensörleri, görüş alanındaki en yakın nesnenin sensörden ne kadar uzakta olduğunun oldukça doğru bir ölçümünü verebilir, yani eğer doğru şekilde ayarladıysanız, kolunuzu veya asansörünüzü ne zaman durdurabilirsiniz? arzu ettiğiniz noktalara gelir. Genellikle bu ölçümler 2-3 inç'e kadar doğru olacaktır, yani çok daha fazla doğruluğa ihtiyacınız varsa, bu bölümde farklı bir sensöre bakmak isteyebilirsiniz, ancak bu çoğu vaka için yeterince iyi bir yöntemdir.

**Kızılötesi Mesafe Sensörleri**  
Kızılötesi mesafe sensörleri ultrasonik sensörlere çok benziyor, sadece farklı bir ölçüm yöntemi kullanıyorlar. Bu sensörler öğreticilerimizde açık bir şekilde ele alınmamıştır, ancak bu sensörlerin çoğunun bir analog çıkışı vardır, yani çoğu durumda belirli bir mesafeye dönüşen sensörden bir voltaj elde etmek için analog giriş sınıfını kullanabilirsiniz. Ultrasonik sensörlerle karşılaştırıldığında kızılötesi sensörlerin avantajı, gürültülü bir stadyumdan etkilenmemeleridir, bazı durumlarda bir ultrasonik sensör, bir rekabette ne kadar gürültü olduğu için daha az doğru bir değer elde edebilir.

#### Counters ve Enkoderler

Kolunuz veya asansörünüz bir motor tarafından sürülürse, kolun veya yükselticinin ne kadar yüksek olduğunu bulmak için motorun döndüğü dönüş sayısını ölçebilirsiniz. Bu yöntem bilinen bir yükseklik ölçümü kontrolünden daha fazla bir tahmin ve kontrol yöntemidir, ancak birkaç test ve bazı baskı satırı deyimleriyle, belirli yüksekliklere ulaşmak için gereken dönüş sayısını kolayca bulabilirsiniz, sadece ölçümünüzü unutmayın. her zaman bir şeye dayanır ve eğer sabit kodlanmış rotasyon sayınız zemine dayanırsa ve hava biraz bir maçla başlarsa, tepeye çıktığı zaman yanlış ayar noktasında olacaktır. Başlangıçta nasıl ayarlanacağını bilmek önemlidir.

**Potansiyometre**

Kolun açısını bilmeniz gerekiyorsa, potansiyometre sizin için bir iş olacaktır, hareket açısını okunabilir analog değere dönüştürür. Bu, nişancınızın nereye vurulduğunu veya kolunuzun ne kadar yüksek olduğunu bilmek için çok iyi çalışır ve bazı sensörler ile de doğrusal olarak kat edilen mesafeyi ölçebilirsiniz, böylece bir asansörle çalışabilir.

**Akselerometre**

Bir yüzeyin eğimini ölçmek, bir ivme ölçer ile mümkün olabilir; bu, açıyı belirlemek için iyi bir fikir verecek bir kol için, eğer ölçmek istediğiniz buysa. Bunlar, robotun bundan daha fazla gitmemesi ya da muhtemelen kendisini kırması gibi nedenlerle tuttuğunuz sınırlar için gerçekten yararlıdır ve bazen kesin hedefleme için yeterince doğru değildir.

### **Düz Sürüş**

Bazen robotunuz, robot yönündeki küçük sapmaları kolayca düzeltebilen bir insan tarafından kontrol edilmemektedir. Kendinize doğru sürmek için robotunuza ihtiyacınız olduğunda, işi yapmak için çalışacak bir çift sensörünüz var.

#### Jiroskop\(Gyro\)

Jiroskop, bir yöne işaret eden bir sensördür ve bu yönden saptığında ve ne kadar uzak olduğunuzda size söyleyecektir. Bu, tahrik motorlarından birinin diğerinden biraz daha yavaş olmasını veya otonom olduğumuzda ne kadar uzandığımızı doğru bir şekilde ölçmemize yardımcı olabilir. Ayrıca bir başlangıç noktasından da ölçüm yaparlar, bu yüzden robot yanlış yere konursa, bunu bilmeyecektir.

#### Enkoder

Tahrik motorlarında enkoderler varsa, tekerleklerin ne kadar dönmüş olduğunu ölçebilir ve bunlardan biri diğerinden daha fazla ölçüm yaparsa, bunu düzeltebilirsiniz. Bu, özellikle dönerken tekerleklerin kayması nedeniyle etkili değildir ve enkoderler bu ölçümler için jiroskoplar kadar doğru değildir.

#### Ne kadar uzağa gittim?  Enkoder

Enkoderler, bir motoru en son sıfırladığınızdan bu yana geçen dönüşlerin sayısını ölçer. Bu, farklı dişli ve kasnak oranları için matematik yaparak, robotunuz için mesafe hesaplamasına dönüşleri hesaplayabileceğiniz anlamına gelir. Bu, enkoderinizi koyduğunuz tekerleklerinizden biraz daha az hassaslaşır, çünkü kasnaklardaki gevşeklik mesafesini, kasnaklarda mandalları atlayan kayışları ve yüzeyde kaymasını engelleyen tekerlekler, böylece daha uzun bir mesafe verir. Gerçekten gittiğinden daha çok. Bu, ölçtükleri rotasyonların ortalaması alınarak birden fazla enkodere sahip olduğunuzda, bundan kaçınılır, böylece herhangi bir kayma daha iyi verilere sahip olacak şekilde azaltılır.

**Mesafe sensörleri**  
Robotu sahada kurmanın pratik kaygılarından dolayı çok yaygın olmamasına rağmen, mesafe sensörleri, ölçülecek bir noktanız varsa ne kadar ileri gittiğinizi söylemek için kullanılabilir. Bir alan elemanından veya duvardan ölçtüğünüz için, bir dönüşten sonra ne kadar ilerlediğinizi veya eğer statik bir nesneden çok uzaktaysa ne kadar ilerlediğinizi söylemek genellikle mümkün değildir.

### **Kamera ve Vision**

Çoğu zaman, bir problemin nasıl çözüleceğini düşündüğünüzde, kulak tıkaçları ile büyük yastıklı eldivenlerle gözü kapalı bir şekilde yapmaya çalışmayın. Bu, robotun hiçbir sensör olmadan dünyayı nasıl deneyimlediğidir. oyun parçasını göremiyorum, bu tüpü ne kadar sıkı tuttuğunu hissedemezsin, sensörler bu şeyleri algılayamazsa, robot işini doğru yapıp yapmadığını bilemez. İnsanların dünyamızdan bilgi almak için yaptıkları en önemli şeylerden biri, ona bakmak, konum ve mesafe gibi şeyleri yargılamak ve etrafımızdaki önemli şeylerin yerlerini belirlemek.

**Neden Vision Kullanmalı?**

Vision çok güçlü bir araçtır, bir şeyden ne kadar uzak olduğunuzu, önünüzde kaç öğenin bulunduğunuzu, nereye işaret ettiğinizi ve ne kadar hızlı hareket ettiğinizi, hepsi bir sensörden almanızı sağlar. Bir şeyin ne kadar uzakta olduğu gibi şeyler, kameranın görüş açısını, çözünürlüğü ve görünümde bilinen bir nesnenin boyutunu öğrenerek ölçülebilir. Maddelerin sayısının sayılması, nesne algılama ve tanıma meselesidir ve hareket, işlerin size ne kadar hızlı gittiğini ölçmektedir. Bunlar aynı zamanda kameraların görüntülemesini sürücüye aktarabilme yeteneğiyle de birleştirilebilir, böylece sürücü ve operatör camın arkasındaki alanın tamamı boyunca robotların perspektifinden görülebilir.

**Neden vision kullanmıyorsunuz?**

Vision kullanmak için birkaç şeye sahip olmanız gerekir: Kaliteli bir kamera, görsel bilgiyi anlamlı veriye dönüştürmenin bir yolu ve çok gelişmiş görsel tanımlamanın dışarıdaki kütüphaneler tarafından nasıl yapıldığını bilen veya istekli olan biri. Roborio ile kamera videosunu anlamlı veriye dönüştürmek için gerekli olan hesaplamaların bir kısmını ya da tamamını yapabilmemiz mümkündür ve kameralar parçaların setinde mevcuttur, ancak daha gelişmiş görsel işlemler için ek işlemlere ihtiyacınız olabilir. güç ve yüksek kaliteli kameralar. Çünkü bu takımların çoğu temel şeyler için vision kullanıyor ya da sadece sürücünün kullanabileceği daha fazla bilgi olarak kullanıyor.

### Diğer Sensörler ve Sorunlar

Sonuç olarak, robotlarındaki sensörler söz konusu olduğunda bireysel sorunlarına çözüm bulmak ekiplere kalmıştır. Bazen takımların kullanabileceği sensörler yeterince iyi değildir, enkoderler ihtiyacınız olan hızlarda okuyamaz, ultrasonik sensörler belli bir mesafeden sonra da yanlış olurlar, bunlar çözülmesi gereken zorluklardır ve gerçekten burada olmanızın sebebi de budur. Bu zorluklar, öğrencilere ve akıl hocalarına, takımların meydan okuma ve son teslim tarihlerinin önüne koyulduğunda ne kadar yaratıcı olabileceğini öğreten şeydir. Bu, sorunlara en iyi ve en yaratıcı çözümlerin bazılarının yaratıldığı zamandır.

