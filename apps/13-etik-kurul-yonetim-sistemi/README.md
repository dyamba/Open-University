# Etik Kurul Yönetim Sistemi

Üniversite etik kurullarının başvuru, değerlendirme ve onay süreçlerinin yönetimi.

## 🎯 Amaç

Araştırma etiği başvurularının dijital ortamda alınması, değerlendirilmesi ve karara bağlanması süreçlerinin yönetimi.

## 📋 Kapsam

- Etik kurul başvuruları
- Değerlendirme süreçleri
- Kurul toplantı yönetimi
- Karar ve onay takibi
- Belge yönetimi
- Raporlama

## ✨ Temel Özellikler

### Başvuru Yönetimi
- Online başvuru formu
- Proje bilgileri
- Araştırma yöntemi
- Katılımcı bilgileri
- Riskli değerlendirme
- Aydınlatılmış onam formu
- Destekleyici belgeler

### Etik Kurul Türleri
- İnsan Araştırmaları Etik Kurulu
- Hayvan Deneyleri Etik Kurulu
- Klinik Araştırmalar Etik Kurulu
- Sosyal ve Beşeri Bilimler Etik Kurulu
- Yayın Etiği Kurulu

### Değerlendirme Süreci
- Ön inceleme
- Hakem ataması
- Hakem değerlendirmesi
- Revizyon talebi
- Kurul gündemine alma
- Nihai karar

### Kurul Yönetimi
- Kurul üyeleri
- Toplantı planlama
- Gündem oluşturma
- Toplantı tutanakları
- Karar kayıtları
- Üye çıkar çatışması kontrolü

### Karar Türleri
- Onay
- Koşullu onay
- Revizyon talebi
- Red
- Muafiyet

### Belge Yönetimi
- Başvuru formları
- Onam formları
- Değerlendirme raporları
- Karar belgeleri
- Etik onay sertifikası

### Takip & İzleme
- Proje ilerleme raporları
- Yan etki bildirimleri
- Protokol değişiklikleri
- Proje sonlandırma

## 🔗 Entegrasyonlar

| Sistem | Entegrasyon Türü | Açıklama |
|--------|------------------|----------|
| SSO | Kimlik Doğrulama | Merkezi oturum |
| Proje Yönetimi | Veri Paylaşımı | Proje bilgileri |
| Personel Sistemi | Veri Çekme | Araştırmacı bilgileri |
| EBYS | Belge | Resmi yazışmalar |
| Email Hub | Bildirim | Süreç bildirimleri |
| Klinik Araştırma Hub | Entegrasyon | Klinik çalışmalar |

## 👥 Kullanıcı Rolleri

| Rol | Yetkiler |
|-----|----------|
| **Araştırmacı** | Başvuru yapma, takip |
| **Kurul Sekreteri** | Başvuru ön inceleme, gündem |
| **Hakem** | Değerlendirme |
| **Kurul Üyesi** | Toplantı, oylama |
| **Kurul Başkanı** | Karar onay |
| **Sistem Yöneticisi** | Konfigürasyon |

## 🗃️ Veritabanı Şeması (Temel)

```
ethics_committees
├── id
├── name
├── code
├── type
├── chair_id
├── secretary_id
├── members[]
├── meeting_frequency
├── status
└── timestamps

applications
├── id
├── application_number
├── committee_id
├── applicant_id
├── project_title
├── project_type
├── research_method
├── participant_info (JSON)
├── risk_assessment
├── funding_source
├── status
├── submitted_at
└── timestamps

application_documents
├── id
├── application_id
├── document_type
├── file_name
├── file_url
├── version
└── timestamps

reviewers
├── id
├── application_id
├── reviewer_id
├── assigned_by
├── assigned_at
├── due_date
├── status
└── timestamps

reviews
├── id
├── application_id
├── reviewer_id
├── recommendation
├── comments
├── checklist_responses (JSON)
├── submitted_at
└── timestamps

meetings
├── id
├── committee_id
├── meeting_date
├── location
├── agenda_items[]
├── attendees[]
├── minutes
├── status
└── timestamps

decisions
├── id
├── application_id
├── meeting_id
├── decision_type
├── conditions
├── valid_until
├── decision_date
├── certificate_number
└── timestamps

amendments
├── id
├── application_id
├── amendment_type
├── description
├── status
├── submitted_at
├── approved_at
└── timestamps

adverse_events
├── id
├── application_id
├── event_date
├── description
├── severity
├── action_taken
├── reported_by
├── reported_at
└── timestamps
```

## 🛠️ Teknik Gereksinimler

### Backend
- Runtime: Node.js 18+
- Framework: NestJS
- PDF Generation: Puppeteer / PDFKit

### Frontend
- Framework: Next.js 14+
- Forms: React Hook Form
- UI: Tailwind CSS

### Veritabanı
- Primary: PostgreSQL 15+
- File Storage: S3-compatible

## 📁 Modül Yapısı

```
13-etik-kurul-yonetim-sistemi/
├── src/
│   ├── modules/
│   │   ├── committees/        # Kurul yönetimi
│   │   ├── applications/      # Başvurular
│   │   ├── reviews/           # Değerlendirmeler
│   │   ├── meetings/          # Toplantılar
│   │   ├── decisions/         # Kararlar
│   │   ├── amendments/        # Değişiklikler
│   │   ├── adverse-events/    # Yan etkiler
│   │   ├── documents/         # Belgeler
│   │   └── reports/           # Raporlama
│   ├── templates/             # Form şablonları
│   └── certificates/          # Sertifika oluşturma
├── tests/
├── docs/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - Temel (MVP)
- [ ] Başvuru formu
- [ ] Temel değerlendirme akışı
- [ ] Karar kayıtları

### Faz 2 - Genişletme
- [ ] Hakem atama sistemi
- [ ] Toplantı yönetimi
- [ ] Sertifika oluşturma

### Faz 3 - İleri Özellikler
- [ ] Çoklu kurul desteği
- [ ] İlerleme raporları
- [ ] Yan etki takibi
- [ ] Gelişmiş raporlama

## 📊 KPI'lar

- Ortalama değerlendirme süresi
- Başvuru onay oranı
- Revizyon oranı
- Kurul toplantı verimliliği

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı

**Öncelik:** Orta

**Tahmini Süre:** 3-4 ay
