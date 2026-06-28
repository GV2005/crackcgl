# CrackCGL — SSC CGL 2025–26 Mock Test Platform
**Live on GitHub Pages. Cost: ₹0. Deploy time: 5 minutes.**

---

## ⚡ Deploy in 5 Minutes

### Step 1 — Create GitHub Repo
```bash
git init
git add .
git commit -m "CrackCGL launch"
```
Go to github.com → New repository → Name: `crackcgl` → Public → Create

```bash
git remote add origin https://github.com/YOUR_USERNAME/crackcgl.git
git branch -M main
git push -u origin main
```

### Step 2 — Enable GitHub Pages
GitHub repo → **Settings** → **Pages** → Source: `Deploy from branch` → Branch: `main` → Folder: `/ (root)` → **Save**

Your site is live at: `https://YOUR_USERNAME.github.io/crackcgl`

**That's it. Done. Live.**

---

## 🗄️ Connect Supabase (Real Data — Still Free)

### Step 1 — Create Supabase Project
1. https://supabase.com → Sign up → New Project
2. Name: `crackcgl`, Region: **Southeast Asia (Singapore)**
3. Note down your database password

### Step 2 — Run SQL Schema
Supabase Dashboard → **SQL Editor** → New Query → Paste this:

```sql
-- Simple schema for GitHub Pages static site
-- No backend needed — Supabase JS SDK calls DB directly

CREATE TABLE IF NOT EXISTS test_attempts (
  id           UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id      UUID REFERENCES auth.users(id) ON DELETE CASCADE,
  test_name    TEXT NOT NULL,
  score        FLOAT,
  total_marks  INTEGER,
  correct      INTEGER DEFAULT 0,
  wrong        INTEGER DEFAULT 0,
  unattempted  INTEGER DEFAULT 0,
  accuracy     FLOAT,
  section_scores JSONB,
  status       TEXT DEFAULT 'completed',
  finished_at  TIMESTAMPTZ DEFAULT NOW()
);

-- RLS: users can only see their own attempts
ALTER TABLE test_attempts ENABLE ROW LEVEL SECURITY;
CREATE POLICY "own_attempts" ON test_attempts USING (user_id = auth.uid());

-- Leaderboard view (public read)
CREATE OR REPLACE VIEW public_leaderboard AS
SELECT
  u.raw_user_meta_data->>'name' AS name,
  MAX(ta.score) AS best_score,
  COUNT(ta.id) AS tests_taken
FROM test_attempts ta
JOIN auth.users u ON ta.user_id = u.id
WHERE ta.finished_at >= NOW() - INTERVAL '30 days'
GROUP BY u.id, u.raw_user_meta_data->>'name'
ORDER BY best_score DESC
LIMIT 50;
```
Click **Run**.

### Step 3 — Get Your Keys
Settings → API:
- Copy **Project URL**
- Copy **anon/public** key (safe to put in frontend)

### Step 4 — Update index.html
Open `index.html`, find these two lines near the top of the `<script>`:
```javascript
const SUPABASE_URL  = 'YOUR_SUPABASE_URL';
const SUPABASE_KEY  = 'YOUR_SUPABASE_ANON_KEY';
```
Replace with your actual values. Push to GitHub — site auto-updates.

### Step 5 — Enable Google OAuth (Optional)
Supabase → Authentication → Providers → Google → Enable
- Get credentials from: console.cloud.google.com
- Add redirect URL: `https://YOUR_USERNAME.github.io/crackcgl`

---

## 📁 File Structure
```
crackcgl/
├── index.html          ← Entire app (single file)
├── README.md           ← This file
└── .gitignore
```

## ✅ What Works Without Supabase
- All 6 test types with timer
- Question bank (40 questions across 4 subjects)
- Full answer review with explanations
- Score calculation with negative marking
- Dashboard with localStorage persistence (survives tab close)
- Keyboard shortcuts (A/B/C/D or 1/2/3/4 to pick options)

## ✅ What Works After Supabase Setup
- User registration & login (email + Google)
- Scores saved to cloud (accessible on any device)
- Real leaderboard
- History across sessions

---

## 🚀 Growth Plan (Month by Month)

| Month | Goal | Action |
|-------|------|--------|
| 1 | 100 users | Share in SSC Telegram groups |
| 2 | 500 users | Daily question posts on Instagram |
| 3 | 1,000 users | Add more questions, PYQs |
| 4 | Validate monetization | Survey users — what would they pay for? |
| 5+ | Launch Pro plan | ₹99/month for unlimited + analytics |

## 💰 AWS $100 Credit — Save For Later
Use AWS when you need:
- Custom domain email (SES — ~$0/month for small volumes)
- PDF report generation (Lambda function)
- Bulk question import processing
- After 2,000+ users when Supabase free tier limits approach

**Not before then.** GitHub Pages + Supabase handles 10,000 users/month for ₹0.

---

## 🔑 Keyboard Shortcuts (Exam Screen)
| Key | Action |
|-----|--------|
| A / 1 | Select option A |
| B / 2 | Select option B |
| C / 3 | Select option C |
| D / 4 | Select option D |
| → Arrow | Save & Next |

---

*Built for SSC CGL 2025–26. Income Tax Inspector (CBDT) — you've got this.*
