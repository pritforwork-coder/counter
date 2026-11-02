# Counter — Margin Calculator

A clean, mobile-first pricing calculator. Enter a cost price and target margin (or markup) and it instantly gives you the sell price, snapped to the nearest charm price (ending in 9¢).

**Live demo:** https://rfc-counter.netlify.app

---

## Features

- **Find price** — enter cost + desired margin % or markup % → get sell price
- **Check price** — enter cost + sell price → see actual margin, markup, profit
- Charm pricing — auto-snaps to nearest price ending in 9¢
- Save calculations to history (stored locally + optionally synced to Supabase)
- History auto-expires after **30 days** (local and remote)
- Works offline — no backend required for core calculator

---

## Deploy in 1 minute

### Option A — Netlify (recommended)

1. Fork this repo
2. Go to [netlify.com](https://netlify.com) → **Add new site → Import from Git**
3. Select your fork → Deploy
4. Done — you get a live URL instantly

### Option B — GitHub Pages

1. Fork this repo
2. Go to **Settings → Pages → Source: main branch / root**
3. Save — live at `https://yourusername.github.io/counter`

### Option C — Any static host

Upload `index.html` anywhere that serves HTML files (Vercel, Cloudflare Pages, S3, etc.)

---

## Supabase Setup (optional — for cross-device history sync)

Without Supabase, history saves to browser localStorage only.

To enable cloud sync:

### 1. Create a free Supabase project

Go to [supabase.com](https://supabase.com) → New project → choose **Free** tier.

### 2. Run this SQL once

In your Supabase project → **SQL Editor**:

```sql
CREATE TABLE pricing_calculations (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  mode TEXT,
  basis TEXT,
  cost_price DECIMAL(10,4),
  target_pct DECIMAL(6,2),
  sell_price DECIMAL(10,4),
  margin_pct DECIMAL(6,4),
  markup_pct DECIMAL(6,4),
  profit_per_unit DECIMAL(10,4),
  note TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

ALTER TABLE pricing_calculations ENABLE ROW LEVEL SECURITY;

CREATE POLICY "anon_all" ON pricing_calculations
  FOR ALL TO anon USING (true) WITH CHECK (true);
```

### 3. Get your keys

**Project Settings → API** → copy:
- Project URL (`https://xxxx.supabase.co`)
- Anon public key (`eyJ...`)

### 4. Connect in the app

Open the app → tap **⚙️** → paste URL + key → **Connect**.

The app auto-connects on every visit after that.

---

## History policy

- Calculations are kept for **30 days** then deleted automatically
- On connect, records older than 30 days are purged from Supabase
- On load, expired entries are pruned from localStorage

---

## Stack

- Pure HTML / CSS / JS — zero dependencies, zero build step
- Supabase (optional) — Postgres + REST API for cloud history
- Netlify — static hosting

---

## License

MIT
