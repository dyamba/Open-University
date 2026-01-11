# CMS - İçerik Yönetim Sistemi

Üniversitenin web sitesi, portal ve dijital içeriklerinin merkezi yönetim platformu.

## 🎯 Amaç

Üniversitenin tüm dijital içeriklerinin (web sitesi, duyurular, haberler, etkinlikler) teknik bilgi gerektirmeden kolayca yönetilmesini sağlamak.

## 📋 Kapsam

- Kurumsal web sitesi yönetimi
- Çoklu site desteği (fakülte, bölüm, merkez siteleri)
- Duyuru ve haber yönetimi
- Etkinlik takvimi
- Medya kütüphanesi
- Çok dilli içerik desteği

## ✨ Temel Özellikler

### Site Yönetimi
- Çoklu site mimarisi
- Tema ve şablon yönetimi
- Menü düzenleme
- SEO ayarları
- URL yönetimi (slugs)
- Site haritası oluşturma

### Sayfa Yönetimi
- Sürükle-bırak sayfa oluşturucu
- Zengin metin editörü (WYSIWYG)
- Sayfa versiyonlama
- Taslak/yayın durumu
- Zamanlı yayınlama
- Şablon sistemi

### Duyuru & Haber
- Duyuru kategorileri
- Haber akışı
- Öne çıkan içerikler
- Arşiv yönetimi
- RSS feed
- Sosyal medya paylaşımı

### Etkinlik Yönetimi
- Etkinlik takvimi
- Etkinlik kategorileri
- Kayıt formu entegrasyonu
- Hatırlatma bildirimleri
- Takvim dışa aktarma (iCal)

### Medya Kütüphanesi
- Görsel yükleme ve optimizasyon
- Video yönetimi
- Doküman depolama
- Galeri oluşturma
- Alt yazı ve etiketleme
- CDN entegrasyonu

### Çok Dilli Destek
- Türkçe / İngilizce (varsayılan)
- Sınırsız dil ekleme
- Çeviri iş akışı
- Dil bazlı içerik yönetimi

### Form Oluşturucu
- Sürükle-bırak form tasarımı
- Form gönderimlerini toplama
- Email bildirimleri
- Veri dışa aktarma

## 🔗 Entegrasyonlar

| Sistem | Entegrasyon Türü | Açıklama |
|--------|------------------|----------|
| SSO | Kimlik Doğrulama | Editör girişi |
| Email Hub | Bildirim | Duyuru dağıtımı |
| Merkezi İletişim | Veri Paylaşımı | Duyuru senkronizasyonu |
| Anket Sistemi | Embed | Anket yerleştirme |
| Akademik Takvim | Veri Çekme | Akademik etkinlikler |
| Google Analytics | Analitik | Site istatistikleri |
| CDN | Medya | Dosya dağıtımı |

## 👥 Kullanıcı Rolleri

| Rol | Yetkiler |
|-----|----------|
| **Editör** | İçerik oluşturma ve düzenleme |
| **Yayıncı** | İçerik onaylama ve yayınlama |
| **Site Yöneticisi** | Site ayarları, menü, tema |
| **Medya Yöneticisi** | Medya kütüphanesi yönetimi |
| **Süper Admin** | Tüm siteler, tam yetki |

## 🗃️ Veritabanı Şeması (Temel)

```
sites
├── id
├── name
├── domain
├── theme_id
├── default_language
├── settings (JSON)
├── status
└── timestamps

pages
├── id
├── site_id
├── parent_id
├── title
├── slug
├── content (JSON - blocks)
├── meta_title
├── meta_description
├── template
├── status (draft/published)
├── published_at
├── author_id
└── timestamps

posts
├── id
├── site_id
├── title
├── slug
├── excerpt
├── content
├── featured_image
├── category_id
├── tags[]
├── status
├── published_at
├── author_id
└── timestamps

events
├── id
├── site_id
├── title
├── description
├── location
├── start_date
├── end_date
├── registration_url
├── category_id
├── featured_image
├── status
└── timestamps

media
├── id
├── site_id
├── filename
├── original_name
├── mime_type
├── size
├── url
├── alt_text
├── folder_id
├── uploaded_by
└── timestamps

menus
├── id
├── site_id
├── name
├── location
├── items (JSON)
└── timestamps
```

## 🛠️ Teknik Gereksinimler

### Backend
- Framework: Node.js + Strapi / Directus / Custom
- API: REST + GraphQL
- Cache: Redis
- Search: Elasticsearch / Meilisearch

### Frontend
- Framework: Next.js 14+ (SSR/SSG)
- Styling: Tailwind CSS
- Editor: TipTap / Lexical

### Veritabanı
- Primary: PostgreSQL 15+
- Media: S3-compatible storage

### CDN & Performans
- CloudFlare / AWS CloudFront
- Image optimization (Sharp)
- Lazy loading

## 📁 Modül Yapısı

```
03-cms/
├── src/
│   ├── modules/
│   │   ├── sites/             # Site yönetimi
│   │   ├── pages/             # Sayfa yönetimi
│   │   ├── posts/             # Haber/duyuru
│   │   ├── events/            # Etkinlikler
│   │   ├── media/             # Medya kütüphanesi
│   │   ├── menus/             # Menü yönetimi
│   │   ├── forms/             # Form oluşturucu
│   │   ├── themes/            # Tema sistemi
│   │   └── i18n/              # Çoklu dil
│   ├── editor/                # Sayfa editörü
│   ├── public/                # Frontend renderer
│   └── admin/                 # Yönetim paneli
├── themes/                    # Hazır temalar
├── tests/
├── docs/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - Temel (MVP)
- [ ] Tek site desteği
- [ ] Sayfa CRUD
- [ ] Temel editör
- [ ] Medya yükleme

### Faz 2 - Genişletme
- [ ] Çoklu site
- [ ] Haber/duyuru modülü
- [ ] Etkinlik takvimi
- [ ] Çok dilli destek

### Faz 3 - İleri Özellikler
- [ ] Gelişmiş blok editör
- [ ] Form oluşturucu
- [ ] SEO araçları
- [ ] A/B testing
- [ ] Headless API

## 📊 KPI'lar

- Sayfa yüklenme süresi
- İçerik güncelleme sıklığı
- SEO skorları
- Kullanıcı etkileşim oranları

## 🎨 Tema Sistemi

```
themes/
├── default/
│   ├── layouts/
│   │   ├── base.html
│   │   ├── home.html
│   │   └── page.html
│   ├── components/
│   │   ├── header.html
│   │   ├── footer.html
│   │   └── sidebar.html
│   ├── assets/
│   │   ├── css/
│   │   └── js/
│   └── theme.json
```

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı

**Öncelik:** Yüksek

**Tahmini Süre:** 4-5 ay
