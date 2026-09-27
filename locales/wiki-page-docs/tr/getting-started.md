---
title: FloWorks'e Başlarken
description: Ortamı yapılandırma, ilk akışınızı çalıştırma ve taşınabilir sürüme erişim için hızlı başlangıç kılavuzu.
---

# 🚀 FloWorks'e Başlarken

Bu kılavuz sizi sıfırdan ilk sinyal işleme akışınızı çalıştırana kadar götürecektir. FloWorks, Python ve PySide6 ile inşa edilmiş, gerçek donanımı (VISA/SCPI), entegre simülasyonu, gelişmiş betiklemeyi ve çalışırken dil değişimini destekleyen bir akış şeması uygulamasıdır.

---

## 🌊 İlk örnek akışınız

Basit bir akış oluşturalım: bir sinüs sinyali üretelim ve onu gerçek zamanlı olarak görselleştirelim.

1. **Düğümleri ekleme**
   Üst araç çubuğunda `Kaynak` → `Gelişmiş Sinyal Jeneratörü`nü seçin. Ardından, `İşleme` altından örneğin `Spektral`i seçin.
2. **Bağlama**
   Jeneratörün çıkış portuna (`sağ`) `Ctrl+Tık` yapın. Sonra osiloskopun giriş portuna (`sol`) tıklayın. Ya da basitçe çıkış portuna tıklayıp sürükleyin (tıklı tutarak) sonraki düğümün giriş portuna kadar.
3. **Yapılandırma (isteğe bağlı)**
   Bir düğüme tıklayın, sol yan panelde seçili düğümün çalışma koşullarını ayarlamak için bir parametre editörü belirecektir. Alt kısımda, düğümler tarafından üretilen veya edinilen verileri görsel olarak temsil eden bir grafik görünümü vardır.
4. **Çalıştırma**
   `F5`e basın veya araç çubuğundaki ▶ düğmesine tıklayın. Topolojik motor çalışma sırasını hesaplayacak, verileri işleyecek ve grafik panelinde dalgayı göreceksiniz. **Bağlayıcı, aktif akışı gösteren bir animasyonla canlanacaktır!**

---

## 🧠 Portları anlama: renklere göre kategoriler

FloWorks'te her port, bir renkle tanımlanan bir **işlevsel kategoriye** aittir. Geçerli bağlantılar **her zaman aynı renkteki portlar arasında** yapılır: bir kategorinin çıkışı yalnızca aynı kategorinin girişine bağlanır. Ayrıca bağlayıcı çizgi, bağladığı portların rengini otomatik olarak alır, böylece görsel okuma kolaylaşır.

| Tür | Renk | Amaç | Tipik örnek |
|------|-------|-----------|----------------|
| `control` | Beyaz | Kontrol akışı / etkinleştirme. | Bir edinim düğümüne başlatma sinyali. |
| `exec` | Gri | İşlem veya adım yürütme. | Bir işlevin veya geri çağrının tetiklenmesi. |
| `data` | Yeşil | Genel veriler / sayısal sinyaller. | Bir jeneratörün veya sensörün çıkışı. |
| `int` | Mavi | Tam sayılar. | İndeks, tampon boyutu, kimlik. |
| `float` | Camgöbeği | Kayan noktalı sayılar. | Genlik, frekans, eşik. |
| `string` | Mor | Metin dizeleri. | Dosya adı, etiket. |
| `bool` | Pembe | Boole değerleri (`True`/`False`). | Durum bayrağı, etkinleştirme. |
| `array` | Koyu mavi | Diziler / vektörler. | Çok kanallı sinyal, örnek listesi. |
| `trigger` | Turuncu | Tetikleyiciler / ayrık olaylar. | Senkronizasyon darbesi, kenar. |

**Altın kural:**

- Yalnızca **tam olarak aynı renkteki** portlar bağlanır (çıkış ↔ aynı kategorinin girişi).
- Sistem geçersiz bağlantıları engeller ve sürüklerken uyumlu portları görsel olarak vurgular.
- Bağlayıcı çizgi, bağlı portların rengini alır; böylece her yol bir bakışta tanınır.

**FloWorks felsefesi:**
Veri portları dizilerin **boyutsallığını korur**. Asla otomatik düzleştirme uygulanmaz: bir matris girerse, bir matris çıkar, böylece çok boyutlu sinyallerinizin bütünlüğü korunur.

![FloWorks](assets/tipos_de_puertos.PNG)

---

## 🖱️ Tuvalde (Canvas) gezinme

Bu hareketlerle çalışma alanınızı hakimiyetle kullanın:

| Eylem | Nasıl yapılır |
|--------|--------------|
| **Yakınlaştırma** | Fare tekerleği veya `Ctrl + tekerlek` |
| **Kaydırma (pan)** | `Boşluk` tuşunu basılı tutup sürükleyin veya fare orta tuşunu kullanın |
| **Düğüm seçme** | Düğüme sol tık |
| **Çoklu seçim** | Sol tuşla bir dikdörtgen çizin veya birden fazla düğüme `Ctrl + tık` yapın |
| **Seçimi taşıma** | Seçili düğümlerden birini sürükleyin |
| **Yapılandırmayı açma** | Düğüme çift tık |

**İpucu:** Sol panel, seçili düğümün yapılandırmasıyla otomatik olarak güncellenir; ek pencere açmaya gerek yoktur.

---

## ⚡ Klavye kısayolları ve gelişmiş hareketler

Bu kısayollar normal bir kullanıcıyı **güç kullanıcısına** dönüştürür:

| Kısayol | Eylem |
|-------|--------|
| `F5` | Akışı çalıştır |
| `Ctrl + S` | Projeyi kaydet (`.sflow`) |
| `Ctrl + Tık` | Düğümleri bağla (çıkış portuna tık → giriş portuna tık) |
| `Ctrl + C` / `Ctrl + V` | Seçili düğümleri kopyala / yapıştır |
| `Ctrl + Z` / `Ctrl + Y` | Geri al / yinele |
| `Ctrl + Shift + L` | Tuvaldeki düğümleri otomatik düzenle |
| `Del` | Seçili düğümleri sil |
| `Ctrl + A` | Tüm düğümleri seç |

**Gelişmiş hareketler:**

- **Bir akışı çoğaltma:** bir düğüm grubu seçin, `Ctrl + C`, `Ctrl + V` yapın ve kopyayı başka bir bölgeye sürükleyin.
- **Izgarayı temizleme:** tek bir komutla tüm tuvali düzenlemek için `Ctrl + Shift + L` kullanın.
- **Hızlı bağlantı:** çıkış portuna `Ctrl + Tık` yapın ve ardından giriş portuna normal tık; FloWorks bağlantıyı otomatik olarak çizer.

---

## 🎨 Ortamı kişiselleştirme

FloWorks size uyar, tersi değil.

### Çalışırken tema değiştirme
Üst çubuktan **Görünüm → Tema** menüsünde açık, koyu ve diğerleri arasında seçim yapın. Arayüz **anında** değişir, yeniden başlatma veya iş akışı kaybı olmadan.

### Yazı tipi boyutu
**Görünüm → Yazı tipi boyutu** altından önceden tanımlı veya özel bir değer seçin. Tüm arayüz anında kendini ayarlar.

### Dil
**Görünüm → Dil** menüsünden istediğiniz dili seçin. FloWorks **çalışırken değişimi** destekler: menüler, düğmeler ve mesajlar uygulamayı yeniden başlatmadan çevrilir.

---

## ❗ Yaygın sorunların çözümü

| Sorun | Olası neden | Çözüm |
|----------|---------------|----------|
| Akış çalışmıyor | Yapılandırılmamış düğümler veya kopuk bağlantılar var | Tüm düğümlerin geçerli parametrelere sahip olduğunu ve bağlantıların uyumlu portlar arasında olduğunu kontrol edin |
| Grafik güncellenmiyor | Akış duraklatılmış veya veri akmıyor | `F5` veya ▶ ya bastığınızdan ve kaynak düğümlerin veri ürettiğinden emin olun |
| İki düğümü bağlayamıyorum | Portlar farklı tipte | Her iki portun da **veri** veya her ikisinin de **kontrol** olduğunu dorulayın |
| Büyük akışlarda program yavaş | Çok fazla düğüm veya gerçek zamanlı grafik | Kullanılmayan analiz panellerini kapatın veya kaynak düğümlerin örnekleme hızını düşürün |
| Tema değişmiyor | Bazı gereçler kayıtlı olmayabilir | Uygulamayı yeniden başlatıp tekrar deneyin (gelecek sürümlerde çözülecektir) |

---

## 🧪 Hızlı pratik örnekler

İlk sinüsal akışa ek olarak, FloWorks'i ustalaşmak için şu mini projeleri deneyin:

| Örnek | Dahil olan düğümler | Beklenen sonuç |
|---------|-------------------|--------------------|
| **Alçak geçiren filtre** | Jeneratör → Filtre → Grafik Görüntüleyici | Filtrelenmiş sinyali göreceksiniz |
| **Simüle edilmiş edinim** | Jeneratör → THD Analizörü | Sinyalin harmonik bozunum değeri |
| **Manuel kontrol** | Jeneratör → Veri Müfettişi | Jeneratörün gönderdiği sinyal değerlerinin tablosu |
| **Sinyal karşılaştırma** | İki jeneratör → Toplayıcı → Grafik Görüntüleyici | İki dalganın tek grafikte toplamı, farkı, çarpımı veya bölümü |

Bu akışların her biri bir dakikadan kısa sürede kurulabilir; bu da FloWorks'ün geleneksel kodlamaya karşı çevikliğini gösterir.

---

## 📚 Sırada ne var?

| Kaynak | Açıklama |
|---------|-------------|
| [🗺️ Ana Arayüz Anatomisi Kılavuzu](interface-anatomy.md) | Grafik arayüzün mimarisi ve felsefesi |
| [🗺️ Kod ve Mimari Haritası](philosophy.md) | Tam yapı, yöneticiler, sözleşmeler ve DPI farkındalığı. |
| [🧩 Düğüm Teknik Referansı](node-reference.md) | Katalog, `ScriptNode`, çok kanallı ve sistemi nasıl genişleteceğiniz. |
| [🌐 Ulusallaştırma Kılavuzu](translation-guide.md) | Dil ekleme, JSON doğrulama ve `tr()` anahtarlarını yönetme. |
| [📦 Taşınabilir Derleme Kılavuzu](guia-ejecutable-portable.md) | PyInstaller, hooklar, `--onefile`, hata çözümü ve dijital imza. |

---

!!! warning "Uyumluluk ve kullanım notları"
    1. **Python sürümü:** 3.9+ ve 64 bit sistemler kullanabilirsiniz.
    2. **Windows Güvenlik Duvarı:** Gerçek donanım kullanıyorsanız (VISA/SCPI osiloskop), güvenlik duvarında `FloWorks.exe`ye izin verin. Bağlantı engellenirse uygulama özel bir iletişim kutusu gösterir ( `--windowed` kipinde sistem iletişim kutusu görünmez).
    3. **Anahtar kısayollar:** `F5` (çalıştır), `Ctrl+S` (`.sflow` kaydet), `Ctrl+Tık` (bağla), `Boşluk+tık` (serbest kaydırma), `Ctrl+Shift+L` (otomatik yerleşim).
    4. **Veri koruma:** Motor dizilere asla `flatten()` uygulamaz. Vektörleştirme gerekiyorsa yerel kopyalar üzerinde çalışın.
