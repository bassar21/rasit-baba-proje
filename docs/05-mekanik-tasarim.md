# 05 – Mekanik Tasarım

## 1. Gövde

| Seçenek | Artı | Eksi |
|---|---|---|
| Katamaran (iki gövde) | Çok iyi denge, geniş güverte, panel kolları için uygun | Daha karmaşık yapım |
| Tek gövde + dengeleyici kollar | Basit | Panel kolları devrilme riski yaratır |
| Hazır RC tekne gövdesi | Hızlı başlangıç | Modifikasyon gerekir |

**Öneri:** Katamaran, ~60 × 40 cm, hedef ağırlık ≤ 3 kg, güverte ortasında elektronik kutu.

Yüzdürme hesabı: Toplam ağırlık × 1.5 güvenlik = gereken yer değiştirme hacmi. (3 kg → ≥ 4.5 L batmış hacim.)

## 1.1 Boyut ve Ağırlık Kontrolü (10 panel × 10 W, 60 cm gemi)

| Kalem | Değer | Yorum |
|---|---|---|
| Panel alanı | 10 × (≈ 25×17 cm) ≈ 0.43 m² | Güverte ≈ 0.24 m² (60×40) → tek katta sığmaz |
| Panel ağırlığı | cam/alüminyum ≈ 0.5–0.8 kg × 10 = 5–8 kg; esnek/ETFE ≈ 0.2–0.4 kg × 10 = 2–4 kg | Hafif panel tercih |
| Toplam gemi ağırlığı (tahmin) | 8–12 kg (batarya, motor, gövde dahil) | Önceki 3 kg varsayımı geçersiz |
| Gereken batmış hacim | ≥ 1.5 × ağırlık → 12–18 L | Katamaran gövdeleri bu hacmi sağlamalı |

Çözüm yönleri:
1. **Çok katlı/katlanır kollar:** her yanda 5 panel, yelpaze veya akordeon düzeni; indirme sırasında suya paralel/yan konum.
2. **Geniş katamaran** (genişlik 50–60 cm) + yan dengeleyici şamandıralar.
3. **Hafif (ETFE/esnek) paneller** ve ince çerçeve.
4. 10 panelin tamamı yerine başlangıçta **4–6 panel** ile prototip, sonra çoğaltma (risk azaltma).
5. Suya inen panellerin tamamı suya girmesin; **su yüzeyine yakın yüzen** konfigürasyon karşılaştırılsın.

Yapısal not: 5–8 kg panelin kol uzunluğu boyunca oluşturduğu moment (tork) servo için yüksektir; **lineer aktüatör veya dişli kutulu motor** gerekebilir. Ağırlık merkezi güvertenin yan tarafına kayar → yalpa riski.

## 1.2 Ekip Kararları (güncel)
1. **Tüm paneller birlikte iner/kalkar** → tek komut (`D`/`U`), tek hareket "sahnesi". Panel başına bağımsız kontrol yok.
2. **Panel sayısı değişebilir** → tasarım parametrik olmalı (tabloya bakın); mekanizma en kötü senaryo (en çok panel) için boyutlandırılmalı.
3. **Servo/mekanizma henüz seçilmedi** → aşağıdaki karar matrisi ve tork hesabı ile seçilecek.
4. Uno kullanılacak (pin/güç kısıtı için 03’e bakın).

### Panel sayısına göre parametrik tablo (panel ≈ 10 W, 25×17 cm)

| Panel (N) | Nominal | Gerçek (~%55) | Alan | Ağırlık (ETFE 0.3 – cam 0.65 kg/adet) | 80 Wh şarj süresi |
|---|---|---|---|---|---|
| 4 | 40 W | ~22 W | 0.17 m² | 1.2 – 2.6 kg | ~3.6 sa |
| 6 | 60 W | ~33 W | 0.26 m² | 1.8 – 3.9 kg | ~2.4 sa |
| 8 | 80 W | ~44 W | 0.34 m² | 2.4 – 5.2 kg | ~1.8 sa |
| 10 | 100 W | ~55 W | 0.43 m² | 3.0 – 6.5 kg | ~1.5 sa |

Seyir tüketimi (~15 W) ≈ **3 panelle** dengelenir; N ≥ 4 yeterli güvenlik payı verir. İlk prototip için **N = 4–6** önerilir; mekanizma ve gövde bunu taşıyorsa artırılır.

### Tork hesabı (mekanizma seçimi için)
Formül (havada, en kötü hâl): **Tork (kg·cm) = kol başına kütle (kg) × ağırlık merkezi uzaklığı (cm)**, sonra **×2 güvenlik payı**.
Varsayım: iki kol (sol/sağ), ağırlık merkezi menteşeden 20 cm.

| N | Kol başına kütle (ETFE – cam) | Gerekli tork (×2 pay) |
|---|---|---|
| 4 | 0.6 – 1.3 kg | 24 – 52 kg·cm |
| 6 | 0.9 – 2.0 kg | 36 – 80 kg·cm |
| 10 | 1.5 – 3.3 kg | 60 – 130 kg·cm |

MG996R ≈ 10 kg·cm → **tek başına yetmez**. Seçenekler: yüksek torklu servo (25–35 kg·cm, örn. DS3225/DS3235 sınıfı), servo + dişli/kaldıraç oranı, lineer aktüatör (kendini kilitler), dişli kutulu DC motor + limit anahtarı. Panel suya girdiğinde yüzdürme kuvveti yükü azaltır ama su direnci ekler; torku havada hesaplayıp ölçerek doğrulayın.

### Mekanizma karar matrisi (1 en kötü – 5 en iyi)

| Kriter | Servo (doğrudan) | Servo + dişli | Lineer aktüatör | DC dişli motor + limit | Makara + halat (vinç) |
|---|---|---|---|---|---|
| Tork/yük kapasitesi | 1–2 | 3 | 4–5 | 4 | 4 |
| Uno ile kolaylık | 5 | 4 | 4 (sürücü gerekir) | 3 (sürücü + limit) | 3 |
| Güç kesilince kilit | 1 | 2 | 5 | 4 (kendini kilitleyen dişli) | 3 (fren) |
| Su yalıtımı kolaylığı | 2 | 2 | 3 (hazır IP’li var) | 3 | 4 (motor gövde içinde) |
| Maliyet | 5 | 4 | 3 | 4 | 3 |
| Konum bilgisi | 4 (açı) | 4 | 3 (limit/potansiyometre) | 2 (limit anahtarı) | 2 |

**Öneri:** Prototip için **kendini kilitleyen lineer aktüatör (sol/sağ ya da tek ortak)** veya **dişli kutulu DC motor + 2 limit anahtarı**. Küçük N (≤ 4, ETFE) için yüksek torklu servo da düşünülebilir. Karar için önce **panel sayısı + panel kütlesi** kesinleşmeli.

### Eş zamanlılık (tüm paneller birlikte)
- Tek mil/ortak tahrik: iki kolu **tek mekanizma** (ortak mil veya bağlantı çubuğu) hareket ettirir → mekanik senkron, en basit ve Uno dostu.
- İki ayrı tahrik kullanılırsa: aynı komut, aynı hız profili, limit anahtarlarıyla her kol ayrı doğrulanır; biri takılırsa ikisi de durur.
- Hareket sırasında tekne dengesi: indirme/kaldırma **gemi duruyorken**, yavaş (3–5 sn).

## 2. Panel Kolu Mekanizması

Amaç: Paneller gövde kenarlarından **aşağı (suya)** ve **yukarı (güverteye/açık hava)** hareket eder.

```
Üstten görünüm                     Yandan görünüm (sağ kol)

 [Panel]──┐                          Gövde ┌──────┐
          ├─ menteşe ─ Gövde                │      │ ← menteşe + servo
 [Panel]──┘                          Panel ╱ (aşağı pozisyon: suya batık)
                                      ─── su hattı ───
```

Tasarım gereksinimleri:
1. **İki uç pozisyon:** Üst (kapalı/seyir), alt (suda). Limit anahtarları ile doğrulanır.
2. **Mekanik kilit:** Servo gücü kesilince panel yerinde kalmalı (kendi kendine kilitleyen dişli veya mandal).
3. **Hız:** Yavaş hareket (≈ 3–5 sn) – ani darbe ve su sıçraması olmasın.
4. **Eş zamanlı hareket:** İki kol aynı anda; denge bozulmasın.
5. **Kuvvet:** Su direnci + panel ağırlığı; servo torku ≥ 2× hesaplanan yük.
6. **Seyir sırasında kollar yukarıda** (sürtünme ve hasar).

### 2.1 Alternatif mekanizmalar
| Mekanizma | Uygunluk |
|---|---|
| Servo + doğrudan menteşe | Hafif paneller, basit |
| Servo + dişli/kol | Tork artışı |
| Lineer aktüatör | Büyük/ağır panel, kilitli |
| Makara + halat (vinç) | Panelin dikey indirilmesi |
| Kaydırmalı/teleskopik ray | Liman için yan açılma |

## 3. Panel Suya İndirme – Yapısal Notlar
- Su altında kalacak panel **tam kapsüllenmeli** (epoksi/laminasyon, şeffaf kaplama) ve kenarları sızdırmaz olmalıdır.
- Kablo çıkışı tek noktadan, damlama halkalı (drip loop) ve su geçirmez rakorlu.
- Su altı yüzeyde alg/kirlenme ve kir birikimi; temizlenebilir yüzey.
- Panel tipi: su altı uyumlu (ör. kapsüllenmiş) olmalı; standart panel suyu kaldırmaz.

## 4. Su Yalıtımı Standartları

| Bölge | Hedef |
|---|---|
| Elektronik kutu | IP65 minimum, silika jel paketi |
| Servo | Su geçirmez servo veya silikon/yağ ile yalıtım |
| Motor şaftı | Salmastra + gres |
| Kablo geçişleri | PG rakor + silikon |
| Lehim noktaları | Heat‑shrink + conformal coating |

Sızdırmazlık testi: Elektronik **olmadan** kutu içinde kuru kağıt peçete ile su testi (15 dk).

## 5. Ağırlık ve Denge
- Ağırlık merkezi geminin ortasında ve düşük olmalı (batarya alt kısımda).
- Panel inince ağırlık merkezi ve hidrodinamik yük değişir → **havuz stabilite testi** (07).
- Balast ağırlıkları ile trim ayarı.

## 6. Üretim Yöntemi Önerileri
- 3B baskı (PETG/ASA): menteşe, bağlantı parçaları, motor yuvaları.
- Gövde: XPS köpük + fiber/epoksi veya PVC boru katamaran.
- Alüminyum profil: panel çerçevesi/kollar.

## 7. Bakım
- Her testten sonra: tatlı suyla durulama, kurutma, conta kontrolü.
- Haftalık: cıvata sıkılığı, korozyon, kablo yıpranması.
