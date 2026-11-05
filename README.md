# PillCare — local website project

This is a Vite + React + TypeScript frontend starter for the PillCare caregiver dashboard. It is structured for Vercel deployment and includes a Supabase client placeholder plus a starter SQL schema.

## 1. Requirements
- Node.js 20.19+ or 22.12+
- npm
- A Supabase account (optional for running the UI demo)
- A Vercel account (optional for local development)

## 2. Run on Windows Command Prompt

1. Extract `pillcare.zip`.
2. Open Command Prompt in the extracted `pillcare` folder.
3. Run:

```bat
npm install
copy .env.example .env
npm run dev
```

4. Open the local URL printed in the terminal (usually http://localhost:5173).

The dashboard runs in demo mode without Supabase credentials. The compartment buttons and add-dose form are interactive, but demo changes are held in browser memory and are not saved to a database.

## 3. Connect Supabase

1. Create a Supabase project.
2. Open **Project Settings → API** and copy the Project URL and publishable/anon key.
3. Put them in `.env`:

```env
VITE_SUPABASE_URL=https://YOUR_PROJECT_ID.supabase.co
VITE_SUPABASE_ANON_KEY=YOUR_SUPABASE_PUBLISHABLE_OR_ANON_KEY
```

4. In Supabase, open **SQL Editor**, paste `supabase/schema.sql`, and run it.
5. Restart the local server after editing `.env` (Ctrl+C, then `npm run dev`).

The Supabase client is initialized when those environment variables exist. This starter does not yet persist dashboard data, authenticate users, or ingest device events; those integrations are the next development stage.

## 4. Deploy to Vercel

### Option A — GitHub (recommended)
1. Create a GitHub repository and upload the extracted project.
2. In Vercel, choose **Add New → Project** and import the repository.
3. Framework preset: **Vite**. Build command: `npm run build`. Output directory: `dist`.
4. Add `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` in Vercel → Project → Settings → Environment Variables.
5. Deploy.

### Option B — Vercel CLI
Install and sign in:

```bat
npm install -g vercel
vercel login
vercel
```

Follow the prompts. Add the same environment variables in Vercel project settings, then redeploy.

## 5. Hardware and production notes
- Sensor access is not proof that medication was swallowed. UI labels therefore say “accessed” rather than “taken” where possible.
- Do not expose Supabase service-role keys in frontend code or on an ESP32.
- For real devices, send authenticated sensor events to a Supabase Edge Function using a per-device credential. Add server-side schedule evaluation and notification delivery before relying on missed-dose alerts.
- The SQL file enables RLS. The initial policies are intentionally limited; add and test patient/caregiver access policies before using real patient data.
