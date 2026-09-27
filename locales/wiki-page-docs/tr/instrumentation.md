---
title: VISA/SCPI Enstrümantasyonu
description: FloWorks'te VISA/SCPI standardı ile gerçek ve simüle donanımın bağlanması, yapılandırılması ve kullanımına ilişkin kılavuz.
---

# 🔌 VISA/SCPI Enstrümantasyonu

FloWorks, gerçek laboratuvar cihazlarıyla doğrudan iletişimi **SCPI** (Standard Commands for Programmable Instruments) protokolü üzerinden **VISA** (Virtual Instrument Software Architecture) soyutlama katmanı kullanarak entegre eder. Ayrıca, fiziksel donanım gereksinimi olmadan akışları geliştirmek, test etmek ve paylaşmak için tamamen Python tabanlı simülatörler sunar.

---

## 🌐 VISA/SCPI Nedir?

| Teknoloji | Açıklama |
|-----------|----------|
| **VISA** | Fiziksel arayüzü (USB-TMC, Ethernet/LAN, GPIB, RS‑232) soyutlayan standart katman. Bağlantı dizesini değiştirerek gerçek bir cihazdan simüle edilmiş bir cihaza geçiş yapılmasını sağlar. |
| **SCPI** | Jeneratörleri, osiloskopları, multimetreleri, LCR metreleri vb. kontrol etmek için standartlaştırılmış ASCII komut dili. Üreticiler standardı genişletir, ancak temel evrenseldir. |
| **PyVISA** | FloWorks tarafından kullanılan Python arka ucu. `@py` (saf simülasyon) ve yerel arka uçları (`@ni`, `@ivi`, `@keysight`, vb.) destekler. |

---

## ⚙️ Tipik Yapılandırma

=== "📍 Bağlantı Dizesi (Resource String)"
    Standart VISA formatı:
    - `USB0::0x1AB1::0x0588::DS1ZA123456789::INSTR` (USB Osiloskop)
    - `TCPIP0::192.168.1.100::inst0::INSTR` (LAN/Ethernet)
    - `ASRL1::INSTR` (RS-232 Seri Port)
    - `GPIB0::1::INSTR` (Eski GPIB)

=== "⏱️ Zaman Aşımı ve Seçenekler"
    - **Timeout**: ms cinsinden yapılandırılabilir. Cihaz uzun ölçümler veya frekans taramaları gerektiriyorsa artırın.
    - **Başlatma**: Bazı düğümler, bağlanırken özel SCPI komutları enjekte etmeye izin verir (örn. `*CLS`, `SYST:PRES`, `:CHAN1:DISP ON`).

---

## 📡 Kullanılabilir Donanım Düğümleri

<div class="grid cards" markdown>

- **🔭 SCPI Osiloskop**
  Zaman domaini dalga formlarını yakalar. Çok kanallı destek, otomatik ölçeklendirme, donanım tetikleme ve sıcak kanal değiştirme için "Kanalı göster" menüsü sunar.

- **⚡ LCR Metre**
  Empedans, endüktans, kapasitans, direniç ve kayıp faktörü ölçer. Tek bir alımda birincil ve ikincil verileri içeren `master_payload` döndürür.

- **🎛️ Keyfi Fonksiyon Jeneratörü**
  SDG donanımına sinyal gönderir veya çıkışları simüle eder. Modülasyon (AM/FM/PM), lineer/logaritmik sweep, burst ve fazı yapılandırır.

- **📊 Dijital Multimetre (DMM)** *(Geliştirme aşamasında)*
  DC/AC voltaj, akım, direni ve frekans ölçümleri için SCPI arayüzü. Keithley, Agilent ve Rigol ile uyumlu.

- **🔋 Programlanabilir Güç Kaynağı** *(Geliştirme aşamasında)*
  OVP/OCP koruması ile çıkış voltajı/akımı kontrolü. Otomatik test bankaları için kullanışlıdır.

</div>

---

## 🔄 Tipik Çalışma Akışı

1. **Düğümü ekle**: Araç çubuğundan (`Kaynaklar` veya `Cihazlar`) Tuval'e düğüm ekle.
2. **Bağlantıyı yapılandır**: Arka uç seç, VISA dizesini gir ve zaman aşımı/başlatma ayarla.
3. **Akışa bağla**: Cihaz çıkışını işleme (FFT, filtreler, aritmetik) veya görselleştirme düğümlerine bağla.
4. **Çalıştır (`F5`)**: Topolojik motor alım talep eder, sürücü SCPI yanıtını ayrıştırır ve verileri paketler.
5. **Görselleştir/Dışa aktar**: Veriler graf üzerinden akar ve sonraki düğümler tarafından işlenir.

---

## 🛠️ Sorun Giderme

!!! warning "1. VISA cihazı bulamıyor (`VI_ERROR_RSRC_NFOUND`)"
    - **Neden:** Yanlış dize, bağlı olmayan kablo veya arka uç cihazı algılamıyor.
    - **Çözüm:** Geçerli kaynakları listelemek için `pyvisa-shell` veya üretici aracını (NI MAX, Keysight Connection Expert) çalıştır. Kullanıcı izinlerini kontrol et.

!!! warning "2. Alım sırasında zaman aşımı"
    - **Neden:** Yavaş tarama, tetik gerçekleşmiyor veya cihaz başka bir görevle meşgul.
    - **Çözüm:** Düğümde zaman aşımını artır. Osiloskop tetik ayarlarını doğrula (`AUTO` veya `NORMAL`). Başlangıçta `*CLS` kullan.

!!! warning "3. Simülasyon yanıt vermiyor veya başarısız oluyor"
    - **Neden:** `PyVISA-py` yüklü değil veya başka bir arka uçla çakışma var.
    - **Çözüm:** `pip install pyvisa-py`. Düğümde açıkça `@py` arka ucunu seç.

!!! warning "4. SCPI hataları (`Command Error`, `Execution Error`)"
    - **Neden:** Komut firmware tarafından desteklenmiyor veya sözdizimi hatalı.
    - **Çözüm:** Cihazınızın SCPI programlama kılavuzuna bakın. Bazı üreticiler `:` önekleri veya `\n` sonlandırıcıları gerektirir. FloWorks `\n` ekler, ancak sonlandırıcı sürücüde ayarlanabilir.

!!! info "5. Desteklenmeyen bir cihaz için düğüm oluşturma"
    - `BaseNode`'den kalıtım al ve `instrument/` içinde `DeviceBase` desenini kullan.
    - `(x, y)` demetleri veya `master_payload` döndüren `headless` bir sürücü uygula.
    - Portları, serileştirmeyi ve i18n kaydetmek için [📘 Kılavuz: Yeni düğüm ekle](adding-a-new-node.md) izle.

---

## 📚 İlgili Kaynaklar

- [🧩 Düğüm Teknik Referansı](node-reference.md) → `oscilloscope_node`, `generator_node` ve serileştirme sözleşmeleri ayrıntıları.
- [📦 Taşınabilir Derleme Kılavuzu](guia-ejecutable-portable.md) → Güvenlik duvarı yönetimi, `resource_path()` ve PyInstaller paketleme.
- [📘 Yeni düğüm ekle](adding-a-new-node.md) → `instrument/` dizinini genişletme ve özel sürücüleri kaydetme.
- [📄 `.sflow` Formatı](sflow-format.md) → Donanım yapılandırmaları ve yakalanan dizilerin nasıl kalıcı hale getirildiği.
