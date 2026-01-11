# Proje Yönetim Sistemi

Üniversitenin araştırma projelerinin başvurudan kapanışa kadar tüm yaşam döngüsünü yöneten sistem.

## 🎯 Amaç

TÜBİTAK, BAP, AB Horizon, Sanayi işbirlikleri ve diğer tüm araştırma projelerinin merkezi takibi, bütçe yönetimi ve raporlaması.

## 📋 Kapsam

- Proje başvuru süreçleri
- Proje yaşam döngüsü yönetimi
- Bütçe planlama ve harcama takibi
- Proje ekibi yönetimi
- Çıktı ve deliverable takibi
- Fon kaynağı yönetimi
- Raporlama ve analitik

## ✨ Temel Özellikler

### Başvuru Yönetimi
- Çağrı takibi ve hatırlatmaları
- Başvuru formu hazırlama
- Bütçe taslağı oluşturma
- İç onay süreçleri
- Başvuru durumu takibi

### Proje Yaşam Döngüsü
- Proje açılış işlemleri
- Milestone ve iş paketi tanımlama
- Gantt chart görünümü
- İlerleme takibi
- Proje değişiklik yönetimi
- Proje uzatma/kapanış

### Bütçe Yönetimi
- Bütçe kalemlerinin tanımlanması
- Harcama talepleri
- Onay iş akışları
- Bütçe transferleri
- Kalan bütçe takibi
- Mali raporlar

### Ekip Yönetimi
- Proje ekibi oluşturma
- Rol ve sorumluluk tanımlama
- Adam-ay planlaması
- Bursiyerler ve yardımcılar
- Danışmanlar

### Çıktı Yönetimi
- Deliverable tanımlama
- Teslim takibi
- Yayın ve patent çıktıları
- Raporlar
- Yazılım ve veri setleri

### Fon Kaynakları
- TÜBİTAK (1001, 1002, 3501, vb.)
- BAP (Bilimsel Araştırma Projeleri)
- AB Horizon Europe
- Sanayi işbirlikleri
- Kalkınma Ajansları
- Uluslararası fonlar

### Raporlama
- Dönemsel ilerleme raporları
- Mali raporlar
- Fon bazlı analizler
- Araştırmacı performansı
- Kurumsal AR-GE metrikleri

## 🔗 Entegrasyonlar

| Sistem | Entegrasyon Türü | Açıklama |
|--------|------------------|----------|
| SSO | Kimlik Doğrulama | Merkezi oturum |
| Personel Sistemi | Veri Çekme | Araştırmacı bilgileri |
| Akademik Profil | Veri Aktarımı | Proje portföyü |
| Finans Sistemi | Veri Paylaşımı | Bütçe ve harcama |
| EBYS | Belge | Resmi yazışmalar |
| Satın Alma | Entegrasyon | Malzeme talepleri |
| TÜBİTAK PBS | API | Proje başvuru/takip |
| Etik Kurul | Entegrasyon | Etik onay takibi |

## 👥 Kullanıcı Rolleri

| Rol | Yetkiler |
|-----|----------|
| **Araştırmacı** | Kendi projelerini görüntüleme |
| **Proje Yürütücüsü** | Proje yönetimi, harcama talebi |
| **BAP Koordinatörü** | BAP projeleri yönetimi |
| **Proje Ofisi** | Tüm projeler, raporlama |
| **Mali İşler** | Bütçe onay, mali raporlar |
| **Rektörlük** | Kurumsal dashboard |
| **Sistem Yöneticisi** | Tam yetki |

## 🗃️ Veritabanı Şeması (Temel)

```
projects
├── id
├── code
├── title
├── abstract
├── funding_source_id
├── call_id
├── principal_investigator_id
├── department_id
├── start_date
├── end_date
├── total_budget
├── status
├── project_type
└── timestamps

funding_sources
├── id
├── name
├── code
├── type (ulusal, uluslararası, sanayi)
├── requirements (JSON)
└── timestamps

calls
├── id
├── funding_source_id
├── title
├── deadline
├── budget_limit
├── requirements
├── status
└── timestamps

project_team
├── id
├── project_id
├── employee_id
├── role
├── person_months
├── start_date
├── end_date
└── timestamps

work_packages
├── id
├── project_id
├── code
├── title
├── description
├── leader_id
├── start_month
├── end_month
├── budget
└── timestamps

milestones
├── id
├── project_id
├── work_package_id
├── title
├── due_date
├── status
├── deliverables[]
└── timestamps

budget_items
├── id
├── project_id
├── category (personel, malzeme, seyahat, vb.)
├── description
├── planned_amount
├── spent_amount
└── timestamps

expenditures
├── id
├── project_id
├── budget_item_id
├── amount
├── description
├── receipt_date
├── status
├── approved_by
└── timestamps

deliverables
├── id
├── project_id
├── milestone_id
├── title
├── type
├── due_date
├── submitted_date
├── status
├── file_url
└── timestamps
```

## 🛠️ Teknik Gereksinimler

### Backend
- Runtime: Node.js 18+ / Python 3.11+
- Framework: NestJS / FastAPI
- Workflow: Temporal / Camunda

### Frontend
- Framework: React 18+ / Next.js 14+
- Gantt: DHTMLX Gantt / Bryntum
- Charts: Recharts

### Veritabanı
- Primary: PostgreSQL 15+
- Document Store: MongoDB (proje dosyaları)

## 📁 Modül Yapısı

```
06-proje-yonetim-sistemi/
├── src/
│   ├── modules/
│   │   ├── projects/          # Proje yönetimi
│   │   ├── applications/      # Başvurular
│   │   ├── funding/           # Fon kaynakları
│   │   ├── team/              # Ekip yönetimi
│   │   ├── budget/            # Bütçe yönetimi
│   │   ├── workpackages/      # İş paketleri
│   │   ├── milestones/        # Milestones
│   │   ├── deliverables/      # Çıktılar
│   │   ├── expenditures/      # Harcamalar
│   │   └── reports/           # Raporlama
│   ├── workflows/
│   └── integrations/
├── tests/
├── docs/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - Temel (MVP)
- [ ] Proje CRUD
- [ ] Temel bütçe takibi
- [ ] Ekip yönetimi

### Faz 2 - Genişletme
- [ ] Başvuru modülü
- [ ] İş paketi ve milestone
- [ ] Harcama onay akışı

### Faz 3 - İleri Özellikler
- [ ] Gantt chart
- [ ] TÜBİTAK entegrasyonu
- [ ] Gelişmiş raporlama
- [ ] Proje portföy yönetimi

## 📊 KPI'lar

- Başvuru başarı oranı
- Bütçe kullanım oranı
- Proje tamamlanma oranı
- Ortalama proje süresi

## 📈 Proje Yaşam Döngüsü

```
Fikir → Başvuru → Değerlendirme → Kabul → Açılış → Yürütme → Kapanış → Arşiv
         ↓                          ↓                   ↓
       Revizyon                  Red               Uzatma
```

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı

**Öncelik:** Yüksek

**Tahmini Süre:** 5-6 ay
