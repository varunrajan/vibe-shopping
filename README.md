# Vibe Shopping - Shopping List App

A modern, beautiful shopping list application built with React and Supabase.

## Features

- **Thing Input**: Required text field to add items to your shopping list
- **Amount Input**: Optional field for quantities and measurements (e.g., "2 lbs", "1 gallon")
- **Store Multi-Select**: Optional dropdown with multiple store options:
  - Hmart, 99 Ranch, Trader Joe's, Costco, Farmer's Market
  - Selected stores appear as pills with dismissible "×" buttons
- **Persistent storage**: Items are saved to Supabase and survive page reloads
- **Anonymous sessions**: Start using immediately — no sign-up required
- **Auth prompt**: After 3+ items or on return visits, prompted to create an account
- **Email magic link**: Sign up or log in with a passwordless email link
- **Google OAuth**: One-click sign-in with Google
- **Item migration**: Items added anonymously are carried over when you sign up or log in

## Getting Started

### 1. Run the dev server

The app must be served over HTTP (not opened as a local file) for auth redirects to work:

```bash
python3 -m http.server 3001
```

Then open [http://localhost:3001](http://localhost:3001).

### 2. Configure Supabase (one-time dashboard setup)

Visit the [Supabase Dashboard](https://supabase.com/dashboard/project/jcvimtxvmpmxpjrsfjcm) and complete these steps:

#### Enable Anonymous Sign-ins
Authentication → Settings → scroll to **User Signups** → toggle **Allow anonymous sign-ins** on.

#### Set Redirect URLs
Authentication → URL Configuration:
- **Site URL**: `http://localhost:3001`
- **Redirect URLs**: add `http://localhost:3001/**`

> For production, replace `localhost:3001` with your deployed domain.

#### Configure Google OAuth (optional)
Authentication → Providers → Google:
1. Enable Google provider
2. Create OAuth 2.0 credentials in [Google Cloud Console](https://console.cloud.google.com/apis/credentials)
   - Authorized redirect URI: `https://jcvimtxvmpmxpjrsfjcm.supabase.co/auth/v1/callback`
3. Paste Client ID and Client Secret into the Supabase Google provider settings

## Usage

1. Open the app — items are immediately usable (no account needed)
2. Add items with Thing (required), Amount (optional), and Store(s)
3. Check off purchased items — they move to the bottom with a strikethrough
4. Delete items with the "×" button
5. After 3 items, you'll be prompted to save your list with an account
6. Sign up with email magic link or Google to keep your list across devices

## Tech Stack

- React 18 (via CDN)
- Supabase (PostgreSQL, Auth, RLS)
- Vanilla CSS (no framework)
- Babel Standalone (JSX in browser — no build step)

## Database

**Project:** `jcvimtxvmpmxpjrsfjcm` (vibe-shopping, us-west-1)

**Table:** `shopping_items`
- `id` uuid (PK)
- `user_id` uuid (FK → auth.users, RLS-enforced)
- `thing` text (required)
- `amount` text (optional)
- `stores` text[] (optional)
- `purchased` boolean
- `created_at` timestamptz

Row Level Security ensures every user can only read and write their own items — including anonymous users.
