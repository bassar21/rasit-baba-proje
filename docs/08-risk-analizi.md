# 08 – Risk Analizi

Olasılık (O) ve Etki (E): 1 (düşük) – 5 (yüksek). Skor = O × E.

| # | Risk | O | E | Skor | Önlem |
|---|---|---|---|---|---|
| R1 | **Panel suya inince enerji kazanımı beklentisi gerçekleşmez** (üretim düşer) | 4 | 5 | 20 | Erken deney (T7); hedefi yeniden tanımla (soğutma/hidro/limanda yüzey) |
| R2 | Su girişi → elektronik arıza | 4 | 5 | 20 | IP kutu, rakor, silika, conformal coating, T4 |
| R3 | Li‑ion batarya yangın/şişme | 2 | 5 | 10 | BMS, sigorta, güvenli saklama, su geçirmez kutu |
| R4 | Gemi devrilir / denge kaybı | 3 | 4 | 12 | Katamaran, balast, T5 |
| R5 | Bluetooth kopması → kontrol kaybı | 4 | 4 | 16 | Failsafe, menzil sınırı, ipli test, ileride uzun menzilli link |
| R6 | Motor/servo akımı → Arduino reset (brownout) | 4 | 3 | 12 | Ayrı buck, kondansatör, ayrı hatlar |
| R7 | Servo tork yetersizliği/yanma | 3 | 3 | 9 | Metal dişli servo, 2× tork payı, limit + zaman aşımı |
| R8 | Panel mekanizması sıkışır (alg, tuz, yabancı cisim) | 3 | 3 | 9 | Temizlik, aralık, takılma algılama |
| R9 | Korozyon | 3 | 3 | 9 | Paslanmaz/ kaplamalı, tatlı su durulama |
| R10 | Gemi kaybı (su üstünde kontrolsüz sürüklenme) | 3 | 4 | 12 | İp/şamandıra, GPS izleme, açık suda test yok |
| R11 | Otonom mod çarpışma | 3 | 4 | 12 | Düşük hız, ultrasonik, manuel devralma |
| R12 | Zaman/bütçe aşımı | 4 | 3 | 12 | Fazlara bölme, MVP, %15 tampon |
| R13 | Parça tedariği gecikmesi | 3 | 3 | 9 | Alternatifler (02), erken sipariş |
| R14 | Ekip içi iş dağılımı belirsizliği | 3 | 3 | 9 | Görev matrisi (09), haftalık toplantı |
| R15 | Güneş enerjisi prototip için yetersiz kalır | 4 | 3 | 12 | Beklenti yönetimi (04), "destekli" tanım, daha çok panel |
| R16 | Yasal/izin (gerçek ölçekte denizcilik) | 2 | 4 | 8 | Prototip için gerekmez; gerçek ölçekte klas kuruluşu |
| R17 | Kişisel güvenlik (suda test, elektrik) | 2 | 5 | 10 | Kuralları (10), yalnız test yapma |

## Yeni Riskler (10 panel × 10 W, 60 cm, Uno kararı)

| # | Risk | O | E | Skor | Önlem |
|---|---|---|---|---|---|
| R18 | 10 panel (0.43 m², 2–8 kg) 60 cm gemiye sığmaz / ağır gelir, gemi batar veya devrilir | 5 | 5 | 25 | Hafif panel, katamaran, kademeli panel sayısı (4–6 ile başla), stabilite hesabı |
| R19 | Panel kollarının torku servoyu aşar | 4 | 4 | 16 | Lineer aktüatör/dişli motor, denge ağırlığı |
| R20 | 100 W panel için batarya/şarj kontrolcüsü yetersiz (2S küçük paket) | 4 | 4 | 16 | 3S 10 Ah paket, ≥ 10 A MPPT, BMS şarj akımı |
| R21 | Uno pin/seri port yetersizliği (çok servo, GPS+BT) | 4 | 3 | 12 | PCA9685, GPS’i erteleme, Mega/ESP32 yedek plan |
| R22 | Kısmi gölge nedeniyle seri bağlı panel verimi çöker | 4 | 3 | 12 | Paralel/grup bağlantı, blokaj diyodu, MPPT |
| R23 | Yüksek akım hat ısınması/kısa devre (≥ 6 A) | 3 | 4 | 12 | Kalın kablo, sigorta, konnektör kalitesi |
| R24 | Maliyet artışı (10 panel + MPPT + batarya) | 4 | 3 | 12 | Bütçe şablonu (02), kademeli satın alma |

| R25 | Mekanizma seçimi gecikir; panel sayısı belirsizliği sipariş/tasarımı bloke eder | 4 | 3 | 12 | Önce panel sayısı + kütle kararı (05 §1.2), parametrik tasarım, kartondan model |
| R26 | Tüm panellerin tek hareketle inmesi: ani ağırlık/ağırlık merkezi kayması (yalpa) | 3 | 4 | 12 | Yalnız gemi duruyorken ve yavaş hareket, balast, stabilite testi (T5) |
| R27 | Tek mekanizma arızası tüm panelleri inik/yukarı kilitler | 3 | 3 | 9 | Mekanik kilit, manuel serbest bırakma, limit + zaman aşımı |

## Öncelikli Aksiyonlar (skor ≥ 15)
0. R18: **Ağırlık/yüzdürme hesabı ve karton/köpük model** (panel sayısı ve yerleşim kararı) — ilk iş.
0. R19–R20: Panel kolu torku ve batarya/MPPT seçimi 02/04 ile kesinleştirilmeli.
1. R1: T7 deneyi ilk fırsatta, küçük panelle, suda/havada kıyas.
2. R2: Sızdırmazlık prosedürü mekanik tasarımda zorunlu.
3. R5: Failsafe gereksinimi yazılımda ilk özellik.
