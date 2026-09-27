---
title: .sflow Dosya Formatı
description: FloWorks değişim standardının teknik özellikleri, iç yapısı ve kullanım kılavuzu
---

# 📄 `.sflow` Dosya Formatı

`.sflow` formatı, **FloWorks**'ün yerli değişim ve kalıcılık standardıdır. Graf topolojisini, düğüm parametrelerini, işlenmiş verileri ve yapışkan notları içeren tek bir dosyada eksiksiz bir iş akışını paketlemenize olanak tanır; bu da deneylerin deterministik olarak paylaşılmasını, arşivlenmesini veya yeniden üretilmesini kolaylaştırır.

---

##  `.sflow` dosyası nedir?

Bir `.sflow` dosyası, özünde **yeniden adlandırılmış bir ZIP dosyasıdır**. Uzantısını `.zip` olarak değiştirerek, içeriğini herhangi bir dosya yöneticisi veya komut satırı aracıyla inceleyebilirsiniz.

Minimum iç yapısı şunlardan oluşur:

| Bileşen | Açıklama |
|------------|-------------|
| `diagram.json` | Ana manifest: düğümleri, bağlantıları, görünümü, yapışkan notları ve serileştirme meta verilerini tanımlar. |
| `data/` | Her düğümün verilerini `.npy` formatında (NumPy ikili dizileri) içeren klasör. |
| `metadata.json` *(isteğe bağlı)* | Tamamlayıcı bilgiler: yazar, FloWorks sürümü, açıklama ve etiketler. |

=== "🌳 Görsel Yapı"
    ```text
    mi-flujo.sflow
    ├── diagram.json
    ├── metadata.json
    └── data/
        ├── node_1.npy
        ├── node_2.npy
        └── script_state.npy (isteğe bağlı, ScriptNode persist için)
    ```

---

## 🧩 `diagram.json` – Akışın Kalbi

Bu JSON dosyası, kaydetme anında tuvaldeki öğelerin konumunu ve görünüm durumunu da içerecek şekilde tam topolojiyi açıklar.

### Minimum Örnek
```json
{
  "nodes": [
    {
      "id": "n1",
      "type": "OscilloscopeNode",
      "pos": [150, 200],
      "params": { "channel": "primary", "simulation": false }
    },
    {
      "id": "n2",
      "type": "GraphExporterNode",
      "pos": [450, 200],
      "params": { "theme": "dark", "export_format": "png" }
    }
  ],
  "connections": [
    {
      "from": "n1",
      "to": "n2",
      "from_port": "out",
      "to_port": "data_in"
    }
  ],
  "viewport": { "x": 0, "y": 0, "scale": 1.0 },
  "stickers": [
    { "x": 600, "y": 100, "width": 200, "height": 150, "text": "Eşiği kontrol et", "user_modified": true }
  ]
}
```

### Ana Alanlar
| Alan | Tür | Açıklama |
|-------|------|-------------|
| `nodes` | `Array` | `{id, type, pos, params}` nesnelerinin listesi. `type`, `node_registry.py` ile eşleşmelidir. |
| `connections` | `Array` | `{from, to, from_port, to_port}` bağlantılarının listesi. Portlar dizin değil, dizgedir. |
| `viewport` | `Object` | Tuvalin tam konumunu ve yakınlaştırmasını geri yüklemek için `(x, y, scale)`. |
| `stickers` | `Array` | 96 dpi'ye normalize edilmiş koordinatlarla serileştirilmiş yapışkan notlar. |

!!! tip "Düğüm Serileştirme"
    Her düğümün özel parametreleri `SerializableMixin` aracılığıyla yönetilir. Yalnızca `SERIALISABLE = [...]` içinde bildirilen öznitelikler kaydedilir. Sistemde kayıtlı olmayan düğümler yükleme sırasında otomatik olarak atlanır.

---

## 💾 `data/` – İşlenmiş Veriler ve NumPy Dizileri

Bir akış çalıştırıldığında, düğümler sonuçlarını bu klasörün içindeki `.npy` dosyalarına kaydedebilir.

- Dosya adı genellikle düğümün `id`'si veya dahili referanslarıyla eşleşir.
- Diziler NumPy ikili formatında saklanır, **orijinal boyutsallığı katı bir şekilde koruyarak** (1D, 2D, 3D, vb.). Motor asla `flatten()` uygulamaz.
- `diagram.json`'da veriler `__npy__:` önekiyle referanslanır:
  ```json
  "params": { "cached_output": "__npy__:node_2.npy" }
  ```
- Bir düğüm veri üretmez veya kalıcı hale getirmeyecek şekilde yapılandırılmışsa, ilgili dosya atlanabilir.

??? note "Harici Uyumluluk"
    `.npy` dosyaları Python ekosisteminde evrenseldir. Bunları FloWorks dışında şu şekilde okuyabilirsiniz:
    ```python
    import numpy as np
    veriler = np.load("data/node_1.npy")
    print(veriler.shape)
    ```

---

## 🏷️ `metadata.json` (İsteğe bağlı)

Yürütmeyi etkilemeyen, izlenebilirlik ve proje yönetimi için ideal açıklayıcı bilgiler içerir:

```json
{
  "floworks_version": "2.1.0",
  "author": "María Gómez",
  "description": "Üç fazlı motorda titreşim analizi",
  "created": "2026-05-10T10:30:00Z",
  "tags": ["mühendislik", "titreşim", "FFT", "çok kanallı"]
}
```

---

## 🔄 Kaydetme ve Yükleme Süreci

FloWorks, veri bütünlüğünü garanti altına almak için sağlam bir mekanizma uygular:

1. **Kaydetme:**
   - Graf gezilir ve düğümler `SerializableMixin` aracılığıyla serileştirilir.
   - Diziler `data/` klasörüne çıkarılır ve JSON'da `__npy__:` ile referanslanır.
   - Her şey `.sflow` uzantılı bir ZIP olarak paketlenir.
2. **Güvenli Yükleme:**
   - Mevcut diyagramın **bellekte geçici bir yedeği** oluşturulur.
   - Yeni `.sflow` çıkarılır ve ayrıştırılır.
   - Herhangi bir hata oluşursa (geçersiz JSON, eksik düğümler, bozuk `.npy`), **yedek otomatik olarak geri yüklenir** ve iş kaybedilmez.
3. **Özel ScriptNode:**
   - `script`, `params`, `dynamic_inputs`, `dynamic_outputs`, `persist` ve `python_path` değerlerini kaydeder.
   - Yükleme sırasında kodu yeniden derler, dinamik portları yeniden oluşturur ve `persist` durumunu otomatik olarak geri yükler.
4. **StickyNotes ve DPI:**
   - Koordinatlar ve boyutlar kaydedilirken **96 dpi'ye normalize edilir**.
   - Yüklenirken mevcut monitörün DPI'sına ölçeklenir; bu da farklı çözünürlükler arasında görsel tutarlılık sağlar.

---

## 🛠️ Harici Kullanım ve Otomasyon

`.sflow` formatı şeffaf ve programlı olacak şekilde tasarlanmıştır. Harici betiklerden okuyabilir veya oluşturabilirsiniz:

=== "🐍 Python (Okuma)"
    ```python
    import zipfile
    import json
    import numpy as np

    with zipfile.ZipFile("mi-flujo.sflow") as z:
        graph = json.loads(z.read("diagram.json"))
        if "metadata.json" in z.namelist():
            meta = json.loads(z.read("metadata.json"))

        veri_n1 = np.load(z.open("data/node_1.npy"))
        print(f"Düğümler: {len(graph['nodes'])}")
        print(f"Veriler: {veri_n1.shape}")
    ```

=== "📤 Python (Temel Oluşturma)"
    ```python
    import zipfile
    import json
    import numpy as np

    graph = {
        "nodes": [{"id": "gen", "type": "GeneratorNode", "pos": [100, 100], "params": {}}],
        "connections": [],
        "viewport": {"x": 0, "y": 0, "scale": 1.0}
    }

    with zipfile.ZipFile("nuevo.sflow", "w", zipfile.ZIP_DEFLATED) as z:
        z.writestr("diagram.json", json.dumps(graph, indent=2))
        z.writestr("data/gen.npy", np.array([1.0, 2.0, 3.0]))
    ```

---

## 🔮 Uyumluluk ve Gelecekteki Genişletilebilirlik

`.sflow` formatı **genişletilebilir ve geriye uyumlu tasarım** ilkelerini takip eder:

- ✅ **Yeni bölümler:** Gelecek sürümler, eski yükleyicileri bozmadan `thumbnails/`, `logs/` veya `plugins/` gibi klasörler ekleyebilir.
- ✅ **İsteğe bağlı alanlar:** Ayrıştırıcı, `diagram.json`'daki bilinmeyen anahtarları yok sayar; bu da deneysel meta veriler eklenmesine olanak tanır.
- ✅ **Sürüm oluşturma:** `metadata.json`'daki `floworks_version` alanı, format evrilirse uygulamanın otomatik geçişler uygulamasını sağlar.

!!! warning "Altın Kural"
    Uygulama açıkken asla `diagram.json`'ı manuel olarak değiştirmeyin. Sistem, topoloji, diziler ve görünüm durumu arasındaki tutarlılığa bağlıdır. Her zaman yerel kaydetme/yükleme akışlarını kullanın.

---

## 📚 İlgili Kaynaklar
- [🗺️ Kod ve Mimari Haritası](architecture-ii.md) → `file_io.py` ve `SerializableMixin`'in formatı nasıl yönettiği.
- [📦 Taşınabilir Derleme Kılavuzu](guia-ejecutable-portable.md) → Kaynaklar için paketleme ve güvenli yollar.
- [🧩 Düğüm Teknik Referansı](node-reference.md) → Düğüm türüne göre serileştirme sözleşmeleri.
