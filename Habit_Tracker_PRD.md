# Personal Habit Tracker — Product Requirements Document

**Owner:** Nada
**Purpose:** A minimal, personal, single-user web application to track daily discipline and recurring routines, without pressure, scoring, or clutter.
**Status:** v5 — Live

---

## Version History

### v5 (August 12, 2026)
**Supabase backend integration + deployment:**
- ✅ Full Supabase backend — all data persists to a real hosted database
- ✅ `supabase-config.js` — shared credentials file imported by all pages
- ✅ Login page — real `signInWithPassword` + "Create Account" via `signUp` (both on login screen)
- ✅ Main tracker — loads habits + completions from DB; saves toggles/inputs via upsert; completions cached by date to minimize DB calls
- ✅ Manage Habits — full CRUD against DB; `sort_order` batch-updated after each drag-to-reorder
- ✅ Habit Form — add/edit habits with auto sort_order assignment per frequency group
- ✅ Profile page — loads real email from Supabase Auth, real `signOut()`
- ✅ Edit Profile page — update email + password via `db.auth.updateUser()`
- ✅ `index.html` redirect — GitHub Pages root now redirects to login
- ✅ Prayer completions stored as JSON string in completions table under the prayer habit's UUID
- ✅ Mobile double-tap zoom prevented — `touch-action: manipulation` + `user-scalable=no`
- ✅ Touch drag fix — `.habit-actions` buttons remain tappable even with touch drag handler active

**Files Created/Updated:**
- `index.html` — NEW: GitHub Pages entry point (instant redirect to login)
- `supabase-config.js` — NEW: shared Supabase URL + anon key
- `habit_tracker_login.html` — real Supabase auth (sign in + create account)
- `habit_tracker_mockup.html` — loads from DB, saves to DB
- `habit_tracker_manage_habits_mockup.html` — full DB CRUD
- `habit_tracker_habit_form.html` — DB add/edit
- `habit_tracker_profile.html` — real user data + sign out
- `habit_tracker_edit_profile.html` — real email/password update

---

### v4 (August 12, 2026)
**New features:**
- ✅ Added `section` field (Morning / Evening) to habit model
- ✅ Daily view sections are now fully dynamic
- ✅ Habit form now shows a Morning / Evening picker when Daily frequency is selected
- ✅ Added Notes text box at the bottom of the Daily view — per-date, persists while navigating
- ✅ Fixed drag-and-drop reordering on mobile — full touch event implementation

**Backend design (sort order):**
- ✅ Reorder strategy confirmed: integer `sort_order` index stored per habit; batch-updated after each drag

---

### v3 (August 12, 2026)
**Bug fixes & UI improvements:**
- ✅ Fixed drag-and-drop reordering on Manage Habits page
- ✅ Replaced emoji icons with clean SVG icons
- ✅ Fixed wake-up time field showing "true"
- ✅ Future days now default all habits to "no" and wake-up time to `00:00`
- ✅ Clicking "Discipline" title returns to today's daily view
- ✅ Removed "Show archived habits" checkbox
- ✅ Stats weight chart improvements
- ✅ Created `habit_tracker_edit_profile.html`

---

### v2 (August 12, 2026)
- ✅ Added Weekly View, redesigned Daily View, dynamic date navigation
- ✅ Created Habit Form, Manage Habits, Login, Stats, Profile pages
- ✅ Completed color system documentation

### v1 (Initial Concept)
- Defined vision, design philosophy, color palette, seed habits, interaction patterns

---

## 1. Vision & Design Philosophy

This is a personal accountability tool, not a gamified productivity app. The core principle is **calm minimalism**: no emojis, no bright colors, no large text, no "X out of Y" counters, no judgmental red states. The app should feel like a quiet ritual.

**Platform:** Mobile-first responsive web app, deployed on GitHub Pages. Single user only.

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

### Typography & Layout
- **Font**: `-apple-system, BlinkMacSystemFont, "Segoe UI"`
- **Text sizes**: 9–11px most text; uppercase small labels for section headers
- **Spacing**: 6–14px; Border radius: 4–6px; Transitions: 0.2s ease

### Visual Principles
- SVG icons only — no emojis anywhere
- Dark theme always enabled
- Mobile-first

---

## 3. Technical Stack & Deployment

- **Frontend**: Vanilla HTML/CSS/JS — static files, no build step
- **Backend**: Supabase — hosted Postgres + Auth + REST API
- **Hosting**: GitHub Pages
- **Entry point**: `index.html` → `habit_tracker_login.html` → `habit_tracker_mockup.html`

### Key Patterns
- Completions cached by date key to minimize DB calls
- Optimistic UI updates — local state first, DB write async
- `sort_order` batch-updated after each drag-to-reorder
- All pages redirect to login if no session

---

## 4. Authentication

- Email/password via Supabase Auth
- "Create Account" on login page — creates and signs in immediately
- Persistent session — no repeated logins on same device
- Sign out, email/password update from Profile pages

---

## 5. Database Schema

**`habits`** — `id, user_id, name, frequency, type, unit, section, days, day, sort_order, archived`

**`completions`** — `id, user_id, habit_id, date, value` (value is `'true'`/`'false'`/numeric/JSON)

**`daily_notes`** — `id, user_id, date, note`

Row-level security enabled on all tables.

---

## 6. Habit Model

Four frequency types: **Daily**, **Weekly**, **Monthly**, **Quarterly**.

User can add, edit, delete, archive, and reorder habits directly in the app. Past entries can be backfilled. Future days default to "no" / `00:00`.

---

## 7. Initial Habit List

**Daily — Morning:** skincare, wake-up time, weight, turmeric drink
**Daily — Evening:** skincare, vitamins
**Weekly (Sun/Tue/Thu):** horseback riding
**Weekly (Saturday):** cat litter, water fountain, cat grooming, pill dispenser
**Other Weekly:** weight loss medication, house cleaning, hair mask
**Monthly:** massage, manicure & pedicure, Moroccan bath
**Quarterly:** cat medication & vaccinations

---

## 8. Prayer Tracker

Separate tab (private). Five prayers: Fajr, Dhuhr, Asr, Maghrib, Isha. All five = Complete. Single status rolls up to Daily view as read-only indicator. Stored as JSON in completions.

---

## 9. Views

- **Daily**: Morning / Evening / Prayer sections, weekly habits on their day, notes textarea
- **Prayer**: Individual prayer toggles, Complete/Incomplete indicator
- **Weekly**: 7-day grid, horizontally scrollable
- **Monthly**: Full month grid, horizontally scrollable
- **Stats**: Weight trend line chart with month navigation

---

## 10. Pages & Navigation

| Page | File |
|---|---|
| Entry point | `index.html` |
| Login / Create Account | `habit_tracker_login.html` |
| Main Tracker (5 tabs) | `habit_tracker_mockup.html` |
| Manage Habits | `habit_tracker_manage_habits_mockup.html` |
| Habit Form | `habit_tracker_habit_form.html` |
| Profile | `habit_tracker_profile.html` |
| Edit Profile | `habit_tracker_edit_profile.html` |

---

## 11. Out of Scope
Multi-user, streaks, gamification, per-habit reminders, desktop layout, yearly frequency.

---

## 12. Future Additions
- Meal photo logging

---

## 13. Resolved Decisions

- Supabase for backend; GitHub Pages for hosting
- Accounts created via app "Create Account" (not Supabase dashboard)
- SVG icons only; dark theme only
- Prayer stored as JSON in completions
- Archived habits always visible at bottom of Manage Habits
- Edit Profile = single page for email + password
