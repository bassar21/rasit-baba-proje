# 03 – Devre ve Bağlantı Tasarımı

> Arduino Uno varsayımıyla. Pin numaraları öneridir; kod yazılırken ekip son halini bu tabloya işlemelidir.

## 1. Güç Dağılım Şeması

```
Güneş paneli (sol+sağ) ──► Şarj kontrolcü ──► [BMS] ──► Batarya 2S (7.4 V)
                                                          │
                                                   Sigorta 5 A
                                                          │
                                                   Ana şalter / acil stop
                                                          │
                      ┌───────────────────┬───────────────┴──────────────┐
                      ▼                   ▼                              ▼
              L298N (motorlar)     Buck 6 V (servolar)          Buck 5 V (Arduino, HC‑05, sensörler)
```

Kurallar:
- **Ortak toprak (GND)** tüm hatlarda birleştirilmeli.
- Servolar ve motorlar **Arduino 5 V pininden beslenmez**.
- Motor çıkışlarına 100 nF seramik kondansatör (motor uçları arasında), besleme hattına 470–1000 µF.
- INA219 ile batarya ve panel hattı izlenir (I²C).

## 1.1 Ekip Kararına Göre Güncel Notlar (Uno, ~100 W panel)

- Batarya hattı artık **3S (11.1 V)** varsayılır: L298N 12 V girişine uygundur; buck’lar 5 V ve 6 V üretir.
- Panel tarafı: 10 panel → **sol grup 5 + sağ grup 5**; her grup kendi sigorta + Schottky diyot + INA219 ölçümü ile MPPT’ye girer. Panel hattı kablosu ≥ 1 mm² (grup başına ~3 A).
- Ana güç hattı (batarya→sigorta→şalter): akım artık ≈ 3–6 A tepe → **10 A sigorta**, ≥ 1.5 mm² kablo.
- **Uno kısıtları:**
  - Tek donanım seri port (USB ile paylaşılıyor). Bluetooth SoftwareSerial’de kalırsa GPS aynı anda dinlenemez → **GPS’i erteleyin** ya da Mega/ESP32’ye geçin.
  - 10 panel ayrı servo ile hareket edecekse Uno pinleri yetmez → **PCA9685 (I²C)** kullanılmalı. 6 V servo hattı tepe akımı yüksek olacağı için servoları **sırayla** hareket ettirmek ve ≥ 5 A buck şart.
  - Panelleri tek mekanizmayla (sol/sağ kol başına bir servo/aktüatör) indirmek Uno için en basit çözüm (tavsiye).
- **Karar güncellemesi:** Tüm paneller birlikte inip kalktığı için Uno tarafında **tek bir "panel mekanizması" çıkışı** yeterli; PCA9685 gerekmez. Mekanizma henüz belli olmadığından pin ayırma planı:
  - Servo seçilirse: tek servo sinyal pini (D3 veya D9), servo kendi buck’ından beslenir.
  - Lineer aktüatör / dişli DC motor seçilirse: ikinci bir motor sürücü (ör. ek L298N kanalı, BTS7960 veya röle çifti) + **2 limit anahtarı** (D2 üst, A2 alt) — mevcut pin haritasındaki panel servo pini (D3/D9) sürücü IN pinleri için kullanılabilir.
  - Mekanizma gerilimi/akımı seçime göre değişir; seçimden sonra bu bölüm ve 02 kesinleştirilecek.
- Panel sayısı değişkeni: MPPT ≥ 10 A ve 10 A ana sigorta **10 panele kadar** yeterli olduğundan elektrik tarafı panel sayısından bağımsız tutulabilir; yalnız panel kablosu/diyot sayısı değişir.
- I²C hattında 2–3 INA219 + opsiyonel pusula: adresler çakışmamalı (0x40, 0x41, 0x44).

## 2. Pin Haritası (Arduino Uno)

| Pin | Bağlantı | Not |
|---|---|---|
| D0/D1 | USB seri (yükleme) | HC‑05 buraya takılmaz |
| D2 | Panel üst limit anahtarı | Dahili pull‑up, kesme (interrupt) kullanılabilir |
| D3 | Sağ panel servo sinyali | |
| D4 | L298N IN3 | Sağ motor yön |
| D5 | L298N ENA (PWM) | Sol motor hız |
| D6 | L298N ENB (PWM) | Sağ motor hız |
| D7 | L298N IN1 | Sol motor yön |
| D8 | L298N IN2 | Sol motor yön |
| D9 | Sol panel servo sinyali | |
| D10 | HC‑05 TX → Arduino RX (yazılım seri) | |
| D11 | HC‑05 RX ← Arduino TX (**gerilim bölücü ile**) | 5 V → 3.3 V |
| D12 | L298N IN4 | Sağ motor yön |
| D13 | Durum LED’i | |
| A0 | Ultrasonik TRIG | |
| A1 | Ultrasonik ECHO | |
| A2 | Panel alt limit anahtarı | |
| A3 | Buzzer | |
| A4/A5 | I²C SDA/SCL | INA219, pusula/IMU |
| GPS | Mega/ESP32’ye geçilirse donanım seri | Uno’da pin yetersizliği |

> Uno’da seri/pin kaynakları kısıtlı: GPS + Bluetooth + USB için **Arduino Mega veya ESP32 önerilir**.

## 3. Bağlantı Detayları

### 3.1 HC‑05
- VCC → 5 V (modül kartı regülatörlü), GND → GND
- HC‑05 TX → D10
- HC‑05 RX ← D11 üzerinden 1 kΩ + 2 kΩ gerilim bölücü (D11 ── 1 kΩ ──┬── HC‑05 RX, 2 kΩ ── GND)
- Varsayılan eşleşme şifresi 1234 / 0000; ilk kurulumda AT modunda isim ve şifre değiştirilmeli.

### 3.2 L298N
- 12 V girişine batarya (+), GND ortak
- 5 V regülatör jumper’ı: batarya > 12 V değilse takılı bırakılabilir ancak **Arduino ayrı buck’tan beslenmeli**
- OUT1/OUT2 sol motor, OUT3/OUT4 sağ motor
- ENA/ENB jumper’ları çıkarılıp PWM pinlerine bağlanır

### 3.3 Servolar
- Besleme: 6 V buck çıkışı (kırmızı), GND ortak (kahverengi), sinyal Arduino pini (turuncu)
- Servo kalkış akımı yüksek olabilir (MG996R ≈ 0.5–2.5 A); 6 V buck en az 3 A.
- Servo kolları sırayla hareket ettirilirse tepe akım düşer.

### 3.4 Sensörler
- Ultrasonik: VCC 5 V, TRIG A0, ECHO A1 (su geçirmez JSN‑SR04T tavsiye).
- Limit anahtarları: bir ucu pin, diğer ucu GND (INPUT_PULLUP mantığı).
- INA219: I²C, adres 0x40 (ikinci için A0 lehimi ile 0x41).

## 4. Acil Durdurma
- Acil stop butonu **güç hattını fiziksel olarak keser** (motor + servo hattı) ya da motor sürücü enable’ını GND’ye çeker.
- Kablosuz komutla durdurma yazılım tarafında ayrıca bulunur (bkz. 06).

## 5. Kablolama Standartları
| Renk | Kullanım |
|---|---|
| Kırmızı | +Batarya |
| Siyah | GND |
| Sarı | +5 V |
| Turuncu | Sinyal |
| Mavi | Motor/sürücü çıkışı |

Tüm bağlantılar: lehim + heat‑shrink; su geçirmez konnektör; kablo etiketleme.

## 6. Şema Çizimi
Ekip, KiCad / Fritzing / Tinkercad Circuits ile şemayı çizip `hardware/` klasörüne koymalıdır. Bu belge çizimin ön tasarım referansıdır.
