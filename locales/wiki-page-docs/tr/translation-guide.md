---
title: Uluslararasılaştırma Kılavuzu (i18n)
description: FloWorks'te çevirileri ekleme ve yönetme için adım adım talimatlar
---

# 🌐 Uluslararasılaştırma Kılavuzu (i18n)

Bu belgede, FloWorks'e yeni bir dil ekleme ve çeviri dosyalarını etkili bir şekilde yönetme açıklanmaktadır.

---

## ➕ Yeni Bir Dil Nasıl Eklenir

### Adım 1: JSON Dosyasını Oluşturma
`locales/` klasörüne gidin. `en.json` dosyasını kopyalayın ve ilgili iki harfli [ISO 639-1](https://tr.wikipedia.org/wiki/ISO_639-1) kodunu kullanarak yeniden adlandırın (örn. `fr.json` için Fransızca, `de.json` için Almanca).

### Adım 2: Dizeleri Çevirme
Yeni JSON dosyasını bir metin düzenleyicide açın.

!!! warning "Anahtarları Değiştirmeyin"
    **Anahtarları (her çiftin sol tarafını) asla değiştirmeyin.** Sadece değerleri (sağ tarafı) çevirin.

**Orijinal (`en.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "File",
    "edit": "Edit"
  }
}
```

**Çevrilmiş Örnek (`es.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "Archivo",
    "edit": "Editar"
  }
}
```
Kök anahtarın `"language_name"` içinde dilin yerel adının bulunduğundan emin olun (örn. `"Français"`, `"Deutsch"`, `"Español"`).

### Adım 3: JSON Doğrulama
Dosyanın geçerli bir JSON olduğunu doğrulayın (sonda virgül olmamalı, doğru tırnak işaretleri, uygun escape karakterleri). [JSONLint](https://jsonlint.com) gibi çevrimiçi doğrulayıcıları kullanabilir veya şunu çalıştırabilirsiniz:
```bash
python -m json.tool locales/es.json
```

### Adım 4: Yeni Dili Test Etme
1. FloWorks'ü başlatın.
2. **INFO → Dil** bölümüne gidin ve yeni dili seçin.
3. Menülerin, panellerin, iletişim kutularının, düğüm etiketlerinin vb. hemen güncellendiğini doğrulayın.

### Adım 5: Otomatik Algılama (İsteğe Bağlı)
Kullanıcının sistem yerel ayarı yeni dilin koduyla eşleşirse, FloWorks bunu ilk başlatmada otomatik olarak kullanacaktır (daha önce `QSettings`'te bir tercih kaydedilmediyse).

---

## 🌍 Mevcut Diller
- **İngilizce** (`en`) – Temel / yedek dil
- **İspanyolca** (`es`)

---

## ⚙️ Önemli Notlar ve En İyi Uygulamalar

!!! info "Yedekleme Mekanizması"
    Temel dil **İngilizce**'dir. Bir dil dosyasında bir çeviri anahtarı eksikse, FloWorks otomatik olarak İngilizce dizesini yedek olarak kullanır.

!!! warning "UI Taşmasını Önleme"
    Tasarım kırılmalarını önlemek için çevirileri özlü tutun. Çevrilen metin önemli ölçüde daha uzunsa, kısaltmayı veya dinamik ölçeklendirmeyi tema sistemine bırakmayı düşünün.

!!! tip "HTML ve Yer Tutucuları Koruma"
    - **HTML Etiketleri:** Tüm HTML etiketlerini olduğu gibi koruyun (örn. `<h3>`, `<b>`, `<pre>`, `<br>`).
    - **Yer Tutucular:** Kullanıldığı yerlerde `{variable}` sözdizimini koruyun (örn. `"Idioma cambiado a: {name} ({code})"`). Yeniden sıralamayın veya kaldırmayın.

---

## 🔗 İlgili Dokümantasyon
- [📖 Kod Haritası ve Mimarisi](architecture-ii.md)
- [📦 Derleme ve Dağıtım Kılavuzu](build.md)
- [🧩 Düğüm Referansı](node-reference.md)
