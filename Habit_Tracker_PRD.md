# Personal Habit Tracker — Product Requirements Document

**Owner:** Nada
**Purpose:** A minimal, personal, single-user web application to track daily discipline and recurring routines, without pressure, scoring, or clutter.
**Status:** Draft v2

---

## Version History

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
- ✅ Reorganized Main Tracker header — Profile (👤) | Discipline (center) | Settings (⚙)
- ✅ Stats tab positioned far right — Visual separation from main tracking tabs
- ✅ Month navigation on Stats view — Navigate between months to see historical trends

**Files Created/Updated:**
- `habit_tracker_login.html` — Login with demo mode
- `habit_tracker_mockup.html` — Main tracker with 5 tabs (Today, Prayers, Week, Month, Stats)
- `habit_tracker_manage_habits_mockup.html` — Habit management interface
- `habit_tracker_habit_form.html` — Unified add/edit form
- `habit_tracker_stats.html` — Standalone stats page
- `habit_tracker_profile.html` — User profile & settings (NEW)

**Navigation Flow:**
- Login → Main Tracker
- Main Tracker: 👤 Profile | ⚙ Settings (Manage Habits)
- Stats tab shows full-month weight trends with line chart
- All pages fully connected

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
- No emojis, no bright colors, no judgment
- Soft color marks instead of harsh heatmaps
- Calm, ritual-like feel — minimal motion
- Mobile-first, responsive design
- Dark theme by default

---

## 3. Authentication

- Simple login with username/password.
- **Persistent session** — no repeated logins required on the same device once authenticated.
- Single-user system; no account management, roles, or permissions needed.

---

## 4. Habit Model

Habits are organized into four frequency types:

- **Daily**
- **Weekly**
- **Monthly**
- **Quarterly**

*(No yearly frequency needed.)*

### 3.1 Habit Management (Self-Service)
The user must be able to, directly in the app, without developer involvement:
- Add new habits, assigning name and frequency
- Edit existing habits
- **Delete** a habit permanently
- **Archive/pause** a habit (removes it from active view but preserves historical data)
- **Reorder habits** via drag-and-drop, so the list reflects personal priority/flow rather than creation order or alphabetical sort

### 3.2 Editable History
- Past entries can be edited/backfilled — e.g., checking off yesterday's habit if forgotten at the time.

---

## 5. Initial Habit List (Seed Data)

### Daily (appear every day)
- **Morning skincare routine** — yes/no
- **Wake-up time log** — text input (user enters time, e.g., "6:30")
- **Daily weight log** — numeric input (kg)
- **Lemon-ginger-turmeric drink** — yes/no
- **Evening skincare routine** — yes/no
- **Evening vitamins** — yes/no
- **Prayer tracker** (see Section 5 — rolls up into main view as read-only line item, controlled from Prayer tab)

### Recurring (appear on specific days)
- **Horseback riding lessons** — Sunday, Tuesday, Thursday; appears in Evening section when applicable (yes/no)

### Weekly (appear only on Saturday, at bottom of daily table)
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

## 6. Prayer Tracker (Special Feature)

- Separate, tucked-away tab — not shown prominently on the main daily view (private by design, not for display).
- Contains individual checkboxes for the five daily prayers: **Fajr, Dhuhr, Asr, Maghrib, Isha**.
- The app calculates completion automatically: if all five are checked, the day is "done"; otherwise "not done."
- This single done/not-done status **rolls up as one quiet line item** on the main daily habit list, alongside all other habits — no prayer-level detail shown there.

---

## 7. Views

### 6.1 Daily View (Home Screen)
- **Date navigation** at top with < > arrows to move between days; displays day name and date, or "Today" if current date.
- **Daily habits** organized into sections:
  - **Morning**: skincare, wake-up time (text input, e.g., "6:30"), daily weight (numeric input, kg), turmeric drink
  - **Evening**: skincare, vitamins, horseback riding (only shows Sunday, Tuesday, Thursday)
  - **Prayer**: prayers (single read-only status indicator, updated only from Prayer tab)
- **Weekly section at bottom** (Saturday only): cat litter, water fountain, cat grooming, pill dispenser refill
- Yes/No buttons (✕ for no, ✓ for yes, coral when active)
- Number/text inputs for wake-up time and weight

### 6.2 Prayer Tab
- Separate tab showing all five daily prayers with individual checkboxes
- Status indicator at top: "Complete" or "Incomplete" based on all five being done
- Simple yes/no buttons for each prayer
- Prayer status rolls up to Daily view as read-only indicator

### 6.3 Weekly View
- **Week navigation** at top with < > arrows to jump between weeks (±7 days)
- **Date range** display showing the week (e.g., "August 12 - August 18")
- **7-day grid** showing that week's progress: days down left side (Sun-Sat), habits across top with vertical labels
- Each cell shows soft mark for "done" (coral, #d97757) and muted for "not done" (#2a2925)
- Same vertical habit labels as monthly view for consistency

### 6.4 Monthly Matrix View
- **Month navigation** at top with < > arrows to move between months; displays month and year
- **Full month** (all 31 days) displayed as vertical grid: days running down left side, habits across top
- **Habit labels are vertical** (rotated text, left-aligned) at the top so full text is visible
- Each cell shows soft mark for "done" and muted for "not done"
- Gentle visual density map, not harsh heatmap or numeric indicators

---

## 8. Reminders / Notifications

- Optional reminders feature, controlled by a **single global on/off switch** for the entire app (not per-habit).
- Off by default is acceptable; user can toggle on whenever they want nudges.

---

## 9. Interaction Patterns

### Navigation & Tabs
- **Four main tabs**: Today (daily view), Prayers, Week (weekly view), Month (monthly view)
- **Dynamic date navigation** at top:
  - **Today/Prayers tabs**: Daily navigation (< > arrows move 1 day, show "Today" or day name + date)
  - **Week tab**: Weekly navigation (< > arrows move ±7 days, show week date range)
  - **Month tab**: Monthly navigation (< > arrows move between months, show month + year)

### Yes/No Buttons
- Every daily/weekly yes/no habit has two buttons: ✕ (no, gray) and ✓ (yes, coral highlight)
- Click to toggle between states
- Visual feedback: coral background fills when yes, gray when no
- Prayer status in Daily view is read-only indicator — must toggle from Prayer tab

### Number/Text Inputs
- Wake-up time: text input, user enters time like "6:30"
- Daily weight: numeric input, user enters weight in kg with decimal precision
- Text fields display inline next to habit label

## 10. Explicitly Out of Scope (v1)

- Multi-user support, roles, or sharing
- Numeric streak counts, "X of Y" completion stats, longest-streak records
- Per-habit reminder configuration (global toggle only)
- Bright colors, emojis, large typography, celebratory/gamified UI elements
- Desktop-optimized layout (mobile-first only for now)

---

## 11. Future Additions (Not in Initial Build)

- **Meal photo logging** — ability to take/attach photos of breakfast, lunch, dinner, and optionally snacks, logged daily.

---

## 12. Resolved Design Decisions

- **Color palette**: True black (#0a0a0a), off-white text (#f5f1ed), coral accent (#d97757) — Claude's actual interface colors
- **Tab structure**: Four tabs (Today, Prayers, Week, Month) for different views and time ranges
- **Date navigation**: Context-aware, changes behavior by tab (daily for Today/Prayers, weekly for Week, monthly for Month)
- **Daily layout reorganization**: 
  - Morning, Evening, Prayer sections
  - Horseback riding only shows in Evening on Sun/Tue/Thu
  - Cat weekly tasks moved to bottom "Weekly" section (Saturday only)
- **Weekly view**: 7-day grid with week navigation, shows progress for that week
- **Input types**: Yes/No buttons (✕/✓) for binary habits, text inputs for wake-up time, numeric for weight
- **Prayer tab**: Fully separate, controls prayer completion; Daily view shows read-only status only
- **Monthly & Weekly views**: Full month/week coverage, vertical habit labels (rotated, left-aligned), subtle color marks (no numbers)

## 13. Open Items for Build Phase

- Technical stack and hosting/deployment approach
- Data persistence layer (backend API, database design)
- Add/remove habit management UI in settings
- Drag-and-drop reordering implementation for habit order
- Login/authentication system
- Data backup and recovery strategy
