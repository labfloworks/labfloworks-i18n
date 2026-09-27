---
title: FloWorks'a Yeni Bir Düğüm Ekleme Rehberi
description: FloWorks akış motorunda özel düğümler oluşturmak, kaydetmek ve entegre etmek için adım adım öğretici.
---

# 📘 Geliştiriciler İçin Rehber: FloWorks'a Yeni Bir Düğüm Nasıl Eklenir

Bu rehber, FloWorks'te yeni bir düğüm türü oluşturma sürecini, akış motoru, kullanıcı arayüzü, görsel temalar ve yerelleştirme sistemiyle doğru şekilde entegre edilmesini sağlayacak şekilde açıklar.

---

## 📋 İçindekiler
- [📘 Geliştiriciler İçin Rehber: FloWorks'a Yeni Bir Düğüm Nasıl Eklenir](#-geliştiriciler-için-rehber-floworks'a-yeni-bir-düğüm-nasıl-eklenir)
  - [📋 İçindekiler](#-i̇çindekiler)
  - [1. Mimariye Giriş](#1-mimariye-giriş)
  - [2. `template_node.py` Şablonunu Kullanma](#2-template_nodepy-şablonunu-kullanma)
  - [3. Adım Adım: Özel Bir Düğüm Oluşturma](#3-adım-adım-özel-bir-düğüm-oluşturma)
    - [3.1. Şablonu Kopyalama ve Yeniden Adlandırma](#31-şablonu-kopyalama-ve-yeniden-adlandırma)
    - [3.2. Portları ve Etiketleri Tanımlama](#32-portları-ve-etiketleri-tanımlama)
    - [3.3. İşleme Mantığını Uygulama](#33-i̇şleme-mantığını-uygulama)
    - [3.4. Görünümü Özelleştirme (İsteğe Bağlı)](#34-görünümü-özelleştirme-i̇steğe-bağlı)
    - [3.5. Yapılandırılabilir Parametreler Ekleme (İsteğe Bağlı)](#35-yapılandırılabilir-parametreler-ekleme-i̇steğe-bağlı)
    - [3.6. Düğümü Serileştirmeyi Etkinleştirme (yapılandırmaları kaydetme / yükleme)](#36-düğümü-serileştirmeyi-etkinleştirme-yapılandırmaları-kaydetme--yükleme)
  - [4. Sisteme Entegrasyon](#4-sisteme-entegrasyon)
  - [5. Yerelleştirme (i18n)](#5-yerelleştirme-i18n)
  - [6. Görsel Temalar](#6-görsel-temalar)
  - [7. Kontrol Listesi ve Sorun Giderme](#7-kontrol-listesi-ve-sorun-giderme)
    - [✅ Kontrol Listesi](#-kontrol-listesi)
    - [🐛 Yaygın Sorunlar](#-yaygın-sorunlar)
  - [8. Sonuç](#8-sonuç)

---

## 1. Mimariye Giriş

FloWorks, PySide6 üzerine kuruludur ve bir sinyal işleme akışını temsil eden bağlanabilir düğümler modeli kullanır.

---

## 2. `template_node.py` Şablonunu Kullanma

Yeni düğümler oluşturmayı kolaylaştırmak için `nodes/template_node.py` dosyası sağlanmıştır. Bu şablon şunları içerir:
- Yerelleştirme için tam destek (`languageChanged` bağlantısı, `update_language` yöntemi).
- Temalar için tam destek (`update_theme` yöntemi).
- Üç bölümlü HTML formatında yerleşik yardım.
- Yapılandırılabilir birden fazla giriş/çıkış portunun yönetimi.
- `get_output_for_port` ile birden fazla çıkış.
- `get_display_signal` aracılığıyla çizimde görselleştirme.
- Çevrilebilir bağlam menüsü.

Yeni bir düğüm geliştirirken her zaman bu şablondan başlamanız önerilir.

---

## 3. Adım Adım: Özel Bir Düğüm Oluşturma

### 3.1. Şablonu Kopyalama ve Yeniden Adlandırma
1. `nodes/template_node.py` dosyasını yeni düğümünüzün adıyla kopyalayın, örneğin `nodes/mi_nodo.py`.
2. Sınıf adını `TemplateNode`dan açıklayıcı bir şeye değiştirin, örn. `MiNodoNode`.
3. Gerekirse içe aktarmaları ayarlayın.

### 3.2. Portları ve Etiketleri Tanımlama
!!! warning "Önemli: İsim Eşleşmesi"
    `PORTS`, `PORT_LABELS` içindeki port adları ve `execute_program` tarafından döndürülen sözlüğün anahtarları **tam olarak aynı** olmalıdır (büyük/küçük harf dahil). Şablon artık daha fazla sağlamlık için bir takma ad eşlemesi içerir (`'data_in'` → ilk sol port).

Dosyanın üst kısmındaki `PORTS` sözlüğünü düzenleyin. Her girişin formatı şöyledir:
```python
"port_adi": ("taraf", kesir)
```
- **Olası taraflar:** `"left"`, `"right"`, `"top"`, `"bottom"`.
- **Kesir:** Taraf boyunca konumu belirten `0.0` ile `1.0` arası bir değer.

**Tek girişli ve iki çıkışlı bir düğüm örneği:**
```python
PORTS = {
    "input":     ("left",  0.5),
    "magnitude": ("right", 0.35),
    "phase":     ("right", 0.65),
}
```
`PORT_LABELS` sözlüğü, her portun yanında görünecek metni içerir. Sabit metin yerine çeviri anahtarları kullanılması önerilir (bkz. Yerelleştirme bölümü).

### 3.3. İşleme Mantığını Uygulama
Anahtar yöntem `execute_program(self, input_data)`dır. Bu yöntem, düğüm veri aldığında akış motoru tarafından çağrılır.

**`input_data` şunlar olabilir:**
- Giriş yoksa `None`.
- Zaman sinyalleri için bir `(x, y)` demeti.
- 1B bir dizi.
- Birden fazla girişi olan düğümlerde bir `{port_adi: veri}` sözlüğü.

**Dönüş değeri:**
- Tek çıkışlı düğümler için verileri doğrudan döndürün (örn. bir `(x, y)` demeti).
- Birden fazla çıkışlı düğümler için, anahtarların `PORTS` içinde tanımlanan çıkış portu adlarıyla eşleştiği bir sözlük döndürün.

```python
def execute_program(self, input_data):
    # input_data'yı işleyin ve sonuçları oluşturun
    sonuc_magnitude = (freq, mag)
    sonuc_phase = (freq, phase)
    return {
        "magnitude": sonuc_magnitude,
        "phase": sonuc_phase
    }
```

!!! tip "Genel port adları hakkında not"
    Akış motoru bazen gerçek port adı yerine `'data_in'` gibi anahtarlar içeren bir sözlük geçebilir (özellikle kullanıcı tam daire üzerine tıklamadıysa). Şablon bu durumu ele almak için kod içerir:
    ```python
    if isinstance(input_data, dict):
        if 'data_in' in input_data:
            input_data = input_data['data_in']
    ```
    Bu, düğümün hassas olmayan bir bağlantı nedeniyle çökmesini önler.

Şablon zaten açıklamalı bir örnek içerir. Ayrıca, motorun her çıkışı yönlendirmesi için `get_output_for_port(self, port_name)` uygular:
```python
def get_output_for_port(self, port_name):
    return self.output_data.get(port_name)
```

### 3.4. Görünümü Özelleştirme (İsteğe Bağlı)
`paint()` yöntemi arka planı, başlığı, durumu ve ek metni çizer. Şunları değiştirebilirsiniz:
- Renkler (`update_theme` ile otomatik olarak güncellenir).
- Durum metni (`self._status` özelliğini kullanarak).
- Özet bilgiler (örn. genlik tepe değeri).

Şablon temel bir örnek gösterir.

### 3.5. Yapılandırılabilir Parametreler Ekleme (İsteğe Bağlı)
Düğümünüz kullanıcı tarafından ayarlanabilir parametreler gerektiriyorsa (örn. pencere boyutu, kesme frekansı), şunları yapabilirsiniz:
1. `__init__` içinde öznitelikler ekleyin (örn. `self.window_size = 512`).
2. Bir yapılandırma iletişim kutusu oluşturun (`QDialog`'dan türetin).
3. İletişim kutusunu `open_config_dialog()` içinde bağlayın (yöntem şablonda zaten mevcut).
4. Parametreleri iletişim kutusundan güncelleyin ve `self.update()` çağırın.

### 3.6. Düğümü Serileştirmeyi Etkinleştirme (yapılandırmaları kaydetme / yükleme)
Düğümün kopyala/yapıştır, geri al/yinele veya Dosya menüsündeki Kaydet/Aç komutları kullanılırken parametrelerini kaydedip geri yükleyebilmesi için serileştirme mixin'inden türemeli ve özniteliklerini bildirmelidir.

1. Mixin'i dosyanıza içe aktarın:
    ```python
    from nodes.serializable import SerializableMixin
    ```
2. Sınıf kalıtımını `QGraphicsObject`'ten önce mixin'i dahil edecek şekilde değiştirin:
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
    ```
3. Saklamak istediğiniz öznitelik adlarıyla sınıf düzeyinde `SERIALISABLE` listesini tanımlayın. Yalnızca basit türleri (`int`, `float`, `str`, `bool`), listeleri, sözlükleri veya NumPy dizilerini destekler (sonuncusu `.sflow` içinde otomatik olarak `.npy` dosyaları olarak saklanır).
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
        SERIALISABLE = ['frecuencia', 'amplitud', 'configuracion']
    ```
4. Bu özniteliklerin `__init__` içinde başlatıldığından emin olun:
    ```python
    self.frecuencia = 1000.0
    self.amplitud = 1.0
    self.configuracion = {'tipo': 'seno', 'fase': 0}
    ```

Bununla birlikte, `serialize`/`deserialize` yöntemlerini yazmanıza gerek yoktur; mixin değerlerin kaydedilmesi ve geri yüklenmesiyle otomatik olarak ilgilenir.

Düğümünüz yükleme sırasında ek mantık gerektiriyorsa (örn. bir donanım aygıtını yeniden bağlama), önce üst yöntemi çağırarak `deserialize` yöntemini geçersiz kılabilirsiniz:
```python
def deserialize(self, data):
    super().deserialize(data)   # SERIALISABLE özniteliklerini geri yükler
    self._iniciar_dispositivo()
```

---

## 4. Sisteme Entegrasyon

Düğüm dosyası oluşturulduktan sonra, arayüzde görünmesi ve sistemin geri kalanıyla çalışması için `nodes` klasörüne yapıştırmanız yeterlidir.

---

## 5. Yerelleştirme (i18n)

Görünür tüm metinler `tr("anahtar", default="...")` aracılığıyla çevrilebilir olmalıdır. Şablon bunu zaten uygular. İlgili anahtarları `locales/` içindeki JSON dosyalarına eklemeniz gerekir.

**Önerilen yapı:**
```json
{
   "nodes": {
     "mi_nodo": {
       "title": "Mi Nodo",
       "tooltip": "Açılır açıklama",
       "ports": {
         "input": "Giriş",
         "output1": "Çıkış 1",
         "output2": "Çıkış 2"
      },
       "status": {
         "no_data": "Veri yok",
         "ready": "Hazır"
      },
       "menu": {
         "show_output": "Çıkışı göster",
         "configure": "Yapılandır..."
      },
       "help_title": "Yardım - Mi Nodo",
       "help_html": "<h3>🎛️ Filter Node</h3>
<p>Applies a <b>digital filter</b>...</p>"
    }
  },
   "toolbar": {
     "add_mi_nodo": "Mi Nodo"
  }
}
```

HTML yardımı, tüm düğümler için ortak olan üç bölümlü formatı izler (özel açıklama + "Sistem hakkında nasıl düşünülür" + "Kısayollar ve ipuçları"). Şablon `get_help_text()` içinde yapıyı zaten içerir.

---

## 6. Görsel Temalar

`update_theme(self, theme)` yöntemi, geçerli tema tarafından tanımlanan renkleri içeren bir sözlük alır. Şablon otomatik olarak şunları günceller:
- Düğüm arka planı (`node_normal_bg`)
- Kenarlık (`node_selected_border`)
- Başlık ve metin rengi (`node_normal_text`)
- Port renkleri (`port_circle`, `port_outline`, `port_inline`, `port_text`)

Tema değiştiğinde `MainWindow` (veya `ThemeUpdater`) içinde her düğüm için `node.update_theme()` çağrıldığından emin olun.

---

## 7. Kontrol Listesi ve Sorun Giderme

### ✅ Kontrol Listesi
- [ ] Düğüm araç çubuğundan doğru şekilde oluşturuluyor.
- [ ] Portlar beklenen konumlarda görünüyor ve bağlantılar için algılanabiliyor (`Ctrl+tık`).
- [ ] Giriş verileri alındığında `execute_program` çağrılıyor ve sinyal işleniyor.
- [ ] Çıkışlar bağlı düğümlere doğru şekilde yayılıyor.
- [ ] Bağlam menüsü görüntüleme kanalını değiştirmeye izin veriyor (birden fazla çıkış varsa).
- [ ] Düğüme tıklandığında seçili sinyal çizim widget'ında çiziliyor.
- [ ] Çift tıklama uygun formatta yardımı açıyor.
- [ ] Dil doğru şekilde değişiyor (başlık, port ve menü metinleri).
- [ ] Tema doğru şekilde uygulanıyor (düğüm ve port renkleri).
- [ ] Kopyala/yapıştır hatasız çalışıyor.

!!! tip "Hassas port bağlantısı"
    Düğümleri bağlarken hedef portun dairesinin tam üzerine tıkladığınızdan emin olun. Düğüm gövdesine tıklarsanız, sistem genel bir ad kullanır (`'data_in'`). Şablon artık bu adları tolere ediyor, ancak birden fazla çıkışın doğru yönlendirilmesini garanti altına almak için doğrudan daireye bağlanmak iyi bir uygulamadır.

### 🐛 Yaygın Sorunlar

| Belirti | Olası Neden | Çözüm |
|---------|---------------|----------|
| Balantı oku porta tutunmuyor. | Port dairesinde `setData(0, port_name)` yok veya `get_port_scene_pos` uygulanmamış. | `_create_ports` içinde `circle.setData(0, port_name)` yapıldığını ve `get_port_scene_pos` bu adı kullandığını doğrulayın. |
| Çıkışlar bağlı düğümlere ulaşmıyor. | `execute_program` bir sözlük döndürmüyor (birden fazla çıkış için) veya `get_output_for_port` uygulanmamış. | `execute_program`ın `{port_adi: veri}` döndürdüğünden ve `get_output_for_port`ın ilgili değeri döndürdüğünden emin olun. |
| Düğüme tıklandığında hiçbir şey çizilmiyor. | `get_display_signal` geçerli bir `(x, y)` demeti döndürmüyor veya `display_channel` mevcut bir çıkışla eşleşmiyor. | `get_display_signal`ın seçili kanalı kullandığını ve verilerin NumPy dizileri olduğunu kontrol edin. |
| Metinler dil değiştiğinde güncellenmiyor. | `languageChanged` sinyali bağlanmamış veya `update_language` öğeleri güncellemiyor. | `__init__` içindeki bağlantıyı doğrulayın: `language_manager.languageChanged.connect(self.update_language)`. |
| Tema uygulanmıyor. | Düğüm oluşturulurken veya tema değişirken `update_theme` çağrılmıyor. | `MainWindow` içinde, düğüm oluşturulduktan sonra `node.update_theme(self.theme_manager.current_theme())` çağırın. |
| Ok düğümün merkezini gösteriyor. | Daire yerine gövdeye tıklandı veya ad `PORTS` ile eşleşmiyor. | Doğrudan daireye tıklayın. `get_port_scene_pos`ın takma ad eşlemesine sahip olduğunu doğrulayın. |
| İçe aktarmada `NameError: name 'self' is not defined`. | Örnek öznitelikleri `__init__` dışında bildirilmiş. | `self.benim_parametrem` gibi tüm öznitelikler `__init__` içinde tanımlanmalıdır. |
| Parametreler kopyalama/`.sflow` açma sırasında kayboluyor. | Düğüm `SerializableMixin`'den türemiyor veya `SERIALISABLE` tanımlanmamış. | Bu rehberin 3.6 adımını uygulayın. |

---

## 8. Sonuç

Bu rehberi ve `template_node.py` şablonunu takip ederek, FloWorks'e sistemin geri kalanıyla tutarlı bir şekilde yeni düğümler verimli bir şekilde ekleyebileceksiniz. Profesyonel bir kullanıcı deneyimi için i18n ve temalarla uyumluluğu her zaman koruduğunuzdan emin olun.

Kendi düğümlerinizle katkıda bulunmaya davetlisiniz!
