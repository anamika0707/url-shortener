# Trimrr — URL Shortener

A focused, minimal URL shortener built with React + Vite and backed by Supabase. It includes authentication, user profiles (avatar upload), link creation (custom and generated short URLs), QR code generation, and simple analytics.

This README explains how to run the project locally, what environment variables are required, where important code lives, and common troubleshooting steps.

---

## Features

- Email/password authentication via Supabase Auth
- User profiles with avatar upload (Supabase Storage)
- Create, view, edit, and delete shortened links
- Basic device/location stats for short links
- Tailwind CSS with dark mode via CSS variables

---

# Trimrr — URL Shortener

Trimrr is a production-oriented URL shortener implemented with React + Vite and Supabase. This README documents project structure, local development steps, required environment variables, and implementation notes for core behaviors.

---

## Summary

- Authentication: Supabase Auth (email/password)
- Profile storage: Supabase Storage (avatar uploads stored in `profile_pic` bucket)
- Link management: create (auto/custom slug), read, delete
- Per-link QR code generation and download
- Basic link analytics (device/location)

---

## Local development

1. Install dependencies

```powershell
npm install
```

2. Create a `.env` file at the repository root with required variables:

```env
VITE_SUPABASE_URL=https://<your-project-id>.supabase.co
VITE_SUPABASE_KEY=<your-anon-or-public-key>
```

3. Start development server

```powershell
npm run dev
```

App runs at: http://localhost:5173

---

## Required environment variables

- `VITE_SUPABASE_URL` — Supabase project URL
- `VITE_SUPABASE_KEY` — Supabase anon/public key

Environment variables are read from `import.meta.env` by the Supabase client (`src/db/supabase.js`).

---

## Project layout (high level)

- `src/main.jsx` — application entry
- `src/App.jsx` — router setup
- `src/layouts/app-layout.jsx` — root layout (header, outlet, footer)
- `src/pages/` — `landing.jsx`, `auth.jsx`, `dashboard.jsx`, `link.jsx`, `redirect-link.jsx`
- `src/components/` — components: `header.jsx`, `signup.jsx`, `login.jsx`, `create-link.jsx`, `link-card.jsx`, etc.
- `src/components/ui/` — UI primitives (Avatar, Button, Card, Dialog, Input, etc.)
- `src/db/` — Supabase client and API wrappers (`supabase.js`, `apiAuth.js`, `apiUrls.js`, `apiClicks.js`)
- `src/hooks/` — custom hooks, e.g., `use-fetch.jsx`
- `src/index.css` — Tailwind base, variables and utilities

---

## Avatar and profile image flow

1. Signup component uploads the provided file to Supabase Storage (bucket: `profile_pic`).
2. The created public URL is saved to the user's `user_metadata.profile_pic` during signup.
3. The header component reads `user.user_metadata.profile_pic` and renders the avatar.

Implementation notes:
- Confirm the storage bucket's access policy if client-side public URLs are used.

---

## Hard-coded short domain locations

The codebase contains literal occurrences of the short-domain string used for rendering or copying short links. The primary locations are:

- `src/components/link-card.jsx`
- `src/components/create-link.jsx`
- `src/pages/link.jsx`

To change the domain centrally, introduce a `VITE_BASE_URL` environment variable and consume it where necessary.

---

## Scripts

- `npm run dev` — start dev server
- `npm run build` — create production build
- `npm run preview` — preview production build
- `npm run lint` — run ESLint

---

## Developer notes

- Tailwind at-rules (`@tailwind`, `@apply`) may be reported by some editors; Vite's build pipeline processes them correctly.
- Supabase client initialization is in `src/db/supabase.js`. API wrappers live under `src/db/`.
- If uploaded images return 403, check bucket permissions and the generated public URL.

---

## License

No license file is included in the repository.











