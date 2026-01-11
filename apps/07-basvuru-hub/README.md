# Başvuru Hub

Üniversiteye yapılan tüm başvuruların (öğrenci, personel, etkinlik, vb.) merkezi yönetim platformu.

## 🎯 Amaç

Farklı türdeki başvuruların tek bir noktadan toplanması, değerlendirilmesi ve takip edilmesi.

## 📋 Kapsam

- Öğrenci başvuruları (lisans, lisansüstü, yatay geçiş)
- Personel başvuruları (akademik, idari)
- Burs başvuruları
- Yurt başvuruları
- Etkinlik başvuruları
- Genel dilekçe ve talepler

## ✨ Temel Özellikler

### Başvuru Türleri
- Lisans kayıt başvurusu
- Lisansüstü başvuru (YL/DR)
- Yatay geçiş başvurusu
- Dikey geçiş başvurusu
- Özel öğrenci başvurusu
- Akademik personel başvurusu
- İdari personel başvurusu
- Burs başvurusu
- Yurt başvurusu
- Etkinlik/salon başvurusu
- Genel dilekçe

### Form Yönetimi
- Dinamik form oluşturucu
- Koşullu alanlar
- Dosya yükleme
- Çoklu dil desteği
- Form versiyonlama
- Form şablonları

### Başvuru Süreci
- Online başvuru
- Başvuru durumu takibi
- Eksik belge bildirimi
- Otomatik bilgilendirmeler
- Başvuru düzenleme
- Başvuru iptali

### Değerlendirme
- Değerlendirici atama
- Puanlama kriterleri
- Çoklu değerlendirici desteği
- Otomatik eleme kriterleri
- Mülakat planlama
- Sonuç bildirimi

### Dönem Yönetimi
- Başvuru dönemleri tanımlama
- Kontenjan belirleme
- Açılış/kapanış tarihleri
- Sonuç ilan tarihleri

### Raporlama
- Başvuru istatistikleri
- Dönem karşılaştırmaları
- Demografik analizler
- Dönüşüm raporları

## 🔗 Entegrasyonlar

| Sistem | Entegrasyon Türü | Açıklama |
|--------|------------------|----------|
| SSO | Kimlik Doğrulama | Başvuru girişi |
| E-Devlet | Veri Doğrulama | Kimlik, eğitim doğrulama |
| ÖSYM | Veri Çekme | Sınav sonuçları |
| Akademik Sistem | Veri Aktarımı | Kabul edilen öğrenciler |
| Personel Sistemi | Veri Aktarımı | Kabul edilen personel |
| Email Hub | Bildirim | Başvuru bildirimleri |
| SMS Gateway | Bildirim | Anlık bildirimler |
| Sanal Pos | Ödeme | Başvuru ücreti |

## 👥 Kullanıcı Rolleri

| Rol | Yetkiler |
|-----|----------|
| **Başvuru Sahibi** | Başvuru yapma, takip |
| **Değerlendirici** | Atanan başvuruları değerlendirme |
| **Birim Sorumlusu** | Birim başvuruları yönetimi |
| **Başvuru Yöneticisi** | Tüm başvurular, raporlama |
| **Sistem Yöneticisi** | Form tasarım, konfigürasyon |

## 🗃️ Veritabanı Şeması (Temel)

```
application_types
├── id
├── code
├── name
├── description
├── form_schema (JSON)
├── workflow_id
├── settings (JSON)
├── status
└── timestamps

application_periods
├── id
├── application_type_id
├── name
├── start_date
├── end_date
├── quota
├── evaluation_start
├── result_date
├── status
└── timestamps

applications
├── id
├── application_type_id
├── period_id
├── applicant_id
├── application_number
├── form_data (JSON)
├── status
├── submitted_at
├── score
└── timestamps

applicants
├── id
├── national_id
├── email
├── phone
├── first_name
├── last_name
├── password_hash
├── email_verified
└── timestamps

application_documents
├── id
├── application_id
├── document_type
├── file_name
├── file_url
├── status (beklemede, onaylı, reddedildi)
├── review_notes
└── timestamps

evaluations
├── id
├── application_id
├── evaluator_id
├── criteria_scores (JSON)
├── total_score
├── notes
├── recommendation
├── evaluated_at
└── timestamps

interviews
├── id
├── application_id
├── scheduled_at
├── location
├── interviewers[]
├── score
├── notes
├── status
└── timestamps
```

## 🛠️ Teknik Gereksinimler

### Backend
- Runtime: Node.js 18+
- Framework: NestJS
- Form Engine: JSON Schema + AJV

### Frontend
- Framework: Next.js 14+
- Form Renderer: React Hook Form + JSON Schema
- UI: Tailwind CSS + Shadcn/ui

### Veritabanı
- Primary: PostgreSQL 15+
- File Storage: S3-compatible

## 📁 Modül Yapısı

```
07-basvuru-hub/
├── src/
│   ├── modules/
│   │   ├── types/             # Başvuru türleri
│   │   ├── periods/           # Dönem yönetimi
│   │   ├── applications/      # Başvurular
│   │   ├── applicants/        # Başvuru sahipleri
│   │   ├── documents/         # Belgeler
│   │   ├── evaluations/       # Değerlendirmeler
│   │   ├── interviews/        # Mülakatlar
│   │   ├── forms/             # Form yönetimi
│   │   └── reports/           # Raporlama
│   ├── form-builder/          # Form tasarımcısı
│   ├── public-portal/         # Başvuru portali
│   └── admin/                 # Yönetim paneli
├── tests/
├── docs/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - Temel (MVP)
- [ ] Temel başvuru akışı
- [ ] Form oluşturucu (basit)
- [ ] Dönem yönetimi

### Faz 2 - Genişletme
- [ ] Değerlendirme modülü
- [ ] Mülakat planlama
- [ ] E-Devlet entegrasyonu

### Faz 3 - İleri Özellikler
- [ ] Gelişmiş form builder
- [ ] Otomatik değerlendirme
- [ ] Analytics dashboard
- [ ] Çoklu dil desteği

## 📊 KPI'lar

- Başvuru tamamlanma oranı
- Ortalama değerlendirme süresi
- Başvuru-kabul oranı
- Kullanıcı deneyimi skoru

## 📋 Örnek Başvuru Akışı

```
Başvuru Oluştur → Form Doldur → Belge Yükle → Ödeme (varsa) → Gönder
                                                                ↓
Sonuç ← Karar ← Değerlendirme ← Belge Kontrolü ← Başvuru Alındı
```

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı

**Öncelik:** Yüksek

**Tahmini Süre:** 4-5 ay
