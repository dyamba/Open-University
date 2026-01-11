# Mezun Bilgi Sistemi

Üniversite mezunlarının takibi, iletişimi ve kariyer gelişimlerinin izlenmesi için platform.

## 🎯 Amaç

Mezunlarla sürdürülebilir ilişki kurulması, kariyer takibi, kurumsal hafıza oluşturulması ve mezun katkılarının yönetimi.

## 📋 Kapsam

- Mezun veri tabanı yönetimi
- Mezun iletişim ve etkinlikleri
- Kariyer takibi
- İşveren-mezun eşleştirme
- Bağış ve katkı yönetimi
- Mentörlük programları

## ✨ Temel Özellikler

### Mezun Profili
- Kişisel bilgiler
- Eğitim geçmişi
- Kariyer geçmişi
- Yetkinlikler
- Sosyal medya bağlantıları
- Fotoğraf ve biyografi

### Mezun Kaydı
- Otomatik mezun aktarımı
- Self-servis kayıt
- Profil doğrulama
- Mezuniyet belgesi entegrasyonu

### Kariyer Takibi
- İş durumu güncelleme
- Şirket ve pozisyon bilgileri
- Sektör ve lokasyon
- Maaş aralığı (opsiyonel)
- Kariyer değişikliği geçmişi

### İletişim Yönetimi
- Toplu email gönderimi
- Segmentasyon (bölüm, yıl, sektör)
- Newsletter yönetimi
- Bildirim tercihleri
- İletişim geçmişi

### Etkinlikler
- Mezun buluşmaları
- Networking etkinlikleri
- Kariyer günleri
- Online webinarlar
- Etkinlik kaydı ve katılım

### Mentörlük Programı
- Mentor-mentee eşleştirme
- Program tanımlama
- İlerleme takibi
- Geri bildirim toplama

### İş İlanları
- Mezun şirketlerinden ilanlar
- İş arayanlar havuzu
- Eşleştirme ve öneriler
- Başvuru takibi

### Bağış Yönetimi
- Bağış kampanyaları
- Online bağış toplama
- Bağışçı takibi
- Teşekkür ve tanıma
- Mali raporlama

### Anketler
- Mezun memnuniyeti
- İstihdam anketleri
- Program değerlendirme
- YÖKAK zorunlu anketleri

## 🔗 Entegrasyonlar

| Sistem | Entegrasyon Türü | Açıklama |
|--------|------------------|----------|
| SSO | Kimlik Doğrulama | Mezun girişi |
| Akademik Sistem | Veri Aktarımı | Mezuniyet verileri |
| LinkedIn | API | Kariyer verileri (izinli) |
| Email Hub | Bildirim | Toplu email |
| CMS | Veri Paylaşımı | Mezun haberleri |
| Anket Sistemi | Entegrasyon | Mezun anketleri |
| Sanal Pos | Ödeme | Bağış işlemleri |
| Kariyer Merkezi | Veri Paylaşımı | İş ilanları |

## 👥 Kullanıcı Rolleri

| Rol | Yetkiler |
|-----|----------|
| **Mezun** | Profil yönetimi, etkinlik kaydı |
| **Mezun Temsilcisi** | Bölüm mezunları |
| **Mezun Koordinatörü** | İletişim, etkinlik yönetimi |
| **Kariyer Danışmanı** | İş ilanları, eşleştirme |
| **Mezun Müdürü** | Tam yetki |
| **Sistem Yöneticisi** | Konfigürasyon |

## 🗃️ Veritabanı Şeması (Temel)

```
alumni
├── id
├── student_id (eski öğrenci kaydı)
├── first_name
├── last_name
├── email
├── phone
├── graduation_year
├── department_id
├── program_id
├── degree_type
├── profile_photo
├── bio
├── linkedin_url
├── communication_preferences (JSON)
├── is_verified
├── last_login
└── timestamps

career_history
├── id
├── alumni_id
├── company
├── position
├── sector
├── location
├── start_date
├── end_date
├── is_current
├── salary_range
└── timestamps

alumni_events
├── id
├── title
├── description
├── event_type
├── date
├── location
├── capacity
├── registration_deadline
├── is_online
├── meeting_url
├── status
└── timestamps

event_registrations
├── id
├── event_id
├── alumni_id
├── registration_date
├── attended
├── feedback
└── timestamps

mentorship_programs
├── id
├── name
├── description
├── start_date
├── end_date
├── status
└── timestamps

mentorship_pairs
├── id
├── program_id
├── mentor_id (alumni)
├── mentee_id (student/alumni)
├── status
├── notes
└── timestamps

donations
├── id
├── alumni_id
├── campaign_id
├── amount
├── payment_method
├── transaction_id
├── status
├── donated_at
└── timestamps

donation_campaigns
├── id
├── title
├── description
├── target_amount
├── collected_amount
├── start_date
├── end_date
├── status
└── timestamps

job_postings
├── id
├── posted_by (alumni_id)
├── company
├── position
├── description
├── requirements
├── location
├── salary_range
├── application_deadline
├── status
└── timestamps
```

## 🛠️ Teknik Gereksinimler

### Backend
- Runtime: Node.js 18+
- Framework: NestJS
- Email: SendGrid / Amazon SES

### Frontend
- Framework: Next.js 14+
- UI: Tailwind CSS + Shadcn/ui

### Veritabanı
- Primary: PostgreSQL 15+
- Search: Elasticsearch (mezun arama)

## 📁 Modül Yapısı

```
09-mezun-bilgi-sistemi/
├── src/
│   ├── modules/
│   │   ├── alumni/            # Mezun yönetimi
│   │   ├── careers/           # Kariyer takibi
│   │   ├── events/            # Etkinlikler
│   │   ├── mentorship/        # Mentörlük
│   │   ├── jobs/              # İş ilanları
│   │   ├── donations/         # Bağış yönetimi
│   │   ├── communications/    # İletişim
│   │   ├── surveys/           # Anketler
│   │   └── reports/           # Raporlama
│   ├── portal/                # Mezun portali
│   └── admin/                 # Yönetim paneli
├── tests/
├── docs/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - Temel (MVP)
- [ ] Mezun kaydı ve profil
- [ ] Temel iletişim
- [ ] Etkinlik modülü

### Faz 2 - Genişletme
- [ ] Kariyer takibi
- [ ] İş ilanları
- [ ] Mentörlük programı

### Faz 3 - İleri Özellikler
- [ ] Bağış yönetimi
- [ ] LinkedIn entegrasyonu
- [ ] Gelişmiş analytics
- [ ] Mobil uygulama

## 📊 KPI'lar

- Aktif mezun oranı
- Etkinlik katılım oranı
- İstihdam oranı
- Bağış toplama başarısı
- Mentörlük eşleştirme oranı

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı

**Öncelik:** Orta

**Tahmini Süre:** 4-5 ay
