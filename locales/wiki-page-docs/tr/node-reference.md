---
title: Düğüm Teknik Referansı
description: FloWorks düğüm sisteminin güncellenmiş kataloğu, uzantı sözleşmeleri ve gelişmiş yetenekleri
---

# 🧩 Düğüm Teknik Referansı

FloWorks statik bir kataloğa bağlı değildir. Açık sözleşmelere dayalı **dinamik bir kayıt sistemi** kullanır. Bu, topolojik motoru dokunmadan platformu genişletmeyi mümkün kılar. Aşağıda uygulanan katalog, gerçek teknik yetenekler ve güvenli genişletme protokolü detaylandırılmıştır.

---

## 📂 Çekirdek kategorileri

=== "📦 Katman görünümü"
    <div class="grid cards" markdown>

    - **📥 Kaynaklar/Giriş**
      Başlangıç sinyallerini üretir veya yakalar. Entegre simülasyon, gerçek donanım (VISA/SCPI) ve çok kanallı modu destekler.
    - **⚙️ İşleme**
      Verileri dönüştürür, birleştirir veya analiz eder. Boyutsallığı korur ve gerekirse otomatik olarak enterpole eder.
    - **🔀 Kontrol/Akış**
      Çalıştırmayı dallara ayırır, yineler veya koşullar. Etkinleştirme sinyalleri için yerel destek içerir.
    - **🐍 Scripting/Gelişmiş**
      Parametrik portlar (`# @param`), dinamik portlar (`# @input`/`# @output`) ve durum kalıcılığı (`persist`) ile dinamik Python kodu çalıştırır.
    - **🔌 Donanım/Enstrümantasyon**
      Osiloskoplar, LCR metreler ve jeneratörler için arayüzler.
    - **📤 Çıkış/Dışa aktarma**
      Sonuçları görselleştirir, dışa aktarır veya arşivler. Görsel temalar, kullanıcı profilleri ve profesyonel formatları (PNG/PDF/SVG) destekler.

    </div>

---

## 📋 Uygulanan teknik katalog

| Düğüm | Tür | Ana sorumluluk | Temel özellikler |
|-------|-----|---------------|-----------------|
| `SumNode` | İşleme | İki giriş için aritmetik operatör (+, -, *, /). | Farklı çözünürlüklü sinyalleri (FFTs) otomatik enterpole eder. Boyutsallığı korur. |
| `RhombusNode` | Kontrol | Koşullu dallanma (Evet/Hayır). | İki çıkış portu. Eşik veya boole mantığıyla koşulu değerlendirir. |
| `TriggerNode` | Kontrol | Harici tetiklemeli yineleyici/akkumülatör. | `(x, y, "trigger")` alır. N yinelemeye kadar biriktirir ve yığılmış/ortalamalı sonuç yayar. |
| `ScriptNode` | Gelişmiş | Entegre Python script ortamı. | QScintilla, otomatik tamamlama, `# @param`, dinamik portlar, `persist`, şablonlar, hata konsolu, zaman aşımlı harici yorumlayıcı. |
| `OscilloscopeNode` | Donanım | Osiloskoplardan (SDS) veya LCR metrelerinden yakalama. | Simülasyon modu, entegre güvenlik duvarı iletişim kutusu, **çok kanallı destek** (`out_primary`, `out_secondary`), "Kanalı göster" menüsü. |
| `GeneratorNode` | Kaynak | Jeneratörlere (SDG) sinyal gönderir veya çıkışları simüle eder. | Modülasyon/sweep yapılandırması, entegre simülasyon iletişim kutusu. |
| `GraphExporterNode` | Çıkış | Profesyonel grafik dışa aktarıcı. | Çift tıklama yapılandırması, özel eksenler, temalar, kayıtlı profiller vb. |

---

## 🔍 ScriptNode: Temel yetenekler

> **🐍 Entegre Script Ortamı**
>
> - **Entegre kod düzenleyici:** Temel sözdizimi vurgulama, satır numaralandırma ve kod katlama.
> - **Dinamik parametre paneli:** `# @param AD : tip = değer` yönergeleri, yan panelde düzenlenebilir denetimler (spinbox, metin alanı vb.) enjekte eder.
> - **Dinamik portlar:** `# @input ad` ve `# @output ad`, gerçek zamanlı portlar oluşturur. Script bir `inputs` sözlüğü alır ve `outputs` döndürür.
> - **Durum kalıcılığı:** Çalıştırmalar arasında değerleri koruyan global `persist` sözlüğü.
> - **Şablonlar ve İçe/Dışa Aktar:** Temel scriptlerle açılır menü. Kullanıcı scriptlerini `nodes/script_node/templates/` konumuna kaydedebilir veya harici `.py` dosyalarını içe/dışa aktarabilir.
> - **Entegre hata konsolu:** Editörde tam satırı işaretleyerek sözdizimi/çalışma zamanı hatalarını gösterir.
> - **Yardım ve i18n:** Bağlamsal ipuçları, hızlı kılavuzlu `?` düğmesi ve tüm metinler çeviri için `tr()` kullanır.
> - **Zaman aşımlı harici yorumlayıcı:** Yapılandırılabilir yol (`# @python_path` veya "Gözat…" düğmesi). Zaman sınırlı yalıtılmış Çalıştırma ve dahili yorumlayıcıya geri dönüş.
> - **Tam serileştirme:** Script, parametreler, dinamik portlar ve `persist` durumunu kaydeder. Bir `.sflow` yüklenirken portları ve parametreleri otomatik olarak yeniden oluşturur.

---

## 📚 İlgili kaynaklar

- [📖 Kod Haritası ve Mimarisi](architecture-ii.md) → Modül başına sorumluluklar ve iş akışları.
- [🌐 Uluslararasılaştırma Kılavuzu (i18n)](i18n.md) → Dil ekleme ve `tr()` anahtarlarını yönetme.
- [🛠️ Yeni düğüm ekle (öğretici)](adding-a-new-node.md) → Pratik örneklerle adım adım.
- [📦 Derleme ve Dağıtım Kılavuzu](build.md) → PyInstaller paketleme, hooks ve dijital imzalar.

---

💡 **Katalogda bir düğüm mü eksik?**
FloWorks genişletilebilir olarak tasarlanmıştır. İhtiyacınız olan bir düğüm yoksa, `BaseNode` sözleşmesine göre oluşturun ve kaydedin. Topluluk ve gelecekteki marketplace, uyumluluğu bozmadan ekosistemi sürekli genişletecektir.
