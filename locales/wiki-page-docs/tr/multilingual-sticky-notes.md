# Yapışkan Notlar (Sticky Notes) – Kullanıcı Kılavuzu

## Yapışkan notlar nedir?

Yapışkan notlar (veya *sticky notes*), diyagram üzerinde özgürce yerleştirebileceğiniz küçük metin bloklarıdır. Şunlar için kullanılır:

- Tuval üzerine doğrudan hatırlatıcılar, başlıklar veya açıklamalar eklemek.
- Projenizi kullanan kişiyi yönlendiren adım adım öğreticiler oluşturmak.
- FloWorks'ten çıkmadan iş akışının bölümlerini belgelemek.
- Kendiniz veya diğer işbirlikçiler için yorumlar bırakmak.

Notlar yeniden boyutlandırılabilir (köşelerini sürükleyerek), diyagramın herhangi bir yerine taşınabilir ve proje ile birlikte kaydedilir. Bir `.sflow` dosyası açıldığında, tüm notlar tam olarak bıraktığınız yerde görünür.

---

## Yenilik: çok dilli notlar

Yapışkan notlar, uygulama için seçtiğiniz dilde metni otomatik olarak görüntüleyebilir.  
Son mesajı tek bir dilde yazmak yerine, FloWorks dil değiştiğinde kendiliğinden çevrilecek **özel işaretçiler** ekleyebilirsiniz.

Böylece aynı not, metni her seferinde düzenleme gereği duymadan İspanyolca, İngilizce veya mevcut başka bir dilde okunabilir.

---

## Çok dilli not nasıl yazılır

Bir notun içinde (çift tıklayarak veya Araç çubuğundaki 📝 düğmesiyle oluşturun) iki tür işaretçi kullanabilirsiniz:

### 1. `tr(…)` kelimesi ile
`tr("anahtar")` yazın ve `anahtar` yerine ifade için açıklayıcı bir ad verin.

Örnek:

```
tr("tutorial.paso1.titulo")
tr("tutorial.paso1.mensaje")
```

### 2. Çift süslü parantez `{{…}}` ile
Aynı şekilde `{{anahtar}}` yazın.

Örnek:

```
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

Her iki biçim de aynı şekilde çalışır; size daha uygun olanı seçin (hatta aynı notta birleştirebilirsiniz).

> **Önemli**: Notu düzenlerken gördüğünüz metin, orijinal işaretçileri içerir (örneğin `{{tutorial.paso1.titulo}}`).  
> Düzenlemeyi bitirip diyagramın normal görünümüne döndüğünüzde, işaretçiler uygulamanın geçerli diline çevrilmiş ifadeyle değiştirilir.

---

## Dil değiştiğinde davranış

- FloWorks menüsünden dil değiştirdiğinizde (örneğin İspanyolca'dan İngilizce'ye), **işaretçi içeren tüm yapışkan notlar otomatik olarak güncellenir**.
- Projeyi kapatıp yeniden açmanız veya her notu tek tek elle düzenlemeniz gerekmez.
- Yalnızca normal metin içeren notlar (işaretçi olmayan) etkilenmez; her dilde aynı şeyi gösterir.

---

## İşaretçi kullanmanın avantajları

- **Anında çok dilli öğreticiler** – Aynı not, farklı dillerdeki kullanıcılara rehberlik edebilir.
- **Tutarlılık** – Çeviriyi tek bir yerde (geliştirme ekibinizin yönettiği dil dosyasında) değiştirirseniz, bu anahtarı kullanan tüm notlar güncellenir.
- **Kolay bakım** – İçeriği bir kez yazıp birden fazla notta yeniden kullanabilirsiniz.
- **Esneklik** – Sabit metin ile işaretçileri birleştirin. Örneğin:

```
🎯 ADIM 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

---

## Pratik örnek: adım adım bir öğretici

Bir öğreticinin ilk adımını açıklayan bir not eklemek istediğinizi varsayalım.  
Düzenleme modunda şunu yazarsınız:

```
🎯 ADIM 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

Düzenlemeyi bitirip uygulamayı İspanyolca kullanırken şunu görürsünüz:

```
🎯 PASO 1
¡Bienvenido a FloWorks!
Arrastre un nodo fuente de señal para comenzar.
```

Dili İngilizce'ye değiştirirseniz, aynı not şunu gösterir:

```
🎯 STEP 1
Welcome to FloWorks!
Drag a signal source node to begin.
```

Ve yapılandırılmış diğer herhangi bir dil için de aynı şekilde devam eder.

---

## Özet

- Yapışkan notlar, diyagramlarınızı metinsel bilgilerle zenginleştirir.
- Artık `tr("anahtar")` veya `{{anahtar}}` işaretçileriyle **çok dilli** olabilirler.
- Düzenlerken anahtarları görürsnüz; görüntülerken çevrilmiş metni.
- Uygulama dilini değiştirin, tüm notlar anında uyum sağlar.
- Birden fazla dilde çalışması gereken görsel belgeler, öğreticiler veya uyarılar için mükemmeldir.

Projelerinizi daha erişilebilir ve paylaşılabilir kılmak için bu özelliği kullanın!
