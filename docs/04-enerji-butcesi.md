# 04 – Enerji Bütçesi

> Girdi (ekip): 60 cm gemi, ~10 panel, panel başına ~10 W, Arduino Uno. **Batarya tipi belli değil** → aşağıda öneri kullanıldı (3S Li‑ion, 11.1 V, 10 Ah ≈ 111 Wh). Tüm değerler ölçümle doğrulanmalıdır.

## 1. Üretim

| Parametre | Değer |
|---|---|
| Panel | 10 × 10 W = **100 W nominal** |
| Gerçekçi verim (açı, bulut, gölge, sıcaklık, kablo ve şarj kaybı) | %50–65 |
| **Gerçek üretim (güneşli gün)** | **≈ 50–65 W** |
| Günlük (4–5 etkin güneş saati) | ≈ 250–320 Wh |
| Bulutlu gün | ≈ 10–20 W (%15–30) |

Önemli kısıtlar:
- Tüm panellerin aynı anda güneşe dönük ve gölgesiz olması **mümkün değildir** (paneller yan yana/üst üste, kol yapısı). Gölge, seri bağlı panelleri orantısız düşürür → paralel bağlantı veya panel başına diyot tercih edilmeli.
- Suya inmiş panelde ışık sönümlenmesi nedeniyle üretim düşer; **suda üretim ölçülmeden hesaba katılmamalı** (bkz. 01 §6).

## 2. Tüketim (60 cm gemi, tahmini)

| Yük | Tepe | Ortalama (varsayım) |
|---|---|---|
| 2 × DC motor (~11 V, ~1.5 A tepe) | ≈ 30 W | ≈ 10 W (%40–50 gaz) |
| Panel kolu motorları/servoları (hareket anında) | ≈ 10–30 W | ≈ 1 W (nadir hareket) |
| Arduino Uno + HC‑05 + sensörler | ≈ 1.5 W | 1.5 W |
| L298N kaybı (~2 V düşüm) | – | ≈ 2 W |
| **Seyir toplamı** | | **≈ 14–15 W** |
| **Limanda bekleme** | | **≈ 1.5 W** |

## 3. Batarya Boyutlandırma

Eski varsayım (2S, 2200 mAh ≈ 16 Wh) 60 W’lık üretim için **çok küçük**: 1C şarj sınırı ≈ 16 W, panel gücünün büyük kısmı kullanılamaz.

| Seçenek | Enerji | Maks. şarj (0.5C) | Not |
|---|---|---|---|
| 3S Li‑ion 11.1 V, 10 Ah (3S4P 18650) | ≈ 111 Wh | ≈ 55 W | **Önerilen**, BMS şart |
| 3S LiPo 11.1 V, 8 Ah | ≈ 89 Wh | ≈ 44 W | Hafif, bakım dikkat |
| LiFePO4 12.8 V, 6 Ah | ≈ 77 Wh | ≈ 38 W | Güvenli, ağır |
| Kurşun‑asit 12 V 7 Ah | ≈ 84 Wh | ≈ 20 W | Ucuz ama ~2.5 kg, 60 cm gemi için ağır |

Önerilen paket: kullanılabilir (%80) ≈ 89 Wh.

## 4. Süre Hesapları (111 Wh paket)

| Senaryo | Süre |
|---|---|
| Sürekli seyir (≈15 W, panelsiz) | ≈ 6 saat |
| Seyir + güneş (≈ 50 W üretim) | **net pozitif**, teorik olarak sınırsız seyir |
| Limanda şarj (boştan %80, 50 W) | ≈ 1.8 saat |
| Bekleme (1.5 W) | ≈ 60 saat |

**Çıkarımlar:**
1. 100 W panel seviyesinde sistem gerçekten "güneş enerjili" hâle gelir: seyir tüketimi (≈ 15 W) üretimin altındadır.
2. Kritik soru: panellerin **seyirde havada** (güverte/kol yukarıda) da üretim yapması. Suya indirme avantajı belirsiz olduğundan, "yukarıda çalışırken de üret" tasarımı daha güvenli.
3. Şarj kontrolcü **≥ 10 A MPPT** olmalı (50–65 W / 12 V ≈ 5–6 A, tepe 8–9 A).
4. Panel alanı sorunu: 10 W panel ≈ 25 × 17 cm ≈ 0.04 m²; 10 adet ≈ 0.43 m². 60 cm gemide güverte ≈ 0.24 m². Paneller kollarla dışarı açılacağı için alan **kol geometrisine** bağlı (bkz. 05).

## 5. Panel Elektrik Bağlantısı (öneri)

12 V sınıfı panel varsayımı: Vmp ≈ 18 V, Imp ≈ 0.55 A.

| Seçenek | Sonuç | Yorum |
|---|---|---|
| 10 paralel | 18 V, 5.5 A | Gölgeye dayanıklı, kalın kablo |
| 2 grup × 5 paralel (sol / sağ) | her grup 2.75 A | Sol/sağ kol bağımsız izlenir, **önerilen** |
| 2 seri × 5 paralel | 36 V, 2.75 A | MPPT giriş limiti kontrol |

Her grup için: sigorta, blokaj diyodu, INA219 ile ölçüm (Uno I²C’de 2–3 adet INA219 rahat).

## 6. Ölçüm Planı
- INA219 ile panel grubu V/I/P, batarya V/I.
- Aynı güneş şartında panel **havada vs suda** karşılaştırma (en öncelikli deney).

| Saat | Hava | Panel V | Panel I | P (W) | Batarya V |
|---|---|---|---|---|---|
| | | | | | |

## 8. Doğrulama Bekleyen Veri: 115 × 90 Panel "~10 W"

Ekip bildirimi: bazı paneller 115 × 90 ölçüsünde, ölçümde yaklaşık 10 W.

Fiziksel kontrol (tam güneş 1000 W/m²):

| Birim varsayımı | Alan | Teorik üst sınır (%15–22 verim) | 10 W ile uyum |
|---|---|---|---|
| 115 × 90 **mm** | 0.0104 m² | ≈ 1.6 – 2.3 W | **Uyumsuz** (~5 kat fazla) |
| 115 × 90 **cm** | 1.035 m² | ≈ 155 – 230 W | Uyumsuz (çok az); ve 60 cm gemiye sığmaz |
| ~ 250 × 170 mm (tipik 10 W) | 0.0425 m² | ≈ 6 – 9 W (gerçekte etiket 10 W) | Uyumlu |

Olası açıklamalar (hangisi olduğu ekibin ölçümüyle netleşir):
1. Ölçü 115 × 90 mm ve "10 W" aslında **10 V** ya da açık devre gerilimi × kısa devre akımı çarpımı (Voc × Isc), yani gerçek güç değil.
2. "10 W" panel etiketi/kaynak bilgisi ve ölçüm koşulu (lamba, kısa mesafe) gerçek güneşi temsil etmiyor.
3. Ölçü başka birimde ya da başka paneli anlatıyor.

**İstenen bilgi (ekip):** panel üzerindeki etiket (Vmp, Imp, Pmax), nasıl ölçüldü (multimetre V ve A, ortam: güneş/lamba), birim (mm/cm), kaç panel bu ölçüde.

Planlama kuralı: **doğrulanana kadar** 115 × 90 mm panel için **≤ 2 W** kullanılır. Bu durumda 10 panel ≈ 10–20 W nominal, gerçekte ≈ 6–11 W; seyir tüketimi (~15 W) güneşle karşılanamaz, sistem "limanda şarj/menzil uzatma" olarak tanımlanır (bkz. §4 sonuçları değişir). Panel sayısı ve alanı bu durumda artırılmalıdır (örneğin 5 W için ~ 8–10 panel).

Kontrol deneyi (kolay): Gerçek güneşte panelin gerilimi (V) ve kısa devre akımını (A) ölç; Pmax ≈ 0.7–0.8 × Voc × Isc. Veya yük direnciyle (ör. 10 Ω) ölç, P = V²/R.

## 7. Ölçeklendirme (gerçek gemi)
- Gerçek ölçekte güneş enerjisi çoğunlukla yardımcı sistemleri ve hibrit desteği karşılar.
- Hedef netleştirilmeli: tam elektrikli mi, hibrit mi, yardımcı enerji mi.
