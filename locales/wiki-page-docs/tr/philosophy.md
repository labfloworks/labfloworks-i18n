# Felsefe

## Genel Bakış

FloWorks, görsel akış şemaları aracılığıyla sinyal işleme zincirleri oluşturmanıza olanak tanıyan bir masaüstü uygulamasıdır.
Düğümleri sürükleyin, bağlayın ve yapılandırın; sonuç gerçek zamanlı olarak hesaplanır ve görüntülenir.
Simüle edilmiş sinyallerle çalışın veya kod yazmaya gerek kalmadan gerçek enstrümanları (osiloskoplar, jeneratörler, LCR multimetreleri) bağlayın; ancak işlevselliği genişletmek isterseniz güçlü bir betikleme ortamı da mevcuttur.

---

## Temel Özellikler

- **Etkileşimli Diyagramlar** – Veri akışını temsil eden çizgilerle düğümleri birleştirerek iş akışınızı oluşturun.
- **Gerçek Zamanlı İşleme** – Her değişiklik grafiklerde ve görselleştirmelerde anında yansır.
- **Simülasyon ve Gerçek Donanım** – Test sinyalleri oluşturun veya laboratuvar enstrümanlarından doğrudan veri yakalayın.
- **Gelişmiş Betik Düğümü** – Otomatik tamamlama, düzenlenebilir dinamik parametreler ve çalıştırmalar arası kalıcı bellek ile kendi Python kodunuzu dahil edin.
- **Profesyonel Görselleştirme** – Dışa aktarıma hazır yüksek kaliteli sinyaller, spektrumlar, spektrogramlar ve grafikler.
- **Çok Dilli** – Arayüz sistem dilini algılar ve İspanyolca, İngilizce ve diğer diller arasında istediğiniz zaman geçiş yapmanıza olanak tanır.
- **Görsel Temalar** – Tercihlerinize veya erişilebilirlik ihtiyaçlarınıza uygun koyu, açık ve yüksek kontrast modları.
- **Kapsamlı Proje Yönetimi** – Çalışmanızı `.sflow` dosyalarına kaydedin ve sınırsız geri alma / yeniden yapma ile tam olarak bıraktığınız gibi geri yükleyin.

---

## FloWorks ile Çalışma

### Düğümler
Bir düğüm, işlemenin bir parçasıdır. Üç kategoride organize edilmişlerdir:

- **Kaynaklar** – Akışın başlangıcına sinyaller ekler. Örneğin, bir osiloskop (gerçek veya simüle), bir fonksiyon jeneratörü veya bir matematiksel işlem.
- **İşleme** – Verileri dönüştürür. Toplama, çıkarma, koşullar, filtreler… dahil olmak üzere Python'da kendi betiklerinizi yazmak için özel bir düğüm.
- **Havuzlar** – Sonuçları görüntüler veya dışa aktarır. Grafik görüntüleyici ve profesyonel grafik dışa aktarıcı en çok kullanılanlardır.

### Bağlantılar
Düğümler arasındaki birleşimler, pürüzsüz eğriler veya dik hatlar olarak çizilir. Bir akış animasyonu, verilerin yönünü her an size gösterir. Sistem, kabloların üst üste binmemesi için bunları otomatik olarak düzenler.

### Görselleştirme
Bir düğüm bir sinyal ürettiğinde, bu entegre grafik panelinde görüntülenebilir. Farklı temsilleri (dalga formu, spektrum, spektrogram) keşfedebilir ve ölçeği fare ile ayarlayabilirsiniz.

---

## Öne Çıkan Düğümler
Bunlar, programın felsefesinin anlamlı olması için gereken asgari düzeyde zorunlu düğümlerdir.

### Sinyal Jeneratörü Düğümü
Kullanıcı tanımlı dalga formu simülasyonları oluşturabilen bir sinyal kaynağıdır. İstenilen dalga formunu seçmek veya girmek için bağlam menüsü sunar.

### Betik Düğümü
Diyagram içinde eksiksiz bir programlama ortamıdır:

- **Sözdizimi vurgulamalı editör**, otomatik tamamlama ve hata konsolu.
- **Dinamik Parametreler** – Kodu değiştirmeden düğüm panelinden düzenlenebilir değişkenler tanımlayın.
- **Yapılandırılabilir Portlar** – Editörden doğrudan ek giriş ve çıkışlar ekleyin.
- **Kalıcı Durum** – Çalıştırmalar arasında değerleri kaydedin; her şey proje ile birlikte saklanır.

### Grafik Dışa Aktarıcı
Raporlar veya yayınlar için yüksek kaliteli görüntüler oluşturan bir havuz düğümüdür. Boyut, çözünürlük, format ve diğer ayarları yapılandırmanıza olanak tanır.

---

## Kişiselleştirme

- **Dil** – Uygulama sistem dilini otomatik olarak algılar ve tercihinizi kaydeder. Yeniden başlatmadan menüden değiştirebilirsiniz.
- **Görünüm** – Ortam ışığına veya görsel ihtiyaçlarınıza göre koyu, açık veya yüksek kontrast tema seçin.

---

## Projeler ve Dosyalar

Tüm diyagramınızı bir `.sflow` dosyasına kaydedin.
Açtığınızda tüm düğümleri, bağlantıları, betikleri, parametreleri ve görselleştirme ayarlarını geri yükleyeceksiniz.
Geri alma ve yeniden yapma eylemleri, önceki çalışmanızı kaybetme korkusu olmadan deneme yapmanıza olanak tanır.

---

FloWorks, uygulamanın teknik detayları yerine sinyal analizine odaklanmanız için tasarlanmıştır. Sürükleyin, bağlayın ve keşfedin.
