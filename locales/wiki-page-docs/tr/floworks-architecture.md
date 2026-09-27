---
title: FloWorks Mimarisi
description: Son kullanıcı için bileşenlerin ve iç işleyişin genel görünümü
---

# FloWorks Mimarisi – Kullanıcı için Genel Bakış

FloWorks, akış şemaları aracılığıyla sinyal işleme zincirleri oluşturmanıza olanak tanıyan bir masaüstü uygulamasıdır. Etkileşimli bir tuval üzerinde blokları (düğümleri) bağlayın ve sonuçları gerçek zamanlı olarak görün. Bunu mümkün kılmak için uygulama, birlikte çalışan birkaç modüle ayrılmıştır. Aşağıda teknik ayrıntılara girmeden, her bir parçanın ne yaptığı ve birbirleriyle nasıl ilişkili olduğu açıklanmaktadır.

---

## Genel yapı

Uygulama aşağıdaki işlevsel alanlardan oluşur:

| Alan | Ne işe yarar? |
|------|------------|
| **Başlangıç ve ana pencere** | Programı başlatır, pencereyi, menüleri görüntüler ve tüm kullanıcı eylemlerini koordine eder. |
| **Yürütme motoru** | Düğümlerin hangi sırayla çalıştırılacağını hesaplar, bağımlılıkları ve döngüleri algılar ve verileri düğümden düğüme iletir. |
| **Sahne ve diyagram** | Düğümleri yerleştirdiğiniz tuvali, bunlar arasındaki bağlantıları, yapışkan notları ve geri al/yinele eylemlerini yönetir. |
| **Düğümler ve işleme** | Kullanabileceğiniz tüm blok türlerini içerir: sinyal kaynakları, matematiksel işlemler, özel betikler, grafik dışa aktarımı vb. |
| **Görsel bağlayıcılar** | Düğümleri birbirine bağlayan çizgileri (yumuşak eğriler veya ortogonal yollar) çizer, veri akışını göstermek için bunları canlandırır ve çakışmayı önler. |
| **Kullanıcı arayüzü** | Diyagram görünümünü (yakınlaştırma, kaydırma), araç çubuğunu, parametre tablosunu, analiz panellerini (istatistikler, imleçler) ve yapılandırma iletişim kutularını içerir. |
| **Gerçek donanım desteği** | Gerçek sinyalleri yakalamak veya oluşturmak için laboratuvar cihazlarıyla (osiloskoplar, jeneratörler, LCR multimetreleri) iletişim kurmayı sağlar. |
| **Grafik dışa aktarımı** | Tam görsel özelleştirme ile yüksek kaliteli görseller (PNG, PDF, SVG) oluşturur. |
| **Temalar ve görünüm** | Uygulamanın tüm görünümünü (koyu, açık, yüksek kontrast) değiştirir ve yazı tipi boyutunu ayarlamanıza olanak tanır. |
| **Diller** | Tüm arayüzü birkaç dile çevirir ve dili anında değiştirmenizi sağlar. |
| **Proje yönetimi** | Tüm diyagramı, yapılandırmaları, betikleri ve sonuçları içeren `.sflow` dosyalarını kaydeder ve açar. |
| **Testler ve tanılama** | Her şeyin düzgün çalıştığını doğrulamak için dahili araçlar (son kullanıcıya görünmez). |

---

## İçeriden nasıl çalışır

### Başlangıç ve ana pencere
FloWorks'i açtığınızda grafik ortam yapılandırılır, ekranınızın piksel yoğunluğu algılanır (böylece 4K veya normal monitörlerde her şey keskin görünür) ve ana pencere görüntülenir. Bu pencere, çizim alanı, menüler, araç çubuğu ve yan paneller dahil tüm öğeleri merkezileştirir.

### Akış motoru
"Çalıştır"a bastığınızda (veya F5'e bastığınızda), dahili bir motor bağlantılara saygı göstererek düğümleri doğru sırayla işler. Hangi düğümlerin diğerlerine bağlı olduğunu bilir ve sonsuz döngüleri engeller. Bir düğümün birden fazla adlandırılmış girdiyi almasını ve birden fazla çıktı üretmesini destekler. Veriler düğümler arasında orijinal yapılarını koruyarak hareket eder.

### Diyagram sahnesi
Diyagramlarınızı oluşturduğunuz tuval akıllı bir sahnedir:

- Düğüm ekleme, taşıma, bağlama ve seçme olanağı sunar.
- Her işlem için sınırsız geri al/yinele desteği sağlar.
- Projeyle birlikte kaydedilen, istediğiniz yere yerleştirebileceğiniz yeniden boyutlandırılabilir yapışkan notlar içerir.
- Düğümleri düzenli bir şekilde yeniden yerleştiren otomatik bir düzenleyiciye sahiptir (Ctrl+Shift+L).
- Kaydedildiğinde, tüm diyagram düğüm tanımları, bağlantılar, notlar ve ilişkili sayısal verileri içeren bir `.sflow` dosyasına paketlenir.

### Bağlayıcılar
Düğümleri birleştiren çizgiler yumuşak eğriler veya ortogonal yollar olarak çizilir. Nokta veya tire animasyonu veri akış yönünü gösterir. Bir şerit yöneticisi, aynı düğümler arasındaki birden fazla bağlantının üst üste binmesini önler; bunları otomatik olarak ayırarak her şeyin okunabilir olmasını sağlar.

### Düğüm türleri
Düğümler temel yapı taşlarıdır. Üç kategoriye ayrılırlar:

- **Kaynaklar** – Sinyaller üretir. Dalgaları (sinüs, kare vb.) simüle edebilir veya bağlı bir osiloskop veya multimetreden gerçek verileri okuyabilir. Eş zamanlı çok kanallı destek sunar (örneğin, bir LCR'den empedans ve faz).
- **İşleme** – Verileri dönüştürür. Aritmetik işlemleri (toplama, çıkarma, çarpma, bölme), koşullu kararları (Evet/Hayır dallanması) ve kendi Python kodunuzu görsel yardımlarla yazmanıza olanak tanıyan güçlü bir betik düğümünü içerir.
- **Hedefler** – Sonuçları görüntüler veya dışa aktarır. En yaygın olanı grafik görüntüleyicisidir (sanal osiloskop), ancak profesyonel kalitede bir grafik dışa aktarıcı da mevcuttur.

Her düğümün giriş (sol/üst) ve çıkış (sağ/alt) portları vardır. Bir çıkış portunu bir giriş portuna bağladığınızda sinyal aralarında akar.

#### Gelişmiş betik düğümü
Betik düğümü özel bir bahsi hak eder. FloWorks'ten çıkmadan kendi işlemenizi eklemek isteyen ileri düzey kullanıcılar için tasarlanmıştır. Şunları sunar:

- Sözdizimi vurgulama, otomatik tamamlama ve satır numaraları içeren bir düzenleyici.
- Kodu dokunmadan düğüm panelinden düzenlenebilir parametreler tanımlama olanağı (örneğin, betikte kullanılan bir sayısal değer).
- Dinamik giriş ve çıkış portları: betiğe özel yorumlar ekleyerek yeni konektörler oluşturabilirsiniz.
- Kalıcı bellek: yürütmeler arasında değerini koruyan özel bir değişken (`persist`), biriktiriciler veya durum makineleri için kullanışlıdır.
- Hazır betik şablonları ve kendi şablonlarınızı kaydetme seçeneği.
- Entegre bir yardım sistemi ve çalışma zamanı hatalarını gösteren bir konsol.

### Kullanıcı arayüzü
Tuvalin yanı sıra arayüz şunları içerir:

- Düğümleri kategorilere göre düzenleyen, dil, tema ve yazı tipi boyutu menüleri ile günlük görüntüleyicisine erişim sağlayan bir **araç çubuğu**.
- Seçili düğümler hakkında bilgi gösteren ve olası uyumsuzlukları (örneğin, farklı uzunluktaki sinyallerle işlem yapmaya çalışmak) vurgulayan bir **parametre tablosu**.
- **Yerleştirilebilir analiz panelleri**: istatistikler (maksimum, minimum, etkin değer), fark ölçümü için A/B imleçleri ve tepe noktası işaretleyicili bir nişangah.
- Ekran çözünürlüğünüze uyum sağlayan ve başlangıç seçenekleri sunan bir **hoş geldiniz iletişim kutusu**.

### Gerçek cihazlarla bağlantı
Uyumlu donanıma sahipseniz (Siglent SDS osiloskoplar, LCR multimetreleri, SDG jeneratörleri), FloWorks standart VISA/SCPI protokolü üzerinden bunlarla iletişim kurabilir. Yapılandırma uygulama içindeki özel panellerden yapılır. Çok kanallı bir sinyal yakaladığınızda (örneğin, bir LCR'den genlik ve faz), kaynak düğümü tüm kanalları paketler ve basit bir bağlam menüsüyle hangisini görüntüleyeceğinizi seçebilirsiniz.

### Profesyonel grafik dışa aktarımı
Grafik dışa aktarıcı, raporlar veya yayınlar için hazır görseller üretmenizi sağlar. Üzerine çift tıkladığınızda birçok seçenek sunan bir iletişim kutusu açılır: renkleri, çizgi türlerini, etiketleri, ölçekleri özelleştirebilir, PNG, PDF veya SVG arasında seçim yapabilir ve tercihlerinizi yeniden kullanılabilir profiller olarak kaydedebilirsiniz.

### Görsel özelleştirme
FloWorks, uygulamanın tüm görünümünü yeniden başlatmaya gerek kalmadan anında değiştiren çeşitli temalar (koyu, açık, yüksek kontrast) içerir. Ayrıca menüden (Bilgi → Yazı tipi boyutu) genel yazı tipi boyutunu ayarlayabilirsiniz; düğüm içi metinler, yapışkan notlar ve grafikler dahil tüm öğeler buna göre yeniden boyutlandırılır.

### Dil sistemi
Uygulama ilk açılışta sistem dilinizi otomatik olarak algılar ve tercihinizi kaydeder. Menüden istediğiniz zaman dili değiştirebilirsiniz; tüm metinler, menüler ve yardım içerikleri anında güncellenir.

### Projeler ve `.sflow` dosyaları
Tüm çalışmanız `.sflow` uzantılı tek bir dosyaya kaydedilir. Bu dosya tüm diyagramı içerir: düğümler, bağlantılar, notlar, yapılandırmalar, betikler ve üretilen sayısal veriler. Başkalarıyla paylaşabilirsiniz; başka bir bilgisayarda açıldığında notlar ve düğümler o ekranın piksel yoğunluğuna otomatik olarak uyarlanır.

---

## Tipik iş akışları

1. **Basit bir diyagram oluşturma**  
   Araç çubuğundan bir kaynak düğümü (örn. Jeneratör) ve bir Görüntüleyici düğümü seçin.  
   Jeneratörün çıkışını görüntüleyicinin girişine bağlayın (çıkış portuna Ctrl+tık, ardından girişe tık).  
   Çalıştırmak için F5'e basın. Sinyali grafikte göreceksiniz.

2. **Özel bir betik kullanma**  
   bir Betik düğümü ekleyin.  
   Düzenleyicide Python kodunuzu yazın; düzenlenebilir parametreler ve ekstra portlar tanımlayabilirsiniz.  
   Giriş ve çıkışlarını diğer düğümler gibi bağlayın.  
   Akışı çalıştırın; betik verilerinizle işlenir.

3. **Gerçek bir osiloskoptan veri yakalama**  
   Cihazı bağlayın ve Osiloskop düğümü panelinden iletişimi yapılandırın.  
   Düğüm sinyali yakalar ve çıkış portlarından (kanal başına bir) iletir.  
   Bu portları diğer işleme düğümlerine veya görüntüleyiciye bağlayın.

4. **Bir rapor için grafik dışa aktarma**  
   İstenen sinyali Grafik Dışa Aktarcı düğümüne bağlayın.  
   Düğümde sağ tıklayarak grafik görünümünü yapılandırın.  
   Ayrıca profilleri yükleyip kaydederek rapora hazır grafikler elde etmeyi hızlandırabilir ve seçtiğiniz uzantıda görüntü dosyası alabilirsiniz.

---

## Tüm bunlar ne işe yarar

Bu mimari, programın iç organizasyonu hakkında endişelenmeden sinyal analizine odaklanabilmeniz için tasarlanmıştır. Her bileşenin net bir işlevi vardır ve birlikte, simülasyondan gerçek enstrümantasyona, görsel özelleştirmeden sonuç dışa aktarımına kadar sorunsuz bir deneyim sunar.

FloWorks yeteneklerini genişletmeniz gerekirse (örneğin, yeni düğüm türleri eklemek veya farklı bir cihaz bağlamak), bunu sağlayan modüler bir yapı olduğunu bilin, ancak bu geliştiricilerin alanıdır. Son kullanıcı olarak bu tasarımın sağladığı esnekliğin keyfini çıkarın.
