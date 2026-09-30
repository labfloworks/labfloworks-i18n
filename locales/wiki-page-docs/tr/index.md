---
title: FloWorks
description: Sinyal işleme, bilimsel enstrümantasyon ve otomasyon için evrensel görsel laboratuvar.
---

<div style="text-align: center; margin: 1em 0;">
  <img src="../assets/FloWorks.svg" alt="FloWorks" style="width: 60%; max-width: 600px; height: auto;">
</div>

<div class="hero-section" markdown>

## Sinyaller, enstrümantasyon ve yapay zeka için evrensel görsel laboratuvar

Bilimsel işleme • DSP • VISA/SCPI • Otomasyon • Machine Learning

![FloWorks Ekran Görüntüsü](assets/screenshot.PNG){ .hero-image }

<div class="hero-buttons" markdown>

[FloWorks'e Başlangıç](getting-started.md){ .md-button }
[Arayüz Anatomisi](interface-anatomy.md){ .md-button .md-button--primary }
[Felsefe](philosophy.md){ .md-button .md-button--primary }

</div>
</div>

---

## FloWorks Nedir?
FloWorks, kod satırları yazmak yerine blokları (düğümleri) birleştirerek sistemler oluşturduğunuz **açık kaynaklı görsel bir laboratuvardır** (Python + PySide6).

Sinyal jeneratörlerini, matematiksel filtreleri, donanım kontrolcülerini (VISA/SCPI) ve yapay zeka modellerini sanal kablolarla bir araya getirdiğiniz dijital bir Tuval hayal edin. Her şey **veri akışına** dayanır: bir bloğun çıkışını diğerinin girişine bağlayarak bilgi işleyebilir, cihazları otomatikleştirebilir veya sonuçları gerçek zamanlı analiz edebilirsiniz.

Öğrenciler, araştırmacılar, mühendisler ve geleneksel programlama bariyeri olmaksızın sezgisel bir şekilde denemek, öğrenmek veya karmaşık sistemlerin prototipini oluşturmak isteyen herkes için tasarlanmıştır.

### Misyon
Deneysel iş akışını tek bir görsel, açık ve erişilebilir araçta merkezileştirmek. Kullanıcıların yazılım karmaşıklığı veya lisans maliyetleriyle mücadele etmek yerine *deney yapma ve keşiflere* odaklanmalarını istiyoruz.

### Vizyon
Deneysel bir fikir ile onun yürütülmesi arasındaki tek bariyerin deneycinin merakı olduğu bir dünya. FloWorks, bilim ve teknik için küresel topluluk tarafından ve topluluk için inşa edilmiş referans bir platform olmayı hedefliyor ve özel araçların duvarlarını yıkıyor.

### İlkeler
* **Tam Özgürlük:** Bilgi ve araçlar herkes için erişilebilir olmalıdır. FloWorks'ün kullanımı ücretsizdir ve açık, genişletilebilir bir çekirdeğe bağlıdır.
* **Sonsuz Genişletilebilirlik:** Bir blok eksikse, herkes Python kullanarak onu oluşturabilir ve ekosisteme entegre edebilir.
* **Görsel Şeffaflık:** Sürecin her adımı grafiksel olarak incelenebilir, hata ayıklanabilir ve anlaşılabilir.
* **Gerçek Dünya ile Bağlantı:** Sadece simülasyon değil; Tuval üzerinden doğrudan gerçek bilimsel enstrümantasyonu kontrol etmeye olanak tanır.

Kapalı veya son derece uzmanlaşmış araçların aksine, FloWorks, her bileşenin yeniden kullanılabilir ve bağlanabilir bir düğüm olduğu modüler ve genişletilebilir bir ekosistem olarak tasarlanmıştır.

---

## Temel Yetenekler

<div class="grid cards" markdown>

-   **:material-puzzle-outline: Genişletilebilir Düğüm Ekosistemi**

    Katmanlara organize edilmiş teknik katalog: Kaynaklar, İşleme, Kontrol, Donanım ve Scripting.

    Dinamik kayıt, bildirimsel serileştirme ve hızlı geliştirme için net sözleşmeler.

    [:material-arrow-right: Düğüm Referansı](node-reference.md)

-   **:material-connection: VISA/SCPI Entegrasyonu**

    Osiloskoplar, LCR metreler ve jeneratörlerle doğrudan bağlantı.

    Çok kanallı destek, `PyVISA-py` ile entegre simülasyon ve taşınabilir modda güvenlik duvarı yönetimi.

    [:material-arrow-right: Enstrümantasyon](instrumentation.md)

-   **:material-package-variant-closed: Taşınabilir `.sflow` Formatı**

    JSON grafiği, `.npy` dizileri ve meta verileri içeren kendi kendine yeten ZIP standardı.

    Deneylerin tam tekrarlanabilirliği ve otomatik DPI normalizasyonu.

    [:material-arrow-right: .sflow Formatı](sflow-format.md)

-   **:material-translate: Gelişmiş Uluslararasılaştırma**

    Uygulamayı yeniden başlatmadan çalışırken dil değiştirme.

    Hiyerarşik JSON çevirileri ve tercihlerin kalıcılığı.

    [:material-arrow-right: i18n Kılavuzu](translation-guide.md)

-   **:material-tools: SDK ve Hızlı Geliştirme**

    Temel şablon (`template_node.py`), serileştirme mixin'i ve adım adım kılavuzlar.

    Eklentilere ve topluluk genişlemesine hazır mimari.

    [:material-arrow-right: Düğüm Oluşturma](adding-a-new-node.md)

</div>

---

## Uygulama Alanları

| Alan | Uygulamalar |
|------|-------------|
| 🎓 **Eğitim** | Fizik, elektronik, matematik, STEM laboratuvarları |
| ⚙️ **Mühendislik** | DSP, kontrol, enstrümantasyon, metroloji |
| 🤖 **AI** | ML, optimizasyon, hibrit pipeline'lar |
| 🔬 **Araştırma** | Otomasyon ve veri toplama |
| 🔌 **Donanım** | VISA/SCPI, simülasyon ve hibrit sistemler |

---

!!! tip "FloWorks'te yeni misiniz?"

    **FloWorks'e Başlangıç** bölümüyle başlayın, ardından grafik arayüz mimarisini anlamak için **Arayüz Anatomisi**'ne ve son olarak veri akışını ve topolojik motor yapısını anlamak için **Genel Mimarisi**'ni keşfedin.

---

<div markdown="1" style="text-align: center;">

## FloWorks

Görsel işleme • Enstrümantasyon • Bilim • AI

<small>MkDocs Material ile oluşturulmuş dokümantasyon</small>

</div>
