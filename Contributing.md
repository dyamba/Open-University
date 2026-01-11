# Katkıda Bulunma Rehberi

Open University projesine katkıda bulunmak istediğiniz için teşekkür ederiz! Bu rehber, projeye nasıl katkıda bulunabileceğinizi açıklamaktadır.

## İçindekiler

- [Davranış Kuralları](#davranış-kuralları)
- [Nasıl Katkıda Bulunabilirim?](#nasıl-katkıda-bulunabilirim)
- [Geliştirme Ortamı](#geliştirme-ortamı)
- [Kod Standartları](#kod-standartları)
- [Commit Mesajları](#commit-mesajları)
- [Pull Request Süreci](#pull-request-süreci)
- [Issue Oluşturma](#issue-oluşturma)
- [Modül Geliştirme](#modül-geliştirme)
- [Dokümantasyon](#dokümantasyon)
- [Topluluk](#topluluk)

---

## Davranış Kuralları

Bu proje, açık ve kapsayıcı bir topluluk oluşturmayı hedeflemektedir. Tüm katılımcılardan beklentilerimiz:

- **Saygılı olun** — Farklı görüşlere ve deneyimlere saygı gösterin
- **Yapıcı olun** — Eleştirilerinizi yapıcı bir şekilde ifade edin
- **Kapsayıcı olun** — Herkesin katılımını teşvik edin
- **Sabırlı olun** — Yeni katkıda bulunanlara yardımcı olun

Kabul edilemez davranışlar: taciz, aşağılama, kişisel saldırılar ve profesyonel olmayan davranışlar.

---

## Nasıl Katkıda Bulunabilirim?

### 🐛 Hata Bildirimi
Bir hata bulduysanız, lütfen GitHub Issues üzerinden bildirin. Hata raporunuzda şunları belirtin:
- Hatanın açık bir açıklaması
- Hatayı yeniden oluşturma adımları
- Beklenen davranış
- Gerçekleşen davranış
- Ekran görüntüleri (varsa)
- Ortam bilgileri (işletim sistemi, tarayıcı, versiyon)

### 💡 Özellik Önerisi
Yeni bir özellik önermek için:
- Önce mevcut issue'ları kontrol edin
- Özelliğin hangi sorunu çözdüğünü açıklayın
- Olası çözüm yaklaşımlarını paylaşın
- Eğitim kurumları için faydalarını belirtin

### 💻 Kod Katkısı
1. Projeyi fork edin
2. Feature branch oluşturun (`git checkout -b feature/yeni-ozellik`)
3. Değişikliklerinizi commit edin
4. Branch'inizi push edin (`git push origin feature/yeni-ozellik`)
5. Pull Request açın

### 📝 Dokümantasyon
- Hataları düzeltin
- Eksik bölümleri tamamlayın
- Örnekler ekleyin
- Çeviriler yapın

### 🌍 Çeviri
Projeyi kendi dilinize çevirerek katkıda bulunabilirsiniz. Çeviri dosyaları `/docs/i18n/` klasöründe bulunmaktadır.

---

## Geliştirme Ortamı

### Gereksinimler

xxxxxxx
xxxxxxx
xxxxxxx
xxxxxxx
xxxxxxx
xxxxxxx
xxxxxxx


### Proje Yapısı

```
open-university/
├── apps/                    # Uygulama modülleri
│   ├── academic/           # Akademik Yönetim Sistemi
│   ├── lms/                # Öğrenme Yönetim Sistemi
│   ├── hr/                 # İnsan Kaynakları
│   ├── cms/                # İçerik Yönetim Sistemi
│   ├── quality/            # Kalite Güvence
│   └── ...                 # Diğer modüller
├── packages/               # Paylaşılan paketler
│   ├── ui/                 # UI bileşenleri
│   ├── auth/               # Kimlik doğrulama
│   ├── database/           # Veritabanı katmanı
│   └── utils/              # Yardımcı fonksiyonlar
├── docs/                   # Dokümantasyon
├── scripts/                # Yardımcı scriptler
└── docker/                 # Docker yapılandırmaları
```

---

## Kod Standartları

### Genel Kurallar

- **Dil:** TypeScript tercih edilir
- **Formatlama:** Prettier kullanılır
- **Linting:** ESLint kurallarına uyulmalıdır
- **Test:** Her özellik için test yazılmalıdır

### Dosya İsimlendirme

```
# Bileşenler (PascalCase)
UserProfile.tsx
CourseCard.tsx

# Servisler ve yardımcılar (camelCase)
userService.ts
dateUtils.ts

# Sabitler (SCREAMING_SNAKE_CASE)
API_ENDPOINTS.ts
ERROR_CODES.ts
```

### Kod Stili

```typescript
// ✅ İyi
export async function getUserById(id: string): Promise<User | null> {
  const user = await db.user.findUnique({ where: { id } });
  return user;
}

// ❌ Kaçının
export async function getUserById(id) {
  var user = await db.user.findUnique({ where: { id } });
  return user;
}
```

### Test Yazımı

```typescript
describe('UserService', () => {
  describe('getUserById', () => {
    it('should return user when valid id provided', async () => {
      const user = await userService.getUserById('valid-id');
      expect(user).toBeDefined();
      expect(user.id).toBe('valid-id');
    });

    it('should return null when user not found', async () => {
      const user = await userService.getUserById('invalid-id');
      expect(user).toBeNull();
    });
  });
});
```

---

## Commit Mesajları

[Conventional Commits](https://www.conventionalcommits.org/) standardını kullanıyoruz.

### Format

```
<tip>(<kapsam>): <açıklama>

[isteğe bağlı gövde]

[isteğe bağlı dipnot]
```

### Tipler

| Tip | Açıklama |
|-----|----------|
| `feat` | Yeni özellik |
| `fix` | Hata düzeltmesi |
| `docs` | Dokümantasyon değişikliği |
| `style` | Kod formatı değişikliği (fonksiyonellik değişmez) |
| `refactor` | Kod yeniden yapılandırması |
| `test` | Test ekleme veya düzeltme |
| `chore` | Yapılandırma, build değişiklikleri |
| `perf` | Performans iyileştirmesi |

### Örnekler

```bash
feat(lms): ders oluşturma sayfası eklendi
fix(auth): oturum zaman aşımı sorunu düzeltildi
docs(readme): kurulum adımları güncellendi
refactor(academic): öğrenci servisi yeniden yapılandırıldı
test(hr): personel CRUD testleri eklendi
```

---

## Pull Request Süreci

### PR Açmadan Önce

1. ✅ Kodunuz lint kontrolünden geçiyor mu?
2. ✅ Tüm testler başarılı mı?
3. ✅ Yeni özellik için test yazdınız mı?
4. ✅ Dokümantasyonu güncellediniz mi?
5. ✅ Commit mesajları standartlara uygun mu?

### PR Şablonu

```markdown
## Açıklama
[Değişikliğin kısa açıklaması]

## Değişiklik Tipi
- [ ] Hata düzeltmesi
- [ ] Yeni özellik
- [ ] Breaking change
- [ ] Dokümantasyon

## İlgili Issue
Closes #[issue-numarası]

## Test
[Testlerin nasıl yapılacağı]

## Ekran Görüntüleri
[Varsa ekran görüntüleri]

## Kontrol Listesi
- [ ] Kod standartlarına uygun
- [ ] Testler yazıldı ve başarılı
- [ ] Dokümantasyon güncellendi
```

### İnceleme Süreci

1. En az 1 onay gereklidir
2. Tüm CI kontrolleri başarılı olmalıdır
3. Çakışmalar çözülmüş olmalıdır
4. Reviewer yorumları yanıtlanmalıdır

---

## Issue Oluşturma

### Hata Raporu Şablonu

```markdown
## Hata Açıklaması
[Hatanın net açıklaması]

## Yeniden Oluşturma Adımları
1. '...' sayfasına gidin
2. '...' butonuna tıklayın
3. Hatayı görün

## Beklenen Davranış
[Ne olması gerektiği]

## Gerçekleşen Davranış
[Ne olduğu]

## Ortam
- İşletim Sistemi: [örn. Windows 11]
- Tarayıcı: [örn. Chrome 120]
- Versiyon: [örn. 1.0.0]

## Ekran Görüntüleri
[Varsa ekran görüntüleri]

## Ek Bilgi
[Başka ilgili bilgiler]
```

### Özellik İsteği Şablonu

```markdown
## Özellik Açıklaması
[Özelliğin net açıklaması]

## Problem
[Bu özellik hangi sorunu çözüyor?]

## Çözüm Önerisi
[Nasıl çalışması gerektiği]

## Alternatifler
[Düşündüğünüz alternatif çözümler]

## Ek Bilgi
[Başka ilgili bilgiler]
```

---

## Modül Geliştirme

Yeni bir modül geliştirirken şu yapıyı takip edin:

### Modül Yapısı

```
apps/yeni-modul/
├── README.md               # Modül dokümantasyonu
├── package.json            # Bağımlılıklar
├── src/
│   ├── index.ts           # Ana giriş noktası
│   ├── components/        # UI bileşenleri
│   ├── pages/             # Sayfa bileşenleri
│   ├── services/          # İş mantığı
│   ├── hooks/             # Custom hooks
│   ├── utils/             # Yardımcı fonksiyonlar
│   ├── types/             # TypeScript tipleri
│   └── constants/         # Sabitler
├── tests/                 # Test dosyaları
└── docs/                  # Modül dokümantasyonu
```

### Entegrasyon Gereksinimleri

Her modül şunları sağlamalıdır:

1. **Kimlik Doğrulama Entegrasyonu** — Merkezi auth sistemini kullanmalı
2. **API Standardı** — RESTful API kurallarına uymalı
3. **Veritabanı Migration'ları** — Şema değişiklikleri migration ile yapılmalı
4. **Event Sistemi** — Modüller arası iletişim için event'ler kullanılmalı
5. **Loglama** — Merkezi loglama sistemine entegre olmalı

---

## Dokümantasyon

### Dokümantasyon Türleri

1. **API Dokümantasyonu** — OpenAPI/Swagger formatında
2. **Kullanıcı Kılavuzu** — Son kullanıcılar için
3. **Geliştirici Kılavuzu** — Teknik dokümantasyon
4. **Kurulum Kılavuzu** — Deployment rehberi

### Dokümantasyon Yazım Kuralları

- Açık ve anlaşılır dil kullanın
- Örnekler ekleyin
- Ekran görüntüleri ile destekleyin
- Güncel tutun

---

## Topluluk

### İletişim Kanalları

- **GitHub Discussions** — Genel tartışmalar ve sorular
- **GitHub Issues** — Hata bildirimleri ve özellik istekleri
- **Discord** — Anlık iletişim (yakında belki?)

### Toplantılar

- **Haftalık Geliştirici Toplantısı** — Her Cuma 14:00 (UTC+3)
- **Aylık Topluluk Toplantısı** — Her ayın ilk Pazartesi'si

### Katkıda Bulunanların Tanınması

Tüm katkıda bulunanlar README dosyasında ve CONTRIBUTORS.md dosyasında listelenecektir.

---

## Sorular?

Herhangi bir sorunuz varsa:

1. Önce [dokümantasyonu](docs/) kontrol edin
2. [GitHub Discussions](https://github.com/[kullanici-adi]/open-university/discussions)'da arayın
3. Yeni bir discussion başlatın

---

Katkılarınız için teşekkür ederiz! 🎉

*Her katkı, eğitim topluluğunu güçlendirir.*
