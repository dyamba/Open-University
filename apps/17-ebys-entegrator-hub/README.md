# EBYS Entegrator Hub

Farklı EBYS (Elektronik Belge Yönetim Sistemi) çözümleri ile entegrasyonu sağlayan ara katman.

## 🎯 Amaç

Üniversite sistemlerinin kullanılan EBYS çözümü ile standart ve kesintisiz entegrasyonunun sağlanması.

## 📋 Kapsam

- EBYS API entegrasyonu
- Belge oluşturma ve gönderme
- Belge alma ve işleme
- İmza süreçleri
- Arşiv entegrasyonu
- Çoklu EBYS desteği

## ✨ Temel Özellikler

### Belge Gönderimi
- Otomatik belge oluşturma
- Şablon tabanlı belgeler
- Metadata ekleme
- İmza akışı başlatma
- Dağıtım listesi

### Belge Alma
- Gelen evrak bildirimi
- Otomatik yönlendirme
- Belge indirme
- Metadata okuma

### İmza Entegrasyonu
- e-İmza entegrasyonu
- Mobil imza
- İmza onay akışı
- Toplu imza

### Şablon Yönetimi
- Belge şablonları
- Değişken alanlar
- Şablon versiyonlama
- Departman bazlı şablonlar

### Webhook & Bildirimler
- Durum değişikliği bildirimleri
- İmza tamamlanma bildirimi
- Hata bildirimleri

### Arşiv
- Arşiv sorgulama
- Belge arama
- Arşiv aktarımı

## 🔗 Desteklenen EBYS Sistemleri

| EBYS | Durum |
|------|-------|
| BIMER | Planlandı |
| e-Yazışma | Planlandı |
| Türksat EBYS | Planlandı |
| UYAP | Planlandı |
| Özel EBYS Çözümleri | API ile |

## 🔗 İç Sistem Entegrasyonları

| Sistem | Kullanım |
|--------|----------|
| SSO | Kimlik doğrulama |
| Tüm Sistemler | Belge gönderme/alma |
| Email Hub | Bildirimler |

## 👥 Kullanıcı Rolleri

| Rol | Yetkiler |
|-----|----------|
| **Sistem Kullanıcısı** | Otomatik belge işlemleri |
| **Entegratör** | Servis yönetimi |
| **Sistem Yöneticisi** | Tam yetki |

## 🗃️ Veritabanı Şeması (Temel)

```
ebys_providers
├── id
├── name
├── api_type
├── base_url
├── auth_config (JSON, encrypted)
├── status
└── timestamps

document_templates
├── id
├── name
├── code
├── provider_id
├── template_content
├── variables[]
├── department_id
└── timestamps

outgoing_documents
├── id
├── provider_id
├── template_id
├── source_system
├── source_id
├── document_data (JSON)
├── ebys_document_id
├── status
├── sent_at
├── error_message
└── timestamps

incoming_documents
├── id
├── provider_id
├── ebys_document_id
├── document_type
├── sender
├── subject
├── received_at
├── processed
├── target_system
├── target_id
└── timestamps

signature_requests
├── id
├── document_id
├── signer_id
├── signature_type
├── status
├── requested_at
├── signed_at
└── timestamps
```

## 🛠️ Teknik Gereksinimler

### Backend
- Runtime: Node.js 18+
- Framework: NestJS
- Queue: Bull (asenkron işlemler)

### Veritabanı
- Primary: PostgreSQL 15+
- Queue: Redis

### Güvenlik
- SSL/TLS
- API key yönetimi
- Şifreli depolama

## 📁 Modül Yapısı

```
17-ebys-entegrator-hub/
├── src/
│   ├── providers/             # EBYS sağlayıcıları
│   │   ├── bimer/
│   │   ├── eyazisma/
│   │   └── generic/
│   ├── modules/
│   │   ├── documents/         # Belge işlemleri
│   │   ├── templates/         # Şablon yönetimi
│   │   ├── signatures/        # İmza işlemleri
│   │   ├── archive/           # Arşiv
│   │   └── webhooks/          # Webhook yönetimi
│   ├── gateway/               # API Gateway
│   └── queue/                 # İş kuyruğu
├── tests/
├── docs/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - Temel (MVP)
- [ ] Generic API gateway
- [ ] Belge gönderme
- [ ] Durum takibi

### Faz 2 - Genişletme
- [ ] Şablon motoru
- [ ] İmza entegrasyonu
- [ ] Webhook desteği

### Faz 3 - İleri Özellikler
- [ ] Çoklu EBYS desteği
- [ ] Arşiv entegrasyonu
- [ ] Gelişmiş raporlama

## 📊 KPI'lar

- Belge gönderim başarı oranı
- Ortalama işlem süresi
- İmza tamamlanma süresi
- Hata oranı

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı

**Öncelik:** Yüksek

**Tahmini Süre:** 3-4 ay
