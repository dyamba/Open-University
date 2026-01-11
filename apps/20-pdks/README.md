# PDKS - Personel Devam Kontrol Sistemi

Personel giriş-çıkış ve devam takip sistemi.

## 🎯 Amaç

Personelin çalışma saatlerinin takibi, devamsızlık yönetimi ve puantaj raporlarının oluşturulması.

## 📋 Kapsam

- Giriş/çıkış kaydı
- Mesai takibi
- Fazla mesai hesaplama
- İzin entegrasyonu
- Puantaj raporları

## ✨ Temel Özellikler

### Giriş/Çıkış Kaydı
- Kart okuyucu entegrasyonu
- Mobil check-in (GPS)
- QR kod ile giriş
- Biyometrik (opsiyonel)
- Manuel kayıt (yetkili)

### Mesai Yönetimi
- Çalışma takvimi tanımlama
- Vardiya yönetimi
- Esnek çalışma saatleri
- Uzaktan çalışma takibi

### Hesaplamalar
- Çalışma saati hesaplama
- Fazla mesai hesaplama
- Eksik mesai takibi
- Geç kalma/erken çıkma

### İzin Entegrasyonu
- İzinli günler otomatik işaretleme
- Resmi tatil yönetimi
- Mazeret kaydı

### Raporlama
- Günlük puantaj
- Aylık puantaj özeti
- Birim raporları
- Bordro entegrasyonu için export

## 🔗 Entegrasyonlar

| Sistem | Entegrasyon Türü |
|--------|------------------|
| SSO | Kimlik Doğrulama |
| Personel Sistemi | İzin verileri |
| Bordro | Puantaj aktarımı |
| Kart Okuyucu | Hardware |

## 🗃️ Veritabanı (Temel)

```
attendance_records
├── id
├── employee_id
├── record_type (in/out)
├── record_time
├── source (card/mobile/manual)
├── location
└── timestamps

work_schedules
├── id
├── name
├── work_days[]
├── start_time
├── end_time
├── break_duration
└── timestamps

employee_schedules
├── id
├── employee_id
├── schedule_id
├── effective_from
├── effective_to
└── timestamps
```

## 📁 Modül Yapısı

```
20-pdks/
├── src/
│   ├── modules/
│   │   ├── attendance/        # Devam kayıtları
│   │   ├── schedules/         # Çalışma takvimleri
│   │   ├── overtime/          # Fazla mesai
│   │   ├── reports/           # Raporlama
│   │   └── devices/           # Cihaz yönetimi
│   └── integrations/          # Hardware entegrasyonu
├── tests/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - MVP
- [ ] Manuel giriş/çıkış
- [ ] Temel puantaj

### Faz 2
- [ ] Kart okuyucu entegrasyonu
- [ ] Fazla mesai hesaplama

### Faz 3
- [ ] Mobil check-in
- [ ] Gelişmiş raporlama

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı | **Öncelik:** Orta | **Süre:** 3-4 ay
