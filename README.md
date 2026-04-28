# ⛵ NullZero — Guild Captain Tracker

A web app for **CapybaraGo** guilds to track daily captain rotation.

---

## 🌊 Live Site

**[nakfrg.com](https://nakfrg.com)**

---

## ✨ Features

### For everyone (no login required)
- See the **Captain of the Day** displayed prominently
- Browse the **Remaining** list — who is still in the rotation this cycle
- Browse the **Completed** list — who has already had a turn, and whether they were confirmed or skipped
- All data is **live and shared** — the page updates in real time for every visitor

### For admins (password protected, stays logged in for 15 days)
- **Pick Captain** — randomly selects someone from the remaining pool
- **Cancel** — undoes the pick and restores the previous captain of the day
- **Confirm Captain** — locks in the selected person as captain of the day
- **Skip · Reshuffle Back In** — returns the person to the remaining pool at a random position
- **Skip · Remove from Cycle** — moves the person to completed, marked as Skipped
- **⚓ Set as Captain** (from the Remaining list) — manually assign any remaining member as captain; bumps any current pending pick back into the pool
- **✓ Done** (from the Remaining list) — mark someone as completed without going through the pick flow
- **↩ Restore** (from the Completed list) — send someone back into the remaining pool
- **💀 Skull tracking** — log mistakes per member, up to 3 skulls each; click any skull to remove it; works from the Remaining, Completed, and Manage Crew tabs
- **Manage Crew** tab — add or remove guild members, reset the cycle, or clear all data
- **Change password** — update the admin password from within the app

### Automatic cycle reset
When the last remaining member has been picked, the app automatically starts a new cycle and reshuffles everyone back in.

---

## 🗄️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Plain HTML, CSS, JavaScript (single file) |
| Database | [Supabase](https://supabase.com) (PostgreSQL, free tier) |
| Hosting | GitHub Pages |
| Fonts | Pacifico, Righteous, Nunito (Google Fonts) |
| Realtime | Supabase Postgres Changes (live sync across all visitors) |

---

## 🚀 Setup Guide

### 1. Supabase database

1. Go to [supabase.com](https://supabase.com) and create a free account
2. Create a new project
3. Go to the **SQL Editor** and run the following:

```sql
CREATE TABLE guild_state (
  id TEXT PRIMARY KEY DEFAULT 'main',
  data JSONB NOT NULL
);

INSERT INTO guild_state (id, data) VALUES ('main', '{
  "team": [],
  "remaining": [],
  "done": [],
  "current": null,
  "adminPw": "captain123"
}'::jsonb);

ALTER TABLE guild_state ENABLE ROW LEVEL SECURITY;

CREATE POLICY "public read"
  ON guild_state FOR SELECT USING (true);

CREATE POLICY "public write"
  ON guild_state FOR UPDATE USING (true);
```

4. Go to **Project Settings → API** and copy your **Project URL** and **anon public** key

### 2. Configure the HTML file

Open `index.html` and update these two lines near the bottom of the `<script>` section:

```js
const SUPABASE_URL = 'https://your-project.supabase.co';
const SUPABASE_KEY = 'your-anon-key';
```

### 3. Deploy to GitHub Pages

1. Create a new **public** GitHub repository
2. Upload `index.html` — rename it to `index.html` if it isn't already
3. Go to **Settings → Pages**, set the source branch to `main`, save
4. Your site will be live at `https://yourusername.github.io/your-repo`

---

## 🔄 How the Rotation Works

1. All guild members start in the **Remaining** pool, shuffled randomly
2. Each day, an admin picks a captain — either randomly or by manually selecting someone
3. Once confirmed, that captain is displayed to everyone until the next pick
4. Confirmed captains move to **Completed** (marked Done); skipped members can be marked Done or reshuffled back in
5. When everyone has been through the cycle, it automatically resets and reshuffles the whole team

---

## 💀 Skull System

Skulls are used to log mistakes or incidents per crew member:
- Each member can have **0–3 skulls**
- Click the **💀 button** on any row to add a skull
- Click an **existing skull** on a member's name to remove it (in case of error)
- Skulls are visible to all visitors and persist across cycles
- Skulls can be managed from the Remaining, Completed, and Manage Crew tabs

---

*Built for the NullZero guild · CapybaraGo*
