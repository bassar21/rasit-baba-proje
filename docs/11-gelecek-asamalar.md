# 11 – Gelecek Aşamalar (Otonomi ve Gerçek Ölçek)

## 1. Prototipten Gerçeğe Kademeleri

| Seviye | Tanım | Ölçek |
|---|---|---|
| P1 | Bluetooth kumandalı model | ~60 cm |
| P2 | Otonom engelden kaçınma + liman modu | ~60–100 cm |
| P3 | Büyük ölçekli model (RC, uzun menzil, GPS rota) | 1–3 m |
| P4 | Kanal/göl insansız yüzer platform | 3–10 m |
| P5 | Kaptanlı gemi + otonom yardım | gerçek |

## 2. Prototipten Çıkarken Değişecekler

| Konu | Prototip | Gerçek |
|---|---|---|
| Kontrol | Arduino, Bluetooth | PLC/endüstriyel kontrolcü, CAN/NMEA 2000 |
| Haberleşme | Bluetooth (10 m) | LoRa / telsiz / 4G‑5G / uydu |
| Sürüş | Diferansiyel itiş | Dümen + pervane / azimut pod |
| Enerji | 7.4 V batarya | Yüksek gerilim batarya sistemi (LiFePO4), MPPT dizileri |
| Panel mekanizması | Servo | Hidrolik/elektrik vinç, endüstriyel kilitleme |
| Güvenlik | Failsafe, buton | Yedekli sistemler, alarm, klas kuralları |

## 3. Kaptan ve Otonom Çalışma Modeli (gerçek hayat)
- **Kaptan modu:** Dümen/gaz kolları ile; panel kontrolü köprüüstü panelinden.
- **Otonom mod:** Rota noktaları; kaptan her an müdahale edebilir (override).
- **Hibrit:** Seyirde otonom, limanda kaptan.
- Mod geçişleri açık, sesli/görsel uyarı ve kayıt altında.

## 4. Otonomi Teknolojileri
- GNSS/RTK, IMU, pusula, radar, AIS, LiDAR/kamera, sonar.
- Çarpışma önleme (COLREG), rota planlama, hava/akıntı kestirimi.
- Liman yanaşma otomasyonu (sensörlü iskele algılama).

## 5. Panel Sistemi İleri Fikirler
- Güneş takibi ve açı optimizasyonu.
- Akıllı panel yönetimi: hava, dalga, verim ve konuma göre aç/kapat.
- Panel temizleme (fırça/su) mekanizması.
- Dalga/akıntı enerjisi ile hibrit (su altı türbini fikri).
- Panel gövdesinde yüzer şamandıralar.

## 6. Veri ve Bulut
- Telemetri kaydı, enerji analitiği, uzaktan izleme panosu, OTA güncelleme.

## 7. Yasal / Standartlar (gerçek ölçek)
- Denizcilik otoritesi tescil/ruhsat, klas kuruluşu onayı, IMO/MASS (otonom gemi) düzenlemeleri, batarya güvenliği standartları.
- Çevre ve emisyon raporlaması.

## 8. Ticari/Akademik Değerlendirme
- Hedef pazar: liman içi hizmet gemileri, feribot, araştırma tekneleri, turizm.
- Değer önerisi: yakıt/emisyon tasarrufu, limanda ek şarj.
- Fizibilite: panel alanı / enerji ihtiyacı oranı, yatırım geri dönüşü.
- Patent/faydalı model araştırması önerilir.

## 9. Açık Araştırma Soruları
1. Panel su altında hangi koşulda net enerji kazandırır?
2. Hareketli panel kollarının hidrodinamik maliyeti nedir?
3. Limanda yanaşma sırasında panel açmak için yer/güvenlik koşulları?
4. Otonom–kaptan devri güvenlik prosedürü nasıl olmalı?
