# 10 – Kullanım Kılavuzu ve Güvenlik

## 1. Kullanım Öncesi Kontrol Listesi
- [ ] Batarya şarjlı, şişme/hasar yok
- [ ] Elektronik kutu kapağı ve contalar tamam
- [ ] Panel kolları **yukarıda**, mekanizma serbest
- [ ] Pervane etrafı temiz
- [ ] Acil stop butonu çalışıyor
- [ ] Telefon Bluetooth eşleşti
- [ ] Test alanında insan/hayvan yok, ikinci kişi hazır
- [ ] Kurtarma ipi / kayık / uzun sopa hazır

## 2. Çalıştırma Adımları
1. Ana şalteri aç. LED yanar (hazır).
2. Telefondan Bluetooth ile bağlan.
3. Kısa test: dur komutu, düşük hızda ileri.
4. Suya bırak, sığ suda deneme.
5. Seyir sırasında paneller **yukarıda** kalır.
6. Limana/hedefe gelince: dur → paneli indir (`D`) → şarj izle.
7. Kalkış öncesi: paneli kaldır (`U`), limit onayı bekle.

## 3. Kumanda Özeti

| Eylem | Komut |
|---|---|
| İleri / Geri / Sol / Sağ | F / B / L / R |
| Dur | S |
| Panel indir / kaldır | D / U |
| Otonom / Manuel | A / M |
| Acil durdurma | X (veya fiziksel buton) |

## 4. Acil Durum Prosedürleri

| Durum | Yapılacak |
|---|---|
| Kontrol kaybı | Fiziksel acil stop, ipli/ayakla kurtarma, motorlar zaten failsafe ile durur |
| Su alma | Hemen gücü kes, gemiyi sudan çıkar, kutuyu aç, kurut, batarya incele |
| Batarya ısınma/duman | Gücü kes, suya sokma, güvenli yere (kum) koy, bölgeyi havalandır |
| Panel sıkışması | Motorları durdur, kolu elle serbest bırak, nedeni bul |
| Çarpışma | Dur, hasar kontrolü, sızıntı kontrolü |

## 5. Güvenlik Kuralları
1. Bataryayı **yalnız** uygun şarj aleti ve BMS ile şarj et; şarj sırasında gözetimsiz bırakma.
2. Lehim/kablo çalışmalarında bataryayı çıkar.
3. Suda test ederken tek başına kalma; kıyıya yakın ol.
4. Hasarlı veya şişmiş Li‑ion pili kullanma, normal çöpe atma (toplama noktasına).
5. Hareketli parçaya (pervane, kol) elini yaklaştırma; güç kapalıyken müdahale et.
6. Açık suda (deniz/nehir) izin ve güvenli hava koşulu olmadan test yapma.
7. Çevreye zarar: yağ/atık, pil su kirletmesin; deney sonrası yüzeyi temizle.

## 6. Bakım Programı

| Sıklık | İş |
|---|---|
| Her test sonrası | Tatlı su durulama, kurutma, görsel kontrol |
| Haftalık | Cıvata, kablo, korozyon, servo boşluğu |
| Aylık | Conta/silikon yenileme, batarya sağlığı, yazılım yedeği |

## 7. Sorun Giderme

| Belirti | Olası neden | Çözüm |
|---|---|---|
| Bluetooth bağlanmıyor | Şifre/eşleşme, besleme | Eşleşmeyi sil, 5 V kontrol |
| Arduino resetleniyor | Motor/servo akımı | Ayrı buck, kondansatör, kablo kalınlığı |
| Motor tek yönde | Sürücü pini/kablo | Pin haritası (03) kontrol |
| Servo titrer/ısınır | Yetersiz besleme/yük | 6 V ≥ 3 A, mekanik sürtünme |
| Panel üretmiyor | Gölge/su/bağlantı | Gerilimi ölç, konnektör, şarj kontrolcü |
| Gemi bir yana yatar | Trim/balast | Ağırlık yeniden dağıt |
