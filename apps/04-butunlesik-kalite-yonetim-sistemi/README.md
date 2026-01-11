# Bütünleşik Kalite Yönetim Sistemi

Üniversitenin kalite güvence süreçlerini, akreditasyon hazırlıklarını ve sürekli iyileştirme faaliyetlerini yöneten sistem.

## 🎯 Amaç

YÖKAK (Yükseköğretim Kalite Kurulu) standartları, ulusal ve uluslararası akreditasyon gereksinimleri doğrultusunda kalite süreçlerinin sistematik yönetimi.

## 📋 Kapsam

- Kurumsal kalite güvence sistemi
- İç ve dış değerlendirme süreçleri
- Akreditasyon takibi (MÜDEK, ABET, THEQC vb.)
- Performans göstergeleri (KPI) yönetimi
- Sürekli iyileştirme döngüsü (PUKÖ)
- Paydaş memnuniyet yönetimi

## ✨ Temel Özellikler

### Kalite Standartları Yönetimi
- YÖKAK kalite göstergeleri
- Akreditasyon kriterleri tanımlama
- Standart eşleştirme matrisleri
- Uyumluluk takibi
- Kanıt yönetimi

### Öz Değerlendirme
- Öz değerlendirme raporları (ÖDR)
- Birim bazlı değerlendirme
- SWOT analizi
- Kanıt toplama ve ilişkilendirme
- Dönemsel karşılaştırma

### Dış Değerlendirme
- Dış değerlendirme takvimleri
- Değerlendirici atama
- Ziyaret planlaması
- Bulgu ve önerilerin takibi
- Aksiyon planları

### Akreditasyon Yönetimi
- Program akreditasyonları
- Kurumsal akreditasyonlar
- Başvuru süreç takibi
- Döküman hazırlama
- Deadline hatırlatmaları

### KPI & Metrik Yönetimi
- Performans göstergesi tanımlama
- Hedef belirleme
- Veri toplama ve analiz
- Trend raporları
- Dashboard görünümleri
- Benchmark karşılaştırmaları

### Süreç Yönetimi
- Süreç tanımlama ve modelleme
- Süreç sahipliği
- Prosedür yönetimi
- İş akışı tasarımı
- Süreç performansı ölçümü

### İyileştirme Yönetimi
- İyileştirme önerileri
- Düzeltici faaliyetler
- Önleyici faaliyetler
- Aksiyon takibi
- Etkinlik değerlendirme

### Anket & Geri Bildirim
- Paydaş memnuniyet anketleri
- Öğrenci geri bildirimleri
- Mezun takibi
- İşveren memnuniyeti
- Sonuç analizi

## 🔗 Entegrasyonlar

| Sistem | Entegrasyon Türü | Açıklama |
|--------|------------------|----------|
| SSO | Kimlik Doğrulama | Merkezi oturum |
| Akademik Sistem | Veri Çekme | Eğitim istatistikleri |
| Personel Sistemi | Veri Çekme | Akademik kadro verileri |
| Anket Sistemi | Veri Çekme | Memnuniyet sonuçları |
| BI Dashboard | Veri Aktarımı | KPI görselleştirme |
| EBYS | Belge | Resmi yazışmalar |
| Proje Yönetimi | Veri Çekme | Araştırma verileri |

## 👥 Kullanıcı Rolleri

| Rol | Yetkiler |
|-----|----------|
| **Birim Kalite Temsilcisi** | Birim verisi girişi, öz değerlendirme |
| **Kalite Koordinatörü** | Fakülte düzeyi yönetim |
| **Kalite Müdürü** | Kurumsal kalite yönetimi |
| **Akreditasyon Sorumlusu** | Akreditasyon süreç yönetimi |
| **Üst Yönetim** | Dashboard, raporlama |
| **Sistem Yöneticisi** | Konfigürasyon, tam yetki |

## 🗃️ Veritabanı Şeması (Temel)

```
quality_standards
├── id
├── code
├── name
├── description
├── standard_type (YÖKAK, MÜDEK, vb.)
├── parent_id
├── criteria[]
├── status
└── timestamps

evaluations
├── id
├── unit_id
├── evaluation_type (iç/dış)
├── period
├── status
├── start_date
├── end_date
├── evaluators[]
└── timestamps

evaluation_items
├── id
├── evaluation_id
├── standard_id
├── score
├── evidence_ids[]
├── findings
├── recommendations
└── timestamps

kpis
├── id
├── code
├── name
├── description
├── unit_id
├── formula
├── target_value
├── measurement_frequency
├── data_source
├── responsible_id
└── timestamps

kpi_measurements
├── id
├── kpi_id
├── period
├── value
├── target
├── status (hedef altı/üstü)
├── notes
└── timestamps

improvement_actions
├── id
├── title
├── description
├── source (öz değerlendirme, dış değerlendirme, öneri)
├── source_id
├── responsible_id
├── due_date
├── status
├── effectiveness_review
└── timestamps

evidence
├── id
├── title
├── description
├── file_url
├── standard_ids[]
├── uploaded_by
└── timestamps

accreditations
├── id
├── program_id
├── accreditation_body
├── application_date
├── visit_date
├── result
├── valid_until
├── status
└── timestamps
```

## 🛠️ Teknik Gereksinimler

### Backend
- Runtime: Node.js 18+ / Python 3.11+
- Framework: NestJS / FastAPI
- Workflow Engine: Temporal / Camunda

### Frontend
- Framework: React 18+ / Next.js 14+
- Charts: Recharts / Apache ECharts
- UI: Tailwind CSS + Shadcn/ui

### Veritabanı
- Primary: PostgreSQL 15+
- Analytics: ClickHouse (opsiyonel)

## 📁 Modül Yapısı

```
04-butunlesik-kalite-yonetim-sistemi/
├── src/
│   ├── modules/
│   │   ├── standards/         # Kalite standartları
│   │   ├── evaluations/       # Değerlendirmeler
│   │   ├── kpis/              # Performans göstergeleri
│   │   ├── processes/         # Süreç yönetimi
│   │   ├── improvements/      # İyileştirme aksiyonları
│   │   ├── accreditations/    # Akreditasyonlar
│   │   ├── evidence/          # Kanıt yönetimi
│   │   ├── surveys/           # Anket entegrasyonu
│   │   └── reports/           # Raporlama
│   ├── workflows/             # İş akışları
│   └── dashboards/            # Gösterge panelleri
├── tests/
├── docs/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - Temel (MVP)
- [ ] Standart tanımlama
- [ ] KPI yönetimi
- [ ] Temel değerlendirme

### Faz 2 - Genişletme
- [ ] Öz değerlendirme modülü
- [ ] Kanıt yönetimi
- [ ] İyileştirme takibi

### Faz 3 - İleri Özellikler
- [ ] Akreditasyon modülü
- [ ] Süreç modelleme
- [ ] Gelişmiş analytics
- [ ] Benchmark sistemi

## 📊 KPI'lar (Meta)

- Öz değerlendirme tamamlanma oranı
- Aksiyon tamamlanma süresi
- KPI hedef tutturma oranı
- Paydaş memnuniyet trendi

## 📋 YÖKAK Uyumu

Sistem, YÖKAK'ın belirlediği 5 temel alan için hazır şablonlar içerir:

1. **Kalite Güvence Sistemi**
2. **Eğitim-Öğretim**
3. **Araştırma-Geliştirme**
4. **Toplumsal Katkı**
5. **Yönetim Sistemi**

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı

**Öncelik:** Yüksek

**Tahmini Süre:** 5-6 ay
