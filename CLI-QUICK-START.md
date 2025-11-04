# Nguard CLI - Hızlı Başlangıç

Yeni bir Next.js 16+ projesine Nguard'ı kurmanın en hızlı yolu.

## Yükleme ve Kurulum (2 Adım)

### 1. Nguard'ı Yükle

```bash
npm install nguard
```

### 2. CLI Setup Wizard'ı Çalıştır

```bash
npx nguard-setup
```

Veya projen içinde:

```bash
npm run setup
```

## Ne Yapılacak?

Sihirbaz otomatik olarak:

✅ **Sorar:**
- TypeScript mi, JavaScript mi?
- App directory nerede?
- Oturum çerezi adı?
- Hangi API rotaları oluşturulsun?

✅ **Oluşturur:**
- `lib/auth.ts` - Server tarafı kimlik doğrulama
- `app/api/auth/login/route.ts` - Login API
- `app/api/auth/logout/route.ts` - Logout API
- `app/api/auth/validate/route.ts` - Validasyon
- `app/api/auth/refresh/route.ts` - Yenileme
- `proxy.ts` - Next.js 16 ara yazılım
- `.env.local.example` - Çevre değişkenleri şablonu

✅ **Güncelleştirür:**
- `tsconfig.json` - Path aliases (`@/*`)

## Kurulum Sonrası (3 Adım)

### 1. Çevre Değişkenlerini Ayarla

```bash
cp .env.local.example .env.local
```

Edit `.env.local`:
```env
NGUARD_SECRET=openssl rand -base64 32 ile oluştur
BACKEND_API_URL=http://localhost:8080/api
NODE_ENV=development
```

### 2. SessionProvider Ekle

`app/layout.tsx`'de:

```typescript
'use client';

import { SessionProvider } from 'nguard/client';

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <SessionProvider>{children}</SessionProvider>
      </body>
    </html>
  );
}
```

### 3. Kullan

**Server Component'te:**
```typescript
import { auth } from '@/lib/auth';

export default async function Dashboard() {
  const session = await auth();
  if (!session) return <div>Giriş yapmalısın</div>;
  return <div>Hoş geldin {session.email}</div>;
}
```

**Client Component'te:**
```typescript
'use client';

import { useSession, useLogin } from 'nguard/client';

export default function LoginPage() {
  const { login, isLoading } = useLogin();

  const handleLogin = async (email, password) => {
    const response = await login({ email, password });
    if (response.session) {
      // Başarı
    }
  };

  return (
    <button onClick={() => handleLogin('user@example.com', 'pass')}>
      Giriş Yap
    </button>
  );
}
```

## Komut Referansı

### Local Development
```bash
npm run setup        # CLI sihirbazını çalıştır
npm run dev          # Watch mode'de TypeScript derle
npm run build        # Projeyi derle
```

### Global (NPM yükledikten sonra)
```bash
npx nguard-setup     # Sihirbazı çalıştır
```

## Bağlantılar

- 📖 [CLI Setup Guide](./docs/en/CLI-SETUP.md) - Detaylı rehber
- 📖 [Turkish Guide](./docs/tr/CLI-SETUP.md) - Türkçe rehber
- 🚀 [Quickstart](./docs/en/QUICKSTART.md) - Hızlı başlangıç
- 📚 [Full Documentation](./docs/) - Tüm belgeler

## Sorun?

1. Typescript hatası → `npm run build` ile kontrol et
2. Session kalıcı değil → `.env.local`'da `NGUARD_SECRET` var mı?
3. Routes çalışmıyor → `app/api/auth/*/route.ts` dosyaları var mı?

## Şablon Projeler

Örnek Next.js projeleri `examples/` klasöründe:
- Basic middleware setup
- Next-intl integration
- Session validation patterns
- Client hook examples
