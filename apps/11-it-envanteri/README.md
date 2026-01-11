# IT Envanteri

Üniversitenin tüm bilişim teknolojileri varlıklarının takip ve yönetim sistemi.

## 🎯 Amaç

Donanım, yazılım, lisans ve ağ bileşenlerinin merkezi envanterinin tutulması, yaşam döngüsü yönetimi ve maliyet optimizasyonu.

## 📋 Kapsam

- Donanım envanter yönetimi
- Yazılım ve lisans takibi
- Ağ bileşenleri yönetimi
- Varlık yaşam döngüsü
- Zimmet ve tahsis
- Bakım ve destek takibi

## ✨ Temel Özellikler

### Donanım Yönetimi
- Bilgisayar (masaüstü, dizüstü, sunucu)
- Yazıcı ve tarayıcılar
- Ağ ekipmanları (switch, router, firewall)
- Projeksiyon ve akıllı tahta
- Telefon ve mobil cihazlar
- Depolama sistemleri

### Yazılım Yönetimi
- Yazılım lisansları
- Lisans türleri (perpetual, subscription, site license)
- Kullanım takibi
- Yenileme hatırlatmaları
- Uyumluluk kontrolü

### Varlık Takibi
- Barkod/QR kod etiketleme
- Lokasyon takibi (bina, kat, oda)
- Zimmetli kişi/birim
- Durum takibi (aktif, arızalı, atıl, hurda)
- Garanti takibi

### Yaşam Döngüsü
- Satın alma kaydı
- Devreye alma
- Bakım geçmişi
- Yenileme planlaması
- Hurdaya çıkarma

### Zimmet Yönetimi
- Personel zimmet
- Birim tahsis
- Zimmet formu oluşturma
- Zimmet transferi
- İade işlemleri

### Bakım & Destek
- Arıza kayıtları
- Bakım planlaması
- Servis takibi
- Yedek parça yönetimi
- Destek sözleşmeleri

### Raporlama
- Envanter raporları
- Maliyet analizleri
- Yaşlanma raporları
- Kullanım raporları
- Bütçe planlaması

## 🔗 Entegrasyonlar

| Sistem | Entegrasyon Türü | Açıklama |
|--------|------------------|----------|
| SSO | Kimlik Doğrulama | Merkezi oturum |
| Personel Sistemi | Veri Çekme | Zimmet bilgileri |
| Satın Alma | Entegrasyon | Tedarik süreçleri |
| Finans | Veri Paylaşımı | Amortisman, maliyet |
| Help Desk | Entegrasyon | Arıza bildirimleri |
| LDAP/AD | Veri Senkron | Cihaz-kullanıcı eşleşme |

## 👥 Kullanıcı Rolleri

| Rol | Yetkiler |
|-----|----------|
| **Personel** | Kendi zimmetlerini görüntüleme |
| **Birim IT Sorumlusu** | Birim envanteri |
| **IT Teknisyeni** | Varlık işlemleri, bakım |
| **Lisans Yöneticisi** | Yazılım lisansları |
| **IT Müdürü** | Tam yetki, raporlama |
| **Sistem Yöneticisi** | Konfigürasyon |

## 🗃️ Veritabanı Şeması (Temel)

```
asset_categories
├── id
├── name
├── code
├── parent_id
├── depreciation_years
└── timestamps

assets
├── id
├── asset_tag
├── serial_number
├── category_id
├── name
├── description
├── brand
├── model
├── specifications (JSON)
├── purchase_date
├── purchase_price
├── warranty_end
├── location_id
├── assigned_to (user_id)
├── assigned_department_id
├── status
├── condition
└── timestamps

software_licenses
├── id
├── software_name
├── vendor
├── license_type
├── license_key
├── seats
├── used_seats
├── purchase_date
├── expiry_date
├── cost
├── status
└── timestamps

license_assignments
├── id
├── license_id
├── asset_id
├── user_id
├── assigned_date
├── unassigned_date
└── timestamps

locations
├── id
├── name
├── building
├── floor
├── room
├── parent_id
└── timestamps

assignments
├── id
├── asset_id
├── assigned_to
├── assigned_by
├── assignment_date
├── return_date
├── notes
├── status
└── timestamps

maintenance_records
├── id
├── asset_id
├── type (arıza, bakım, güncelleme)
├── description
├── performed_by
├── performed_date
├── cost
├── next_maintenance
├── notes
└── timestamps

support_contracts
├── id
├── vendor
├── contract_number
├── assets[]
├── start_date
├── end_date
├── cost
├── coverage_details
├── status
└── timestamps
```

## 🛠️ Teknik Gereksinimler

### Backend
- Runtime: Node.js 18+
- Framework: NestJS

### Frontend
- Framework: Next.js 14+
- UI: Tailwind CSS + Shadcn/ui
- Barcode: react-barcode / qrcode.react

### Veritabanı
- Primary: PostgreSQL 15+

### Ek Araçlar
- Barkod yazıcı entegrasyonu
- SNMP ağ tarama (opsiyonel)

## 📁 Modül Yapısı

```
11-it-envanteri/
├── src/
│   ├── modules/
│   │   ├── assets/            # Varlık yönetimi
│   │   ├── categories/        # Kategori yönetimi
│   │   ├── licenses/          # Lisans yönetimi
│   │   ├── locations/         # Lokasyon yönetimi
│   │   ├── assignments/       # Zimmet yönetimi
│   │   ├── maintenance/       # Bakım kayıtları
│   │   ├── contracts/         # Destek sözleşmeleri
│   │   └── reports/           # Raporlama
│   └── tools/                 # Barkod, tarama araçları
├── tests/
├── docs/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - Temel (MVP)
- [ ] Varlık CRUD
- [ ] Kategori yönetimi
- [ ] Temel zimmet

### Faz 2 - Genişletme
- [ ] Lisans modülü
- [ ] Bakım takibi
- [ ] Barkod sistemi

### Faz 3 - İleri Özellikler
- [ ] Otomatik keşif (SNMP)
- [ ] Amortisman hesaplama
- [ ] Dashboard ve analitik

## 📊 KPI'lar

- Envanter doğruluk oranı
- Ortalama varlık yaşı
- Lisans kullanım oranı
- Bakım maliyeti / varlık değeri

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı

**Öncelik:** Orta

**Tahmini Süre:** 3-4 ay
