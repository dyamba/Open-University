# Klinik Araştırma Hub

Üniversite hastanesi ve tıp fakültesinin klinik araştırma süreçlerinin yönetimi.

## 🎯 Amaç

İlaç, tıbbi cihaz ve klinik çalışmaların başvuru, yürütme ve raporlama süreçlerinin merkezi yönetimi.

## 📋 Kapsam

- Klinik çalışma başvuruları
- Çalışma protokol yönetimi
- Hasta/gönüllü takibi
- Yan etki raporlama (SAE/SUSAR)
- Sponsor ve CRO ilişkileri
- Mevzuat uyumu

## ✨ Temel Özellikler

### Çalışma Yönetimi
- Çalışma kaydı ve protokol
- Faz bilgisi (Faz I-IV)
- Sponsor ve destekçiler
- Araştırma ekibi
- Merkez bilgileri
- Çalışma takvimi

### Hasta/Gönüllü Yönetimi
- Gönüllü kaydı
- Uygunluk kriterleri kontrolü
- Randomizasyon
- Vizit planlaması
- Bilgilendirilmiş onam takibi
- Hasta güvenliği

### Protokol Yönetimi
- Protokol versiyonlama
- Değişiklik (amendment) takibi
- Sapma (deviation) kayıtları
- Protokol ihlalleri

### Güvenlik Raporlama
- Advers olay (AE) kaydı
- Ciddi advers olay (SAE)
- SUSAR bildirimleri
- TİTCK raporlama
- Sponsor bildirimleri

### Mevzuat Uyumu
- TİTCK başvuruları
- Etik kurul onayları
- Sigorta takibi
- Denetim hazırlığı
- GCP uyumu

### Doküman Yönetimi
- Trial Master File (TMF)
- Investigator Site File (ISF)
- Essential documents
- Versiyon kontrolü

### Raporlama
- Çalışma ilerleme raporları
- Hasta alım raporları
- Güvenlik raporları
- Mevzuat raporları

## 🔗 Entegrasyonlar

| Sistem | Entegrasyon Türü | Açıklama |
|--------|------------------|----------|
| SSO | Kimlik Doğrulama | Merkezi oturum |
| Etik Kurul | Entegrasyon | Etik onaylar |
| Hasta Bilgi Sistemi | Veri Çekme | Hasta verileri |
| Proje Yönetimi | Veri Paylaşımı | Proje bilgileri |
| Laboratuvar | Veri Çekme | Lab sonuçları |
| EBYS | Belge | Resmi yazışmalar |

## 👥 Kullanıcı Rolleri

| Rol | Yetkiler |
|-----|----------|
| **Sorumlu Araştırıcı** | Çalışma yönetimi |
| **Alt Araştırıcı** | Hasta takibi, veri girişi |
| **Çalışma Koordinatörü** | Operasyonel yönetim |
| **Veri Yöneticisi** | Veri kalitesi |
| **Güvenlik Sorumlusu** | SAE/SUSAR yönetimi |
| **Klinik Araştırma Müdürü** | Tam yetki |

## 🗃️ Veritabanı Şeması (Temel)

```
clinical_studies
├── id
├── protocol_number
├── title
├── phase
├── therapeutic_area
├── sponsor_id
├── principal_investigator_id
├── status
├── start_date
├── end_date
├── target_enrollment
├── current_enrollment
└── timestamps

subjects
├── id
├── study_id
├── subject_number
├── screening_date
├── randomization_date
├── randomization_code
├── status
├── consent_date
├── withdrawal_date
├── withdrawal_reason
└── timestamps

visits
├── id
├── subject_id
├── visit_number
├── visit_name
├── scheduled_date
├── actual_date
├── status
├── notes
└── timestamps

adverse_events
├── id
├── subject_id
├── event_term
├── start_date
├── end_date
├── severity
├── seriousness
├── causality
├── outcome
├── action_taken
├── is_sae
├── reported_date
└── timestamps

protocol_amendments
├── id
├── study_id
├── amendment_number
├── description
├── submission_date
├── approval_date
├── implementation_date
└── timestamps

protocol_deviations
├── id
├── study_id
├── subject_id
├── deviation_type
├── description
├── deviation_date
├── reported_date
├── corrective_action
└── timestamps
```

## 🛠️ Teknik Gereksinimler

### Backend
- Runtime: Node.js 18+
- Framework: NestJS
- Audit Trail: Tam izlenebilirlik

### Frontend
- Framework: Next.js 14+
- UI: Tailwind CSS

### Veritabanı
- Primary: PostgreSQL 15+
- Audit: Değiştirilemez kayıtlar

### Güvenlik & Uyumluluk
- 21 CFR Part 11 uyumu
- GCP uyumu
- Veri şifreleme

## 📁 Modül Yapısı

```
14-klinik-arastirma-hub/
├── src/
│   ├── modules/
│   │   ├── studies/           # Çalışma yönetimi
│   │   ├── subjects/          # Hasta/gönüllü
│   │   ├── visits/            # Vizit yönetimi
│   │   ├── safety/            # Güvenlik raporlama
│   │   ├── protocols/         # Protokol yönetimi
│   │   ├── documents/         # TMF/ISF
│   │   ├── regulatory/        # Mevzuat
│   │   └── reports/           # Raporlama
│   └── audit/                 # Audit trail
├── tests/
├── docs/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - Temel (MVP)
- [ ] Çalışma kaydı
- [ ] Hasta takibi
- [ ] Vizit yönetimi

### Faz 2 - Genişletme
- [ ] SAE/SUSAR modülü
- [ ] Protokol değişiklikleri
- [ ] Doküman yönetimi

### Faz 3 - İleri Özellikler
- [ ] TİTCK entegrasyonu
- [ ] Randomizasyon sistemi
- [ ] Gelişmiş raporlama

## 📊 KPI'lar

- Hasta alım hızı
- Protokol sapma oranı
- SAE raporlama süresi
- Çalışma tamamlanma oranı

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı

**Öncelik:** Orta (Tıp Fakültesi varsa)

**Tahmini Süre:** 5-6 ay
