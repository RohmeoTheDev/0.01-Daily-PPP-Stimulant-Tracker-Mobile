# Ro's Daily Tracker (PPP Tracker)

Personal productivity tracker for daily habits and activities with cloud sync.

## Current Version: v1.2

**Status:** Stable  
**Deployed:** Netlify (auto-deploy from main)  
**Live URL:** [Check Netlify dashboard]

## Features
- ☀️ Wake-up time tracking (early wake detection 4:30-5:30 AM)
- 📊 10 activity categories with time logging
- ☕ Stimulant tracking (coffee, caffeine gum)
- 💧 Hydration tracking (water in liters)
- ✅ Morning/night checklists
- 📈 Monthly performance chart (stacked bar)
- 📅 Calendar view with color coding
- 📥 CSV export/import
- ☁️ **Cloud sync via Supabase** (v1.1+)

## Tech Stack
- React 18 (via CDN)
- Babel standalone (JSX transform)
- Supabase (PostgreSQL cloud DB)
- IndexedDB (local backup)
- Single HTML file deployment

## Data Storage
1. **Primary:** Supabase cloud (`daily_entries` table)
2. **Backup:** IndexedDB (`RoTrackerDB`)
3. **Fallback:** localStorage

## Supabase Config
- **Project:** `naqlytoimdqpivyiplcp`
- **Table:** `daily_entries`
- **RLS:** Disabled (personal app)

## Quick Start
```bash
# Clone
git clone https://github.com/RohmeoTheDev/0.01---Daily-PPP-Tracker-Mobile-Desktop-.git

# Open in browser
open index.html

# Or deploy to Netlify
```

## File Structure
```
├── index.html          # Main app (single file)
├── README.md           # This file
├── docs/
│   ├── CHANGELOG.md    # Version history
│   └── design-reference.png
├── archive/            # Old versions
└── Versions/           # Version snapshots
```

## For AI Agents
See `docs/CHANGELOG.md` for detailed version history with commit hashes.

Key files:
- `index.html` - All React code, styles, and logic in one file
- Line 32-34: Supabase config
- Line ~145-190: Data load/save hooks
- Line ~265: resetDay function

## Deployment
Auto-deploys to Netlify on push to `main` branch.
