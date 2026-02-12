# Project Extraction Report

## 1. Repository Structure

### 1.1 Folder Tree

```
├── index.html              # The entire app — all HTML, CSS, JSX, and logic in one file
├── README.md               # Project overview and quick-start docs
├── docs/
│   ├── CHANGELOG.md        # Version history with commit hashes
│   └── design-reference.png# A UI reference image (unrelated — shows a signup form mockup)
├── archive/                # Old version snapshots (v0.2 through v0.40), all single .html files
├── Screenshot 1.png        # Live app screenshot — top half (header, activities grid)
├── Screenshot 2.png        # Live app screenshot — bottom half (chart, calendar, physical tracker)
└── .gitignore
```

**Key takeaway:** The entire application is a single `index.html` file. There is no build step, no `node_modules`, no bundler, no separate CSS or JS files.

### 1.2 Entry Points

| Concern | File / Mechanism |
|---|---|
| **HTML entry** | `index.html` (line 1) |
| **JS/TS entry** | `<script type="text/babel">` block inside `index.html` (line 30) |
| **App root component** | `ProductivityTracker` component (line 574), rendered via `ReactDOM.createRoot` (line 989) |
| **Build tooling** | **None.** Babel Standalone transpiles JSX in the browser at runtime. React 18, ReactDOM 18, Babel, and Supabase JS v2 are all loaded from CDN. |
| **Deployment** | Netlify auto-deploy from `main` branch — serves `index.html` as a static file. |

---

## 2. Screen & Navigation Map

### 2.1 Screen / Route Inventory

**This is a single-page, single-view app with no routing.** There is one continuous vertically-scrollable page. The user interacts with sections in order:

| Section | Purpose |
|---|---|
| **Header** | App title "Ro's Daily Tracker" + tagline |
| **Date Navigation** | Arrow buttons, date picker, "Today" shortcut — selects which day to view/edit |
| **Wake Up Time** | Record wake-up time (manual time input or "Now" button) |
| **Activity Cards Grid** | 10 categories in a 2-column layout — tap +15/+20/+30/+45/+60/+100/+10 or subtract |
| **Stimulants & Hydration** | Coffee (count), Caffeine Gum (count), Water (litres) with +/- controls |
| **Monthly Performance Chart** | Stacked bar chart for every day of the current month, with category legend |
| **Calendar View** | Traditional 7-column calendar grid with color-coded cells (early/late wake, activity dots) |
| **Physical Activity Tracker** | Compact 2-column day-by-day grid showing walking/cardio/gym/stretching dots + caffeine/water |
| **Daily Checklists** | Morning (Water, Gratitude, Coffee) and Night (Stretch, Endep, Melatonin, Zinc & Magnesium) toggles |
| **Data** | Reset Day, Export CSV, Import CSV buttons |
| **Footer** | Motivational text |

### 2.2 Navigation Model

- **No router, no tabs, no sidebar, no stack navigation.**
- Single scrollable page. All sections are always visible.
- The **date navigator** is the primary navigation mechanism — it selects which day's data is displayed/edited across all sections.
- Clicking a day in the chart or calendar also changes the selected date.
- Layout is **fixed** (no dynamic showing/hiding of sections).

---

## 3. Visual System & Layout DNA

### 3.1 Visual Identity

| Aspect | Detail |
|---|---|
| **Background** | `linear-gradient(135deg, #ffffff 0%, #f8f9fa 100%)` — near-white with subtle warm-gray gradient |
| **Primary accent** | `#4A90A4` — muted teal/blue, used for selected states, "Today" button, wake-up time display |
| **Text colors** | `#1a1a1a` (body), `#111827` (headings), `#374151` (body text), `#6b7280` (section titles/muted), `#9ca3af` (tertiary/hints) |
| **Category colors** | Warm-to-cool spectrum: reds/browns (Admin, Self Care, Meditation) → ambers (Audiobook, Focus) → blue-grays (Business, Walking, Cardio, Gym, Stretching) |
| **Success/Error** | Green `#10B981`/`#DCFCE7` (checked/early wake), Red `#DC2626`/`#FEF2F2` (late wake/reset confirm) |
| **Caffeine accent** | Purple `#8B5CF6` |
| **Water accent** | Sky blue `#0EA5E9` |
| **Typography** | Poppins (Google Fonts, header only, 700 weight). System font stack for everything else: `-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif`. Weights: 400 (body), 500 (medium), 600 (semi-bold, section titles), 700 (bold, headings/values). |
| **Font sizes** | Very small: 7-9px (chart labels, legend). Small: 10-11px (buttons, section titles). Body: 12-14px. Header: 28px. |
| **Spacing rhythm** | Card-based layout with consistent `16px` padding, `16px` card gap (`marginBottom`). Internal gaps are `6-12px`. |
| **Border radius** | Cards: `16px`. Buttons: `8px`. Activity cards: `12px`. Calendar cells: `6px`. Header bottom: `24px`. Pill badges: `10px`. |
| **Shadows** | Very subtle: `0 4px 6px -1px rgba(0,0,0,0.05)` on cards. Header has deeper: `0 8px 32px rgba(0,0,0,0.06)`. |
| **Glassmorphism** | Cards use `rgba(255,255,255,0.9)` + `backdropFilter: blur(10px)`. Header uses `blur(20px)`. |
| **Mode** | Light mode only. No dark mode. |

### 3.2 Layout Patterns

**Cards** — The dominant structural unit. Every section is wrapped in `styles.card`:
- White background with glass effect
- 1px subtle border (`rgba(229,231,235,0.5)`)
- 16px border-radius, 16px padding
- Soft shadow
- 16px bottom margin

**Activity Cards** — A specialized card variant (`styles.activityCard`):
- Slightly more opaque (`0.95` alpha)
- 12px border-radius, 12px padding
- Contains: color dot + label header, then 2 rows of 4 buttons each

**Section Titles** — `styles.sectionTitle`:
- 11px, uppercase, letter-spacing `0.5px`
- Color: `#6b7280`
- Often paired with an icon/color dot via flexbox

**Two-Column Grid** — Used for activity cards (left 5, right 5) and physical tracker (days 1-16, days 17-31)

**List Rows** — Stimulant items are horizontal flex rows: icon + label block | +/- buttons | value. Separated by `borderTop` dividers.

**Checklist Rows** — Checkbox + label, separated by bottom border, cursor pointer for full-row tap.

**Header** — Full-width (negative margins to break out of `#app` padding), bottom-rounded, centered text.

**Calendar Grid** — 7-column CSS grid, each cell is a fixed `52px` height card with date, wake time, caffeine/water indicators, and activity dots.

### 3.3 Interaction Patterns

**Primary user loop:**
1. Open app → see today's date already selected
2. Record wake-up time
3. Throughout the day, tap activity buttons (+15, +30, etc.) to log minutes
4. Track coffee/gum/water intake
5. Check off morning/nightly items
6. Glance at chart/calendar for monthly overview

**Input density:** Moderate-high. 10 activity categories with 8 buttons each (80 buttons visible). But individual interactions are single-tap — no text fields for activities.

**Save behavior:** **Auto-save.** Every state change to `history` triggers the `useEffect` on line 212 which persists to Supabase + IndexedDB. No "Save" button anywhere.

**Feedback style:** Minimal/subtle. Values update inline immediately (optimistic UI). Import status shows a temporary colored banner (success green / error red) that auto-dismisses after 3 seconds. Reset day has a confirmation step (inline, not a modal). No toasts, no modals, no loading spinners.

---

## 4. Data Architecture

### 4.1 Data Models

**Single entity: Day Entry**

| Field | Type | Description |
|---|---|---|
| `wakeUpTime` | `string \| null` | Formatted as "5:30 AM" |
| `admin` | `number` | Minutes logged |
| `focus` | `number` | Minutes logged |
| `selfCare` | `number` | Minutes logged |
| `audiobook` | `number` | Minutes logged |
| `business` | `number` | Minutes logged |
| `cardio` | `number` | Minutes logged |
| `gym` | `number` | Minutes logged |
| `stretching` | `number` | Minutes logged |
| `meditation` | `number` | Minutes logged |
| `walking` | `number` | Minutes logged |
| `coffee` | `number` | Count (units) |
| `caffeineGum` | `number` | Count (units) |
| `water` | `number` | Litres (decimal) |
| `dailyChecks` | `object` | `{ daily1: bool, daily2: bool, daily3: bool, daily4: bool, daily5: bool }` |
| `nightlyChecks` | `object` | `{ nightly1: bool, nightly2: bool, nightly3: bool, nightly4: bool, nightly5: bool }` |

**Relationships:** None. Each day is independent. The `history` object is a flat `Record<dateString, DayEntry>` keyed by `YYYY-MM-DD`.

**Supabase table:** `daily_entries` with columns `id` (date string), `date`, `data` (JSONB containing the day entry), `updated_at`.

### 4.2 State Handling

- **All state lives in the `useTrackerData` custom hook** (line 133-500).
- Primary state: `history` — a single `useState` object holding all days' data.
- UI state: `selectedDate`, `showResetConfirm`, `importStatus`, `pendingWakeTime` — all local `useState`.
- **No global store, no context, no Redux.** The hook returns `{ state, actions, computed }` and the single `ProductivityTracker` component destructures everything.
- This is a **local-first app with cloud sync**. Data loads from Supabase on mount, falls back to IndexedDB/localStorage if offline.

---

## 5. Persistence & Sync Model

**Three-tier persistence (in priority order):**

| Tier | Storage | Role |
|---|---|---|
| **Primary** | Supabase PostgreSQL (`daily_entries` table) | Source of truth, cloud sync, cross-device |
| **Secondary** | IndexedDB (`RoTrackerDB` database, `history` object store) | Offline cache/backup |
| **Tertiary** | localStorage (`roTrackerHistory` key) | Fallback if IndexedDB fails |

**Load flow:** Supabase first → if fail, IndexedDB → if fail, localStorage.
**Save flow:** Every `history` state change saves to Supabase (upsert per date) AND IndexedDB. localStorage used as IndexedDB fallback.
**Delete sync:** Not working (known issue in CHANGELOG). Reset only deletes from Supabase + local state + IndexedDB cache.

**Sync details:**
- **Sync direction:** Unidirectional. App loads full dataset from Supabase on mount, then writes back on every change.
- **Conflict resolution:** Last-write-wins (upsert). No merging strategy.
- **Granularity:** Saves ALL history entries on every change (not just the changed date).
- **Offline support:** Functional via IndexedDB cache, but changes made offline won't sync back to Supabase until reconnect.
- **No real-time subscription** — no Supabase realtime channels. Data is only fetched on page load.

---

## 6. Reusable Components

### Purely Presentational / Reusable

| Component | Lines | Description | Dependencies |
|---|---|---|---|
| `ActivityCard` | 505-539 | Generic time-tracking card with colored header + 8 increment/decrement buttons | Receives `activity` (label, color, key), `value`, `onAdd`, `onSubtract` props. Uses `styles.activityCard` and `styles.btn`. |
| `ChecklistItem` | 542-571 | Checkbox row with label, supports morning/night color variants | Receives `item` (label), `checked`, `onToggle`, `variant`. Fully independent. |

### Reusable Style System

| Token | Value | Usage |
|---|---|---|
| `styles.card` | Glass card container | Wraps every section |
| `styles.btn` | Neutral bordered button | All interactive buttons |
| `styles.sectionTitle` | Uppercase label | Section headers inside cards |
| `styles.activityCard` | Card variant | Activity tracking boxes |

### Reusable Constants

| Constant | Description |
|---|---|
| `CATEGORIES` | Array of 10 `{ key, label, color }` objects — defines all tracked activities |
| `DAILY_ITEMS` | Array of 3 morning checklist items |
| `NIGHTLY_ITEMS` | Array of 4 night checklist items |
| `EMPTY_DAY` | Default shape for a day entry with all zeros/nulls |

### Reusable Utility Functions

| Function | Description |
|---|---|
| `getTodayKey()` | Returns `YYYY-MM-DD` string for today |
| `getTotal(data)` | Sums all category minutes for a day |
| `getCaffeine(data)` | Calculates total caffeine mg |
| `isEarlyWakeCheck(wakeUpTime)` | Checks if wake time is 4:30-5:30 AM |
| `getCurrentMonthDays()` | Returns array of day objects for current month |
| `getCalendarWeeks()` | Returns 2D array of weeks for calendar grid |

---

## 7. Structural Coupling Observations

### Tightly Coupled to Multi-Feature Tracking

- **`EMPTY_DAY` constant** (line 46-50): Hard-codes all 10 activity keys + coffee + caffeineGum + water + both checklist objects. Every action function references this shape.
- **`CATEGORIES` array** (line 52-63): The chart, calendar, physical tracker, and activity grid all iterate over this. Changing categories requires updating `EMPTY_DAY`, the CSV export column order, and the CSV import parser.
- **CSV export/import** (lines 386-450): Column positions are hard-coded to match the exact field order. Not dynamic.
- **The `useTrackerData` hook** (lines 133-500): Contains ALL state and ALL actions for the entire app. Every feature's logic lives in this one hook — wake time, activities, stimulants, water, checklists, date navigation, CSV, reset.
- **`ProductivityTracker` component** (lines 574-986): A single 400+ line component rendering every section. No section is extracted into its own component (except `ActivityCard` and `ChecklistItem`).

### Modular / Loosely Coupled

- **`ActivityCard`** — fully generic. Works for any `{ key, label, color }` activity with any minute-based value.
- **`ChecklistItem`** — fully generic. Works for any `{ key, label }` item with boolean state.
- **Style tokens** (`styles.card`, `styles.btn`, etc.) — independent of data models.
- **Utility functions** (`getTodayKey`, `getTotal`, `getCurrentMonthDays`, `getCalendarWeeks`) — pure functions with no side effects.
- **Supabase config** — isolated at the top (lines 32-34). Easy to swap or remove.

### Data Model Dependency Map

- **Chart section** → depends on `CATEGORIES`, `history`, `getTotal`, `getCurrentMonthDays`
- **Calendar section** → depends on `history`, `getCalendarWeeks`, `isEarlyWakeCheck`, `getCaffeine`, and specific field names (`gym`, `cardio`, `stretching`, `walking`, `water`, `wakeUpTime`)
- **Physical Activity Tracker** → depends on `history`, `getCurrentMonthDays`, `isEarlyWakeCheck`, and specific field names (`stretching`, `cardio`, `gym`, `walking`, `coffee`, `caffeineGum`, `water`)
- **Stimulants section** → hard-codes `coffee` (90mg) and `caffeineGum` (30mg) inline
- **Checklists** → depends on `DAILY_ITEMS`, `NIGHTLY_ITEMS`, and the `dailyChecks`/`nightlyChecks` sub-objects

### Where Simplification Would Be Structurally Easiest

- **ActivityCard grid** — already abstracted. Reducing categories only requires shortening the `CATEGORIES` array.
- **Checklists** — already abstracted via `ChecklistItem`. Reducing items only requires shortening `DAILY_ITEMS` / `NIGHTLY_ITEMS`.
- **Stimulants/Hydration** — self-contained section, could be removed without affecting other sections (except calendar/physical tracker which show caffeine/water indicators).
- **Physical Activity Tracker** — an additive read-only visualization. Removing it has zero impact on data flow.
- **Chart** — additive read-only visualization. Removing it has zero impact on data flow.
- **Calendar** — additive read-only visualization with date-selection side effect. Could be removed if date nav arrows remain.
