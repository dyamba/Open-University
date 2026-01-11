# Laboratuvar Yönetim Sistemi (LIMS)

Üniversite araştırma ve eğitim laboratuvarlarının yönetimi için sistem.

## 🎯 Amaç

Laboratuvar kaynaklarının, örneklerin, cihazların ve analizlerin merkezi yönetimi ve takibi.

## 📋 Kapsam

- Laboratuvar envanter yönetimi
- Örnek ve numune takibi
- Cihaz yönetimi ve kalibrasyon
- Analiz ve test yönetimi
- Kimyasal madde yönetimi
- Güvenlik ve uyumluluk

## ✨ Temel Özellikler

### Laboratuvar Yönetimi
- Laboratuvar tanımlama
- Kapasite yönetimi
- Çalışma saatleri
- Erişim kontrolü
- Sorumluluk ataması

### Örnek Yönetimi
- Örnek kaydı ve etiketleme
- Barkod/QR kod sistemi
- Örnek lokasyonu takibi
- Saklama koşulları
- Örnek zinciri (chain of custody)
- Örnek imha

### Cihaz Yönetimi
- Cihaz envanteri
- Kalibrasyon takibi
- Bakım planlaması
- Kullanım logları
- Arıza kayıtları
- Sertifika yönetimi

### Analiz/Test Yönetimi
- Test tanımları
- İş emri oluşturma
- Sonuç girişi
- Kalite kontrol
- Sonuç onaylama
- Rapor oluşturma

### Kimyasal Yönetimi
- Kimyasal envanter
- MSDS (Güvenlik Bilgi Formları)
- Stok takibi
- Son kullanma tarihi
- Tehlikeli madde yönetimi
- Atık yönetimi

### Rezervasyon Sistemi
- Cihaz rezervasyonu
- Laboratuvar alanı rezervasyonu
- Çakışma kontrolü
- Onay akışı

### Güvenlik & Uyumluluk
- Güvenlik protokolleri
- Kişisel koruyucu ekipman
- Olay/kaza raporlama
- Eğitim takibi
- ISO 17025 uyumu

## 🔗 Entegrasyonlar

| Sistem | Entegrasyon Türü | Açıklama |
|--------|------------------|----------|
| SSO | Kimlik Doğrulama | Merkezi oturum |
| Envanter Yönetimi | Veri Paylaşımı | Malzeme stoğu |
| Proje Yönetimi | Entegrasyon | Proje analizleri |
| Satın Alma | Entegrasyon | Malzeme talepleri |
| Klinik Araştırma | Veri Paylaşımı | Klinik örnekler |
| Finans | Veri Aktarımı | Maliyet takibi |

## 👥 Kullanıcı Rolleri

| Rol | Yetkiler |
|-----|----------|
| **Araştırmacı** | Örnek kaydı, rezervasyon |
| **Lab Teknisyeni** | Analiz, cihaz kullanımı |
| **Lab Sorumlusu** | Lab yönetimi |
| **Kalite Sorumlusu** | Kalibrasyon, QC |
| **Güvenlik Sorumlusu** | Güvenlik yönetimi |
| **Lab Müdürü** | Tam yetki |

## 🗃️ Veritabanı Şeması (Temel)

```
laboratories
├── id
├── name
├── code
├── building
├── room
├── capacity
├── lab_type
├── supervisor_id
├── safety_level
├── status
└── timestamps

samples
├── id
├── sample_id
├── barcode
├── sample_type
├── source
├── collection_date
├── received_date
├── storage_location
├── storage_conditions
├── status
├── disposed_date
├── project_id
└── timestamps

equipment
├── id
├── name
├── model
├── serial_number
├── laboratory_id
├── purchase_date
├── warranty_end
├── calibration_due
├── status
└── timestamps

calibrations
├── id
├── equipment_id
├── calibration_date
├── next_due
├── performed_by
├── certificate_url
├── result
└── timestamps

tests
├── id
├── test_code
├── name
├── method
├── turnaround_time
├── price
├── laboratory_id
└── timestamps

work_orders
├── id
├── order_number
├── sample_id
├── test_id
├── requested_by
├── requested_date
├── priority
├── status
├── assigned_to
└── timestamps

test_results
├── id
├── work_order_id
├── parameter
├── value
├── unit
├── reference_range
├── status
├── performed_by
├── performed_date
├── approved_by
├── approved_date
└── timestamps

chemicals
├── id
├── name
├── cas_number
├── formula
├── hazard_class
├── storage_requirements
├── quantity
├── unit
├── location
├── expiry_date
├── msds_url
└── timestamps

reservations
├── id
├── resource_type (equipment, lab)
├── resource_id
├── user_id
├── start_time
├── end_time
├── purpose
├── status
└── timestamps
```

## 🛠️ Teknik Gereksinimler

### Backend
- Runtime: Node.js 18+
- Framework: NestJS

### Frontend
- Framework: Next.js 14+
- Calendar: FullCalendar
- UI: Tailwind CSS

### Veritabanı
- Primary: PostgreSQL 15+

### Ek Araçlar
- Barkod yazıcı
- Barkod okuyucu

## 📁 Modül Yapısı

```
15-laboratuvar-yonetim-sistemi/
├── src/
│   ├── modules/
│   │   ├── laboratories/      # Lab yönetimi
│   │   ├── samples/           # Örnek takibi
│   │   ├── equipment/         # Cihaz yönetimi
│   │   ├── calibrations/      # Kalibrasyon
│   │   ├── tests/             # Test/analiz
│   │   ├── work-orders/       # İş emirleri
│   │   ├── chemicals/         # Kimyasal yönetimi
│   │   ├── reservations/      # Rezervasyon
│   │   ├── safety/            # Güvenlik
│   │   └── reports/           # Raporlama
│   └── barcode/               # Barkod işlemleri
├── tests/
├── docs/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - Temel (MVP)
- [ ] Lab ve cihaz kaydı
- [ ] Örnek takibi
- [ ] Temel rezervasyon

### Faz 2 - Genişletme
- [ ] Test/analiz modülü
- [ ] Kalibrasyon takibi
- [ ] Kimyasal yönetimi

### Faz 3 - İleri Özellikler
- [ ] QC/QA modülü
- [ ] Cihaz entegrasyonu
- [ ] ISO 17025 uyumu

## 📊 KPI'lar

- Cihaz kullanım oranı
- Analiz tamamlanma süresi
- Kalibrasyon uyum oranı
- Örnek kayıp oranı

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı

**Öncelik:** Orta

**Tahmini Süre:** 4-5 ay
