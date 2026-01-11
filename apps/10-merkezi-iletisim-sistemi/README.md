# Merkezi İletişim Sistemi

Üniversitenin tüm iç ve dış iletişim kanallarının merkezi yönetim platformu.

## 🎯 Amaç

Farklı iletişim kanallarının (email, SMS, push notification, duyuru) tek bir noktadan yönetilmesi ve hedef kitleye etkili ulaşım sağlanması.

## 📋 Kapsam

- Çok kanallı iletişim yönetimi
- Hedef kitle segmentasyonu
- Şablon yönetimi
- Zamanlı gönderim
- İletişim analitiği
- Bildirim tercihleri yönetimi

## ✨ Temel Özellikler

### Kanal Yönetimi
- Email gönderimi
- SMS gönderimi
- Push notification (mobil)
- In-app bildirimler
- Web duyuruları
- Sosyal medya entegrasyonu

### Hedef Kitle
- Dinamik segmentasyon
- Öğrenci grupları (bölüm, sınıf, durum)
- Personel grupları (birim, unvan)
- Mezun grupları
- Özel listeler
- Filtreleme ve sorgulama

### Mesaj Oluşturma
- Zengin metin editörü
- Şablon sistemi
- Değişken ekleme (ad, soyad, vb.)
- Çoklu dil desteği
- Ek dosya ekleme
- Önizleme

### Gönderim Yönetimi
- Anlık gönderim
- Zamanlı gönderim
- Tekrarlayan gönderimler
- A/B testing
- Gönderim onay akışı

### Bildirim Tercihleri
- Kullanıcı tercih yönetimi
- Kanal bazlı tercihler
- Kategori bazlı tercihler
- Opt-out yönetimi
- KVKK uyumlu

### Analitik
- Gönderim istatistikleri
- Açılma oranları
- Tıklama oranları
- Bounce takibi
- Abonelik yönetimi

### Acil Durum İletişimi
- Toplu acil bildirim
- Çoklu kanal eş zamanlı
- Öncelikli gönderim
- Onay zorunluluğu bypass

## 🔗 Entegrasyonlar

| Sistem | Entegrasyon Türü | Açıklama |
|--------|------------------|----------|
| SSO | Kimlik Doğrulama | Yönetici girişi |
| Akademik Sistem | Veri Çekme | Öğrenci segmentasyonu |
| Personel Sistemi | Veri Çekme | Personel segmentasyonu |
| Email Hub | Altyapı | Email gönderim |
| SMS Gateway | Altyapı | SMS gönderim |
| Mobile App | Push | Push notifications |
| CMS | Veri Paylaşımı | Web duyuruları |

## 👥 Kullanıcı Rolleri

| Rol | Yetkiler |
|-----|----------|
| **Son Kullanıcı** | Tercih yönetimi |
| **Birim İletişimci** | Birim bazlı gönderim |
| **Fakülte İletişimci** | Fakülte bazlı gönderim |
| **Kurumsal İletişim** | Tüm üniversite gönderimi |
| **Acil Durum Yöneticisi** | Acil durum bildirimleri |
| **Sistem Yöneticisi** | Tam yetki, konfigürasyon |

## 🗃️ Veritabanı Şeması (Temel)

```
communication_channels
├── id
├── name
├── type (email, sms, push, web)
├── config (JSON)
├── status
└── timestamps

message_templates
├── id
├── name
├── channel_type
├── subject
├── body
├── variables[]
├── language
├── category
└── timestamps

audiences
├── id
├── name
├── description
├── query_definition (JSON)
├── member_count
├── is_dynamic
├── created_by
└── timestamps

campaigns
├── id
├── name
├── description
├── template_id
├── audience_id
├── channel_types[]
├── scheduled_at
├── sent_at
├── status
├── created_by
├── approved_by
└── timestamps

messages
├── id
├── campaign_id
├── recipient_id
├── recipient_type (student, staff, alumni)
├── channel
├── subject
├── body
├── status (pending, sent, delivered, failed, opened)
├── sent_at
├── opened_at
├── clicked_at
├── error_message
└── timestamps

notification_preferences
├── id
├── user_id
├── user_type
├── channel
├── category
├── enabled
└── timestamps
```

## 🛠️ Teknik Gereksinimler

### Backend
- Runtime: Node.js 18+
- Framework: NestJS
- Queue: Bull / RabbitMQ
- Email: SendGrid / Amazon SES
- SMS: Twilio / Netgsm

### Frontend
- Framework: Next.js 14+
- Editor: TipTap / Lexical
- UI: Tailwind CSS

### Veritabanı
- Primary: PostgreSQL 15+
- Queue: Redis
- Analytics: ClickHouse (opsiyonel)

## 📁 Modül Yapısı

```
10-merkezi-iletisim-sistemi/
├── src/
│   ├── modules/
│   │   ├── channels/          # Kanal yönetimi
│   │   ├── templates/         # Şablon yönetimi
│   │   ├── audiences/         # Hedef kitle
│   │   ├── campaigns/         # Kampanya yönetimi
│   │   ├── messages/          # Mesaj gönderimi
│   │   ├── preferences/       # Tercih yönetimi
│   │   ├── analytics/         # Analitik
│   │   └── emergency/         # Acil durum
│   ├── workers/               # Gönderim işçileri
│   └── providers/             # Email/SMS sağlayıcıları
├── tests/
├── docs/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - Temel (MVP)
- [ ] Email gönderimi
- [ ] Temel şablonlar
- [ ] Manuel segmentasyon

### Faz 2 - Genişletme
- [ ] SMS entegrasyonu
- [ ] Dinamik segmentasyon
- [ ] Zamanlı gönderim

### Faz 3 - İleri Özellikler
- [ ] Push notification
- [ ] Gelişmiş analitik
- [ ] A/B testing
- [ ] Acil durum modülü

## 📊 KPI'lar

- Email açılma oranı
- Tıklama oranı
- Bounce oranı
- Opt-out oranı
- Gönderim başarı oranı

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı

**Öncelik:** Yüksek

**Tahmini Süre:** 4-5 ay
