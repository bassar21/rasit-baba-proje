# Güneş Panelli Gemi Projesi – Analiz Raporu

> Durum: İlk analiz (ekip anlatımına dayalı). Repoda henüz kod yok; yalnızca 18 baytlık `README.md` mevcut.
> Bu rapor ekip yeni bilgi verdikçe güncellenecektir. "Varsayım" ve "Açık soru" etiketli yerler ekip onayı bekler.

## 1. Proje Özeti

Kenarlarında güneş panelleri bulunan, elektriğini güneşten sağlayan bir gemi. Ayırt edici özellik: paneller **istenildiğinde suya indirilebilir** ve bu sayede ek enerji sağlanır. Gemi limana yanaştığında paneller açılarak **ekstra enerji depolanır**.

## 2. Amaç

- Güneş enerjisiyle çalışan, kendi enerjisini üreten bir gemi.
- Kenar panellerinin aşağı/yukarı hareketiyle suya indirilmesi ve enerji kazanımı.
- Limanda bekleme süresinin enerji depolamak için değerlendirilmesi.

## 3. Kapsam

### 3.1 Prototip aşaması (şimdiki hedef)
| Özellik | Yöntem |
|---|---|
| Hareket: ileri, geri, sağ, sol | Arduino + Bluetooth uzaktan kumanda |
| Panel kolları: aşağı / yukarı | Aynı uzaktan kumanda |
| Otonom sürüş | Sensör/algoritma tabanlı (detaylar belirlenmeli) |

### 3.2 Gerçek hayat aşaması (ileri safha)
- Kaptan dümen ile yönetir.
- Otonom sürüş de mümkündür (kaptan/otonom mod geçişi).

## 4. Sistem Mimarisi (önerilen)

```
[Uzaktan kumanda / telefon]
        │ Bluetooth
        ▼
[Arduino (ana kontrolcü)] ──► Motor sürücü ──► Tahrik motorları (ileri/geri/sağ/sol)
        │
        ├──► Panel mekanizması (servo/lineer aktüatör/DC motor) ──► Sol & sağ panel
        ├──► Şarj kontrolcüsü (MPPT/PWM) ──► Batarya ──► Yük
        └──► Sensörler (mesafe, akım/gerilim, uç anahtarlar, GPS/pusula)
```

## 5. Bileşen Önerileri

| Grup | Bileşen | Not |
|---|---|---|
| Kontrol | Arduino Uno/Nano/Mega | Mega: çok pin gerekiyorsa |
| İletişim | HC-05 / HC-06 (klasik BT) veya HM-10 (BLE) | Telefon uygulaması: MIT App Inventor veya hazır seri terminal |
| Tahrik | 2 DC motor + L298N / BTS7960 sürücü, pervane | Sağ-sol diferansiyel itiş ile dönüş |
| Panel mekanizması | Servo (hafif panel) veya lineer aktüatör / dişli motor | Su geçirmezlik kritik |
| Panel | Küçük güneş panelleri (5–12 V) | Suya inecek paneller su altı kullanıma uygun/kapsüllenmeli |
| Enerji | Li-ion/LiPo batarya + BMS + şarj kontrolcüsü | Aşırı şarj/deşarj koruması |
| Sensör | Ultrasonik/ToF (engel), akım-gerilim (INA219), uç anahtar, IMU/pusula, GPS | Otonomi için |
| Gövde | Katamaran veya geniş tekne gövdesi | Denge için tercih edilir |

## 6. Teknik Analiz ve Riskler

1. **Suya inen panel – enerji fiziği:** Güneş paneli suya batınca elektrik üretimi genelde düşer (ışık sönümlenmesi, yansıma). Suyun "enerji sağlaması" ekibin kastettiği mekanizmaya bağlı: (a) panelin serinletilmesi, (b) su altı türbin/hidro üretim, (c) suyun yüzeyine yakın tutularak soğutma. **Açık soru 1.**
2. **Su geçirmezlik:** Paneller, kablo girişleri, servolar. IP67/IP68 muhafaza, silikon/epoksi kaplama, su geçirmez konnektörler.
3. **Denge:** Panel kolları inince ağırlık merkezi ve hidrodinamik direnç değişir; ileri hızda kollar sürtünme yaratır. Hareket halinde kollar kapalı, limanda açık mantığı önerilir.
4. **Bluetooth menzili:** ~10 m (HC-05). Havuz/gölet testi için yeterli, gerçek ölçek için yetersiz. İleride LoRa/RF/telsiz/4G düşünülmeli.
5. **Güç yönetimi:** Motorlar akım dalgalanması yaratır; Arduino'yu ayrı regülatörle besleyin, ortak toprak kullanın, kondansatör ekleyin.
6. **Güvenlik:** Bağlantı koparsa (failsafe) motorlar durmalı. Acil durdurma anahtarı. Pil sıcaklık/akım koruması.
7. **Korozyon:** Tatlı su/deniz suyu farkı; paslanmaz veya kaplamalı bağlantı elemanları.

## 7. Yazılım Tasarımı (önerilen)

### 7.1 Komut seti (seri/Bluetooth, tek karakter)
| Komut | Anlam |
|---|---|
| `F` | İleri |
| `B` | Geri |
| `L` | Sol |
| `R` | Sağ |
| `S` | Dur |
| `U` | Panelleri yukarı al |
| `D` | Panelleri suya indir |
| `A` | Otonom mod aç/kapat |

### 7.2 Durum makinesi
`BEKLEME → MANUEL → OTONOM → LİMAN (paneller açık, şarj)` ; her durumdan `ACİL_DUR`.

### 7.3 Güvenlik mantığı
- Bağlantı zaman aşımı (ör. 1 sn komut gelmezse dur).
- Uç anahtar ile panelin üst/alt limit algılaması.
- Panel inikken hız sınırı.

## 8. Otonom Sürüş

- **Prototip:** Mesafe sensörleriyle engelden kaçınma; pusula/IMU ile yön tutma; opsiyonel GPS ile rota noktaları.
- **Gerçek hayat:** Kaptan/otonom mod geçişi, radar/AIS/kamera, COLREG kuralları, yedeklilik. (Prototip kapsamı dışı.)
- **Liman algılama:** Otomatik panel açma için GPS jeo-çit (geofence) veya manuel komut.

## 9. Eklenebilecek Özellikler
- Telefon uygulaması (düğmeler + batarya/panel durum göstergesi).
- Canlı telemetri: voltaj, akım, üretilen watt, batarya %.
- Panel açı ayarı ve güneş takibi.
- Liman otomasyonu (geofence ile otomatik panel açma).
- Veri kaydı (SD kart) ve enerji verimliliği analizi.
- Düşük batarya durumunda otomatik limana dönüş.
- LED/buzzer durum göstergeleri.

## 10. Değiştirilebilecek / Gözden Geçirilecek Noktalar
- HC-05 yerine BLE (HM-10) veya ESP32 (Wi‑Fi+BT, daha güçlü ve ucuz).
- Servo yerine lineer aktüatör (büyük panellerde).
- Arduino Uno yerine Mega veya ESP32 (pin ve işlem gücü).
- Suya batan panel yerine suya yakın/üstte yüzer panel alternatifi (verim karşılaştırması).

## 10.1 Netleşen Kararlar (ekip)
| Konu | Karar |
|---|---|
| Gemi boyu | ~60 cm |
| Panel | ~10 adet, her biri ~10 W (toplam ~100 W nominal) |
| Kontrolcü | Arduino Uno |
| Kontrol | **Telefon** üzerinden (Bluetooth); fiziksel kumanda yok |
| Panel hareketi | **Tüm paneller birlikte** iner/kalkar (tek komut) |
| Panel tipi | Ekip karar vermedi → **raportör önerisi: esnek ETFE monokristal 10 W, 12 V sınıfı, başlangıç 6 panel** (bkz. 12) |
| Panel ölçüsü | Bazı paneller **115 × 90** (birim belirtilmedi, büyük olasılıkla mm); ekip ölçümüne göre ~10 W. **Tutarsızlık var, doğrulama bekliyor** (bkz. 04 §8). |
| Panel sayısı | **Değişebilir** (parametrik tasarım; ilk prototip için 4–6 öneri, bkz. 05 §1.2) |
| Servo/mekanizma | **Henüz seçilmedi**; 05’teki tork hesabı ve karar matrisi ile seçilecek |
| Batarya | **Belli değil** (öneri: 3S Li‑ion 11.1 V, 10 Ah, BMS ile; bkz. 04) |

Sonuçları:
- 100 W panel gücü 60 cm gemi için çok yüksek; **alan ve ağırlık** ana kısıt (10 panel ≈ 0.43 m², cam/alüminyum çerçeveli 10 W panel ≈ 0.5–0.8 kg → 5–8 kg). Hafif/esnek (ETFE) panel ve geniş katamaran gövde gerekir (bkz. 05).
- Batarya, şarj kontrolcü (≥ 10 A MPPT) ve kablolar önceki 4 W varsayımına göre yeniden boyutlandırıldı (02, 03, 04).
- Uno ile pin/seri port kısıtı: 10 panel için ayrı servo kullanılırsa PCA9685 (I²C servo sürücü) gerekir; GPS + Bluetooth aynı anda SoftwareSerial ile sorunlu (bkz. 03).

## 11. Açık Sorular (ekibe)
1. Panel suya indiğinde enerji **nasıl** elde edilecek? (Yukarıda 6.1)
2. Prototip ölçeği ve gövde tipi (boy, ağırlık, malzeme)?
3. Panellerin sayısı, gücü ve hareket mekanizması (servo/aktüatör)?
4. Batarya türü ve kapasitesi? Tahrik motorları gerilim/akım?
5. Kumanda: telefon uygulaması mı, fiziksel kumanda (ikinci Arduino + joystick) mı?
6. Otonom sürüş prototipte neyi kapsıyor: engelden kaçınma mı, GPS rota mı?
7. Test ortamı (havuz, göl, deniz)?
8. Bütçe ve takvim, ekip görev dağılımı?
9. Liman "yanaşma" prototipte nasıl simüle edilecek?

## 12. Önerilen Yol Haritası
1. **Faz 1:** Gövde + tahrik + Bluetooth ile ileri/geri/sağ/sol.
2. **Faz 2:** Panel kolu mekanizması + uç anahtarlar + kumandadan aşağı/yukarı.
3. **Faz 3:** Enerji sistemi (panel→şarj kontrolcü→batarya) ve ölçüm.
4. **Faz 4:** Su geçirmezlik ve havuz testleri.
5. **Faz 5:** Otonom sürüş (engelden kaçınma → GPS).
6. **Faz 6:** Liman modu, telemetri, dokümantasyon.

## 12.1 Mevcut Repo Durumu
- Aktif dal: `yigit`, ana dal: `main`.
- Dosyalar: `README.md` (18 bayt). Kod, devre şeması, belge yok.
- Öneri: `docs/`, `firmware/`, `app/`, `hardware/` klasör yapısı.

---
*Rapor yazarı: Claude (raporlamacı rolü). Son güncelleme: 2026-10-02.*
