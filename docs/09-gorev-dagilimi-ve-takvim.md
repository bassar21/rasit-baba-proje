# 09 – Görev Dağılımı ve Takvim

> Ekip üyeleri isimlerini ve sürelerini doldurmalıdır. Takvim 12 haftalık **öneridir**.

## 1. Roller

| Rol | Sorumluluk | Kişi |
|---|---|---|
| Proje yöneticisi | Takvim, toplantı, bütçe | |
| Elektronik | Devre, güç, sensörler | |
| Yazılım (firmware) | Arduino kodu, protokol | |
| Mobil/arayüz | Telefon kumandası | |
| Mekanik | Gövde, panel kolu, yalıtım | |
| Test/dokümantasyon | Test, raporlama, kayıt | |

(Küçük ekipte bir kişi birden fazla rol alabilir.)

## 2. Haftalık Plan

| Hafta | Hedef | Çıktı |
|---|---|---|
| 1 | Gereksinim netleştirme, açık soruların cevabı | Karar listesi, 01 güncel |
| 2 | Malzeme siparişi, panel suda deneyi (R1) | BOM kesin, deney sonucu |
| 3 | Gövde yapımı, elektronik masa testi (T1) | Gövde ham, T1 geçti |
| 4 | Tahrik + Bluetooth kumanda | 4 yön çalışıyor |
| 5 | Panel kolu mekanizması | Mekanizma çalışıyor |
| 6 | Entegrasyon, failsafe, limit (T2, T3) | Kuru testler geçti |
| 7 | Su yalıtımı ve sızdırmazlık (T4) | Sızıntısız |
| 8 | Havuz testi: denge, seyir (T5, T6) | Seyir videosu |
| 9 | Enerji sistemi ve ölçüm (T7) | Enerji raporu |
| 10 | Otonom Seviye 1–2 (T8) | Engelden kaçınma |
| 11 | İyileştirme, hata düzeltme | Kararlı sürüm |
| 12 | Final testleri, sunum, dokümantasyon | Sunum + rapor |

## 3. Kilometre Taşları
- M1 (Hf. 4): Uzaktan 4 yön
- M2 (Hf. 6): Panel kolu + güvenlik
- M3 (Hf. 8): Suda çalışan prototip
- M4 (Hf. 10): Otonom demo
- M5 (Hf. 12): Final

## 4. Toplantı Düzeni
- Haftalık 30–45 dk: yapılan / yapılacak / engeller.
- Karar defteri: önemli kararlar tarih ve gerekçeyle yazılır.

## 5. Git İş Akışı Önerisi (kodsuz süreç)
- `main`: kararlı. Kişisel dallar (`yigit` vb.) ile çalış, Pull Request ile birleştir.
- Commit mesajları kısa ve anlamlı, belge ve donanım değişiklikleri de repoya girer.
- Önerilen klasörler: `docs/`, `firmware/`, `app/`, `hardware/`, `media/`.

## 6. Görev Kartı Şablonu

| Görev | Sorumlu | Başlangıç | Bitiş | Durum | Not |
|---|---|---|---|---|---|
| | | | | | |
