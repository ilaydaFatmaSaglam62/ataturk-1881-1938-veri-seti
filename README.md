
# 1881–1938 | Bir Ömrün Ardında Bıraktıkları

Mustafa Kemal Atatürk'ün doğumundan (1881) ölümüne (10 Kasım 1938) uzanan hayatını **tarih sıralı, sorgulanabilir bir veri seti** olarak ele alan kişisel bir veri analizi projesi. Veri seti, 10 Kasım için hazırladığım kısa videonun da dayanağıdır.

> 🎬 **Video:** [https://www.instagram.com/reel/DfjLqA0uG3e/?utm_source=ig_web_copy_link&mdxt=MzRlODBiNWFlZA==&srtk=MzRlODBiNWFlZA==]  

---

## Neler var?

| Sayfa | İçerik |
|---|---|
| `events_clean` | **259 olay.** Temizlenmiş tarihler, standart kategori, yer, doğrulama sonucu |
| `claims_clean` | **291 kaynak iddiası.** Her olayın kaynağında geçen tarih ve not |
| `sources` | **26 kaynak.** Ad, tür, bağlantı, erişim tarihi |
| `kaynak_bulgulari` | **197 bulgu.** Her olayın tarihinin, bağımsız kaynaklarla karşılaştırması (Uyumlu / Farklı / Kısmen) |
| `dogrulama_kaynaklari` | Karşılaştırmada kullanılan 11 kaynak ve güvenilirlik notları |
| `dogrulama_ozeti` | Doğrulama sonuçlarının özeti (formüllerle hesaplanır) |
| `tarih_duzeltme_logu` | Yapılan her tarih düzeltmesinin kaydı (orijinal değer → düzeltilen değer, yöntem) |
| `kategori_haritasi` | Orijinal kategorilerin 13 standart kategoriye eşlemesi |
| `dogrulama` | Veri setinin iç tutarlılık kontrolleri |
| `OKU_BENI` | Dosya içi kısa açıklama |

### Önemli sütunlar (`events_clean`)

| Sütun | Açıklama |
|---|---|
| `event_id` | Olay kimliği (EVT-001 …) |
| `event_date` / `tarih_metni` | Temizlenmiş tarih ve okunur hâli |
| `tarih_duzeyi` | Tarihin kesinliği: Yıl, Ay veya Gün |
| `event_name`, `location` | Olayın adı ve yeri |
| `category_std` | 13 standart kategoriden biri |
| `dogrulama_durumu` | Teyit edildi (2+ / 1 kaynak), Çoğunlukla teyit, Kaynaklar arası fark, Doğrulanamadı |
| `kaynaklara_gore_olasi_tarih` | Kaynaklar arasında fark varsa önerilen tarih |
| `resmi_kaynak_kontrolu` | Resmî kaynakla kontrol edilmesi gereken kayıtlar için alan |

## Veri temizliği

Ham dosyada birçok tarih Excel seri numarası olarak (ör. `772`) kaydedilmişti. Bu kayıtlar takvim tarihine çevrildi ve **her dönüşüm `tarih_duzeltme_logu` sayfasında yöntemiyle birlikte** tutuluyor. Orijinal değerler (`event_date_orijinal`, `verification_status`) değiştirilmeden saklandı. Düzeltmeler ayrı sütunlara yazıldı.

## Tarihsel doğrulama ve sınırları

Olay tarihleri, erişilebilen üniversite ve kurum kaynaklarıyla olay olay karşılaştırıldı.

| Sonuç | Olay sayısı |
|---|---|
| Teyit edildi | 90 |
| Çoğunlukla teyit | 1 |
| Kaynaklar arası fark | 19 |
| Doğrulanamadı (kaynak bulunamadı) | 149 |

**Dürüst bir not:** Bu doğrulama *ikincil kaynaklarla karşılaştırmadır*, **resmî bir tarih tespiti değildir.** Birincil/resmî kaynaklara (MSB, ATAM, Atatürk Ansiklopedisi) erişilemediği için "Kaynaklar arası fark" olarak işaretlenen kayıtların bu kurumlardan ayrıca kontrol edilmesi gerekir. `resmi_kaynak_kontrolu` sütunu bu iş için ayrılmıştır. Hata veya eksik görürseniz lütfen bir *Issue* açın.

## Örnek sorgular

**Excel:** Kategoriye göre olay sayısı için `kategori_haritasi` sayfasındaki `olay_sayisi` sütunu hazırdır. Kendi analizin için `events_clean` üzerinde `COUNTIFS` kullanabilirsin.

**Python (pandas):**

```python
import pandas as pd

df = pd.read_excel("Ataturk_Arsiv_Dogrulanmis.xlsx", sheet_name="events_clean")

# Yıllara göre olay sayısı
print(df.groupby("yil").size())

# Kategorilere göre dağılım
print(df["category_std"].value_counts())

# Kaynaklar arası farkı olan olaylar
print(df[df["dogrulama_durumu"] == "Kaynaklar arası fark (inceleme)"][["event_id", "tarih_metni", "event_name"]])
```

> Not: Dosyadaki bazı sütunlar formüllerle hesaplanır. pandas bunları okurken boş gelebilir. Böyle bir durumda dosyayı Excel'de bir kez açıp kaydet.

## Lisans

Veri seti **CC BY 4.0** ile paylaşılmıştır: kaynak göstererek kullanabilir, paylaşabilir ve üzerine çalışabilirsiniz. Videodaki fotoğraflar bu lisansın kapsamında **değildir** ve bu depoda yer almaz.

## Hazırlayan

**İlayda Fatma Sağlam**, Yönetim Bilişim Sistemleri mezunu, ERP alanında çalışıyor.  
LinkedIn:[www.linkedin.com/in/ilayda-fatma-sağlam]

---

*Bu proje bir tarih araştırması değil, bir veri analizi çalışmasıdır. Amaç, bir hayatı sayılarla ve tarihlerle sorgulanabilir hâle getirmektir.*
