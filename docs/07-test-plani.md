# 07 – Test Planı

## 1. Test Aşamaları

| Aşama | Ortam | Amaç |
|---|---|---|
| T1 – Bileşen testi | Masa üstü | Her parça tek tek çalışıyor mu |
| T2 – Entegrasyon | Masa üstü | Parçalar birlikte, güç sorunu yok mu |
| T3 – Kuru test | Karada gemi | Mekanizma, kumanda, failsafe |
| T4 – Su sızdırmazlık | Küvet/kova | Su girişi var mı (elektronik kapalı) |
| T5 – Yüzdürme/denge | Havuz/küvet | Trim, denge, panel inince stabilite |
| T6 – Seyir | Havuz/göl | Hız, manevra, menzil |
| T7 – Enerji | Güneşli gün | Üretim/tüketim ölçümü |
| T8 – Otonom | Açık alan | Engel kaçınma, rota |

## 2. Test Senaryoları

### T1 – Bileşen
| ID | Test | Beklenen |
|---|---|---|
| T1.1 | Batarya gerilim / BMS | 7.4–8.4 V, koruma çalışıyor |
| T1.2 | Buck çıkışları | 5.0 V ve 6.0 V ±%5 |
| T1.3 | HC‑05 eşleşme | Telefon bağlanır, veri gelir |
| T1.4 | Her motor yön/hız | İki yönde döner, PWM ile hız değişir |
| T1.5 | Servo aralığı | Tam strok, takılma yok |
| T1.6 | Limit anahtarları | Basınca sinyal |
| T1.7 | Ultrasonik doğruluk | ±2 cm (cetvelle) |
| T1.8 | INA219 | Multimetreyle ±%3 |

### T2 – Entegrasyon
- Motor+servo aynı anda çalışırken Arduino resetlemiyor mu (brownout)?
- Bluetooth motor gürültüsünde kopuyor mu?
- Sıcaklık: 10 dk sürekli yükte L298N/buck ısınma ölçümü (< 70 °C).

### T3 – Kuru Test
| ID | Test | Kabul |
|---|---|---|
| T3.1 | Tüm yön komutları | Doğru motor/yön |
| T3.2 | Panel indir/kaldır | Limit’te durur, 3–5 sn |
| T3.3 | **Bağlantı kesme** (telefon BT kapat) | Motorlar ≤ 2 sn’de durur |
| T3.4 | Acil stop butonu | Güç kesilir |
| T3.5 | Panel inikken hız sınırı | Uygulanır |
| T3.6 | Düşük batarya simülasyonu | Uyarı/kısıtlama |

### T4 – Su Sızdırmazlık
- Elektronik kutu: 30 dk batık, içi kuru.
- Servo/şaft çıkışları: 30 dk, damla var mı.
- Panel kapsülü: 1 saat batık, panel çıkışı normal.

### T5 – Yüzdürme/Denge
- Ağırlık ölçümü, su hattı işaretleme.
- Panel inikken yalpa (roll) açısı: < 10° itme altında.
- Tek taraf panel inik senaryosu (asimetri) stabilite.

### T6 – Seyir
- Maksimum hız (10 m mesafe, kronometre).
- Dönüş yarıçapı, yerinde dönüş.
- Panel inikken manevra ve sürtünme.
- Bluetooth menzili (gerçek su ortamında) – kaydet.

### T7 – Enerji
- 1 saat panel üretim logu (V, I, P).
- Panel suda vs havada üretim karşılaştırması (**kritik açık soru**).
- 30 dk seyir: batarya düşüşü.

### T8 – Otonom
- Sabit engel: ≥ 50 cm önce durma/kaçınma.
- Yön tutma: 10 m’de sapma < 1 m.
- Bağlantı kaybı/otonom geçişi: manuel devralma ≤ 1 sn.

## 3. Kabul Kriterleri (prototip)
1. 4 yön + panel komutları %95+ başarıyla çalışır.
2. Failsafe 2 sn içinde motorları durdurur (%100).
3. Su sızıntısı yok (T4).
4. Panel mekanizması 50 çevrimde arızasız.
5. En az 30 dk kesintisiz seyir.
6. Otonom engelden kaçınma sahada gösterilir.

## 4. Test Kayıt Şablonu

| Tarih | Test ID | Yapan | Sonuç (G/K) | Ölçüm/Not | Düzeltme |
|---|---|---|---|---|---|
| | | | | | |

## 5. Test Güvenliği
- Her suda testte **can simidi/ip** ve ikinci kişi.
- Önce kuru, sonra sığ su.
- Batarya yanına yangın söndürücü/kum; hasarlı Li‑ion kullanılmaz.
