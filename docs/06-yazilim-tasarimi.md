# 06 – Yazılım Tasarımı (kodsuz spesifikasyon)

> Bu belge **kod değildir**; kod yazılırken uyulacak gereksinim ve tasarımı tanımlar.

## 1. Yazılım Bileşenleri

| Bileşen | Görev |
|---|---|
| Haberleşme | Bluetooth’tan komut alma, telemetri gönderme |
| Komut çözücü | Komutu doğrula, duruma göre uygula |
| Hareket kontrolü | Motor hız/yön, yumuşak kalkış/duruş |
| Panel kontrolü | Servo hareketi, limit okuma, yavaş açma/kapama |
| Güvenlik | Zaman aşımı, acil durdurma, düşük batarya |
| Sensör okuma | Mesafe, V/I, pusula, GPS |
| Otonomi | Engelden kaçınma, yön tutma, rota |
| Telemetri | Periyodik durum raporu |

## 2. Komut Protokolü

Basit, tek karakter + isteğe bağlı sayı. Satır sonu `\n`.

| Komut | Anlam | Not |
|---|---|---|
| `F` | İleri | |
| `B` | Geri | |
| `L` | Sola dön | |
| `R` | Sağa dön | |
| `S` | Dur | Her durumda geçerli |
| `0`–`9` | Hız kademesi | 0 = dur, 9 = tam |
| `U` | Panelleri yukarı | Panel durumuna göre |
| `D` | Panelleri indir | Hız sınırı koşulu |
| `A` | Otonom aç | |
| `M` | Manuel moda dön | |
| `X` | Acil durdurma | Kilitlenir, sıfırlama gerekir |
| `?` | Durum sorgusu | Telemetri döner |

Telemetri satırı (örnek): `T;mod=MANUEL;pnl=ALT;vbat=7.6;ipnl=0.31;vp=6.1;d=85`

## 3. Durum Makinesi

```
      ┌─────────┐   bağlantı    ┌─────────┐   'A'   ┌─────────┐
      │ BAŞLAT  │──────────────►│ MANUEL  │────────►│ OTONOM  │
      └─────────┘               └────┬────┘◄────────└────┬────┘
                                     │  'M'/engel yok     │
                                'D'  │                    │ liman algılandı
                                     ▼                    ▼
                               ┌───────────┐         ┌───────────┐
                               │ PANEL İNİŞ│         │ LİMAN     │ (paneller açık, şarj)
                               └───────────┘         └───────────┘
   Her durumdan  ──► ACİL_DUR (kilitli, 'sıfırla' ile çıkar)
```

## 4. Güvenlik Kuralları (zorunlu)

1. **Bağlantı zaman aşımı:** 1–2 sn komut gelmezse motorlar durur (failsafe).
2. **Panel inikken hız sınırı** (ör. en çok %30) veya tamamen kısıtlama.
3. **Panel hareketi sırasında** motorlar durdurulur/yavaşlatılır.
4. **Limit anahtarı** hareketi keser; ayrıca hareket zaman aşımı (takılma koruması).
5. **Düşük batarya:** eşik altında (örn. 6.4 V) yeni hareket reddedilir, panel yukarıdaysa limana dönüş uyarısı.
6. **Aşırı akım:** motor/servo akımı eşik üstündeyse durdur.
7. **Yön değiştirirken** önce dur → kısa bekleme → ters yön (motor sürücü koruma).
8. **Başlangıçta** güvenli durum: motorlar durmuş, paneller yukarı.

## 4.0 Panel Kontrolü Gereksinimleri (güncel)
- **Tek komut, tüm paneller:** `D` ve `U` yalnız toplu hareketi başlatır; tek tek panel kontrolü yok.
- **Mekanizma bağımsız tasarım:** Yazılım "panel durumu" soyutlaması kullanmalı: `ÜST`, `ALT`, `HAREKETTE`, `HATA`. Servo/aktüatör/motor seçimi sonradan yalnız bu katmanı değiştirmeli.
- **Konum bilgisi:** Üst/alt limit anahtarları esas; açı bilgisi yoksa hareket **zaman aşımı** ile güvenceye alınır.
- **Takılma/aşırı akım:** Hareket sırasında beklenen süre aşılır veya akım eşiği geçilirse durdur, `HATA`.
- **Hareket ön koşulları:** gemi duruyor (motorlar 0), batarya eşik üstünde, acil durdurma aktif değil.
- **Panel sayısı** yazılımı etkilemez; yalnız telemetride (toplam güç) görünür.
- Parametreler: hareket süresi, zaman aşımı, akım eşiği (mekanizma seçilince belirlenecek).

## 4.1 Yumuşak Hareket
- Hız ve servo açısı rampa ile değişir (ani sıçrama yok).
- Panel hızı: ≈ 3–5 sn tam strok.

## 5. Otonom Sürüş Gereksinimleri (prototip)

| Seviye | Yetenek | Gereksinim |
|---|---|---|
| 0 | Manuel | – |
| 1 | Yön tutma | Pusula/IMU |
| 2 | Engelden kaçınma | Ultrasonik (ön, sağ, sol) |
| 3 | Noktaya gitme | GPS (açık alan) |
| 4 | Liman modu | Geofence + panel açma + şarj izleme |

Otonom modda kullanıcı `S`/`X` ile **her an** kontrolü geri alabilir.

## 6. Telefon Uygulaması Gereksinimleri
**Karar (ekip):** Kontrol **telefon üzerinden** yapılacak; fiziksel kumanda yok. Bu yüzden telefon arayüzü tek kontrol noktasıdır ve güvenlik (bağlantı kopması, acil durdurma) özellikle önemlidir.

Teknik notlar:
- Android: HC‑05 klasik Bluetooth ile doğrudan çalışır (MIT App Inventor `BluetoothClient`). **iPhone klasik Bluetooth SPP desteklemez** → iPhone kullanılacaksa HM‑10/BLE modül gerekir (02’de alternatif). Ekipte hangi telefonlar var, belirlenmeli.
- Uygulama komutları 02 §2’deki protokole göre gönderir; basılı tutma ile hareket (bırakınca `S`) güvenli tasarımdır.
- Uygulama arka plana alınırsa/ekran kapanırsa bağlantı kopabilir → gemi failsafe ile durmalı.
- Yön tuşları (↑ ↓ ← →), Dur, Hız kaydırıcı.
- Panel Aşağı / Yukarı butonları, durum göstergesi.
- Otonom aç/kapat, acil durdurma (büyük kırmızı).
- Telemetri: batarya %, panel gücü, mod, bağlantı durumu.
- Araç: MIT App Inventor (hızlı) veya hazır Bluetooth Serial uygulaması.

## 7. Konfigürasyon Parametreleri (tek yerde tutulacak)
- Motor maksimum PWM, rampa süresi
- Servo üst/alt açı, hareket süresi
- Zaman aşımı süreleri
- Batarya eşikleri
- Mesafe eşikleri (engel)

## 8. Kodlama Dışı Kalite Gereksinimleri
- Sürüm numarası ve değişiklik günlüğü.
- Pin haritası tek dosyada (03 ile uyumlu).
- Kod gözden geçirme (en az iki kişi).
- Her özellik için test kaydı (07).
