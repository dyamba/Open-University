# Stratejik Yönetim Bilgi Sistemi

Üniversitenin stratejik planlama, performans yönetimi ve kurumsal hedef takip sistemi.

## 🎯 Amaç

Stratejik plan hazırlama, hedef belirleme, performans izleme ve kurumsal karar destek süreçlerinin yönetimi.

## 📋 Kapsam

- Stratejik plan yönetimi
- Performans programı
- Faaliyet raporları
- KPI yönetimi
- Bütçe-strateji ilişkisi
- Kurumsal karne (Balanced Scorecard)

## ✨ Temel Özellikler

### Stratejik Plan Yönetimi
- Misyon ve vizyon tanımlama
- Stratejik amaçlar
- Hedefler
- Performans göstergeleri
- Stratejiler ve eylemler
- Plan versiyonlama

### Balanced Scorecard
- Finansal boyut
- Müşteri/paydaş boyutu
- İç süreçler boyutu
- Öğrenme ve gelişme boyutu
- Strateji haritası

### Performans Yönetimi
- Performans göstergesi tanımlama
- Hedef değer belirleme
- Gerçekleşme takibi
- Sapma analizi
- Trend raporları

### Faaliyet Yönetimi
- Faaliyet tanımlama
- Sorumlu birim ataması
- Bütçe ilişkilendirme
- İlerleme takibi
- Faaliyet raporlama

### Bütçe Entegrasyonu
- Stratejik plan-bütçe eşleştirme
- Maliyet analizi
- Kaynak tahsisi
- Bütçe gerçekleşme

### Raporlama
- Performans programı
- Faaliyet raporu
- Yönetim dashboardları
- Sayıştay raporları
- YÖKAK raporları

### Paydaş Yönetimi
- Paydaş tanımlama
- Paydaş analizi
- Beklenti yönetimi
- İletişim planı

## 🔗 Entegrasyonlar

| Sistem | Entegrasyon Türü | Açıklama |
|--------|------------------|----------|
| SSO | Kimlik Doğrulama | Merkezi oturum |
| Kalite Sistemi | Veri Paylaşımı | KPI verileri |
| Finans | Veri Çekme | Bütçe verileri |
| BI Dashboard | Veri Aktarımı | Görselleştirme |
| Akademik Sistem | Veri Çekme | Eğitim metrikleri |
| Proje Yönetimi | Veri Çekme | Araştırma metrikleri |

## 👥 Kullanıcı Rolleri

| Rol | Yetkiler |
|-----|----------|
| **Birim Sorumlusu** | Birim verileri girişi |
| **Strateji Uzmanı** | Plan hazırlama, raporlama |
| **Strateji Müdürü** | Tam yetki |
| **Üst Yönetim** | Dashboard, karar destek |
| **Sistem Yöneticisi** | Konfigürasyon |

## 🗃️ Veritabanı Şeması (Temel)

```
strategic_plans
├── id
├── name
├── period_start
├── period_end
├── mission
├── vision
├── core_values[]
├── status
├── version
└── timestamps

strategic_goals
├── id
├── plan_id
├── code
├── title
├── description
├── perspective (BSC boyutu)
├── order
└── timestamps

objectives
├── id
├── goal_id
├── code
├── title
├── description
├── responsible_unit_id
├── weight
└── timestamps

performance_indicators
├── id
├── objective_id
├── code
├── name
├── definition
├── measurement_method
├── frequency
├── unit
├── data_source
├── responsible_id
└── timestamps

indicator_targets
├── id
├── indicator_id
├── year
├── target_value
├── actual_value
├── status
├── notes
└── timestamps

activities
├── id
├── objective_id
├── code
├── title
├── description
├── responsible_unit_id
├── start_date
├── end_date
├── budget
├── status
├── progress
└── timestamps

budget_allocations
├── id
├── activity_id
├── year
├── allocated_amount
├── spent_amount
├── budget_code
└── timestamps

stakeholders
├── id
├── plan_id
├── name
├── type (iç/dış)
├── expectations
├── influence_level
├── interest_level
└── timestamps
```

## 🛠️ Teknik Gereksinimler

### Backend
- Runtime: Node.js 18+
- Framework: NestJS

### Frontend
- Framework: Next.js 14+
- Charts: Recharts / Apache ECharts
- UI: Tailwind CSS

### Veritabanı
- Primary: PostgreSQL 15+
- Analytics: ClickHouse (opsiyonel)

## 📁 Modül Yapısı

```
18-stratejik-yonetim-bilgi-sistemi/
├── src/
│   ├── modules/
│   │   ├── plans/             # Stratejik plan
│   │   ├── goals/             # Amaçlar
│   │   ├── objectives/        # Hedefler
│   │   ├── indicators/        # Göstergeler
│   │   ├── activities/        # Faaliyetler
│   │   ├── budget/            # Bütçe
│   │   ├── stakeholders/      # Paydaşlar
│   │   ├── bsc/               # Balanced Scorecard
│   │   └── reports/           # Raporlama
│   └── dashboards/            # Yönetim panelleri
├── tests/
├── docs/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - Temel (MVP)
- [ ] Plan ve hedef yönetimi
- [ ] Gösterge tanımlama
- [ ] Temel raporlama

### Faz 2 - Genişletme
- [ ] Faaliyet yönetimi
- [ ] Bütçe entegrasyonu
- [ ] BSC modülü

### Faz 3 - İleri Özellikler
- [ ] Otomatik veri çekme
- [ ] Senaryo analizi
- [ ] Gelişmiş dashboard

## 📊 KPI'lar (Meta)

- Hedef gerçekleşme oranı
- Bütçe kullanım oranı
- Faaliyet tamamlanma oranı
- Veri güncelleme zamanlaması

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı

**Öncelik:** Orta

**Tahmini Süre:** 4-5 ay
