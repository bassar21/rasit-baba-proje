# 02 – Malzeme Listesi (BOM)

> Tüm model/değerler **varsayımdır** (≈60 cm prototip). Fiyat sütunu ekip tarafından doldurulacaktır.

## 1. Elektronik

| # | Malzeme | Adet | Özellik / Gerekçe | Alternatif | Fiyat |
|---|---|---|---|---|---|
| 1 | Arduino Uno R3 (veya Nano) | 1 | Ana kontrolcü | Mega (çok pin), ESP32 (Wi‑Fi+BT, daha güçlü) | |
| 2 | HC‑05 Bluetooth modülü | 1 | Seri BT, telefondan kontrol | HC‑06 (sadece slave), HM‑10 (BLE) | |
| 3 | L298N motor sürücü | 1 | 2 kanal, 2 A/kanal. Voltaj düşümü ≈2 V | TB6612FNG (verimli), BTS7960 (yüksek akım) | |
| 4 | DC motor 6–12 V (su altı/pervane uyumlu) | 2 | İtiş; diferansiyel dönüş | Brushless + ESC + su geçirmez | |
| 5 | Pervane + şaft + salmastra | 2 | Su sızdırmaz şaft çıkışı | Su jeti pompası (bilge pompası ile) | |
| 6 | Servo MG996R (metal dişli) | 2–10 (**mekanizma kararına bağlı**) | Panel kolu, ~10 kg·cm | Lineer aktüatör (büyük/ağır panel), su geçirmez servo | |
| 7 | **Esnek ETFE monokristal panel ~10 W, 12 V sınıfı (Vmp ≈ 18 V), ~250×170 mm, ≤ 0.35 kg** (seçim gerekçesi: 12) | **6 başlangıç** (4–10 arası değişebilir) | Raportör önerisi; 115×90 paneller doğrulanana kadar test amaçlı | Cam çerçeveli 10 W (N ≤ 4) | |
| 7a | Panel destek plakası (PVC köpük/akrilik/karbon 2–3 mm) + epoksi/silikon | panel sayısı kadar | Esnek paneli düz tutmak ve sızdırmazlık | | |
| 8 | MPPT şarj kontrolcüsü ≥ 10 A, 12 V batarya uyumlu (Li‑ion/LiFePO4 profili) | 1 (veya sol/sağ 2×) | 100 W panel için | Basit PWM (verim düşük) | |
| 9 | Batarya: 3S Li‑ion 11.1 V ~10 Ah (**tip belli değil, öneri**) | 1 | Ana güç; 04’e bakın | LiFePO4 12.8 V 6 Ah, LiPo 3S 8 Ah | |
| 10 | 3S BMS (10 A+ koruma, dengeleme) | 1 | Aşırı şarj/deşarj/kısa devre | Hazır koruma kartlı paket | |
| 11 | Buck dönüştürücü (LM2596 / XL4015) 5 V (≥ 2 A) | 1 | Arduino + sensör beslemesi | |  |
| 12 | Buck dönüştürücü 6 V / 5 A+ (servo sayısına göre) | 1 | Servo beslemesi (ayrı hat) | | |
| 12a | PCA9685 16 kanal servo sürücü | 1 (gerekirse) | Uno’da çok servo için I²C | | |
| 12b | Panel başına/ grup başına sigorta ve blokaj diyodu (Schottky) | 10 / 2 | Ters akım ve kısa devre | | |
| 13 | HC‑SR04 ultrasonik sensör (veya su geçirmez JSN‑SR04T) | 1–3 | Engel algılama (otonomi) | VL53L0X ToF | |
| 14 | INA219 akım/gerilim sensörü | 1–2 | Panel ve batarya ölçümü | ACS712 + gerilim bölücü | |
| 15 | Mikro uç anahtar (limit switch) | 4 | Panel üst/alt limit | Hall sensör + mıknatıs | |
| 16 | Pusula/IMU (MPU6050 / HMC5883L/QMC5883L) | 1 | Yön tutma | | |
| 17 | GPS modülü (NEO‑6M) | 1 | Konum, liman geofence | | |
| 18 | Ana şalter + acil durdurma butonu | 1+1 | Güvenlik | | |
| 19 | Sigorta (5 A) + sigorta yuvası | 1 | Ana hat koruması | | |
| 20 | Kondansatör 470–1000 µF, 100 nF seramik | birkaç | Motor gürültüsü/güç filtresi | | |
| 21 | Gerilim bölücü dirençleri (1 kΩ/2 kΩ) | birkaç | HC‑05 RX 5 V→3.3 V | Seviye dönüştürücü | |
| 22 | LED/buzzer | 3+1 | Durum göstergesi | | |
| 23 | Breadboard → ardından delikli plaket/PCB | 1 | Prototip → kalıcı | | |
| 24 | Kablo, konnektör (su geçirmez, ör. IP67) | – | | | |

## 2. Mekanik

| # | Malzeme | Not |
|---|---|---|
| 1 | Gövde: katamaran/ tek gövde (köpük + epoksi, PVC boru, 3B baskı veya hazır RC tekne) | Yüzdürme + denge |
| 2 | Panel kolları: alüminyum profil / 3B baskı PETG | Servo ile menteşe |
| 3 | Menteşe, rulman, paslanmaz cıvata/somun | Korozyon |
| 4 | Su geçirmez elektronik kutusu (IP65+) | Ana elektronik |
| 5 | Kablo rakoru (PG7/PG9 glend) | Kutu çıkışları |
| 6 | Silikon, epoksi, conta, sızdırmazlık yağı | Yalıtım |
| 7 | Balast ağırlıkları | Denge ayarı |

## 3. Yazılım / Araçlar (kodlama hariç)

- Arduino IDE (firmware yüklemek için), MIT App Inventor veya hazır "Bluetooth Serial Controller" uygulaması.
- Multimetre, havya, lehim, heat‑shrink, el aleti.
- Ölçüm: tartı (gövde ağırlığı), kronometre, şamandıra/su kabı testi.

## 4. Tahmini Bütçe Şablonu

| Grup | Tahmin (₺) | Gerçek (₺) |
|---|---|---|
| Elektronik |  |  |
| Mekanik/gövde |  |  |
| Su yalıtımı/sarf |  |  |
| Beklenmeyen (%15 tampon) |  |  |
| **Toplam** |  |  |
