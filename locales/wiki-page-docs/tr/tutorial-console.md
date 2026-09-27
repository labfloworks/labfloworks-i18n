# 🧪 FloWorks Etkileşimli Konsol Eğitimi

FloWorks deney laboratuvarına hoş geldiniz. Bu bölüm daha ileri düzey kullanıcılar içindir; tuvalle ilgili her şeyi, yani düğümleri ve bağlantılarını, sıralı olarak ve kod satırlarıyla kontrol etmek için bir **Python** terminalidir. Programa bağlı bir terminaldir ve program onu kontrol edebilir, daha talepkar kullanıcılar için davranışlar veya rutinler belirleyebilir.

Bu kılavuz, akış şemalarınızı fareye dokunmadan nasıl kontrol edip analiz edeceğinizi **adım adım** gösterecektir. Her örnek etkileşimli konsolda doğrulanmıştır ve programın gerçek veri yapısını yansıtır.

---

## 1. Ortamı tanımak

Konsol üç global nesne enjekte eder: `app` (ana pencere), `graph` (sahne/diyagram) ve `selected_node` (tuvalde şu anda seçili olan düğüm). Tüm komutlar bu üçünden çıkar.

### Tüm düğümleri görüntüleme

```python
>>> graph.nodes
```

**Örnek çıktı:**
```
Sahnedeki düğümler:
  [0] Gelişmiş Sinyal Jeneratörü (tür: SignalSourceNode, kategori: Sources)
  [1] FFT (tür: FFTNode, kategori: Processing)
```

Köşeli parantez içindeki indeks (`[0]`, `[1]`) bir düğüme erişmenin ana yolunuzdur. Sıra, tuvaldeki oluşturulma sırasıdır.

#### Alternatif: düğümleri sayma veya türe göre filtreleme

```python
>>> len(graph.nodes)
>>> [n for n in graph.nodes if 'FFT' in type(n).__name__]
```

### Tüm bağlantıları görüntüleme

```python
>>> graph.connections
```

**Örnek çıktı:**
```
Sahnedeki bağlantılar:
  [0] Gelişmiş Sinyal Jeneratörü (out) → FFT (input)
```

Çıktı, kaynak düğümün adını, çıkış portunu, oku, hedef düğümü ve giriş portunu gösterir. Bir bağlantı görünmüyorsa, akış çalıştırılamaz.

#### Alternatif: tek bir düğümün bağlantılarını görntüleme

```python
>>> selected_node.connectors
```

### Seçili düğümü görüntüleme

Tuvalde bir düğüme tıklayın ve ardından çalıştırın:

```python
>>> selected_node
```

**Örnek çıktı:**
```
Düğüm: FFT
  Tür: FFTNode
  Kategori: Processing
  Portlar: ['input', 'output', 'magnitude', 'phase']
```

> **💡 Not:** Hiçbir düğüm seçili değilse, `selected_node` değeri `None` olur. Bir düğüm seçmek, yan parametre tablosunu da otomatik olarak günceller.

#### Alternatif: kodla bir düğüm seçme

```python
>>> graph.nodes[0].setSelected(True)
>>> app.console.update_namespace(selected_node=graph.nodes[0])
```

---

## 2. Fare kullanmadan düğümleri ve bağlantıları manipüle etme

### Yeni düğüm oluşturma

Düğümün sınıf adının tam olarak bilinmesi gerekir (katalogdaki gibi). Argümanlar: `(tür, x, y)`.

```python
>>> graph.add_catalog_node('SumNode', 300, 200)
```

Düğüm, (300, 200) koordinatlarında tuvalde belirir. Tam adı bilmiyorsanız, kategorileri listeleyin (7. bölüme bakın).

#### Alternatif: aynı anda birden fazla düğüm oluşturma

```python
>>> for i, tur in enumerate(['SignalSourceNode', 'FFTNode', 'OscilloscopeNode']):
...     graph.add_catalog_node(tur, 100 + i*200, 300)
```

### Düğümleri elle bağlama

Sözdizimi: `graph.connect_nodes(kaynak, hedef, 'çıkış_portu', 'giriş_portu')`. Portlar her düğüme göre değişir; adlarını asla varsaymayın.

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', 'port_a')
```

> **💡 Not:** Bağlamadan önce her zaman `graph.nodes[N].PORTS` kontrol edin. Bir FFT düğümü `'input'` ve `'magnitude'` içerir; bir jeneratör `'output'` içerir.

#### Alternatif: varsayılan porta bağlama

Giriş portunun tam adını bilmiyorsanız, bazı düğümler ilk uygun olanı kullanmak için `None` kabul eder:

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', None)
```

### Düğüm silme

```python
>>> graph.remove_node(graph.nodes[2])
```

Düğümü ve tüm ilgili bağlantılarını siler. `graph.nodes` içindeki indeksler yeniden sıralanır, bu yüzden eski referansları saklamayın.

#### Alternatif: bir kategorideki tüm düğümleri silme

```python
>>> for n in list(graph.nodes):
...     if n.META.get('category') == 'Processing':
...         graph.remove_node(n)
```

### Bir düğümün portlarını görüntüleme

```python
>>> graph.nodes[1].PORTS
```

**Örnek çıktı:**
```
{'input': ('left', 0.5), 'output': ('right', 0.25), 'magnitude': ('right', 0.5), 'phase': ('right', 0.75)}
```

Anahtar, port adıdır (dize). Değer, görsel konum içeren bir demettir. Bağlamak için sadece anahtarlar ilginizi çeker.

#### Alternatif: portları basit liste olarak görüntüleme

```python
>>> list(graph.nodes[1].PORTS.keys())
```

---

## 3. Akışı çalıştırma ve sonuçları görüntüleme

### Tüm grafiği çalıştırma

```python
>>> graph.execute_flow()
```

Bu yöntem diyagrama (`graph`) aittir, ana pencereye değil. Tüm düğümleri topolojik sırayla dolaşır, her birini çalıştırır ve sonuçları önbelleğe alır. Hiçbir şey döndürmez; veriler dahili olarak saklanır.

#### Alternatif: belirli bir dalın hesaplamasını zorlama

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
```

Bu, belirtilen düğümün yukarı akışındaki tüm ağacı yeniden hesaplar ve sonucu doğrudan, global önbelleği değiştirmeden döndürür.

### Bir düğümün önbelleğe alınmış verilerini görüntüleme

Belirli bir düğümün işlenmiş verilerine erişmeniz gerekiyorsa, bunu iki şekilde yapabilirsiniz:

#### Doğrudan yol (nesneye göre)
```python
>>> graph.node_values[graph.nodes[1]]
```

**Örnek çıktı:**
```
{'output': None, 'magnitude': (array([0., 78.125, ...]), array([0.0013, 0.0183, ...])), 'phase': (array([0., 78.125, ...]), array([0., 0.687, ...]))}
```

> **⚠️ Uyarı:** Sahnedeki düğüm sırası değişirse (örneğin düğüm silme veya ekleme) veya nesne örneği sözlükte saklanan anahtarla tam olarak eşleşmezse, bu yol `KeyError` ile başarısız olabilir.

#### Alternatif yol (konuma göre)
```python
>>> list(graph.node_values.values())[1]
```

Bu yol **daha kararlıdır** çünkü nesnenin tam kimliğine bağlı değildir. Değerlerin sırası, son `graph.execute_flow()` sırasında düğümlerin çalıştırıldığı sırayı izler. `[1]` indeksi, bu sıradaki ikinci düğüme karşılık gelir.

> **💡 Not:** Her düğümün çalıştırma sırasındaki indeksini görmek isterseniz şunu kullanabilirsiniz:
> ```python
> >>> list(graph.node_values.keys())
> ```

> **⚠️ Dikkat:** `graph.node_values` her zaman doğrudan bir dizi döndürmez. İşlemci düğümler için (FFT, filtreler vb.) her anahtar bir çıkış portu olan bir **sözlük** döndürür. Kaynak düğümler için `(x, y)` demeti döndürür.

#### Alternatif: tüm düğümlerin verilerini tek satırda görüntüleme
```python
>>> {n.name: type(v).__name__ for n, v in graph.node_values.items()}
```

### Kaynak düğümün Y eksenine erişim

Kaynak düğümler (SignalSourceNode, FileInputNode vb.) çalıştırıldığında `(zaman, sinyal)` demeti döndürür. Sadece Y eksenini elde etmek için:

```python
>>> result = graph.get_node_branch_value(graph.nodes[0])
>>> y = result[1]
>>> y.max()
```

**Örnek çıktı:**
```
Scalar NumPy (float64): 1.0
```

#### Alternatif: X eksenini (zaman) elde etme

```python
>>> x = result[0]
>>> x[:5]
```

### Python'da atamalar: hayati bir ayrıntı

Python'da atamalar (`=`) **ifadeler** değil, **deyimlerdir**. Konsol `x, y = ...` sonrası hiçbir şey yazdırmaz çünkü bir dönüş değeri yoktur.

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
```

Çalıştığını doğrulamak için bir sonraki satırda değişkeni değerlendirin:

```python
>>> x
>>> y.shape
```

Veya aynı satırda bir ifadeyi zincirlemek için `;` kullanın:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); y.max()
```

Veya açık `print()` kullanın:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); print(y.max())
```

---

## 4. Ana grafikte çizim

### Grafiği temizleme

```python
>>> app.plot_widget.clear_plot()
```

#### Alternatif: temizle ve hemen yeniden çiz

```python
>>> app.plot_widget.clear_plot(); graph.execute_flow()
```

### Konsoldan rastgele bir sinyal çizme

NumPy ile diziler oluşturabilir ve bunları doğrudan çizim widget'ına, hiçbir düğümden geçirmeden gönderebilirsiniz.

```python
>>> import numpy as np
>>> x = np.linspace(0, 1, 1000)
>>> y = np.sin(2 * np.pi * 10 * x)
>>> app.plot_widget.plot_waveform(x, y)
```

#### Alternatif: sinüs toplamı çizme

```python
>>> y = np.sin(2*np.pi*5*x) + 0.3*np.sin(2*np.pi*50*x) + 0.1*np.random.randn(1000)
>>> app.plot_widget.plot_waveform(x, y)
```

### Kaynak düğümünün sonucunu çizme

Kaynak düğümü `(x, y)` döndürdüğü için doğrudan açılabilir:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x, y)
```

### FFT düğümünün sonucunu çizme (çoklu portlar)

Çoklu çıkışlı düğümler (FFT, zaman-frekans analizi vb.) basit bir demet döndürmez. Her anahtar bir çıkış portu olan bir `dict` döndürür.

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
>>> result.keys()
```

**Örnek çıktı:**
```
dict_keys(['output', 'magnitude', 'phase'])
```

`'output'` düğümün genel bir portu yoksa `None` olabilir. Kullanışlı çıkışlar `'magnitude'` ve `'phase'` olup, bunlar da `(frekanslar, değerler)` demetleridir:

```python
>>> f, mag = result['magnitude']
>>> app.plot_widget.plot_waveform(f, mag)
```

#### Alternatif: büyüklük yerine faz çizme

```python
>>> f, phase = result['phase']
>>> app.plot_widget.plot_waveform(f, phase)
```

#### Alternatif: iki sinyali üst üste bindirme

```python
>>> x1, y1 = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x1, y1)
>>> x2, y2 = graph.get_node_branch_value(graph.nodes[2])  # başka düğüm
>>> app.plot_widget.plot_waveform(x2, y2)  # üst üste biner
```

> **💡 Not:** Bir dict ile `x, y = result` yapmaya çalışırsanız, Python `ValueError: too many values to unpack` atar. Açmadan önce her zaman `type(result)` ve `result.keys()` kontrol edin.

---

## 5. Uygulamayı çalışırken değiştirme

### Yan parametre tablosunu güncelleme

Bir parametreyi kodla değiştirirseniz ve yan tablonun değişikliği yansıtmasını isterseniz:

```python
>>> app.workspace_table.populate()
```

Bu yöntem argüman almaz. Tabloyu seçili düğümün güncel değerleriyle yeniler.

#### Alternatif: başka bir düğüm seçmeye zorlama ve yenileme

```python
>>> graph.nodes[1].setSelected(True)
>>> app.workspace_table.populate()
```

### Araç çubuğundan düğüm ekleme

```python
>>> app.add_node('SignalSourceNode')
```

Araç çubuğundaki "+" düğmesine basmaya eşdeğerdir. Düğüm tuvalde varsayılan bir konuma yerleştirilir.

### Pencere başlığını değiştirme

`setWindowTitle` Qt'nin yerel yöntemidir. Çalışır, ancak uygulamanın `update_title()` çağıran ve otomatik olarak üzerine yazan bir zamanlayıcı veya olayı olabileceğini unutmayın.

```python
>>> app.setWindowTitle('Sinyal laboratuvarım')
>>> app.windowTitle()
```

Uygulamanın iç durumundan (proje adı, dosya vb.) hesapladığı "resmi" başlığı geri yüklemek için:

```python
>>> app.update_title()
```

#### Alternatif: proje adıyla başlık

```python
>>> app.setWindowTitle(f'FloWorks — {graph.nodes[0].name}')
```

---

## 6. Konsolda gezinme ve verimlilik

Konsol sadece bir `print()` değildir. Geçmiş, otomatik tamamlama ve çok satırlı bloklar vardır.

| Tuş / Komut           | Eylem                                                         |
|---------------------------|----------------------------------------------------------------|
| `↑` / `↓`                 | Çalıştırılan komut geçmişinde gezinme                |
| `Tab`                     | Ad uzayı değişken, öznitelik ve yöntemlerini otomatik tamamlama     |
| `Ctrl + L`                | Tüm konsolu temizleme (metni siler, Python durumunu değil)  |
| `if`, `for`, `def`, `class` | `>>>` yerine `...` olur çok satırlı bloklar için     |
| `Ctrl+C` (seçimde)   | Konsol metnini kopyalama                                     |
| `Ctrl+A`                  | Tüm içeriği seçme                                  |

> **💡 Not:** Otomatik tamamlama `rlcompleter` kullanır ve enjekte edilen tüm ad uzayını (`app`, `graph`, `selected_node`) artı oturumda tanımladığınız herhangi bir değişkeni tanır.

---

## 7. İleri düzey tarifler

### Bir düğümün iç parametresini değiştirme

Düğüm parametreleri düz öznitelikler değildir. `'preset'`, `'formula'` veya `'advanced'` gibi alt bölümleri olan `params` sözlüğü içinde iç içedir. Asla `dugum.amplitude = 3.0` yapmayın; bu nesnede yeni bir öznitelik oluşturur ancak gerçek parametreyi değiştirmez.

#### Durum A: preset değiştirme (sinüs, kare vb.)

```python
>>> selected_node.params['mode'] = 'preset'
>>> selected_node.params['preset']['type'] = 'SINE'
>>> selected_node.params['preset']['amplitude'] = 2.0
>>> selected_node.params['preset']['frequency'] = 1000.0
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Durum B: özel formül kullanma

```python
>>> selected_node.params['mode'] = 'formula'
>>> selected_node.params['formula']['expr'] = '2 * sin(2*pi*1000*t)'
>>> selected_node.params['formula']['vars'] = {'amp': 2.0, 'freq': 1000.0, 'offset': 0.0}
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

İfade zaman değişkeni olarak `t` kullanır. `vars` içindeki değerler, formülde başvurabileceğiniz sembollerdir. `vars` atlarsanız, düğüm varsayılan değerleri kullanır ve formül değişikliği yansıtmayabilir.

#### Durum C: gelişmiş parametreleri değiştirme (örnekleme hızı, süre)

```python
>>> selected_node.params['advanced']['duration'] = 0.02
>>> selected_node.params['advanced']['sample_rate'] = 44100
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Durum D: jeneratör olmayan düğümün parametresini değiştirme (örn. FFT)

```python
>>> selected_node.params['window'] = 'hann'
>>> graph.execute_flow()
```

> **💡 Not:** `getattr(obj, '_generate_signal', lambda: None)()` güvenli bir kalıptır: yöntem varsa (jeneratör düğümleri) çağırır; yoksa hiçbir şey yapmaz ve hata atmaz. İşlemci düğümleri için yalnızca `graph.execute_flow()` yeterlidir.

### Mevcut tüm düğüm kategorilerini listeleme

Import kataloğu yükler ancak otomatik göstermez. Python'da başarılı bir import hiçbir şey yazdırmaz; nesneyi değerlendirmeniz gerekir.

```python
>>> from nodes.node_catalog import NODE_CATEGORIES
>>> NODE_CATEGORIES
```

Okunabilir bir özet için:

```python
>>> for cat, dugumler in NODE_CATEGORIES.items():
...     print(f"{cat}: {len(dugumler)} dugum")
```

#### Alternatif: kategoriye göre düğüm adlarını listeleme

```python
>>> {cat: [n.__name__ for n in dugumler] for cat, dugumler in NODE_CATEGORIES.items()}
```

### Herhangi bir yöntemin yardımını görüntüleme

```python
>>> help(graph.connect_nodes)
```

Docstring doğrudan konsolda görünür. Kaynak kodunu açmadan bir yöntemin hangi argümanları beklediğini keşfetmek için kullanışlıdır.

#### Alternatif: filtrelenmiş öznitelikleri görüntüleme

```python
>>> [m for m in dir(graph) if 'connect' in m.lower()]
>>> [m for m in dir(selected_node) if 'param' in m.lower()]
```

---

## 8. Bir şeyler ters giderse ne yapmalı?

- **Konsolda kırmızı hata:** traceback tam olarak gösterilir. Uygulama kapanmaz; komutu düzeltebilir ve yeniden deneyebilirsiniz.
- **Arayüz donuyor:** muhtemelen sonsuz bir döngü yazdınız. Konsol ayrı bir iş parçacığında çalışır, ancak döngü GUI iş parçacığını etkilerse uygulamayı yeniden başlatın.
- **Beklenmeyen `None`:** bir düğüm veri yerine `None` döndürüyorsa, yukarı akışa bağlı olduğunu (`graph.connections`) ve akışın çalıştırıldığını (`graph.execute_flow()`) doğrulayın.
- **`ValueError: too many values to unpack`:** bir dict'i demet gibi açmaya çalışıyorsunuz. Önce `result.keys()` kullanın.
- **`ValueError: not enough values to unpack`:** 2 değer bekliyorsunuz ancak düğüm 1 (dict) veya 3 (spektrogram) döndürüyor. Açmadan önce `type(result)` ile inceleyin.
- **`AttributeError`:** nesnenin bu özniteliği yok. Doğru adı keşfetmek için `dir(obj)` veya `[a for a in dir(obj) if 'kelime' in a.lower()]` kullanın.
- **Çalıştırmada hiçbir şey olmuyor:** zincire bağlı en az bir kaynak düğüm olduğunu ve `graph.execute_flow()` çağrıldığını kontrol edin. İşlemci düğümleri tek başına veri üretmez.
- **Çizim değişmiyor:** parametreleri değiştirdikten sonra `graph.execute_flow()` çağırdığınızdan emin olun. Sadece `params` değiştirmek otomatik olarak yeniden hesaplamaz.

---

© 2026 FloWorks — Sinyal Laboratuvarı
