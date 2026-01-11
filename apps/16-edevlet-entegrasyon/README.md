# E-Devlet Entegrasyon Uygulaması

Türkiye e-Devlet servisleri ile entegrasyonu sağlayan merkezi hub.

## 🎯 Amaç

Üniversite sistemlerinin e-Devlet kapısı servisleri ile güvenli ve standart entegrasyonunun sağlanması.

## 📋 Kapsam

- Kimlik doğrulama (e-Devlet girişi)
- Kimlik ve nüfus servisleri
- Eğitim ve diploma servisleri
- SGK servisleri
- Adres servisleri
- Belge doğrulama

## ✨ Temel Özellikler

### Kimlik Doğrulama
- e-Devlet ile giriş
- T.C. kimlik doğrulama
- Yabancı kimlik doğrulama

### KPS (Kimlik Paylaşım Sistemi)
- Nüfus bilgileri sorgulama
- Adres bilgileri sorgulama
- Aile bilgileri
- Kimlik doğrulama

### YÖKSİS Entegrasyonu
- Öğrenci sorgulama
- Mezuniyet bildirimi
- Diploma sorgulama
- Akademik personel bildirimi

### SGK Servisleri
- Sigortalılık sorgulama
- İşe giriş/çıkış bildirimi
- Prim sorgulama

### MEB Servisleri
- Lise mezuniyet doğrulama
- Eğitim geçmişi sorgulama

### ÖSYM Entegrasyonu
- Sınav sonuç sorgulama
- Yerleştirme bilgileri

### Belge Servisleri
- e-İmza doğrulama
- Belge doğrulama
- QR kod doğrulama

### Log ve Audit
- Tüm sorguların loglanması
- Erişim audit trail
- KVKK uyumlu kayıtlar

## 🔗 Desteklenen e-Devlet Servisleri

| Servis | Açıklama |
|--------|----------|
| KPS | Kimlik Paylaşım Sistemi |
| AKS | Adres Kayıt Sistemi |
| YÖKSİS | Yükseköğretim Bilgi Sistemi |
| SGK | Sosyal Güvenlik Kurumu |
| MEB | Milli Eğitim Bakanlığı |
| ÖSYM | Ölçme Seçme Merkezi |
| e-İmza | Elektronik İmza Doğrulama |
| e-Devlet Kapısı | Kimlik Doğrulama |

## 🔗 İç Sistem Entegrasyonları

| Sistem | Kullanım |
|--------|----------|
| SSO | e-Devlet girişi |
| Akademik Sistem | Öğrenci doğrulama |
| Personel Sistemi | Personel doğrulama |
| Başvuru Hub | Başvuru doğrulama |
| Mezun Sistemi | Mezuniyet bildirimi |

## 👥 Kullanıcı Rolleri

| Rol | Yetkiler |
|-----|----------|
| **Son Kullanıcı** | e-Devlet ile giriş |
| **Sistem Entegratör** | Servis yönetimi |
| **Sistem Yöneticisi** | Tam yetki, konfigürasyon |

## 🗃️ Veritabanı Şeması (Temel)

```
edevlet_services
├── id
├── service_code
├── service_name
├── endpoint_url
├── wsdl_url
├── auth_type
├── status
└── timestamps

service_credentials
├── id
├── service_id
├── username
├── password (encrypted)
├── certificate_path
├── valid_until
└── timestamps

service_logs
├── id
├── service_id
├── request_type
├── request_data (encrypted)
├── response_status
├── response_time_ms
├── error_message
├── requested_by
├── requested_at
└── timestamps

cached_responses
├── id
├── service_id
├── cache_key
├── response_data (encrypted)
├── expires_at
└── timestamps
```

## 🛠️ Teknik Gereksinimler

### Backend
- Runtime: Node.js 18+ / Java
- SOAP Client: soap / node-soap
- XML Parser: fast-xml-parser

### Güvenlik
- SSL/TLS sertifikası
- e-İmza sertifikası
- IP whitelist

### Veritabanı
- Primary: PostgreSQL 15+
- Şifreleme: AES-256

## 📁 Modül Yapısı

```
16-edevlet-entegrasyon/
├── src/
│   ├── services/
│   │   ├── kps/               # Kimlik servisleri
│   │   ├── aks/               # Adres servisleri
│   │   ├── yoksis/            # YÖKSİS
│   │   ├── sgk/               # SGK servisleri
│   │   ├── meb/               # MEB servisleri
│   │   ├── osym/              # ÖSYM servisleri
│   │   └── e-imza/            # e-İmza doğrulama
│   ├── gateway/               # API Gateway
│   ├── cache/                 # Önbellekleme
│   ├── audit/                 # Audit logging
│   └── config/                # Servis konfigürasyonu
├── certs/                     # Sertifikalar
├── tests/
├── docs/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - Temel (MVP)
- [ ] KPS entegrasyonu
- [ ] e-Devlet girişi
- [ ] Temel loglama

### Faz 2 - Genişletme
- [ ] YÖKSİS entegrasyonu
- [ ] SGK entegrasyonu
- [ ] Önbellekleme

### Faz 3 - İleri Özellikler
- [ ] MEB/ÖSYM entegrasyonu
- [ ] e-İmza doğrulama
- [ ] Gelişmiş monitoring

## 📊 KPI'lar

- Servis uptime oranı
- Ortalama yanıt süresi
- Hata oranı
- Cache hit oranı

## 🔒 Güvenlik Notları

- Tüm veriler şifreli saklanır
- KVKK uyumlu log tutma
- Erişim sadece whitelist IP'lerden
- Sertifika bazlı kimlik doğrulama

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı

**Öncelik:** Yüksek

**Tahmini Süre:** 4-5 ay

**Not:** e-Devlet entegrasyonu için resmi başvuru ve sözleşme gereklidir.
