# FiTech — Frontend

The public website and member portals (student / admin / super-admin) of the FiTech club at Adamas University. Next.js 16 (App Router, TypeScript), Tailwind CSS v4, framer-motion, Lenis. Deployed on Vercel.

It talks to two things:

- **Supabase Auth** directly from the browser (sign in, sign up, password reset).
- The **FiTech backend API** (`https://fitech-02.onrender.com`, source in the `fitech_` repository) for everything else, forwarding the user's Supabase access token.

## Setup

1. Create a `.env` file in this directory (never commit it):

   | Variable | Purpose |
   |---|---|
   | `NEXT_PUBLIC_SUPABASE_URL` | Supabase project URL |
   | `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Supabase anon (publishable) key |
   | `NEXT_PUBLIC_API_URL` | Backend base URL — `https://fitech-02.onrender.com` (live) or `http://localhost:4000` (local backend) |
   | `NEXT_PUBLIC_ENABLE_DEMO_LOGIN` | `true` to show the one-click demo login buttons (always shown in development, hidden in production builds otherwise) |

2. `npm install`
3. `npm run dev` → http://localhost:3000

## Quality checks

```bash
npm run lint        # ESLint
npm run typecheck   # TypeScript
npm run build       # production build
npm run test:e2e    # Playwright — builds, serves, runs desktop + mobile
```

The E2E suite covers every public route (renders, no runtime errors, no horizontal overflow), the security headers, the FAQ, the 404 page and the signed-out redirects on all portals. The demo-login test needs the live backend and demo accounts; enable it with `E2E_DEMO_LOGIN=true`. `.github/workflows/ci.yml` runs all of the above on every push and pull request.

## Deploy (Vercel)

Import this repository, framework preset **Next.js**, and set the four `NEXT_PUBLIC_*` variables above in the project settings. The backend must list the deployed origin in its CORS allowlist (`FRONTEND_ORIGIN`); `https://fitech-eta.vercel.app` and `localhost:3000` are always allowed.

## Layout

```
src/app/            routes (home, events, team, projects, domain, gallery, login, portals)
src/components/     home sections, layout (navbar, footer, loader, scroll reveal), modals
src/context/        PortalContext — auth session + all API data for the portals
src/lib/            api client, supabase client, event helpers
src/constants/      shared labels
e2e/                Playwright tests
scripts/            build-bill-dots.mjs — regenerates the landing page's ASCII note
public/intro/       bill-dots.json — the ASCII note data
```
