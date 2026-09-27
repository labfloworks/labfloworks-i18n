---
title: Proje yönetimi
description: FloWorks'te akışlarınızı nasıl kaydedeceğiniz, açacağınız, dışa aktaracağınız ve koruyacağınız.
---

# 📁 Proje yönetimi

FloWorks, akışlarınızı **`.sflow`** uzantılı dosyalarda saklar. Bu dosyalar, proje hakkındaki tüm bilgileri içerir: düğümler, bağlantılar, yapılandırma ve yapışkan notlar.

---

## Oluşturma, açma ve kaydetme

| Eylem | Menü | Kısayol |
|--------|------|-------|
| **Yeni proje** | Dosya → Yeni | `Ctrl + N` |
| **Proje aç** | Dosya → Aç | `Ctrl + O` |
| **Kaydet** | Dosya → Kaydet | `Ctrl + S` |
| **Farklı kaydet…** | Dosya → Farklı kaydet… | `Ctrl + Shift + S` |

**Altın kural:**
Akışlar, FloWorks'ün **tüm sürümleriyle tamamen uyumlu**dur (Core, Lite, Pro). Hiçbir şeyi dönüştürmeniz veya değiştirmeniz gerekmez: sadece açın ve çalıştırın.

---

## Dışa aktarma ve içe aktarma

- Bir **akışı paylaşmak** için `.sflow` dosyasını başka bir cihaza kopyalayın.
- **Harici bir akış getirmek** için **Dosya → Aç**'ı kullanın ve dosyayı seçin.
- **Sayısal verileri dışa aktarmanız** gerekirse (örneğin CSV'ye), yan paneldeki **Hesap tablosu** aracını kullanın ve tabloyu oradan kaydedin.

---

## Beklenmedik kapanmalarda kurtarma

FloWorks **otomatik olarak kaydetmez**. Bu yüzden şunlar önemlidir:

- Sık sık kaydetmek (`Ctrl + S`), özellikle gerçek donanımla akış çalıştırmadan önce.
- Uygulama beklenmedik şekilde kapanırsa, kaydedilmemiş değişiklikler kaybolabilir.
- Tam bir iç huzuruyla çalışmak için, her önemli değişiklikten sonra kaydetmeye alışın.

---

## Önerilen organizasyon

- Her proje veya müşteri için bir klasör oluşturun ve ilgili tüm `.sflow` dosyalarını oraya kaydedin.
- Akışın bölümlerini belgelemek için tuval üzerinde **yapışkan notlar** kullanın.
- Haftalar sonra akışı bulmayı ve anlamayı kolaylaştırmak için düğümlere **açıklayıcı adlar** atayın (çift tıklama → ad).

---

## İyi uygulamalar

- Gerçek enstrümanlarla bir akış çalıştırmadan önce dosyayı kaydedin.
- Ekip halinde çalışıyorsanız, önemli akışların üzerine yazmamak için bir sürüm kontrol sistemi (Git, manuel kopyalar) kullanın.
- Kalibrasyon veya kritik sistemlerin tanı akışlarının yedeklerini alın.

---

> **İpucu:** İyi organize edilmiş ve kaydedilmiş bir akış, FloWorks'te profesyonel çalışmanın temelidir. Net bir adın ve düzenli bir klasörün gücünü hafife almayın.
