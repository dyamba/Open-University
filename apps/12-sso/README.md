# SSO - Single Sign-On

Üniversitenin tüm uygulamalarına tek oturum ile erişim sağlayan merkezi kimlik doğrulama sistemi.

## 🎯 Amaç

Kullanıcıların tek bir kimlik bilgisi ile tüm üniversite sistemlerine güvenli erişim sağlaması ve merkezi kimlik yönetimi.

## 📋 Kapsam

- Merkezi kimlik doğrulama
- Tek oturum açma (SSO)
- Çok faktörlü kimlik doğrulama (MFA)
- Kullanıcı ve rol yönetimi
- Erişim politikaları
- Audit ve güvenlik logları

## ✨ Temel Özellikler

### Kimlik Doğrulama
- Kullanıcı adı / şifre
- E-posta doğrulama
- Çok faktörlü kimlik doğrulama (MFA)
  - SMS OTP
  - TOTP (Google Authenticator)
  - Email OTP
  - Hardware token
- Sosyal giriş (Google, Microsoft)
- Kurumsal LDAP/AD entegrasyonu

### SSO Protokolleri
- SAML 2.0
- OAuth 2.0
- OpenID Connect (OIDC)
- CAS (Central Authentication Service)

### Kullanıcı Yönetimi
- Kullanıcı oluşturma/güncelleme/silme
- Toplu kullanıcı import
- Self-servis kayıt
- Profil yönetimi
- Şifre politikaları
- Şifre sıfırlama

### Rol ve Yetki Yönetimi
- Rol tanımlama
- Rol-kullanıcı ataması
- Uygulama bazlı roller
- Dinamik yetkilendirme
- Yetki devri

### Oturum Yönetimi
- Tek oturum (SSO)
- Oturum zaman aşımı
- Eş zamanlı oturum kontrolü
- Oturum sonlandırma (logout)
- Single logout (SLO)

### Güvenlik
- Brute force koruması
- IP kısıtlamaları
- Cihaz yönetimi
- Şüpheli aktivite algılama
- Güvenlik soruları

### Audit & Loglama
- Giriş/çıkış logları
- Başarısız giriş denemeleri
- Yetki değişiklikleri
- Güvenlik olayları
- SIEM entegrasyonu

### Self-Servis Portal
- Şifre değiştirme
- Şifre sıfırlama
- MFA yönetimi
- Aktif oturumlar
- Güvenlik ayarları

## 🔗 Entegrasyonlar

| Sistem | Entegrasyon Türü | Açıklama |
|--------|------------------|----------|
| Tüm Open University Uygulamaları | SSO | Tek oturum |
| LDAP/Active Directory | Kimlik Kaynağı | Kurumsal dizin |
| Google Workspace | OAuth | Sosyal giriş |
| Microsoft 365 | OAuth/SAML | Sosyal giriş |
| E-Devlet | SAML | Devlet kimlik |
| SMS Gateway | OTP | MFA |
| Email Hub | Bildirim | Şifre sıfırlama |

## 👥 Kullanıcı Türleri

| Tür | Açıklama |
|-----|----------|
| **Öğrenci** | Aktif öğrenciler |
| **Akademik Personel** | Öğretim üyeleri |
| **İdari Personel** | İdari çalışanlar |
| **Mezun** | Mezun kullanıcılar |
| **Misafir** | Geçici erişim |
| **Sistem Hesabı** | Servis hesapları |

## 🗃️ Veritabanı Şeması (Temel)

```
users
├── id
├── username
├── email
├── password_hash
├── user_type
├── first_name
├── last_name
├── phone
├── status (active, inactive, locked)
├── email_verified
├── mfa_enabled
├── mfa_secret
├── last_login
├── failed_login_attempts
├── locked_until
├── password_changed_at
└── timestamps

roles
├── id
├── name
├── code
├── description
├── application_id
├── permissions (JSON)
└── timestamps

user_roles
├── id
├── user_id
├── role_id
├── granted_by
├── granted_at
├── expires_at
└── timestamps

applications
├── id
├── name
├── code
├── client_id
├── client_secret
├── redirect_uris[]
├── allowed_grant_types[]
├── token_lifetime
├── status
└── timestamps

sessions
├── id
├── user_id
├── token
├── refresh_token
├── ip_address
├── user_agent
├── device_info
├── expires_at
├── revoked_at
└── timestamps

mfa_methods
├── id
├── user_id
├── method_type (totp, sms, email)
├── identifier
├── secret
├── is_primary
├── verified
└── timestamps

audit_logs
├── id
├── user_id
├── action
├── resource
├── ip_address
├── user_agent
├── details (JSON)
├── status
└── timestamps

password_resets
├── id
├── user_id
├── token
├── expires_at
├── used_at
└── timestamps
```

## 🛠️ Teknik Gereksinimler

### Backend
- Runtime: Node.js 18+ / Go
- Framework: NestJS / Custom
- Auth Library: Passport.js / ory/hydra

### Frontend
- Framework: Next.js 14+
- UI: Tailwind CSS

### Veritabanı
- Primary: PostgreSQL 15+
- Session Store: Redis
- Cache: Redis

### Güvenlik
- TLS 1.3
- JWT (RS256)
- Bcrypt / Argon2 (password hashing)

## 📁 Modül Yapısı

```
12-sso/
├── src/
│   ├── modules/
│   │   ├── auth/              # Kimlik doğrulama
│   │   ├── users/             # Kullanıcı yönetimi
│   │   ├── roles/             # Rol yönetimi
│   │   ├── applications/      # Uygulama kaydı
│   │   ├── sessions/          # Oturum yönetimi
│   │   ├── mfa/               # Çok faktörlü auth
│   │   ├── oauth/             # OAuth 2.0 sunucu
│   │   ├── saml/              # SAML 2.0 sunucu
│   │   ├── audit/             # Audit logging
│   │   └── admin/             # Yönetim
│   ├── providers/             # Dış kimlik sağlayıcıları
│   └── portal/                # Self-servis portal
├── tests/
├── docs/
└── docker/
```

## 🚀 Yol Haritası

### Faz 1 - Temel (MVP)
- [ ] Kullanıcı yönetimi
- [ ] Temel authentication
- [ ] JWT token sistemi

### Faz 2 - Genişletme
- [ ] OAuth 2.0 / OIDC
- [ ] MFA desteği
- [ ] LDAP entegrasyonu

### Faz 3 - İleri Özellikler
- [ ] SAML 2.0
- [ ] E-Devlet entegrasyonu
- [ ] Gelişmiş güvenlik
- [ ] SIEM entegrasyonu

## 📊 KPI'lar

- Başarılı giriş oranı
- Ortalama giriş süresi
- MFA kullanım oranı
- Güvenlik olayı sayısı

## 🔒 Güvenlik Standartları

- OWASP Authentication Guidelines
- NIST Digital Identity Guidelines
- ISO 27001 uyumlu

---

**Modül Durumu:** 🔴 Geliştirme Başlamadı

**Öncelik:** Kritik (Öncelikli)

**Tahmini Süre:** 4-5 ay

**Not:** SSO, tüm sistemlerin bağımlı olduğu temel altyapıdır. İlk geliştirilmesi gereken modüldür.
