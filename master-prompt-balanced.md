=== PROJECT BRIEF (fill in the blanks; leave any blank to let the AI decide) ===
Project description: ______________________________________________________________
  (one or two sentences, e.g. "A school fee management system for a primary school"
   or "An inventory and sales tracker for a small electronics shop")

System name (optional):          ______________________________
Company/organization (optional): ______________________________
Database name (optional):        ______________________________
Entities to use (optional):      ______________________________
  (e.g. "Student, Invoice, Payment"; leave blank to let the AI choose)
Layout style (optional):         ______________________________
  (e.g. "minimal" / "editorial" / "playful" / "corporate" / "bold" / "modern"; blank = AI picks.
   This is a nudge for the PUBLIC pages — Landing/Login/Register/Recover — not a strict template.)

Reports required (optional — copy the block for more reports; leave blank to let the AI design 2):
  Report 1
    Name:             ______________________________
    What it shows:    ______________________________
    Group / total by: ______________________________   (optional)
    Columns to show:  ______________________________   (optional)
    Filter by:        ______________________________   (optional, e.g. "date (default today)", "month", "status")
  Report 2
    Name:             ______________________________
    What it shows:    ______________________________
    Group / total by: ______________________________   (optional)
    Columns to show:  ______________________________   (optional)
    Filter by:        ______________________________   (optional)

=== INSTRUCTIONS TO THE AI ===
You are a senior full-stack developer. Build the complete application described in the PROJECT BRIEF above.
The brief is the only required input; everything else is your responsibility to design. Any field left
blank, and every [SQUARE BRACKET] placeholder in the sections below, is a value YOU derive with a sensible
default. If a brief field is filled in, honor it exactly. Do not ask the human for anything further unless
the project description is too vague to pick a domain — otherwise make the most reasonable assumption and proceed.

Design everything yourself from the brief:
1. Choose a clear system name, database name, and root folder name (or use the brief's values).
2. Decide the entities — those listed in the brief if given, otherwise a sensible set (default 3 domain
   entities plus Users; use fewer or more if the project clearly needs it).
3. Decide each entity's attributes, types, and constraints (required, unique, numeric, date, dropdown, computed).
4. Infer all relationships yourself from the attributes (see "How to infer relationships" below).
5. Reports: if the brief lists reports, build EXACTLY those (one tab + one endpoint + one MongoDB aggregation
   each), honoring each report's name, columns, grouping, and filter, with blanks sensibly defaulted. If the
   brief lists none, design two useful reports for the domain.
6. Map entities to routes, pages, and API endpoints following the patterns here.
7. Before writing any code, output a brief ASSUMPTIONS SUMMARY (system name, entities/attributes chosen,
   relationships inferred, reports to build), then the ERD, then build the app.

Wherever a section below references [Entity1], [field1], [report1slug], etc., substitute the specifics you designed.

How to infer relationships (the human supplies none — this analysis is entirely yours):
- Treat any attribute whose name references another entity (ends in "Id"/"Ref", or is another entity's name)
  as a foreign-key reference to that entity.
- A reference on entity B pointing to entity A normally means A has-many B (one-to-many): one A, many B.
- If two entities reference each other, or a join/linking entity sits between them, model many-to-many.
- An attribute computed from other fields (e.g. a total) is a derived field, not a relationship.
- State one line of reasoning per inferred relationship before the ERD, so the human sees how you derived the model.

The ERD (a Mermaid erDiagram) must show: all entities, all attributes with types, _id as PK on every entity,
reference fields as FK relationships, correct cardinality notation (||--o{, }o--||, etc.), and relationship
labels (has / belongs to / records). Do NOT ask the human to draw, supply, or confirm it.

=== WORKFLOW PHASES (follow in order; don't begin a later phase's code before the earlier design phases are done) ===
  Phase 1  — Analyze & Plan: decide names, entities, attributes/constraints, the reports, and the dashboard
             metrics; output the assumptions summary.
  Phase 2  — Model & Relationships (ERD): infer all relationships, output the reasoning, then the Mermaid ERD.
  Phase 3  — Backend Foundation: package.json, server.js (cors, JSON parser, session, env validation,
             mongoose.connect, listen), config/db.js, .env.example.
  Phase 4  — Data Layer: one Mongoose model per collection with the inferred refs, indexes, unique constraints,
             and hashed/sensitive-field handling.
  Phase 5  — Backend Logic: controllers (all logic, validation, {data}/{error} responses), thin routes, requireAuth
             (+ requireRole if needed), session auth, registration + 4-digit recovery, report + dashboard
             aggregations, and the idempotent seed (admin + sample data).
  Phase 6  — Frontend Foundation: package.json (pinned versions), vite.config, jsconfig, postcss.config,
             tailwind.config (v3, darkMode 'class'), index.css (tokens incl. exact bg/accent/trend colors + @media
             print), the self-contained shadcn ui components, lib/utils, axiosClient, AuthContext, useTheme.
  Phase 7  — Frontend API Layer: api modules (auth, per-entity, reports incl. getDashboard).
  Phase 8  — Pages & Components: Navbar, ProtectedRoute, and the pages (Landing, Login, Register, Recover,
             Dashboard, entity pages, Reports); wire routing, validation, toasts, and loading/error/empty states.
  Phase 9  — Polish & Verify: apply design/accessibility/performance/reusability rules; confirm every import
             resolves and every API call maps to a real route/controller; ensure the RUNS-IMMEDIATELY guarantee holds.
  Phase 10 — Documentation: write the root README.md, including the "Build Phases" recap.

=== DATABASE RULE (NON-NEGOTIABLE) ===
Use MongoDB with Mongoose ONLY. Never suggest, substitute, or fall back to MySQL, PostgreSQL, SQLite, or any
SQL/relational database under any circumstances — even if it seems simpler or the schema looks relational. All
data modeling, queries, and aggregations are MongoDB/Mongoose. Build with React.js (frontend) and
Node.js/Express.js (backend) with MongoDB as the database.

=== PROJECT STRUCTURE ===
[RootFolderName]/
  backend-project/   (Node + Express + Mongoose):  config/, models/, controllers/, routes/, middleware/
  frontend-project/  (React + Vite + Tailwind + shadcn/ui + lucide-react):
       src/api/, src/context/, src/components/, src/components/ui/, src/pages/, src/lib/

=== DATABASE: [DATABASE_NAME] ===
YOU design the entities and attributes (unless the brief listed them); the human supplies none of this. Once you
have the attributes, infer the relationships yourself and add the appropriate Mongoose `ref` fields, `_id` PKs,
and ObjectId references. The shape below is an EXAMPLE PATTERN — Users is always present; use however many real,
well-named domain entities the project needs (typically 2-4), named after the actual domain (e.g. Student, Invoice, Payment):
  Users({ _id, fullName, username:unique, email:unique, phone:unique, password, recoveryCodeHash })
     // recoveryCodeHash = bcrypt hash of the 4-digit recovery code
  [Entity1]({ _id, [domain fields], [computedField if the domain needs one] })
  [Entity2]({ _id, [domain fields], [dateField if relevant], [attribute referencing another entity if implied] })
  [Entity3]({ _id, [domain fields], [computedField if relevant], [dateField if relevant], [attributes referencing other entities if implied] })
Attributes that name/identify another entity are FK-ref clues — YOU decide they are references, add the `ref`,
and set the cardinality. Computed fields (e.g. TotalPrice = Quantity × UnitPrice) are calculated in the backend
before saving — never trust the client to send computed values.

=== BACKEND REQUIREMENTS ===
1. Use express, cors, mongoose, bcryptjs, express-session, dotenv, nodemon.
2. server.js includes: cors (origin = CLIENT_URL, credentials:true), JSON body parser, session middleware, env
   validation (exit with a clear console.error if MONGODB_URI or SESSION_SECRET is missing), mongoose.connect()
   with success/failure logs, all routes registered, and app.listen() with a startup log.
3. One Mongoose model file per collection under /models.
4. Controller layer: put ALL handler logic in /controllers (one file per resource: auth, [entity1..3], reports).
   Each exports named async handlers containing the try/catch, validation, Mongoose calls, status codes, and JSON
   responses. /routes files are THIN — create the router, map each method+path to its controller function, attach
   middleware (requireAuth), and export. No business logic or DB calls in route files. Routes:
   - POST /api/auth/register   — validate fullName/email/phone/username/password server-side; hash the password;
       reject duplicate username/email/phone (400); generate a random 4-digit recovery code, store ONLY its bcrypt
       hash, return the plaintext code ONCE; start the session.
   - POST /api/auth/login ;  POST /api/auth/logout ;  GET /api/auth/me (session user or 401)
   - POST /api/auth/recover/verify  — { username|email, recoveryCode }; bcrypt.compare against recoveryCodeHash;
       generic error on mismatch; throttle attempts (a 4-digit code is only 10,000 combinations).
   - POST /api/auth/recover/reset   — { username|email, recoveryCode, newPassword }; re-verify the code, then hash
       and save newPassword; issue a NEW 4-digit code (old invalidated) returned once.
   - POST/GET /api/[entity1route] ;  PUT /api/[entity1route]/:id ;  DELETE /api/[entity1route]/:id
   - POST/GET /api/[entity2route] ;  PUT /api/[entity2route]/:id ;  DELETE /api/[entity2route]/:id
   - POST/GET /api/[entity3route] ;  PUT /api/[entity3route]/:id ;  DELETE /api/[entity3route]/:id
       (every DELETE removes a single record only — row-level; never the project or database.
        EVERY domain entity exposes this same POST/GET/PUT/DELETE set — no INSERT-only entities.)
   - GET /api/reports/dashboard  — one aggregation returning the stat-card metrics (counts + at least one SUM/AVG),
       the summary-block rows, and the chart TIME SERIES (an ordered array of { date/label, value } points).
   - GET /api/reports/<reportSlug>  — one endpoint per report; runs that report's MongoDB aggregation.
5. Use Mongoose model methods only — no raw string queries, no SQL.
6. Hash passwords and recovery codes with bcryptjs; compare with bcrypt.compare().
7. Return correct HTTP status codes: 200, 201, 400, 401, 404, 500.
8. Response shape with NO exceptions: success returns { data: ... } JSON; every try/catch failure returns
   { error: message } JSON.
9. Protect all non-auth routes with a session-check middleware that returns 401 if not logged in.

=== FRONTEND REQUIREMENTS ===
1. React + Vite. Install/configure: axios, react-router-dom, tailwindcss, lucide-react, shadcn/ui, recharts
   (dashboard chart), and sonner (toasts). Render a single global <Toaster /> once in App.jsx.
2. Use shadcn/ui components (Button, Input, Card, Table, Dialog, AlertDialog, Badge, Label, Tabs, Select, Alert,
   Toaster) for ALL UI — no manually-styled raw HTML form elements.
2b. shadcn setup MUST be self-contained (do NOT assume a CLI init ran). Either (a) generate every shadcn component
   you import, in full, under src/components/ui/ (button, input, card, table, dialog, alert-dialog, badge, label,
   tabs, select, alert, sonner) together with their deps (class-variance-authority, clsx, tailwind-merge, and the
   `cn` util in src/lib/utils.js), and configure the `@/` path alias in BOTH vite.config.js and jsconfig.json; or
   (b) if a component would be incomplete, fall back to plain Tailwind for that one element. Every shadcn import
   must resolve to a file you generated — NO phantom imports.
3. lucide-react icons ONLY throughout. No emoji anywhere.
3b. Auth state via React Context (src/context/AuthContext.jsx): export AuthProvider + a useAuth() hook holding
   { user, loading } and exposing login/logout/register and a checkAuth() that calls GET /api/auth/me. Wrap the app
   in <AuthProvider> in App.jsx inside BrowserRouter; hydrate the session via /me on mount. ProtectedRoute, Navbar,
   Login, Register, and Recover read auth from useAuth() instead of calling the auth API directly.
3c. Theme (light/dark) is app-wide and available on EVERY page including the public ones: a useTheme()/ThemeProvider
   holding { theme, toggleTheme } that toggles the `dark` class on <html>. On first load, use the saved localStorage
   "theme" if present, else the OS prefers-color-scheme; persist the choice to localStorage on every toggle so it
   survives reloads (localStorage is permitted for the theme preference only); apply the theme before first paint to
   avoid a flash of the wrong theme.

Pages:
  a. LandingPage (route /) — PUBLIC: a marketing showcase for [SYSTEM NAME]. DO NOT default to the same generic
     hero + about + 3-card grid + "how it works" + footer recipe every time — choose a layout that fits the domain
     and the optional brief "Layout style" (split-screen, full-bleed statement, stat strip, editorial, side-panel,
     classic hero + cards, etc.) and make it look meaningfully different from the AI's default each run. Required
     behavior independent of the chosen layout: the system name + a one-line value prop are present; an obvious
     primary "Sign In" CTA (lucide ArrowRight) routes to /login; the page communicates what the system does and its
     core capabilities (managing [Entity1], recording [Entity2], tracking [Entity3], viewing Reports — expressed
     however the layout best supports them); fully responsive; uses ONLY the CSS-variable palette and global font;
     lucide icons only, no emoji, no lorem-ipsum. Has its own minimal top bar (system name on the left; theme toggle
     + "Sign In" on the right). NOT wrapped in ProtectedRoute, no Navbar, no protected API; renders in both themes.
  a2. PUBLIC PAGES MUST LOOK DISTINCTIVE: the public pages (Landing, Login, Register, Recover) must look
     meaningfully different from one another and reflect the system's domain. They are NOT all the same centered
     card on a plain background. Pick layouts that suit the project (split-screen, full-bleed, editorial, stat
     strip, single-statement, classic card, side-panel, etc.) while still using the design tokens and respecting
     accessibility. Coherence via tokens stays; visual sameness across the four public pages does NOT.
  b. LoginPage (route /login): Username, Password (Eye/EyeOff show-hide toggle). On success → /dashboard; on failure
     → inline destructive shadcn Alert. Links: "Don't have an account? Register" → /register, "Forgot password?" →
     /recover, "Back to home" → /.
  c. RegisterPage (route /register) — PUBLIC: Full Name, Email, Phone Number, Username, Password (+toggle), Confirm
     Password (+toggle) — all required, validated client-side before the call AND mirrored server-side (see INPUT
     VALIDATION). On success the API returns a one-time 4-digit recovery code: do NOT redirect immediately — show a
     "Save your recovery code" step (a Dialog/Card) that displays the code large/monospace, explains in one line that
     it's needed to recover the account and is shown ONLY ONCE, provides Copy (lucide Copy) and Download (lucide
     Download → a .txt named "<systemname>-recovery-code.txt" containing username + code) buttons, and requires an
     "I've saved it — continue" confirmation before logging the user in → /dashboard. Failure → destructive Alert.
     Links to /login and /. Not wrapped in ProtectedRoute; no Navbar.
  d. RecoverPage (route /recover) — PUBLIC: lets a user who forgot their password regain access via the 4-digit code.
     Step 1 (Verify): Username|Email + 4-digit Recovery Code (validate /^\d{4}$/), POST /api/auth/recover/verify;
     generic "Invalid details" Alert on mismatch, advance on match. Step 2 (Reset): New Password (+toggle) + Confirm
     (must match, min 6), POST /api/auth/recover/reset → returns a NEW one-time code; show the same "Save your recovery
     code" step, then → /login. "Back to Sign In" link. Not wrapped in ProtectedRoute; no Navbar.
  e. DashboardPage (route /dashboard) — PROTECTED, the default landing after login/registration. MUST present real
     statistics, not just the chart — lead with the stats, chart below:
     - Stat cards: a responsive grid (grid-cols-2 md:grid-cols-4) of AT LEAST FOUR shadcn Cards showing real metrics
       of your entities (totals per entity, today's counts, and at least one aggregated SUM/AVG such as total revenue),
       each with a lucide icon and the accent color; numbers human-formatted (separators, currency where relevant).
     - A summary block: a compact shadcn Table or short list (~5 rows) complementing the cards — e.g. recent records
       or a per-item totals breakdown.
     - ONE chart using recharts, which MUST be a LINE chart (LineChart): plot a meaningful metric over time (time-based
       X axis, value on Y), responsive (ResponsiveContainer), with axes, grid, and tooltip. Do not use bar/pie/other.
       The line color is TREND-BASED, applied PER SEGMENT (not as a single whole-line color): for each pair of
       consecutive points (p[i-1] → p[i]), color the segment between them with --color-trend-up (#16A34A) if
       p[i].value >= p[i-1].value, otherwise with --color-trend-down (#DC2626). The line reads as a sequence of
       short green/red segments reflecting each step's direction. Implement as multiple two-point <Line> series or
       via a per-segment stroke callback — whichever cleanly produces the per-segment colors. Keep dots consistent
       with each segment's own color.
     - All data comes from GET /api/reports/dashboard; show loading/error/empty states. Uses the Navbar and PageWrapper.
  f. [Entity1]Page (route /[entity1route]) — INSERT, SELECT, UPDATE, and record-level DELETE: form fields per Entity1;
     auto-calculate and show read-only any computed field; below the form, a shadcn Table with per-row Edit (Pencil)
     and Delete (Trash2). Edit opens a pre-filled Dialog; Delete shows an AlertDialog confirmation, then DELETEs that
     one record only (never the project/database).
  g. [Entity2]Page (route /[entity2route]) — INSERT, SELECT, UPDATE, and record-level DELETE: an [Entity1] dropdown
     (populated from the API) plus the remaining fields; date fields max = today; a Table with per-row Edit (Pencil)
     and Delete (Trash2). Edit opens a pre-filled Dialog; Delete shows an AlertDialog confirmation, then DELETEs that
     one record only (never the project/database).
  h. [Entity3]Page (route /[entity3route]) — INSERT, SELECT, UPDATE, and record-level DELETE: an [Entity1] dropdown +
     all fields; if a quantity-vs-stock rule applies, the quantity must not exceed the selected [Entity1]'s available
     stock (fetch stock when the dropdown changes); date fields max = today; a Table with per-row Edit (Pencil) and
     Delete (Trash2). Edit opens a pre-filled Dialog; Delete shows an AlertDialog confirmation, then DELETEs that one
     record only (never the project/database).
  EVERY domain entity page follows this same Create + Edit (Dialog) + Delete (AlertDialog) pattern — no INSERT-only pages.
  i. ReportsPage (route /reports): shadcn Tabs, ONE TAB PER REPORT (those in the brief, or the two you designed), each
     rendering its results in a shadcn Table with that report's columns and a filter control (default: a date picker =
     today). Empty state with a lucide icon when no data. Each report has a Print button (lucide Printer, window.print())
     with a print-only header (system name, report title, "Printed on: [datetime]"); filters/nav get the .no-print class;
     the table prints clean (see DESIGN @media print).
  j. Navbar (shared; rendered on all pages except Landing/Login/Register/Recover): links Dashboard, [Entity1], [Entity2],
     [Entity3], Reports, Logout; a light/dark theme toggle (Sun/Moon); the active link is visually distinct (accent
     underline/background); Logout calls useAuth().logout() (POST /api/auth/logout) then → /; collapses to a hamburger
     (Menu) on mobile as a vertical dropdown that closes on link click.
5. ProtectedRoute: wrap all routes except / /login /register /recover; read { user, loading } from useAuth() — show a
   loading state while checking, redirect to /login if there is no user. Public pages never trigger an auth redirect.

=== INPUT VALIDATION (every form; client-side before the API call AND mirrored server-side) ===
- Validate every required field before calling the API.
- Name fields: letters, spaces, hyphens, and apostrophes only (so "O'Brien"/"Jean-Paul" work); trimmed; ≥2 chars per
  word. Regex: /^[A-Za-z]+([ '-][A-Za-z]+)*$/
- Email: /^[^\s@]+@[^\s@]+\.[^\s@]+$/
- Phone Number: exactly 10 digits, starting with 078, 079, 073, or 072. Regex: /^(078|079|073|072)\d{7}$/
- Recovery Code: exactly 4 digits. Regex: /^\d{4}$/
- Rwandan Plate Number: format RLL NNN L — R + two uppercase letters + three digits + one uppercase letter; accept
  case-insensitively, uppercase and normalize spaces, store like "RAB 123 C". Normalized regex /^R[A-Z]{2} \d{3} [A-Z]$/
  (space-tolerant /^R[A-Z]{2}\s?\d{3}\s?[A-Z]$/ after uppercasing). Optionally exclude the letter O only if required.
- Number fields: positive integers/decimals > 0. Dropdowns: a selection is required. Date fields: not in the future
  (HTML max = today's ISO date). [Entity3 quantity]: must not exceed the selected [Entity1]'s available stock.
- Show inline error text below each invalid field (red, small, shadcn style). Disable Submit while the request is in
  flight; reset the form after success; prevent duplicate submits.

=== ERROR HANDLING ===
Wrap every API call in try/catch/finally. Show server errors in a destructive shadcn Alert above the form; show success
in a green-styled default Alert. Show Loader2 (spin) inside the Submit button while loading. On network error show
"Unable to connect to the server. Please try again." Log raw errors with console.error; never expose raw error objects.

=== TOAST NOTIFICATIONS (one per action result) ===
Use sonner: trigger toast.success/error/info on the RESULT of EVERY action (in addition to inline Alerts) — login,
register, logout, recovery-code generated ("Save your recovery code — it won't be shown again"), code copied/downloaded,
recover-verify failure, password reset, create/update/delete on EVERY entity (success + failure), validation blocks, and
network errors. One toast per result, auto-dismiss, dismissible, optional lucide icon, no emoji. <Toaster/> mounted once in App.jsx.

=== DESIGN & UI RULES ===
- Define a full color palette as CSS variables (index.css or Tailwind config): --color-bg, --color-surface,
  --color-accent, --color-text, --color-muted, --color-danger, --color-success. Use ONLY these variables — never
  hardcode hex inline. Trend tokens (exact values): --color-trend-up: #16A34A;  --color-trend-down: #DC2626;
  (Coloring is per-segment: up token when the next point rose, down token when it fell — see DashboardPage.)
- LIGHT & DARK MODE via a `dark` class on <html> (Tailwind v3 darkMode: 'class'). Define every token for BOTH themes
  (light values on :root, dark overrides under .dark). Background token MUST be exactly: light --color-bg: #F6F8FF;
  dark --color-bg: #0A0A0A. Choose the other tokens to pair tastefully and meet WCAG AA contrast in both; the trend
  tokens stay identical in both themes.
- The accent (--color-accent) MUST be ONE of: #094074 | #0D2149 | #003F91 — you pick whichever works best with the
  backgrounds and contrast (a slightly lighter tint allowed in dark mode for #0A0A0A). Use the accent consistently for: active nav
  link, primary buttons, focus rings, table row highlights, and badge backgrounds.
- A light/dark theme toggle (lucide Sun/Moon) appears in the Navbar and the LandingPage top bar.
- No purple gradients on white. No emoji — lucide icons only, consistent sizing (size 16 or 18).
- A distinctive Google font applied globally (e.g. Outfit, Figtree, or Space Grotesk — not generic Arial/Inter).
- Entity (authenticated) forms — the create/edit forms on the [Entity1/2/3] pages — use identical padding (p-6),
  field gap (gap-4), label style, and input height for visual coherence inside the working app. This uniformity
  rule applies ONLY to entity forms; the public auth pages (Login, Register, Recover) are exempt and may use any
  layout that suits the design (split-screen, side-panel, full-bleed, centered card, editorial, etc.) as long as
  they meet ACCESSIBILITY and use the design tokens + shadcn components. All AUTHENTICATED pages share one
  page-wrapper class (consistent max-width + horizontal padding). Button variants stay consistent everywhere:
  default (primary), destructive (delete confirm), outline (cancel). Tables wrapped in overflow-x-auto. Entity
  form cards max-w-xl centered (full-width with px-4 on mobile); public auth pages are exempt. Responsive via
  Tailwind sm:/md:/lg: only.
- @media print rules (in index.css): `.no-print { display:none !important }` on nav/filters/buttons; `.print-only`
  hidden normally and shown in print (the report header); report tables print plain black-on-white, no shadows/rounding, not clipped.

=== API INTEGRATION LAYER ===
src/api/axiosClient.js: baseURL = import.meta.env.VITE_API_URL or http://localhost:5000/api; withCredentials:true; a
  response interceptor that redirects to /login on 401.
authAPI.js: register, login, logout, getMe, recoverVerify, recoverReset.
[entity1..3]API.js: create/getAll/update/delete for every entity. reportsAPI.js: one fetcher per report + getDashboard().
Each function is async, calls axiosClient, and returns the { data } payload (response.data.data); it throws so the
caller's catch shows the { error } message. AuthContext wraps authAPI and exposes auth via useAuth(); auth UI uses
useAuth(); entity/report pages call their api modules directly.

=== SESSION-BASED AUTH ===
express-session({ secret: process.env.SESSION_SECRET, resave:false, saveUninitialized:false, cookie:{ httpOnly:true } })
(set secure + an appropriate sameSite in production). Session stores { userId, username }. The frontend axiosClient sends
withCredentials on every request. AuthContext calls /me on load (401 → /login). Users self-register; password recovery
uses the 4-digit code flow. An idempotent admin seed runs on startup (see SEEDING).

=== REPORTS LOGIC (MongoDB aggregation ONLY) ===
One aggregation pipeline per report — implement those in the brief (honoring name/shows/grouping/columns/filter, blanks
defaulted), else design two. Use $match (default date filter = today), $lookup (join referenced entities), $group
(totals), $sort, and $project (output columns). No raw queries, no SQL. The dashboard endpoint likewise aggregates the
stat-card metrics, the summary-block rows, and the time series for the line chart.

=== CODE QUALITY ===
Clean, readable, consistently formatted; one responsibility per file/function. Descriptive names (PascalCase
components/models, camelCase variables/functions, UPPER_SNAKE constants, files match their default export). No dead code,
no commented-out blocks, no leftover console.log (console.error for real errors only), no magic numbers/strings (hoist to
named constants). Small focused functions; extract repeats into helpers. Keep layers separate (controllers hold logic,
routes are thin, models hold schema; components stay presentational, data/auth live in api modules/AuthContext/hooks).
async/await with try/catch throughout. Consistent module style; ordered imports (external then internal).

=== SEEDING ===
Run an idempotent seed on startup (insert only when the collection is empty, so restarts never duplicate). Seed one admin
if Users is empty: bcrypt the password, set a valid email and phone (078/079/073/072 + 7 digits), and set the recovery code
from SEED_ADMIN_RECOVERY_CODE (stored hashed — a known 4-digit code so the recovery flow is always demoable); log only the
username (never the password or code). Read SEED_ADMIN_USERNAME / SEED_ADMIN_PASSWORD / SEED_ADMIN_RECOVERY_CODE from env
(labelled dev defaults only; never hardcode real credentials). REQUIRED: also seed a few valid sample rows per entity
(respecting relationships and validation, with some rows dated today) so tables, the dashboard, and reports render non-empty
on first run. Seeding must not block or interfere with normal registration.

=== REQUIRED ENVIRONMENT VARIABLES ===
All secrets/environment values come from process.env via dotenv — never hardcoded. backend .env.example lists each with a
placeholder + comment:
  PORT=5000
  MONGODB_URI=mongodb://localhost:27017/[DATABASE_NAME]
  SESSION_SECRET=change-me-to-a-long-random-string
  CLIENT_URL=http://localhost:5173        # CORS origin
  NODE_ENV=development
  SEED_ADMIN_USERNAME=admin
  SEED_ADMIN_PASSWORD=change-me
  SEED_ADMIN_RECOVERY_CODE=1234           # known 4-digit code for the seeded admin (stored hashed)
frontend .env.example: VITE_API_URL=http://localhost:5000/api
server.js validates MONGODB_URI + SESSION_SECRET at startup (clear console.error + exit if missing). Only .env.example is
delivered (never a real .env).

=== DATA-FETCHING UI ===
Every data view has explicit loading (Loader2 spin or skeleton rows; disable dependent actions), error (destructive Alert
with a Retry button; console.error the raw error), and empty (centered lucide icon + short message) states — never a blank
screen. Fetch lists on mount; refresh the affected list after a successful create/update/delete. Dependent dropdowns fetch
on mount (loading/disabled until ready) and refetch on selection change. Network-failure messaging matches ERROR HANDLING.

=== TABLE FEATURES ===
All record tables use the shadcn Table in overflow-x-auto. Provide column headers, zebra/hover styling using the accent,
and the loading/empty states. A client-side search box above each table; sortable columns (clicking a header toggles
asc/desc with a chevron); pagination (or lazy loading) when a table can exceed ~25 rows. Human-format values (dates,
numbers/currency with separators, booleans/enums as accent Badges). EVERY entity table has per-row Edit/Delete; Edit
opens a pre-filled Dialog and Delete always goes through the AlertDialog confirmation before calling the record-level
DELETE endpoint.

=== REUSABILITY ===
Build small reusable pieces (PageWrapper for max-width/padding, FormField = Label + Input + inline error, DataTable for
shared table behavior, ConfirmDialog wrapping AlertDialog, StateBlock for loading/error/empty); pages compose them.
Centralize cross-cutting concerns: validation regexes/helpers in one module, formatting helpers (date/number) in one
module, route paths and option lists as named constants. Reuse the api modules and AuthContext everywhere; never duplicate
axios calls or the /me check. A change to a shared rule should require editing ONE place.

COMPONENT DEFINITION RULE (prevents input-focus loss): EVERY React component — especially FormField, DataTable,
ConfirmDialog, PageWrapper, and any wrapper around Input/Textarea/Select — MUST be declared at MODULE TOP LEVEL
(or in its own file), NEVER inside another component's function body and NEVER inline in JSX. Defining a component
inside a parent creates a new component identity on every render, so React unmounts and remounts the input on every
keystroke — the input loses focus and the user can only type one character at a time. Do NOT write
`const FormField = (...) => ...` (or `function FormField`) inside a page component. Keep input identity stable:
controlled inputs always have a defined `value` (never undefined→defined, which remounts the input);
use stable keys from data (record._id), never Math.random()/Date.now()/array index; declare static options and
validation regexes at module scope (or useMemo) so they don't get a new reference each render.

=== SECURITY ===
Passwords and recovery codes are ALWAYS bcrypt-hashed — never plaintext, never logged, never returned (except the one-time
recovery code at register/reset). Never send the password hash or recoveryCodeHash to the client (use select:false or
projection; strip them from any returned user). Validate and sanitize ALL input server-side; compute derived fields
server-side only. Use httpOnly session cookies (secure + sameSite in production). CORS allows only CLIENT_URL with
credentials:true — never a wildcard. Return generic auth/recovery errors (don't reveal whether a username/email exists);
throttle login and recovery attempts (especially the 4-digit code). Never expose raw errors, stack traces, or DB internals.
Guard NoSQL injection: use Mongoose model methods with typed/validated inputs; never build queries from raw request bodies.

=== AUTHORIZATION (apply when roles are needed) ===
requireAuth enforces login on every non-auth route. If the domain implies permission levels (e.g. admin vs regular user),
add a `role` field to Users and a requireRole(...roles) middleware; protect sensitive/mutating endpoints and return 403
(not 401) when a logged-in user lacks the role. Hide disallowed UI actions (read role from AuthContext) AND enforce on the
backend (the server is the source of truth). If the domain has no meaningful role distinction, use a single role and say so
in the assumptions summary — don't invent unnecessary roles.

=== ACCESSIBILITY ===
Every input has an associated <Label> (htmlFor/id); icon-only buttons have aria-label. Validation errors are programmatically
associated (aria-describedby) and announced (role="alert" / aria-live), never conveyed by color alone. Full keyboard
operability: logical tab order, visible accent focus rings, Enter submits, Esc closes dialogs; dialogs trap and restore
focus. WCAG AA contrast in BOTH themes. Use semantic landmarks (nav, main, header/footer) and meaningful alt text. Respect
prefers-reduced-motion for spinners/animations.

=== PERFORMANCE ===
Backend: index fields that are queried/sorted/filtered often and unique fields (username, email, phone); use lean queries +
projections; do filtering/aggregation in MongoDB (pipelines), not JS over large arrays; avoid N+1 ($lookup/populate);
paginate large list/report endpoints (no unbounded result sets). Frontend: avoid needless re-renders/refetches; fetch
dependent data only when inputs change; memoize expensive derived values (useMemo) and stable callbacks (useCallback) where
it matters; debounce search inputs; lazy-load heavier routes; clean up effects (abort in-flight requests / ignore stale
responses on unmount).

=== STRICT LIBRARY RULES ===
Backend ONLY: express, cors, mongoose, bcryptjs, express-session, dotenv, nodemon (dev); express-rate-limit permitted solely
for throttling. Frontend ONLY: react, react-dom, react-router-dom, axios, tailwindcss, shadcn/ui, lucide-react, sonner,
recharts (dashboard chart), and shadcn's deps (class-variance-authority, clsx, tailwind-merge). MongoDB + Mongoose only for
data. lucide icons only; recharts only (used solely on the Dashboard, as a LINE chart). Do NOT add any other dependency (no
moment, lodash, Redux/Zustand, or other UI/chart libraries) — solve with the allowed stack or note it in the assumptions
summary. Pin EXACT, mutually-compatible versions in both package.json files (no risky ^ majors): react-router-dom v6 (NOT
v7); Tailwind CSS v3 (NOT v4) with the matching postcss.config.js, tailwind.config.js, and v3 @tailwind directives; Vite +
@vitejs/plugin-react compatible with React 18. Every import must resolve to a listed, installed dependency — no deprecated
APIs, no phantom imports.

=== ARCHITECTURE ===
Backend follows a layered flow per request: route (thin: path + middleware → controller) → controller (request handling,
validation, status codes, JSON) → model (schema + data access). Never skip or merge layers; cross-cutting logic (auth/role
checks) lives in /middleware. Single source of truth per concern. Frontend dependency direction: pages/components →
hooks/AuthContext → api modules → axiosClient → backend (UI never calls axios directly). Stateless backend (no server state
beyond the session store; no module-level mutable caches). Clear API contract: noun resources, correct verbs/status codes,
and the response shape with NO exceptions — { data } on success, { error } on failure (lists return { data:[...] }, single
records { data:{...} }); the frontend reads response.data.data. Configuration is injected via env vars. Frontend and backend
are independently runnable and communicate only over the HTTP API (no shared imports). Each file lives in the folder matching
its responsibility.

=== PROJECT README ===
Generate a single root [RootFolderName]/README.md in clean Markdown, accurate to the app you built (real names, entities,
routes). Include: project title + a one-paragraph description; tech stack (React 18 + Vite, react-router-dom v6, Node/Express,
MongoDB/Mongoose, Tailwind v3 + shadcn/ui + lucide-react, recharts); a short features list (auth with registration + 4-digit
recovery, a dashboard with stat cards and a chart, per-entity CRUD, reports with print); a brief structure tree; prerequisites
(Node LTS, a running MongoDB); copy-pasteable setup/run steps for BOTH projects (npm install, cp .env.example to .env, npm run
dev; API on :5000, app on :5173); an env-var table (no real secrets); a seeded-admin note (credentials from the SEED_ADMIN_*
env vars, never printing a real password); an API reference (method + path + one-line purpose, grouped by resource); an
account-recovery note (how the 4-digit code works, the reset flow, and that the seeded admin's code comes from
SEED_ADMIN_RECOVERY_CODE so it can be demoed); and a "Build Phases" section recapping each phase (1-10) you followed, one
truthful sentence each using the real names. Keep it accurate, free of placeholder brackets, and without license/contribution boilerplate.

=== DELIVERABLES ===
Output complete, immediately runnable code for every file. No TODOs, no incomplete functions, no hardcoded credentials.

RUNS-IMMEDIATELY GUARANTEE: the project must run with ZERO code edits after this exact sequence:
  1. cd backend-project && npm install
  2. cp .env.example .env   (fill MONGODB_URI / SESSION_SECRET if needed)
  3. ensure MongoDB is running (local mongod or an Atlas URI in .env)
  4. npm run dev            (backend on http://localhost:5000)
  5. cd ../frontend-project && npm install
  6. cp .env.example .env
  7. npm run dev            (frontend on http://localhost:5173)
Every frontend API call maps to a real backend route; every route maps to a real controller function; every import
resolves; each package.json has a "dev" script (backend nodemon, frontend vite).

NO PER-FILE TRUNCATION: every file is delivered complete — never truncate, summarize, or write "same as above" in place
of real code. You MAY spread files across multiple messages if there's too much for one (continue on your own at clean
file boundaries); that is about length only and never licenses abbreviating any file.

Default assumes 3 domain entities — scale the per-entity files (one model, controller, routes file, api file, and page each)
up or down to match the real count; keep all shared files; use REAL entity names in filenames (Student.js,
student.controller.js, StudentPage.jsx), not the literal "[Entity1]".

Root:
  README.md
Backend:
  package.json ; .env.example ; server.js ; config/db.js ; config/seed.js (idempotent admin + sample seed, called from
  server.js) ; models/User.js + one model per entity ; controllers/{auth,[entity1..3],reports}.controller.js (reports
  includes the /dashboard handler) ; routes/{auth,[entity1..3],reports}.routes.js (reports = dashboard + per-report) ;
  middleware/requireAuth.js (+ middleware/requireRole.js only if the domain needs roles)
Frontend:
  package.json (pinned: React 18, react-router-dom v6, Tailwind v3, recharts) ; vite.config.js (React plugin + `@/`→src) ;
  jsconfig.json (`@/*`) ; postcss.config.js (Tailwind v3 + autoprefixer) ; .env.example ; tailwind.config.js (v3,
  darkMode:'class') ; src/index.css (v3 directives + tokens + font + @media print) ; src/lib/utils.js (cn) ;
  src/components/ui/* (every shadcn component you import, in full) ; src/App.jsx (BrowserRouter + AuthProvider + routes +
  ProtectedRoute + global Toaster) ; src/context/AuthContext.jsx ; src/api/{axiosClient,authAPI,[entity1..3]API,
  reportsAPI}.js (reportsAPI includes getDashboard) ; src/components/{Navbar,ProtectedRoute}.jsx ;
  src/pages/{LandingPage,LoginPage,RegisterPage,RecoverPage,DashboardPage,[Entity1..3]Page,ReportsPage}.jsx

=== FINAL OUTPUT RULES ===
- Work through the WORKFLOW PHASES in order. Output: (1) the assumptions summary (Phases 1-2); (2) the Mermaid ERD (Phase 2);
  (3) every file, built in phase order, including README.md whose "Build Phases" recaps what you did (Phases 3-10).
- Entities, attributes, and relationships are NOT provided (beyond the brief) — design and infer them yourself; don't ask
  the human to confirm them unless the domain is unpickable.
- LandingPage (/) is public with no session; LoginPage /login, RegisterPage /register, and RecoverPage /recover are public
  too. After login/registration the user lands on /dashboard.
- Do NOT include any feature to delete the project, drop the database, or bulk-wipe collections. The only deletion permitted
  is a single record on any entity via DELETE /api/[that entity's route]/:id — never multi-record or whole-collection.
- Do NOT end with similar-project suggestions or "next steps." Stop once the assumptions, ERD, README, and all files are delivered.
