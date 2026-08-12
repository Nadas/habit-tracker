# Personal Habit Tracker — Product Requirements Document

**Owner:** Nada
**Purpose:** A minimal, personal, single-user web application to track daily discipline and recurring routines, without pressure, scoring, or clutter.
**Status:** v7 — Live

---

## Version History

### v7 (August 12, 2026)
**Bug fixes & feature additions:**
- ✅ **Bug:** Weekly habits now correctly appear in Weekly and Monthly matrix views (previously filtered to daily habits only)
- ✅ **Bug:** Weekly matrix shows empty (non-tappable) cells for weekly habits on non-scheduled days
- ✅ **Bug:** Weight chart x-axis now spans all days of the month using actual day number; single-entry divide-by-zero fixed (`Math.max(daysInMonth - 1, 1)` denominator)
- ✅ **Feature:** Weight goal moved from hardcoded value to Edit Profile — stored in `user_metadata.weight_goal`; Stats tab hides the goal line/text when not set
- ✅ **Feature:** Year tab added — shows monthly and quarterly habits only; rows = Jan–Dec, columns = habit names; dots indicate completion
- ✅ **Feature:** Monthly habit recurrence now uses "Nth weekday" picker (e.g. "2nd Thursday") instead of a day-of-month number; stored as `[nth, weekday]` in the existing `days` JSONB column
- ✅ **Feature:** Tappable mini-calendar overlay on daily/prayer tabs — tap the date label to open a month grid and jump to any day; left/right arrows preserved alongside it
- ✅ **UI:** Tab bar redesigned with `|` pipe dividers between groups: `Today · Prayer | Week · Month · Year | Stats · Notes`
- ✅ **Feature:** Notes tab added — shows all recorded daily notes in reverse chronological order (scrollable, read-only)

**Data model changes:**
- `habits.days` column now dual-purpose: for weekly habits it remains an array of weekday numbers 0–6; for monthly habits it stores `[nth, weekday]` (e.g. `[2, 4]` = "2nd Thursday"). `nth` values: 1=1st, 2=2nd, 3=3rd, 4=4th, 5=Last.
- `user_metadata.weight_goal` (number | null) — stored via `db.auth.updateUser({ data: { weight_goal } })`

**Files Updated:**
- `habit_tracker_mockup.html` — all view fixes, Year tab, Notes tab, calendar overlay, tab bar, weight goal from metadata
- `habit_tracker_edit_profile.html` — weight goal field added
- `habit_tracker_habit_form.html` — Nth weekday picker for monthly habits
- `habit_tracker_manage_habits_mockup.html` — `getFreqLabel()` updated to display Nth weekday format

---

### v6 (August 12, 2026)
**Bug fixes & feature additions:**
- ✅ Prayers habit excluded from Morning/Evening sections in Daily view — it was incorrectly appearing as a toggleable row there
- ✅ Prayer tab now conditional — hidden from tab bar unless a "Prayers" habit exists; falls back to Daily tab if prayer habit is removed
- ✅ Weight and number-type habits now correctly show as filled (coral) in weekly/monthly grids when any value is entered (previously only yes/no habits rendered correctly)
- ✅ Weekly/monthly grid columns are now left-aligned — removed `min-width:100%` that caused columns to spread across the full screen width with few habits
- ✅ Consistent app header across all pages — Manage Habits and Habit Form now show the full Discipline header (profile icon | Discipline | settings icon) instead of a plain back arrow
- ✅ Name field added to Edit Profile — saves to Supabase `user_metadata.full_name`; Profile page now shows the saved name instead of the email prefix
- ✅ Menstruating toggle added to Prayer tab — dims prayer checkboxes, shows muted dark red dot with "Menstruating" status on daily view instead of "Incomplete"
- ✅ Menstruating days show as a dark red square (`#7a3535`) in weekly/monthly grids instead of gray

**Prayer data model addition:**
- `menstruating: boolean` added to the prayer JSON stored in completions
- `PRAYER_DEFAULT` constant ensures safe defaults for all prayer fields including `menstruating`

**Files Updated:**
- `habit_tracker_mockup.html` — all grid, prayer, and header fixes
- `habit_tracker_manage_habits_mockup.html` — consistent app header
- `habit_tracker_habit_form.html` — consistent app header
- `habit_tracker_edit_profile.html` — name field added

---

### v5 (August 12, 2026)
**Supabase backend integration + deployment:**
- ✅ Full Supabase backend — all data persists to a real hosted database
- ✅ `supabase-config.js` — shared credentials file imported by all pages
- ✅ Login page — real `signInWithPassword` + "Create Account" via `signUp`
- ✅ Main tracker — loads habits + completions from DB; saves toggles/inputs via upsert; completions cached by date to minimize DB calls
- ✅ Manage Habits — full CRUD against DB; `sort_order` batch-updated after each drag-to-reorder
- ✅ Habit Form — add/edit habits with auto sort_order assignment per frequency group
- ✅ Profile page — real email from Supabase Auth, real `signOut()`
- ✅ Edit Profile page — update email + password via `db.auth.updateUser()`
- ✅ `index.html` redirect — GitHub Pages root redirects to login
- ✅ Prayer completions stored as JSON string in completions table under the prayer habit's UUID
- ✅ Mobile double-tap zoom prevented — `touch-action: manipulation` + `user-scalable=no`

---

### v4 (August 12, 2026)
**New features:**
- ✅ Added `section` field (Morning / Evening) to habit model
- ✅ Daily view sections are fully dynamic — built from each habit's `section` field
- ✅ Habit form shows Morning / Evening picker when Daily frequency is selected
- ✅ Added Notes text box at the bottom of the Daily view — per-date, persists while navigating
- ✅ Fixed drag-and-drop reordering on mobile — full touch event implementation with floating clone

---

### v3 (August 12, 2026)
**Bug fixes & UI improvements:**
- ✅ Fixed drag-and-drop reordering on Manage Habits page
- ✅ Replaced emoji icons with clean SVG icons
- ✅ Fixed wake-up time field showing "true"
- ✅ Future days default all habits to "no" and wake-up time to `00:00`
- ✅ "Discipline" title taps return to today
- ✅ Archived habits always visible at bottom of Manage Habits
- ✅ Stats weight chart improvements
- ✅ Profile: SVG avatar, merged Edit Profile + Change Password into single page

---

### v2 (August 12, 2026)
- Weekly View, redesigned Daily layout, dynamic date navigation
- Unified Habit Form, Manage Habits page, Login, Stats, Profile pages
- Color system documented

### v1 (Initial Concept)
- Vision, design philosophy, color palette, seed data, core views

---

## 1. Vision & Design Philosophy

This is a personal accountability tool, not a gamified productivity app. The core principle is **calm minimalism**: no emojis, no bright colors, no large text, no "X out of Y" counters, no judgmental red states. The app should feel like a quiet ritual — something that invites daily return rather than pressures it.

**Visual direction:** Dark, muted theme — warm neutral darks, soft off-white text, a single gentle accent color used sparingly. Generous white space. Gentle transitions, nothing sharp or jarring.

**Platform:** Mobile-first responsive web app, deployed on GitHub Pages, accessible from Nada's phone. Single user only.

---

## 2. Design & Color System

### Color Palette
- **Background**: `#0a0a0a`
- **Primary text**: `#f5f1ed`
- **Accent (coral)**: `#d97757`
- **Borders**: `#2a2925`
- **Card background**: `#141411`
- **Muted text**: `#8a8985`
- **Disabled state**: `#7a7570`
- **Menstruating indicator**: `#7a3535` (muted dark red — used for prayer dot and grid cell)

### Typography & Layout
- **Font**: `-apple-system, BlinkMacSystemFont, "Segoe UI"` — system sans-serif
- **Text sizes**: 9–11px most text; uppercase small labels for section headers
- **Spacing**: 6–14px padding/gaps; **Border radius**: 4–6px; **Transitions**: 0.2s ease

### Visual Principles
- SVG icons only — no emojis anywhere in the UI
- Dark theme always enabled
- Mobile-first

---

## 3. Technical Stack & Deployment

- **Frontend**: Vanilla HTML/CSS/JS — static files, no build step
- **Backend**: Supabase — hosted Postgres + Auth + REST API, no custom server
- **Hosting**: GitHub Pages
- **Entry point**: `index.html` → `habit_tracker_login.html`; login redirects to main tracker if session exists

### Key Patterns
- Completions cached by date key (`completionsByDate`) to minimize DB calls
- Optimistic UI updates — local state updated immediately, DB write async
- Prayer completions stored as JSON under the prayer habit's UUID in completions
- `sort_order` batch-updated after each drag-to-reorder
- All pages redirect to login if unauthenticated
- Year view completions cached in `state.yearCompletions`, keyed as `${habit_id}_${monthKey}`
- All notes cached in `state.allNotes` (null when unloaded); invalidated on note save

---

## 4. Authentication

- Email/password login via Supabase Auth (`signInWithPassword`)
- "Create Account" on login page uses `signUp` — creates and signs in immediately
- Persistent session — no repeated logins on same device
- Sign out via `db.auth.signOut()` on profile page
- Update email, password, display name, and weight goal via `db.auth.updateUser()` on Edit Profile

---

## 5. Database Schema

**`habits`**
| Column | Type | Notes |
|---|---|---|
| id | uuid | Primary key |
| user_id | uuid | References auth.users |
| name | text | Display name |
| frequency | text | `daily`, `weekly`, `monthly`, `quarterly` |
| type | text | `yes/no` or `number` |
| unit | text | e.g. `kg`, `min` (optional, number habits only) |
| section | text | `morning` or `evening` (daily habits only) |
| days | int[] | Weekday numbers 0–6 (weekly habits); OR `[nth, weekday]` tuple (monthly habits) |
| day | int | Day of month 1–31 (legacy monthly habits only — superseded by `days` tuple) |
| sort_order | int | User-defined order; batch-updated after drag-to-reorder |
| archived | boolean | Soft delete / pause |

**Monthly recurrence encoding (v7):**
`days` stores a two-element array `[nth, weekday]` for monthly habits.
- `nth`: 1=1st, 2=2nd, 3=3rd, 4=4th, 5=Last
- `weekday`: 0=Sun, 1=Mon, 2=Tue, 3=Wed, 4=Thu, 5=Fri, 6=Sat
- Example: `[2, 4]` = "2nd Thursday of every month"

**`completions`**
| Column | Type | Notes |
|---|---|---|
| id | uuid | Primary key |
| user_id | uuid | References auth.users |
| habit_id | uuid | References habits |
| date | date | The day this completion is for |
| value | text | `'true'`, `'false'`, numeric string, or JSON (prayers) |

**`daily_notes`**
| Column | Type | Notes |
|---|---|---|
| id | uuid | Primary key |
| user_id | uuid | References auth.users |
| date | date | The day this note is for |
| content | text | Free-text daily note |

Row-level security enabled on all tables.

### Prayer JSON format
Stored as a JSON string in `completions.value` under the prayer habit's UUID:
```json
{ "fajr": true, "dhuhr": false, "asr": false, "maghrib": true, "isha": false, "menstruating": false }
```
`menstruating: true` means no prayers required that day. All fields default to `false`.

---

## 6. Habit Model

Four frequency types: **Daily**, **Weekly**, **Monthly**, **Quarterly**.

### Habit Management
The user can directly in the app: add, edit, delete permanently, archive/pause, and reorder via drag-and-drop within the same frequency group.

### Monthly Recurrence
Monthly habits recur on the Nth weekday of each month (e.g. "2nd Thursday"). Stored as `[nth, weekday]` in the `days` column. `nth = 5` means "Last" occurrence of that weekday in the month.

### Editable History
Past entries can be backfilled — checking off yesterday's habit if forgotten.

### Future Days
All habits default to "no" / unchecked after today. Wake-up time defaults to `00:00`.

---

## 7. Initial Habit List

### Daily — Morning
- Morning skincare routine — yes/no
- Wake-up time log — number (default `00:00`)
- Daily weight log — number (kg)
- Lemon-ginger-turmeric drink — yes/no

### Daily — Evening
- Evening skincare routine — yes/no
- Evening vitamins — yes/no

### Weekly (Sun, Tue, Thu) — Evening
- Horseback riding lessons — yes/no

### Weekly (Saturday)
- Cat litter change / water fountain cleanup / grooming / pill dispenser refill — yes/no

### Other Weekly
- Weight loss medication / house cleaning / hair mask — yes/no

### Monthly
- Massage, manicure & pedicure, Moroccan bath

### Quarterly
- Cat medication & vaccinations

---

## 8. Prayer Tracker

- Separate tab — only visible if a habit named "Prayers" exists; hidden otherwise
- Five daily prayers: Fajr, Dhuhr, Asr, Maghrib, Isha with individual toggle buttons
- **Menstruating toggle** — when on, dims prayer checkboxes (no prayers required), status shows "Menstruating" with a muted dark red dot; grids show a dark red cell for that day
- All five prayers checked (and not menstruating) = "Complete" with coral dot
- Single status rolls up as a read-only line item on the Daily view (Prayer section)
- Stored as JSON in completions under the prayer habit's UUID

---

## 9. Views

### Tab Bar Layout
Eight tabs arranged in three groups, separated by `|` pipe dividers:

```
Today · Prayer | Week · Month · Year | Stats · Notes
```

Prayer tab is hidden unless a "Prayers" habit exists (group collapses to just "Week · Month · Year" if so).

### Daily View
- Date label is a tappable button — opens mini-calendar overlay for jumping to any day
- Left/right arrow navigation preserved alongside calendar
- "Today" or day name + date shown in the date bar
- Tapping "Discipline" always returns to today
- Morning / Evening sections (prayer habit excluded from these)
- Prayer section: read-only status indicator (Complete / Incomplete / Menstruating)
- Monthly habits due today shown in a "Monthly" section
- Weekly habits at bottom on their designated day(s)
- Notes textarea at bottom — per-date free text

### Mini-Calendar Overlay
- Fixed overlay triggered by tapping the date label in Daily or Prayer tabs
- Shows a full month grid with day buttons; tap a day to navigate there
- Prev/next month navigation within the overlay
- Close by tapping outside or selecting a date

### Prayer Tab
- Visible only when a Prayers habit exists
- Menstruating toggle at top
- Five prayer toggles (grayed out when menstruating)
- Status indicator (Complete / Incomplete / Menstruating)
- Date label also opens mini-calendar overlay

### Weekly View
- 7-day grid: days down left, habits across top (vertical labels), left-aligned
- Includes daily and weekly habits; weekly habits show empty cells on non-scheduled days
- Coral cell = done; dark red cell = menstruating (prayer column); gray = not done

### Monthly View
- Full month grid: same format as weekly, left-aligned
- Includes daily and weekly habits
- Month navigation for historical browsing

### Year View
- Monthly and quarterly habits only
- Rows = month names (Jan–Dec); columns = habit names
- Dot = completed that month; empty = not completed
- Year navigation (< >) at top

### Stats View
- Monthly weight trend line chart; month navigation
- X-axis spans all days of the month (day 1 → last day); single entries plotted at correct position
- Weight goal target line shown only when a goal is set in Edit Profile

### Notes View
- All recorded daily notes in reverse chronological order
- Date label + note content per entry
- Scrollable, read-only (edit notes from the Daily view)

---

## 10. Pages & Navigation

| Page | File |
|---|---|
| Entry point | `index.html` |
| Login / Create Account | `habit_tracker_login.html` |
| Main Tracker (8 tabs) | `habit_tracker_mockup.html` |
| Manage Habits | `habit_tracker_manage_habits_mockup.html` |
| Habit Form (add/edit) | `habit_tracker_habit_form.html` |
| Profile | `habit_tracker_profile.html` |
| Edit Profile | `habit_tracker_edit_profile.html` |

**All pages share the same app header:** profile SVG icon (left) | Discipline (center, taps to today) | settings SVG icon (right). Manage Habits and Habit Form show a secondary subheader row below it for their page-specific title/actions.

**Flow:** Login → Main Tracker → Profile (person icon) or Manage Habits (sliders icon) → Habit Form

---

## 11. Explicitly Out of Scope

- Multi-user support, roles, sharing
- Streak counts, completion stats, gamification
- Per-habit reminders (global toggle only)
- Desktop-optimized layout

---

## 12. Future Additions

- **Meal photo logging** — attach photos of meals, logged daily

---

## 13. Resolved Decisions

- Supabase backend (no custom server, free tier sufficient)
- GitHub Pages hosting with `index.html` redirect
- Accounts created via app "Create Account" — not Supabase dashboard (dashboard users have unreliable password auth)
- SVG icons only, no emojis
- Prayer stored as JSON in completions (avoids 5 separate habit rows)
- Menstruating state stored inside the same prayer JSON
- Prayer tab hidden until "Prayers" habit is added
- `touch-action: manipulation` + `user-scalable=no` prevents double-tap zoom
- Archived habits always shown at bottom of Manage Habits (no toggle)
- Edit Profile is a single page (name + email + password + weight goal)
- Display name stored in `user_metadata.full_name` via `db.auth.updateUser({ data: { full_name } })`
- Weight goal stored in `user_metadata.weight_goal`; null means "no goal set" → Stats hides goal line
- Monthly habits encoded as `[nth, weekday]` in `days` column (no schema change needed)
- Year view only shows monthly + quarterly habits (daily/weekly excluded — too granular for a yearly grid)
- Notes tab is read-only; notes are still edited from the Daily view textarea