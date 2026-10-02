# 12 – Panel Tipi Seçimi (Karar Önerisi)

> Ekip panel tipine karar vermedi ve seçimi rapora bıraktı. Bu belge **raportörün önerisidir**; ekip itiraz etmezse geçerli kabul edilir, aksi hâlde güncellenir.

## 1. Seçim Kriterleri (bu projeye özgü)
1. **Ağırlık:** 60 cm gemide ve tork sınırlı mekanizmada hafif olmalı (05 §1.1–1.2).
2. **Suya batabilme:** Panel suya inecek → kapsüllenebilir, kenarları sızdırmaz yapılabilir olmalı.
3. **Verim/alan:** Sınırlı alan → birim alan başına yüksek güç.
4. **Elektrik uyumu:** 3S batarya + MPPT (Vmp ≈ 18 V sınıfı, 12 V sistem).
5. **Tedarik ve maliyet:** Kolay bulunur, fiyat makul.
6. **Mekanik dayanım:** Dalga, çarpma, titreşim.

## 2. Seçenek Karşılaştırması (1 kötü – 5 iyi)

| Kriter | Cam + alüminyum çerçeveli mono/poli | **Esnek ETFE monokristal (laminasyonlu)** | Epoksi/reçine kaplı küçük hücre panelleri | İnce film (CIGS/amorf) esnek |
|---|---|---|---|---|
| Ağırlık (10 W için) | 1 (≈ 0.6–0.8 kg) | **5 (≈ 0.2–0.35 kg)** | 4 (≈ 0.2–0.4 kg) | 4 |
| Suya dayanım (kapsül) | 2 | **4** (kenar mühürlenirse) | 3–4 | 3 |
| Verim / alan | 4 | **5** (%20+ olabilir) | 3 | 1–2 |
| Mekanik dayanım | 3 (kırılgan cam) | **4** | 2 (kırılgan) | 4 |
| Tedarik / fiyat | 5 | 3 | 5 | 2 |
| Elektrik (Vmp ~18 V seçenek) | 5 | 5 | 2 (çoğu 5–6 V) | 3 |
| **Toplam** | 20 | **26** | 19–20 | 17–18 |

## 3. Karar

**Seçilen tip: ~10 W, 12 V sınıfı (Vmp ≈ 18 V), esnek ETFE laminasyonlu monokristal güneş paneli.**

| Özellik | Hedef |
|---|---|
| Güç | ~10 W / panel (etiket Pmax ≥ 10 W) |
| Boyut | ~ 250 × 170 mm (alan ≈ 0.04 m²) |
| Ağırlık | ≤ 0.35 kg / panel |
| Elektrik | Vmp ≈ 18 V, Imp ≈ 0.55 A, Voc ≈ 21–22 V |
| Bağlantı | Kablo çıkışı lehimli/ konnektörsüz; çıkış noktası **ek sızdırmazlık** ile mühürlenecek |
| Kapsül | Kenar ve kablo çıkışı epoksi/silikon; arkada sert destek plakası |

**Destek yapısı:** Esnek panel eğilmemeli (hücre çatlar). Panelleri 2–3 mm **PVC köpük levha / akrilik / karbon plaka** üzerine yapıştırın (hafif, suya dayanıklı); çerçeve olarak alüminyum profil veya 3B baskı PETG.

## 4. 115 × 90 Paneller ile Uyum
- Bu ölçüdeki paneller doğrulanana kadar **≤ 2 W** kabul edilir (04 §8).
- **Ana enerji panelleri olarak kullanılmaz.** Kullanım önerisi: (a) **test panelleri**: suda ve havada üretim deneyi (R1), (b) küçük yardımcı yükler (LED/sensör), (c) mekanizma ağırlık maketi.
- Ana enerji için yukarıdaki tipte 10 W paneller satın alınır; 115 × 90 paneller ölçümde gerçekten 10 W çıkarsa hepsi aynı gruba alınabilir (elektrik özellikleri uyumluysa).

## 5. Panel Sayısı (başlangıç önerisi)
- **Başlangıç: 6 panel** ≈ 60 W nominal, gerçekte ≈ 33 W (04 parametrik tablo), ağırlık ≈ 2 kg.
- Genişleme: mekanizma torku ve gövde yüzdürmesi onaylanırsa 8–10 panele.
- Sol/sağ grup başına 3 panel, **paralel** bağlantı (gölge toleransı), grup başına sigorta + Schottky diyot.

## 6. Su Altı Davranışı (dikkat)
- ETFE laminasyon yağmura dayanıklıdır, **sürekli su altı kullanımı için üretici garantisi yoktur.** Kenarlar sızdırmaz yapılmazsa nem hücreye işler.
- Prototipte panel suya **kısa süreli** (limanda şarj süresince, saatler) inecek; sızdırmazlık testi (07 T4) panel bazında yapılmalı.
- Su altı üretim ölçülmeden hesaba katılmamalı. Seçilen panel tipi hem **suda hem havada** test edilmeli (07 T7).
- Alg/kir birikimi ve tuzlu su (korozyon) için tatlı su durulama ve yüzey temizliği (10 §6).

## 7. Alternatif Planlar
| Durum | Plan B |
|---|---|
| ETFE panel temin edilemez/pahalı | Cam çerçeveli 10 W panel, ama **N ≤ 4** ve lineer aktüatör |
| Sızdırmazlık sağlanamaz | Panel suya inmesin; **su yüzeyine yakın tutulup yalnız soğutma/yüzme** konfigürasyonu |
| Panel suda çok az üretir | Seyirde yukarıda, limanda **yüzeye paralel** (suya değmeden güneşe açık) konum; "suya inme" özelliği ayrı deney olarak kalır |
