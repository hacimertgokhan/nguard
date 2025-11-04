# Nguard

A Next.js 16 compatible session management library with **callback-based authentication**. Flexible, type-safe, and works with any backend.

## Features

- ✅ Server-side & Client-side Callbacks
- ✅ JWT-based Authentication
- ✅ TypeScript 100%
- ✅ Secure Cookie Management
- ✅ React Hooks (useAuth, useSession, useLogin, useLogout)
- ✅ Works with any backend (Spring, Express, Node.js, etc.)
- ✅ Next.js 16+ Support

## Installation & Setup

### Automatic Setup (Recommended)

```bash
# 1. Install package
npm install nguard

# 2. Run interactive setup wizard
npx nguard-setup
```

The wizard will automatically create:
- ✅ `lib/auth.ts` - Server authentication utilities
- ✅ API routes (`/api/auth/login`, `/api/auth/logout`, etc.)
- ✅ `proxy.ts` - Next.js 16 middleware configuration
- ✅ `.env.local` - Environment variables template
- ✅ TypeScript path aliases

### Manual Setup

For detailed manual setup, see [CLI-QUICK-START.md](./CLI-QUICK-START.md)

## Quick Example (After Setup)

### 1. Configure Environment
```bash
cp .env.local.example .env.local
# Edit with your BACKEND_API_URL and generated NGUARD_SECRET
```

### 2. Client Setup (app/layout.tsx)
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

### 3. Server Component
```typescript
import { auth } from '@/lib/auth';

export default async function Dashboard() {
  const session = await auth();

  if (!session) {
    return <div>Please log in</div>;
  }

  return <div>Welcome {session.email}</div>;
}
```

### 4. Client Component
```typescript
'use client';

import { useSession, useLogin, useLogout } from 'nguard/client';

export function LoginForm() {
  const { session } = useSession();
  const { login, isLoading } = useLogin();
  const { logout } = useLogout();

  if (session) {
    return (
      <div>
        <p>Logged in as {session.email}</p>
        <button onClick={logout}>Logout</button>
      </div>
    );
  }

  const handleSubmit = async (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const formData = new FormData(e.currentTarget);
    const response = await login({
      email: formData.get('email'),
      password: formData.get('password'),
    });
    if (response.session) {
      // Success
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="email" name="email" placeholder="Email" required />
      <input type="password" name="password" placeholder="Password" required />
      <button disabled={isLoading}>
        {isLoading ? 'Logging in...' : 'Login'}
      </button>
    </form>
  );
}
```

## Hooks

- `useAuth()` - Get user, isAuthenticated, login, logout
- `useSession()` - Get full session details
- `useLogin()` / `useLogout()` - Specific functions only
- `useSessionUpdate()` - Update session data

## Callbacks

**Server-side (lib/auth.ts):**
- `onServerLogin()` - User authentication
- `onServerLogout()` - Cleanup on logout
- `onValidateSession()` - Validate session
- `onJWT()` - Transform JWT payload
- `onSession()` - Transform session

**Client-side (app/layout.tsx):**
- `onLogin` - Send credentials to backend
- `onLogout` - Handle logout
- `onInitialize` - Load session on app start

## Documentation

📖 **Getting Started:**
- **[CLI Quick Start](./CLI-QUICK-START.md)** - 2-minute setup guide
- **[CLI Setup Guide](./docs/en/CLI-SETUP.md)** - Complete interactive setup wizard guide

📚 **Full Documentation:**
- **[Turkish (Türkçe)](./docs/tr/)** - Kapsamlı Türkçe dokümantasyon
- **[English](./docs/en/)** - Complete English documentation

### Key Documentation
- [QUICKSTART](./docs/en/QUICKSTART.md) - Quick start guide
- [CLI-SETUP](./docs/en/CLI-SETUP.md) - CLI Setup Wizard guide
- [API Reference](./docs/en/API-CLIENT.md) - Client API reference
- [Middleware Guide](./docs/en/MIDDLEWARE.md) - Middleware system
- [Session Validation](./docs/en/VALIDATION.md) - Validation patterns
- [SETUP-REFERENCE](./SETUP-REFERENCE.md) - Quick reference checklist

## Security

- ✅ HS256 JWT signing
- ✅ Secure cookie flags (HttpOnly, Secure, SameSite)
- ✅ Cryptographic session IDs
- ✅ HTTPS support
- ✅ Server-side validation

## Environment Variables

```env
NGUARD_SECRET=your-secret-min-32-chars
# Generate: openssl rand -base64 32
```

## License

MIT

---

## Documentation Links

🇹🇷 **Türkçe Kullanıcılar:** [docs/tr/README.md](./docs/tr/README.md)

🇬🇧 **English Users:** [docs/en/README.md](./docs/en/README.md)
