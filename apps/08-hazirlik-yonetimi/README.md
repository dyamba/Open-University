# Hazırlık Yönetimi

Üniversite yabancı dil hazırlık programlarının yönetimi için özelleştirilmiş sistem.

## 🎯 Amaç

Yabancı dil hazırlık sınıflarının öğrenci takibi, seviye belirleme, ders programı ve başarı değerlendirmesinin yönetimi.

## 📋 Kapsam

- Seviye belirleme sınavları
- Kur/modül yönetimi
- Sınıf ve öğrenci takibi
- Devam takibi
- Sınav ve değerlendirme
- Muafiyet işlemleri

## ✨ Temel Özellikler

### Seviye Belirleme
- Online yerleştirme sınavı
- Otomatik seviye atama
- Sözlü mülakat planlama
- Seviye itiraz süreci

### Kur Yönetimi
- Kur tanımlama (A1, A2, B1, B2, C1, C2)
- Modül yapısı
- Geçiş kriterleri
- Süre tanımlama

### Sınıf Yönetimi
- Sınıf oluşturma
- Öğrenci ataması
- Kontenjan yönetimi
- Öğretim görevlisi ataması
- Ders programı

### Devam Takibi
- Yoklama alma
- Devamsızlık hesaplama
- Mazeret yönetimi
- Otomatik uyarılar

### Sınav Yönetimi
- Ara sınav / Final
- Quiz ve ödevler
- Speaking sınavları
- Online sınav desteği
- Otomatik puanlama

### Değerlendirme
- Ağırlıklı not hesaplama
- Kur geçme değerlendirmesi
- Koşullu geçiş
- Kur tekrarı

### Muafiyet İşlemleri
- Dış sınav muafiyeti (YDS, YÖKDİL, TOEFL, IELTS)
- Denklik hesaplama
- Muafiyet başvurusu

### Öğrenci Portali
- Ders programı görüntüleme
- Not takibi
- Devam durumu
- Online ödevler
- Sınav sonuçları

## 🔗 Entegrasyonlar

| Sistem | Entegrasyon Türü | Açıklama |
|--------|------------------|----------|
| SSO | Kimlik Doğrulama | Merkezi oturum |
| Akademik Sistem | Veri Paylaşımı | Öğrenci bilgileri, mezuniyet |
| LMS | Entegrasyon | Online dersler, ödevler |
| Sınav Sistemi | Entegrasyon | Online sınavlar |
| Email Hub | Bildirim | Otomatik bildirimler |
| PDKS | Veri Çekme | Öğretim görevlisi devam |

## 👥 Kullanıcı Rolleri

| Rol | Yetkiler |
|-----|----------|
| **Öğrenci** | Ders/not görüntüleme, ödev teslimi |
| **Öğretim Görevlisi** | Yoklama, not girişi, ödev |
| **Koordinatör** | Kur yönetimi, sınıf oluşturma |
| **Hazırlık Müdürü** | Tam yetki, raporlama |
| **Sistem Yöneticisi** | Konfigürasyon |

## 🗃️ Veritabanı Şeması (Temel)

```
levels
├── id
├── code (A1, A2, B1, vb.)
├── name
├── description
├── order
├── duration_weeks
├── passing_grade
└── timestamps

modules
├── id
├── level_id
├── code
├── name
├── duration_weeks
├── order
└── timestamps

classes
├── id
├── level_id
├── module_id
├── code
├── instructor_id
├── capacity
├── current_count
├── schedule (JSON)
├── semester_id
├── status
└── timestamps

prep_students
├── id
├── student_id
├── current_level_id
├── current_class_id
├── placement_score
├── placement_level
├── status (aktif, muaf, başarılı, başarısız)
├── start_date
├── completion_date
└── timestamps

attendance
├── id
├── class_id
├── student_id
├── date
├── status (present, absent, excused)
├── notes
└── timestamps

grades
├── id
├── student_id
├── class_id
├── assessment_type (quiz, midterm, final, speaking)
├── score
├── weight
├── date
└── timestamps

exemptions
├── id
├── student_id
├── exam_type (YDS, TOEFL, IELTS, vb.)
├── score
├── document_url
├── status
├── processed_by
└── timestamps
```

## 🛠️ Teknik Gereksinimler

### Backend
- Runtime: Node.js 18+
- Framework: NestJS

### Frontend
- Framework: Next.js 14+
- UI: Tailwind CSS + Shadcn/ui

### Veritabanı
- Primary: PostgreSQL 15+
- Cache: Redis

## 📁 Modül Yapısı

```
08-hazirlik-yonetimi/
├── src/
│   ├── modules/
│   │   ├── levels/            # Kur/seviye yönetimi
│   │   ├── classes/           # Sınıf yönetimi
│   │   ├── students/          # Hazırlık öğrencileri
│   │   ├── attendance/        # Devam takibi
│   │   ├── grades/            # Not yönetimi
│   │   ├── exams/             # Sınav yönetimi
│   │   ├── exemptions/        # Muafiyet işlemleri
│   │   ├── placement/         # Yerleştirme sınavı
│   │   └── reports/           # Raporlama
│   ├── student-portal/        # Öğrenci portali
│   └── admin/                 # Yönetim paneli
├── tests/
├── docs/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - Temel (MVP)
- [ ] Kur ve sınıf yönetimi
- [ ] Öğrenci kaydı
- [ ] Temel devam takibi

### Faz 2 - Genişletme
- [ ] Yerleştirme sınavı
- [ ] Not ve değerlendirme
- [ ] Muafiyet modülü

### Faz 3 - İleri Özellikler
- [ ] LMS entegrasyonu
- [ ] Online sınav
- [ ] Öğrenci self-servis
- [ ] Gelişmiş raporlama

## 📊 KPI'lar

- Kur geçme oranı
- Ortalama hazırlık süresi
- Devamsızlık oranı
- Muafiyet oranı

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı

**Öncelik:** Orta

**Tahmini Süre:** 3-4 ay
