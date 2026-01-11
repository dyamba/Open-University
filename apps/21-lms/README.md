# LMS - Öğrenme Yönetim Sistemi

Üniversitenin çevrimiçi eğitim ve öğrenme platformu.

## 🎯 Amaç

Ders içeriklerinin yönetimi, ödev ve sınavların dijital ortamda yapılması, öğrenci-öğretim üyesi etkileşiminin sağlanması.

## 📋 Kapsam

- Ders içerik yönetimi
- Ödev ve değerlendirme
- Çevrimiçi sınav
- Tartışma forumları
- Video konferans entegrasyonu
- Öğrenme analitiği

## ✨ Temel Özellikler

### Ders Yönetimi
- Ders oluşturma ve düzenleme
- Modül/hafta bazlı yapı
- İçerik sıralama
- Görünürlük kontrolü
- Ders kopyalama/şablonlar

### İçerik Türleri
- Metin içerik
- Dosya paylaşımı (PDF, DOC, PPT)
- Video içerik
- SCORM paketleri
- Harici bağlantılar
- Etkileşimli içerik (H5P)

### Ödev Yönetimi
- Ödev oluşturma
- Teslim tarihi belirleme
- Dosya yükleme
- Rubrik değerlendirme
- Geç teslim politikası
- Grup ödevleri
- Akran değerlendirme

### Sınav Sistemi
- Çoktan seçmeli
- Doğru/yanlış
- Boşluk doldurma
- Eşleştirme
- Açık uçlu
- Soru bankası
- Rastgele soru seçimi
- Zaman sınırı
- Otomatik puanlama

### İletişim
- Duyurular
- Tartışma forumları
- Mesajlaşma
- Canlı ders (Zoom/Meet entegrasyonu)
- Sanal sınıf

### Değerlendirme
- Not defteri
- Ağırlıklı puanlama
- Geçme notu ayarları
- Not ölçekleri
- Başarı sertifikası

### Öğrenme Analitiği
- Öğrenci aktivite takibi
- İçerik görüntüleme istatistikleri
- Ödev tamamlama oranları
- Risk altındaki öğrenci tespiti
- İlerleme raporları

## 🔗 Entegrasyonlar

| Sistem | Entegrasyon Türü | Açıklama |
|--------|------------------|----------|
| SSO | Kimlik Doğrulama | Merkezi oturum |
| Akademik Sistem | Veri Senkron | Ders, öğrenci, not |
| Video Konferans | Embed | Zoom, Meet, Teams |
| Turnitin | İntihal | İntihal kontrolü |
| Email Hub | Bildirim | Otomatik bildirimler |
| Mobile App | API | Mobil erişim |

## 👥 Kullanıcı Rolleri

| Rol | Yetkiler |
|-----|----------|
| **Öğrenci** | İçerik görüntüleme, ödev teslimi, sınav |
| **Öğretim Üyesi** | Ders yönetimi, değerlendirme |
| **Asistan** | Sınırlı ders yönetimi |
| **Gözlemci** | Salt görüntüleme |
| **LMS Admin** | Sistem yönetimi |

## 🗃️ Veritabanı Şeması (Temel)

```
courses
├── id
├── academic_course_id
├── title
├── description
├── instructor_id
├── semester_id
├── status
├── settings (JSON)
└── timestamps

modules
├── id
├── course_id
├── title
├── description
├── order
├── visible
├── unlock_date
└── timestamps

contents
├── id
├── module_id
├── content_type
├── title
├── body
├── file_url
├── duration
├── order
├── visible
└── timestamps

assignments
├── id
├── course_id
├── title
├── description
├── due_date
├── points
├── submission_type
├── rubric (JSON)
├── allow_late
├── late_penalty
└── timestamps

submissions
├── id
├── assignment_id
├── student_id
├── submitted_at
├── file_url
├── text_content
├── grade
├── feedback
├── graded_by
├── graded_at
└── timestamps

quizzes
├── id
├── course_id
├── title
├── description
├── time_limit
├── attempts_allowed
├── shuffle_questions
├── show_results
├── available_from
├── available_to
└── timestamps

questions
├── id
├── quiz_id
├── question_type
├── question_text
├── options (JSON)
├── correct_answer
├── points
├── order
└── timestamps

quiz_attempts
├── id
├── quiz_id
├── student_id
├── started_at
├── submitted_at
├── score
├── answers (JSON)
└── timestamps

enrollments
├── id
├── course_id
├── user_id
├── role
├── enrolled_at
├── status
└── timestamps

gradebook
├── id
├── course_id
├── student_id
├── item_type
├── item_id
├── grade
├── graded_at
└── timestamps
```

## 🛠️ Teknik Gereksinimler

### Backend
- Runtime: Node.js 18+
- Framework: NestJS
- Real-time: Socket.io
- Queue: Bull

### Frontend
- Framework: Next.js 14+
- Editor: TipTap
- Video: Video.js
- UI: Tailwind CSS

### Veritabanı
- Primary: PostgreSQL 15+
- Cache: Redis
- Search: Elasticsearch

### Storage
- S3-compatible (içerikler için)
- CDN (video streaming)

## 📁 Modül Yapısı

```
21-lms/
├── src/
│   ├── modules/
│   │   ├── courses/           # Ders yönetimi
│   │   ├── modules/           # Modül/bölüm
│   │   ├── contents/          # İçerik yönetimi
│   │   ├── assignments/       # Ödevler
│   │   ├── quizzes/           # Sınavlar
│   │   ├── discussions/       # Forumlar
│   │   ├── gradebook/         # Not defteri
│   │   ├── analytics/         # Öğrenme analitiği
│   │   ├── calendar/          # Takvim
│   │   └── messages/          # Mesajlaşma
│   ├── player/                # İçerik oynatıcı
│   └── integrations/          # Dış entegrasyonlar
├── tests/
├── docs/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - Temel (MVP)
- [ ] Ders ve içerik yönetimi
- [ ] Temel ödev sistemi
- [ ] Dosya paylaşımı

### Faz 2 - Genişletme
- [ ] Sınav modülü
- [ ] Tartışma forumları
- [ ] Not defteri

### Faz 3 - İleri Özellikler
- [ ] Video konferans entegrasyonu
- [ ] Öğrenme analitiği
- [ ] SCORM desteği
- [ ] Mobil uygulama

## 📊 KPI'lar

- Aktif kullanıcı oranı
- İçerik tamamlama oranı
- Ödev teslim oranı
- Öğrenci memnuniyeti

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı

**Öncelik:** Yüksek

**Tahmini Süre:** 6-8 ay
