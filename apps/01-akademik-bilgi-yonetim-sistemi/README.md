# Akademik Bilgi Yönetim Sistemi (OBS/ABS)

Üniversitelerin akademik süreçlerini uçtan uca yöneten merkezi sistem.

## 🎯 Amaç

Öğrenci kayıt, ders yönetimi, not girişi, transkript oluşturma ve tüm akademik süreçlerin tek bir platformdan yönetilmesini sağlamak.

## 📋 Kapsam

- Öğrenci yaşam döngüsü yönetimi (kayıt → mezuniyet)
- Ders ve müfredat yönetimi
- Not ve değerlendirme sistemi
- Akademik takvim yönetimi
- Harç ve ödeme takibi

## ✨ Temel Özellikler

### Öğrenci İşleri
- Öğrenci kayıt ve kabul işlemleri
- Kişisel bilgi yönetimi
- Öğrenci durumu takibi (aktif, pasif, mezun, ilişik kesik)
- Yatay/dikey geçiş işlemleri
- Çift anadal / Yandal başvuruları

### Ders Yönetimi
- Müfredat tanımlama ve versiyonlama
- Ders açma/kapama işlemleri
- Ders programı oluşturma
- Kontenjan yönetimi
- Ön koşul tanımlama

### Kayıt İşlemleri
- Online ders seçimi
- Danışman onay süreci
- Ders ekleme/bırakma
- Mazeretli kayıt işlemleri

### Not Yönetimi
- Not girişi ve düzeltme
- Bağıl/mutlak değerlendirme
- Harf notu dönüşümü
- GNO/GANO hesaplama
- Not itiraz süreci

### Belge Yönetimi
- Transkript oluşturma
- Öğrenci belgesi
- Geçici mezuniyet belgesi
- Diploma hazırlama
- Apostil ve denklik belgeleri

### Raporlama
- Öğrenci istatistikleri
- Başarı analizleri
- Ders doluluk raporları
- YÖKSİS raporları

## 🔗 Entegrasyonlar

| Sistem | Entegrasyon Türü | Açıklama |
|--------|------------------|----------|
| SSO | Kimlik Doğrulama | Merkezi oturum yönetimi |
| E-Devlet | Veri Senkronizasyonu | Öğrenci doğrulama, mezuniyet bildirimi |
| YÖKSİS | Veri Aktarımı | Zorunlu raporlamalar |
| LMS | Veri Paylaşımı | Ders ve öğrenci bilgileri |
| Finans/Pos | Ödeme | Harç tahsilatı |
| EBYS | Belge | Resmi yazışmalar |
| Mezun Sistemi | Veri Aktarımı | Mezuniyet sonrası takip |
| Email Hub | Bildirim | Otomatik bilgilendirmeler |

## 👥 Kullanıcı Rolleri

| Rol | Yetkiler |
|-----|----------|
| **Öğrenci** | Ders seçimi, not görüntüleme, belge talebi |
| **Öğretim Üyesi** | Not girişi, danışmanlık, ders yönetimi |
| **Danışman** | Ders onayı, öğrenci takibi |
| **Bölüm Sekreteri** | Kayıt işlemleri, belge düzenleme |
| **Bölüm Başkanı** | Onay süreçleri, raporlama |
| **Öğrenci İşleri** | Tüm öğrenci işlemleri |
| **Sistem Yöneticisi** | Tam yetki, konfigürasyon |

## 🗃️ Veritabanı Şeması (Temel)

```
students
├── id
├── student_number
├── national_id
├── first_name
├── last_name
├── email
├── phone
├── department_id
├── program_id
├── advisor_id
├── enrollment_date
├── status
└── timestamps

courses
├── id
├── code
├── name
├── credits
├── ects
├── department_id
├── type (zorunlu/seçmeli)
├── prerequisites[]
└── timestamps

enrollments
├── id
├── student_id
├── course_id
├── semester_id
├── grade
├── letter_grade
├── status
└── timestamps

semesters
├── id
├── year
├── term
├── start_date
├── end_date
├── registration_start
├── registration_end
└── timestamps
```

## 🛠️ Teknik Gereksinimler

### Backend
- Runtime: Node.js 18+ / Python 3.11+
- Framework: NestJS / FastAPI
- ORM: Prisma / SQLAlchemy
- Cache: Redis

### Frontend
- Framework: React 18+ / Next.js 14+
- State Management: Zustand / Redux Toolkit
- UI Library: Tailwind CSS + Shadcn/ui

### Veritabanı
- Primary: PostgreSQL 15+
- Search: Elasticsearch (opsiyonel)

### Altyapı
- Container: Docker
- Orchestration: Kubernetes (opsiyonel)
- CI/CD: GitHub Actions

## 📁 Modül Yapısı

```
01-akademik-bilgi-yonetim-sistemi/
├── src/
│   ├── modules/
│   │   ├── students/          # Öğrenci yönetimi
│   │   ├── courses/           # Ders yönetimi
│   │   ├── enrollments/       # Kayıt işlemleri
│   │   ├── grades/            # Not yönetimi
│   │   ├── curriculum/        # Müfredat
│   │   ├── documents/         # Belge yönetimi
│   │   ├── calendar/          # Akademik takvim
│   │   └── reports/           # Raporlama
│   ├── shared/
│   └── config/
├── tests/
├── docs/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - Temel (MVP)
- [ ] Öğrenci CRUD işlemleri
- [ ] Ders tanımlama
- [ ] Basit kayıt sistemi
- [ ] Not girişi

### Faz 2 - Genişletme
- [ ] Müfredat yönetimi
- [ ] Danışmanlık modülü
- [ ] Belge oluşturma
- [ ] E-Devlet entegrasyonu

### Faz 3 - İleri Özellikler
- [ ] YÖKSİS entegrasyonu
- [ ] Gelişmiş raporlama
- [ ] Mobil uygulama desteği
- [ ] AI destekli danışmanlık önerileri

## 📊 KPI'lar

- Kayıt işlemi tamamlama süresi
- Not girişi zamanında tamamlanma oranı
- Öğrenci memnuniyet skoru
- Sistem uptime oranı

## 📚 İlgili Dokümanlar

- [API Dokümantasyonu](./docs/api.md)
- [Kurulum Rehberi](./docs/installation.md)
- [Kullanıcı Kılavuzu](./docs/user-guide.md)

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı

**Öncelik:** Yüksek

**Tahmini Süre:** 6-8 ay
