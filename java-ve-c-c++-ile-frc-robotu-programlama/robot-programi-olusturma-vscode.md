# Robot Programı Oluşturma VSCODE

Bu makalede, Visual Studio Kodunda yeni bir WPILib projesi oluşturacağız. Bu örnekte bir TimedRobot yapacağız, ancak aynı yöntemler mevcut şablonlardan veya örneklerden herhangi birinden proje oluşturmak için de geçerlidir.

#### Command Palete Erişim

Ctrl + ÜstKrkt + P'ye tıklamak komut paletini açacaktır. Komut paleti, projeler oluşturmak ve etkileşimde bulunmak için WPILib komutlarını içerir.

![](../.gitbook/assets/image%20%28117%29.png)

#### WPILib Komutlarına Erişim

Tüm WPILib komutları "WPILib:" ile başlar, bu nedenle WPILib komutlarına erişmek için Command Palete arama çubuğuna "WPILib:" yazın.

![](../.gitbook/assets/image%20%2822%29.png)

#### Yeni Bir WPILib Projesi Oluşturmak

Yeni bir proje oluşturmak için "Yeni bir proje oluştur" komutunu seçin. Bu, yeni projenizi oluşturmak için gereken bilgileri girdiğiniz çeşitli alanların bulunduğu bir form gösterecektir.

![](../.gitbook/assets/image%20%2819%29.png)

#### Yeni proje yaratma penceresi

![](../.gitbook/assets/image%20%2818%29.png)

Yeni proje oluşturma adımları burada ana hatlarıyla belirtilmiştir:  
  


1. Oluşturmak istediğiniz projenin türünü seçin. Örnek bir proje veya WPILib tarafından sağlanan şablon projelerden biri olabilir.
2. Projeniz için kullandığınız dili seçin.
3. Bir şablonda - şablon tipini seçin \(Zamanlı robot, İteratif robot, Komut robotu, vb.\)
4. Projenin yerleştirileceği klasörü seçin.
5. "Yeni klasör oluştur" onay kutusu işaretlenirse, verilen klasörde proje adı ile yeni bir klasör oluşturulur. Onay kutusu işaretli değilse, sağlanan klasörün boş olduğu varsayılır \(değilse bir hata verir\) ve proje dosyaları bu dizine yerleştirilir.
6. Proje adı projede ve ayrıca isteğe bağlı olarak önceki adımdaki onay kutusu işaretliyse yerleştirilecek klasörü oluşturmak için kullanılır.
7. Projenin takım numarası. Bu, paket adları için ve kodu dağıtırken robotunuzu bulmak için kullanılacaktır.

son olarak, "Generate Projectr" i tıklayın ve VS Kodu projeyi belirtilen yerde oluşturacaktır.

![](../.gitbook/assets/image%20%286%29.png)

#### Yeni Projenin Açılması

Projenizi başarıyla oluşturduktan sonra, Visual Studio Code size aşağıda gösterildiği gibi projeyi açma seçeneği sunacaktır. Bunu şimdi veya daha sonra Ctrl-O \(mac'ta Command + O\) yazıp projenizi kaydettiğiniz klasörü seçerek yapabilirsiniz.

Açıldıktan sonra solda proje hiyerarşisini göreceksiniz. Dosyaya çift tıklamak bu dosyayı editörde açacaktır.

![](../.gitbook/assets/image%20%2811%29.png)

#### C ++ Konfigürasyonları \(Sadece C ++\)

![](../.gitbook/assets/image%20%28123%29.png)

C ++ projeleri için IntelliSense'i kurmak için bir adım daha var. Bir projeyi her açtığınızda, C ++ yapılandırmalarını yenilemek isteyen sağ alt köşede bir açılır pencere açmalısınız, IntelliSense'i ayarlamak için Evet'i tıklatın.

#### Robot Kodu Oluşturma ve Deploy etme

Robot projesini oluşturmak için şunlardan birini yapın:  
  


1. Komut Paletini açın ve "Robot Kodu Oluştur" u seçin.
2.  VS Kodu penceresinin sağ üst köşesindeki üç noktalarla gösterilen kısayol menüsünü açın ve "Robot Kodu Oluştur" u seçin.
3.  Proje hiyerarşisindeki build.gradle dosyasına sağ tıklayın ve "Robot Kodu Oluştur" u seçin.

![](../.gitbook/assets/image%20%2866%29.png)

Önceki talimatlardaki üç konumdan herhangi birinde "Deploy Robot Code" u seçerek robot kodunu deploy edin. Bu kodu deploy edecek ve robot programını roboRIO'ya yerleştirecektir. Başarılı olursa, başarılı bir "Oluşturma" mesajı \(2\) göreceksiniz ve RioLog çalışırken robot programından konsol çıkacaktır.

![](../.gitbook/assets/image%20%28116%29.png)

