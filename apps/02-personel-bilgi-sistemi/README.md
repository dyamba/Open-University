# Personel Bilgi Sistemi

Üniversite personelinin (akademik ve idari) tüm bilgi ve süreçlerinin yönetildiği merkezi insan kaynakları sistemi.

## 🎯 Amaç

Akademik ve idari personelin işe alımdan ayrılışa kadar tüm süreçlerinin dijital ortamda yönetilmesi, özlük dosyalarının tutulması ve yasal raporlamaların otomatikleştirilmesi.

## 📋 Kapsam

- Personel özlük bilgileri yönetimi
- İşe alım ve işten ayrılış süreçleri
- Akademik yükseltme ve atama
- İzin yönetimi
- Bordro entegrasyonu
- Yasal raporlamalar (SGK, YÖKSİS)

## ✨ Temel Özellikler

### Özlük Yönetimi
- Kişisel bilgi kayıtları
- İletişim bilgileri
- Eğitim geçmişi
- İş deneyimi
- Sertifika ve yetkinlikler
- Aile bilgileri
- Askerlik durumu
- Sağlık kayıtları

### Akademik Personel
- Akademik unvan takibi
- Yayın ve proje bilgileri
- Ders yükü hesaplama
- Jüri ve komisyon görevleri
- Akademik teşvik başvuruları
- Yurtdışı görevlendirme

### İdari Personel
- Kadro ve pozisyon yönetimi
- Görev tanımları
- Sicil kayıtları
- Disiplin süreçleri

### İşe Alım
- İlan yönetimi
- Başvuru takibi
- Mülakat süreçleri
- Belge kontrolü
- Onay akışları
- İşe başlama prosedürleri

### İzin Yönetimi
- Yıllık izin
- Mazeret izni
- Sağlık izni
- Ücretsiz izin
- Akademik izin (sabbatical)
- İzin bakiye takibi
- Onay workflow'ları

### Bordro Entegrasyonu
- Maaş bilgileri
- Ek ders ücreti
- Fazla mesai
- Kesintiler
- Bordro raporları

### Raporlama
- SGK bildirimleri
- YÖKSİS raporları
- Kadro doluluk raporları
- Personel istatistikleri
- Yaş/kıdem analizleri

## 🔗 Entegrasyonlar

| Sistem | Entegrasyon Türü | Açıklama |
|--------|------------------|----------|
| SSO | Kimlik Doğrulama | Merkezi oturum |
| PDKS | Veri Alımı | Giriş-çıkış kayıtları |
| E-Devlet | Veri Doğrulama | Kimlik, adres doğrulama |
| SGK | Raporlama | Sigorta bildirimleri |
| YÖKSİS | Raporlama | Akademik personel bildirimi |
| Bordro Sistemi | Veri Aktarımı | Maaş hesaplama |
| EBYS | Belge | Resmi yazışmalar |
| Email Hub | Bildirim | Otomatik bilgilendirmeler |
| Akademik Profil | Veri Paylaşımı | Akademik bilgiler |

## 👥 Kullanıcı Rolleri

| Rol | Yetkiler |
|-----|----------|
| **Personel** | Kendi bilgilerini görüntüleme, izin talebi |
| **Yönetici** | Birim personeli yönetimi, izin onayı |
| **İK Uzmanı** | Personel işlemleri, raporlama |
| **İK Müdürü** | Tam yetki, onay süreçleri |
| **Bordro Uzmanı** | Maaş işlemleri |
| **Sistem Yöneticisi** | Konfigürasyon, tam yetki |

## 🗃️ Veritabanı Şeması (Temel)

```
employees
├── id
├── employee_number
├── national_id
├── first_name
├── last_name
├── email
├── phone
├── birth_date
├── gender
├── marital_status
├── address
├── department_id
├── position_id
├── employee_type (akademik/idari)
├── hire_date
├── termination_date
├── status
└── timestamps

academic_info
├── id
├── employee_id
├── title (Prof., Doç., Dr. vb.)
├── specialization
├── promotion_date
├── publications_count
├── projects_count
└── timestamps

positions
├── id
├── title
├── department_id
├── level
├── salary_grade
└── timestamps

leaves
├── id
├── employee_id
├── leave_type
├── start_date
├── end_date
├── days
├── status
├── approved_by
└── timestamps

contracts
├── id
├── employee_id
├── contract_type
├── start_date
├── end_date
├── salary
└── timestamps
```

## 🛠️ Teknik Gereksinimler

### Backend
- Runtime: Node.js 18+ / Python 3.11+
- Framework: NestJS / FastAPI
- ORM: Prisma / SQLAlchemy

### Frontend
- Framework: React 18+ / Next.js 14+
- UI Library: Tailwind CSS + Shadcn/ui

### Veritabanı
- Primary: PostgreSQL 15+
- Document Store: MongoDB (özlük dosyaları için)

### Güvenlik
- Şifreleme: AES-256 (hassas veriler)
- KVKK uyumlu veri saklama

## 📁 Modül Yapısı

```
02-personel-bilgi-sistemi/
├── src/
│   ├── modules/
│   │   ├── employees/         # Personel yönetimi
│   │   ├── academic/          # Akademik personel
│   │   ├── recruitment/       # İşe alım
│   │   ├── leaves/            # İzin yönetimi
│   │   ├── payroll/           # Bordro entegrasyonu
│   │   ├── documents/         # Özlük dosyaları
│   │   ├── reports/           # Raporlama
│   │   └── integrations/      # Dış sistem entegrasyonları
│   ├── shared/
│   └── config/
├── tests/
├── docs/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - Temel (MVP)
- [ ] Personel CRUD işlemleri
- [ ] Temel özlük bilgileri
- [ ] İzin talep/onay

### Faz 2 - Genişletme
- [ ] Akademik personel modülü
- [ ] İşe alım süreçleri
- [ ] Bordro entegrasyonu

### Faz 3 - İleri Özellikler
- [ ] SGK entegrasyonu
- [ ] YÖKSİS entegrasyonu
- [ ] Gelişmiş raporlama
- [ ] Self-servis portal

## 📊 KPI'lar

- İzin onay süresi
- İşe alım süreci süresi
- Personel memnuniyet oranı
- Raporlama doğruluk oranı

## 🔒 Güvenlik & KVKK

- Hassas veriler şifreli saklanır
- Erişim logları tutulur
- Veri saklama süreleri tanımlıdır
- Anonimleştirme/silme prosedürleri mevcuttur

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı

**Öncelik:** Yüksek

**Tahmini Süre:** 5-6 ay
