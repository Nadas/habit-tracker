# Personal Habit Tracker — Product Requirements Document

**Owner:** Nada
**Purpose:** A minimal, personal, single-user web application to track daily discipline and recurring routines, without pressure, scoring, or clutter.
**Status:** Draft v3

---

## Version History

### v3 (August 12, 2026)
**Bug fixes & UI improvements:**
- ✅ Fixed drag-and-drop reordering on Manage Habits page — was missing `dragover` and `drop` event listeners; now fully functional with visual feedback
- ✅ Replaced emoji icons (👤 and ⚙) in home page header with clean SVG icons (person silhouette + sliders)
- ✅ Fixed wake-up time field showing "true" — corrected default value to `00:00`
- ✅ Future days now default all habits to "no" and wake-up time to `00:00` — past and today's data unaffected
- ✅ Clicking the "Discipline" title in the header returns to today's daily view
- ✅ Removed "Show archived habits" checkbox — archived habits are always visible at the bottom of Manage Habits
- ✅ Stats weight chart: removed misplaced "kg" axis label; day numbers now clearly visible along bottom axis
- ✅ Profile page: replaced emoji avatar (👤) with SVG icon; merged "Edit Profile" and "Change Password" into a single "Edit Profile" card
- ✅ Created new `habit_tracker_edit_profile.html` — combined page for editing name, email, and password in one place

**Backend planning:**
- ✅ Decided on Supabase as the backend (auth + database + API, no custom server needed)
- ✅ Defined database schema: `habits` table and `completions` table with row-level security
- ✅ Supabase integration pending credentials from project setup

**Files Created/Updated:**
- `habit_tracker_mockup.html` — SVG icons, future-day defaults, wake-up fix, "Discipline" tap-to-today, chart improvements
- `habit_tracker_manage_habits_mockup.html` — drag-and-drop fixed, archived always shown
- `habit_tracker_profile.html` — SVG avatar, single Edit Profile card
- `habit_tracker_edit_profile.html` — NEW: combined edit profile + change password page

**Navigation Flow (updated):**
- Login → Main Tracker
- Main Tracker header: Profile icon | Discipline (tap = go to today) | Settings icon (Manage Habits)
- Profile → Edit Profile (name, email, password all on one page)
- Stats tab: full-month weight trends with line chart and day labels

---

### v2 (August 12, 2026)
**Changes:**
- ✅ Added Weekly View — 7-day grid showing week's progress with date range navigation
- ✅ Redesigned Daily View layout — Reorganized into Morning, Evening, Prayer sections
- ✅ Horseback riding moved to Evening section (only shows Sun/Tue/Thu)
- ✅ Cat weekly tasks moved to bottom "Weekly" section (appears Saturday only)
- ✅ Added dynamic date navigation — Context-aware, changes by tab (daily/weekly/monthly)
- ✅ Created unified Habit Form page — Single form handles both Add and Edit modes via URL parameters (mode=add/edit)
- ✅ Created Manage Habits page — Full CRUD interface with archive/delete, drag-to-reorder, organized by frequency
- ✅ Full navigation integration — All pages connected seamlessly
- ✅ Added curved arrow (↻) for archive/restore action — Cleaner, minimal aesthetic
- ✅ Completed color system documentation — Added dedicated Design & Color System section
- ✅ Created Login page — Username/password auth with demo mode and "remember me" option
- ✅ Created Stats page — Monthly weight trend chart with line graph, X/Y axes, weight loss tracking
- ✅ Created Profile page — User info, quick stats, preferences (reminders toggle), logout
- ✅ Reorganized Main Tracker header — Profile icon | Discipline (center) | Settings icon
- ✅ Stats tab positioned far right — Visual separation from main tracking tabs
- ✅ Month navigation on Stats view — Navigate between months to see historical trends

**Files Created/Updated:**
- `habit_tracker_login.html` — Login with demo mode
- `habit_tracker_mockup.html` — Main tracker with 5 tabs (Today, Prayers, Week, Month, Stats)
- `habit_tracker_manage_habits_mockup.html` — Habit management interface
- `habit_tracker_habit_form.html` — Unified add/edit form
- `habit_tracker_stats.html` — Standalone stats page
- `habit_tracker_profile.html` — User profile & settings

### v1 (Initial Concept)
**Status:** Discovery & requirements gathering
- Defined vision and design philosophy (calm minimalism)
- Established color palette and typography guidelines
- Listed seed data habits (daily, weekly, monthly, quarterly)
- Outlined core views (Daily, Prayer, Monthly)
- Defined prayer tracker as separate tab
- Documented interaction patterns (yes/no buttons, number inputs)

---

## 1. Vision & Design Philosophy

This is a personal accountability tool, not a gamified productivity app. The core principle is **calm minimalism**: no emojis, no bright colors, no large text, no "X out of Y" counters, no judgmental red states. The app should feel like a quiet ritual — something that invites daily return rather than pressures it.

**Visual direction:** Dark, muted theme inspired by Claude's own color palette — warm neutral darks, soft off-white text, a single gentle accent color used sparingly. Soft-edged, rounded typography. Generous white space. Gentle transitions, nothing sharp or jarring.

**Platform:** Mobile-first responsive web app, deployed online, accessible from Nada's phone. Single user only (no multi-user support needed).

---

## 2. Design & Color System

### Color Palette (Hex Codes)
- **Background (true black)**: `#0a0a0a`
- **Primary text (off-white)**: `#f5f1ed`
- **Accent color (coral)**: `#d97757`
- **Secondary borders (dark brown)**: `#2a2925`
- **Card/section background**: `#141411`
- **Muted text (for labels, hints)**: `#8a8985`
- **Disabled/muted state**: `#7a7570`

### Typography & Layout
- **Font family**: Soft-edged, rounded sans-serif (e.g., system fonts: `-apple-system, BlinkMacSystemFont, "Segoe UI"`)
- **Text size hierarchy**: Minimal; most text 9–11px, headers uppercase and small
- **Spacing**: Generous white space, 6–14px padding/gaps
- **Border radius**: Soft, subtle (4–6px on elements)
- **Transitions**: Gentle 0.2s ease on interactive elements

### Visual Principles
- No emojis in UI — SVG icons only
- Soft color marks instead of harsh heatmaps
- Calm, ritual-like feel — minimal motion
- Mobile-first, responsive design
- Dark theme only (always enabled)

---

## 3. Authentication

- Email/password login via **Supabase Auth**
- **Persistent session** — no repeated logins required on the same device once authenticated
- Change password available from Edit Profile page
- Single-user system; no roles or permissions needed beyond row-level security

---

## 4. Backend & Data Layer

**Stack:** Supabase (hosted Postgres + Auth + REST API — no custom server)

### Database Tables

**`habits`**
| Column | Type | Notes |
|---|---|---|
| id | uuid | Primary key |
| user_id | uuid | References auth.users |
| name | text | Habit display name |
| frequency | text | 'daily', 'weekly', 'monthly', 'quarterly' |
| type | text | 'yes/no' or 'number' |
| unit | text | e.g. 'kg', 'am' (optional) |
| days | int[] | For weekly habits, e.g. [0, 2, 4] |
| day | int | For monthly habits (day of month) |
| sort_order | int | User-defined drag-and-drop order |
| archived | boolean | Soft delete / pause |
| created_at | timestamp | Auto-set |

**`completions`**
| Column | Type | Notes |
|---|---|---|
| id | uuid | Primary key |
| user_id | uuid | References auth.users |
| habit_id | uuid | References habits |
| date | date | The day this completion is for |
| value | text | 'true', 'false', or numeric string e.g. '65.2' |

Row-level security is enabled on both tables — users can only read/write their own data.

---

## 5. Habit Model

Habits are organized into four frequency types:

- **Daily**
- **Weekly**
- **Monthly**
- **Quarterly**

*(No yearly frequency needed.)*

### 5.1 Habit Management (Self-Service)
The user must be able to, directly in the app, without developer involvement:
- Add new habits, assigning name and frequency
- Edit existing habits
- **Delete** a habit permanently
- **Archive/pause** a habit (removes it from active view but preserves historical data)
- **Reorder habits** via drag-and-drop within the same frequency group

### 5.2 Editable History
- Past entries can be edited/backfilled — e.g., checking off yesterday's habit if forgotten at the time.

### 5.3 Future Days
- All habits default to "no" / unchecked on any day after today
- Wake-up time defaults to `00:00` on future days

---

## 6. Initial Habit List (Seed Data)

### Daily (appear every day)
- **Morning skincare routine** — yes/no
- **Wake-up time log** — text input (user enters time, default `00:00`)
- **Daily weight log** — numeric input (kg)
- **Lemon-ginger-turmeric drink** — yes/no
- **Evening skincare routine** — yes/no
- **Evening vitamins** — yes/no
- **Prayer tracker** (rolls up into main view as read-only line item, controlled from Prayer tab)

### Recurring (appear on specific days)
- **Horseback riding lessons** — Sunday, Tuesday, Thursday; appears in Evening section (yes/no)

### Weekly (appear only on Saturday)
- **Cat litter change** — yes/no
- **Cat water fountain cleanup** — yes/no
- **Cat grooming (in-house session)** — yes/no
- **Pill dispenser refill** — yes/no

### Other Weekly (day flexible)
- Weight loss medication — yes/no
- House cleaning — yes/no
- Hair mask — yes/no

### Monthly
- Massage
- Manicure & pedicure
- Moroccan bath

### Quarterly
- Cat medication & vaccinations

---

## 7. Prayer Tracker (Special Feature)

- Separate, tucked-away tab — not shown prominently on the main daily view (private by design).
- Contains individual checkboxes for the five daily prayers: **Fajr, Dhuhr, Asr, Maghrib, Isha**.
- If all five are checked, the day is "done"; otherwise "not done."
- This single done/not-done status **rolls up as one quiet line item** on the main daily habit list — no prayer-level detail shown there.
- Future days show all prayers as incomplete by default.

---

## 8. Views

### 8.1 Daily View (Home Screen)
- **Date navigation** at top with < > arrows to move between days; shows "Today" or day name + date
- Tapping **"Discipline"** in the header always returns to today's view
- **Daily habits** organized into sections:
  - **Morning**: skincare, wake-up time (default `00:00`), daily weight, turmeric drink
  - **Evening**: skincare, vitamins, horseback riding (Sun/Tue/Thu only)
  - **Prayer**: read-only status indicator (Complete / Incomplete)
- **Weekly section at bottom** (Saturday only): cat litter, water fountain, cat grooming, pill dispenser
- Yes/No buttons (✕ for no, ✓ for yes, coral when active)
- Future days: all habits default to unchecked / `00:00`

### 8.2 Prayer Tab
- Separate tab with all five daily prayers and individual toggle buttons
- Status indicator at top: "Complete" or "Incomplete"
- Prayer status rolls up to Daily view as read-only indicator

### 8.3 Weekly View
- **Week navigation** (±7 days), shows week date range
- **7-day grid**: days down left side (Sun–Sat), habits across top with vertical labels
- Coral mark for done, muted for not done

### 8.4 Monthly Matrix View
- **Month navigation** (< > arrows), shows month and year
- Full month grid: days down left side, habits across top with vertical labels
- Gentle density map, no harsh heatmap or numeric indicators

### 8.5 Stats View
- Monthly weight trend as a line chart
- X-axis: day numbers (1, 6, 11, 16, 21, 26, 30)
- Y-axis: weight values (no axis label — values self-explanatory)
- Summary cards: weight lost, current weight vs goal
- Month navigation to view historical trends

---

## 9. Pages & Navigation

| Page | File | Description |
|---|---|---|
| Login | `habit_tracker_login.html` | Email/password auth |
| Main Tracker | `habit_tracker_mockup.html` | 5-tab home screen |
| Manage Habits | `habit_tracker_manage_habits_mockup.html` | CRUD, reorder, archive |
| Habit Form | `habit_tracker_habit_form.html` | Add/edit habit (mode=add/edit) |
| Profile | `habit_tracker_profile.html` | Stats, preferences, logout |
| Edit Profile | `habit_tracker_edit_profile.html` | Name, email, password |

**Navigation flow:**
- Login → Main Tracker
- Main Tracker → Profile (person icon, top left)
- Main Tracker → Manage Habits (sliders icon, top right)
- Profile → Edit Profile
- Edit Profile → back to Profile
- Manage Habits → Habit Form (edit) or + Add button

---

## 10. Reminders / Notifications

- Optional reminders, controlled by a **single global on/off switch** in Profile → Preferences
- Off by default

---

## 11. Interaction Patterns

### Navigation & Tabs
- Five main tabs: Today, Prayers, Week, Month, Stats
- Stats tab pushed to the far right (visual separation)
- Tapping "Discipline" title always returns to today's Daily view

### Yes/No Buttons
- ✕ (no, gray) and ✓ (yes, coral highlight) per habit
- Click to toggle; coral fills when yes, gray when no
- Prayer status in Daily view is read-only — toggle from Prayer tab only

### Number/Text Inputs
- Wake-up time: text input, default `00:00`
- Daily weight: numeric input in kg with decimal precision

### Drag-and-Drop Reorder
- Available on Manage Habits page
- Only allows reordering within the same frequency group (daily → daily, weekly → weekly, etc.)
- Visual feedback: dragged item fades, drop target highlights in coral

---

## 12. Explicitly Out of Scope

- Multi-user support, roles, or sharing
- Numeric streak counts, "X of Y" completion stats, longest-streak records
- Per-habit reminder configuration (global toggle only)
- Bright colors, emojis, large typography, celebratory/gamified UI elements
- Desktop-optimized layout (mobile-first only)
- Yearly habit frequency

---

## 13. Future Additions

- **Meal photo logging** — take/attach photos of meals, logged daily
- **Supabase integration** — connect all HTML pages to live database (in progress)

---

## 14. Resolved Design Decisions

- **Color palette**: True black (#0a0a0a), off-white (#f5f1ed), coral accent (#d97757)
- **Icons**: SVG only — no emojis anywhere in the UI
- **Tab structure**: 5 tabs (Today, Prayers, Week, Month, Stats)
- **Date navigation**: Context-aware by tab
- **Daily layout**: Morning / Evening / Prayer sections
- **Input defaults**: Wake-up time defaults to `00:00`; future days all default to "no"
- **Archived habits**: Always visible at bottom of Manage Habits page (no toggle needed)
- **Edit Profile**: Single page combining profile info + password change
- **Backend**: Supabase (Postgres + Auth + REST API, no custom server)
- **Database**: Two tables — `habits` and `completions` with row-level security
