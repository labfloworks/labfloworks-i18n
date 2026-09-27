# 📊 Hesap Tablosu Eğitimi

Excel tarzı, hafif ve gömülebilir bir hesap tablosu.

---

## 1. Spreadsheet Nedir?

Hesap tablosu bileşeni şunları sunar:

- Formül destekli hücreler (`=` ile başlar)
- Önceden tanımlı fonksiyonlar (TOPLA, ORTALAMA, KOŞULLU, vb.)
- Aritmetik, mantıksal ve karşılaştırma operatörleri
- Uygulamalara entegrasyon için ideal minimalist arayüz

---

## 2. Temel Navigasyon

- Bir hücreyi seçmek için üzerine tıklayın.
- Metin veya sayı girmek için doğrudan yazın.
- **Formül** yazmak için `=` ile başlayın (örn. `=SUM(A1:A5)`).
- Düzenlemeyi onaylamak için **Enter** tuşuna basın.
- Hareket etmek için ok tuşlarını veya fareyi kullanın.

---

## 3. Formüller: Kullanılabilir Fonksiyonlar

Tüm fonksiyonlar büyük harfle yazılır ve aralıkları (örn. `A1:A5`) veya virgülle ayrılmış argümanları destekler.

| Fonksiyon | Ne yapar | Örnek |
| --------- | -------- | ------- |
| `SUM` | Sayıları toplar | `=SUM(A1:A5)` |
| `AVG` / `AVERAGE` | Ortalama | `=AVG(A1:A5)` |
| `COUNT` | Sayıları sayar (boş olmayan) | `=COUNT(A1:A5)` |
| `MAX` | Maksimum değer | `=MAX(A1:A5)` |
| `MIN` | Minimum değer | `=MIN(A1:A5)` |
| `ABS` | Mutlak değer | `=ABS(A1)` |
| `ROUND` | Yuvarlar (2. argüman = ondalık basamak) | `=ROUND(A1, 2)` |
| `IF` | Koşullu (eğer, o zaman, değilse) | `=IF(A1>10, "Evet", "Hayır")` |
| `CONCAT` / `CONCATENATE` | Metinleri birleştirir | `=CONCAT(A1, " ", B1)` |
| `LEN` | Metin uzunluğu | `=LEN(A1)` |
| `INT` | Tam kısım | `=INT(A1)` |
| `SQRT` | Karekök | `=SQRT(A1)` |

### 3.1. Fonksiyon notları

- Aralıklar **iki nokta üst üste** ile belirtilir: `A1:A5`, A1'den A5'e kadar olan tüm hücreleri içerir.
- Fonksiyonlar iç içe kullanılabilir: `=SUM(A1:A5) + MAX(B1:B5)`.
- Metin argümanları çift veya tek tırnak içinde olmalıdır.

---

## 4. Fonksiyonsuz operatörler

Fonksiyonlara ek olarak, formül içinde doğrudan operatör kullanabilirsiniz. Sözdizimi Python'a benzer.

### 4.1. Aritmetik

| İşlem | Örnek |
| ------- | ------- |
| Toplama | `=A1+A2+A3` |
| Çıkarma | `=A1-A2` |
| Çarpma | `=A1*B1` |
| Bölme | `=A1/B1` |
| Modül | `=A1%B1` |
| Üs | `=A1**2` (veya `=A1^2`) |

### 4.2. Karşılaştırmalar

`True` veya `False` döndürür (`Doğru` / `Yanlış` olarak gösterilir).

| Operatör | Anlamı | Örnek |
| -------- | ------ | ----- |
| `>` | Büyüktür | `=A1>B1` |
| `<` | Küçüktür | `=A1<B1` |
| `>=` | Büyük eşit | `=A1>=10` |
| `<=` | Küçük eşit | `=A1<=10` |
| `==` | Eşit | `=A1==B1` |
| `!=` | Eşit değil | `=A1!=B1` |

### 4.3. Mantıksal ve koşullu

`and`, `or`, `not` ile koşulları birleştirebilirsiniz.

```excel
= A1>5 and B1<10
= not(A1==0)
= 10 if A1>5 else 0
```

`if` `else` üçlü operatörü de doğrudan desteklenir.

---

## 5. Pratik örnekler

### 5.1. Satış toplamı

`B2:B10` arasında satışlarınız olduğunu ve toplamı istediğinizi varsayalım:

```excel
=SUM(B2:B10)
```

### 5.2. Koşullu indirim

Toplam (`B12`) 100'ü aşarsa %10 indirim uygula; aksi halde 0:

```excel
= IF(B12>100, B12*0.9, B12)
```

### 5.3. Ortalama ve sayma

`C2:C20` arasındaki notların ortalaması, ancak en az 5 değer varsa:

```excel
= IF(COUNT(C2:C20)>=5, AVG(C2:C20), "Yetersiz veri")
```

### 5.4. Birleştirilmiş metin

Ad (A2) ve soyadı (B2) arasına boşluk koyarak birleştirme:

```excel
= CONCAT(A2, " ", B2)
```

### 5.5. Bir sayının karekökü

```excel
= SQRT(A1)
```

### 5.6. 2 ondalık basamağa yuvarlama

```excel
= ROUND(A1, 2)
```

---

## 6. İpuçları ve püf noktaları

- **Göreli/mutlak referanslar:** şimdilik tüm referanslar görelidir (Excel gibi). `$A$1` henüz desteklenmiyor.
- **Dinamik aralıklar:** `A:A` (tüm sütun) veya `1:1` (tüm satır) gibi aralıklar kullanabilirsiniz.
- **Otomatik tamamlama:** `=` yazarken mevcut fonksiyonların menüsü görünür.
- **Hatalar:** bir formül geçersizse, hücre `#ERROR` gösterir ve ayrıntılı mesaj durum çubuğunda görünür.
- **Yeniden hesaplama:** formüller, bağımlı hücreler değiştirildiğinde otomatik güncellenir.

---

## 8. Sık sorulan sorular

**Veriler nasıl dışa aktarılır?**
Şimdilik yerel dışa aktarım yok, ancak verilere dahili model üzerinden erişebilirsiniz.

**Grafik destekliyor mu?**
Hayır, bu temel bir hesap tablosudur. Görselleştirme için diğer widget'larla birleştirebilirsiniz.

---

Hafif hesap tablosunu kullanmanın tadını çıkarın!

© 2026 — FloWorks
