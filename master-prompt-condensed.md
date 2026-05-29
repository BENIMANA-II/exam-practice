=== PROJECT BRIEF (fill in the blanks; leave any blank to let the AI decide) ===
Project description: ______________  (1-2 sentences, e.g. "A school fee management system for a primary school")
System name (optional): ______________
Company/organization (optional): ______________
Database name (optional): ______________
Entities to use (optional): ______________  (e.g. "Student, Invoice, Payment"; blank = AI chooses)
Layout style (optional): ______________  (e.g. "minimal"/"editorial"/"playful"/"corporate"/"bold"/"modern"; blank = AI picks. Nudge for the PUBLIC pages, not a strict template.)
Reports (optional — copy block per report; blank = AI designs 2):
  Report 1 — Name: ____  | What it shows: ____  | Group/total by: ____ (opt) | Columns: ____ (opt) | Filter by: ____ (opt, e.g. "date (default today)")
  Report 2 — Name: ____  | What it shows: ____  | Group/total by: ____ (opt) | Columns: ____ (opt) | Filter by: ____ (opt)

=== INSTRUCTIONS TO THE AI ===
You are a senior full-stack developer. Build the complete app described in the PROJECT BRIEF. The brief
is the only required input; design everything else yourself. Any blank brief field and every [SQUARE
BRACKET] placeholder is a value YOU derive with a sensible default; honor any filled-in field exactly.
Don't ask the human for more unless the description is too vague to pick a domain — otherwise assume and proceed.

Design yourself: (1) system/database/root-folder names; (2) entities — those in the brief if given, else
a sensible set (default 3 domain entities + Users; fewer/more if needed); (3) each entity's attributes,
types, constraints (required, unique, numeric, date, dropdown, computed); (4) all relationships, inferred
from the attributes (see below); (5) reports — exactly those in the brief (one tab + endpoint + aggregation
each, honoring name/columns/grouping/filter, blanks defaulted), else design 2 for the domain; (6) routes,
pages, endpoints per the patterns here. Before any code, output a brief ASSUMPTIONS SUMMARY (system name,
entities/attributes, inferred relationships, reports) then the ERD, then build.

Infer relationships yourself (the human supplies none): treat any attribute naming another entity (ends in
Id/Ref, or another entity's name) as a foreign-key ref; a ref on B → A means A has-many B (one-to-many);
mutual refs or a join entity = many-to-many; a value computed from other fields is a derived field, not a
relationship. State one line of reasoning per relationship before the ERD. The ERD (Mermaid erDiagram) must
show all entities, attributes with types, _id as PK on each, ref fields as FK relationships, correct
cardinality (||--o{, etc.), and relationship labels. Do NOT ask the human to draw or confirm it.

=== WORKFLOW PHASES (follow in order; don't start a later phase's code before earlier design phases are done) ===
1 Analyze & Plan: decide names/entities/attributes, reports, dashboard metrics; output assumptions summary.
2 Model & Relationships: infer relationships; output reasoning + Mermaid ERD.
3 Backend Foundation: package.json, server.js (cors, JSON, session, env validation, mongoose.connect, listen), config/db.js, .env.example.
4 Models: one Mongoose model per collection with inferred refs, indexes, unique constraints, sensitive-field handling.
5 Backend Logic: controllers (all logic, validation, {data}/{error}), thin routes, requireAuth (+requireRole if needed), session auth, register + 4-digit recovery, report + dashboard aggregations, idempotent seed (admin + sample data).
6 Frontend Foundation: package.json (pinned), vite.config, jsconfig, postcss.config, tailwind.config (v3, darkMode 'class'), index.css (tokens incl. exact bg/accent/trend colors + @media print), self-contained shadcn ui, lib/utils, axiosClient, AuthContext, useTheme.
7 Frontend API Layer: api modules (auth, per-entity, reports incl. getDashboard).
8 Pages & Components: Navbar, ProtectedRoute; pages Landing, Login, Register, Recover, Dashboard, entity pages, Reports; wire routing, validation, toasts, loading/error/empty states.
9 Polish & Verify: apply design/a11y/performance/reusability rules; confirm every import resolves and every API call maps to a real route/controller; ensure RUNS-IMMEDIATELY holds.
10 Documentation: root README.md incl. the "Build Phases" recap.

=== DATABASE RULE (NON-NEGOTIABLE) ===
MongoDB + Mongoose ONLY. Never substitute or fall back to MySQL/PostgreSQL/SQLite or any SQL/relational DB,
even if simpler or the schema looks relational. All modeling, queries, and aggregations are MongoDB/Mongoose.
Build with React.js (frontend) + Node.js/Express.js (backend) + MongoDB.

=== PROJECT STRUCTURE ===
[RootFolderName]/
  backend-project/   (Node + Express + Mongoose):  config/, models/, controllers/, routes/, middleware/
  frontend-project/  (React + Vite + Tailwind + shadcn/ui + lucide-react):  src/api/, src/context/, src/components/, src/pages/, src/lib/, src/components/ui/

=== DATABASE: [DATABASE_NAME] ===
YOU design entities + attributes; infer relationships and add Mongoose `ref` fields, `_id` PKs, and ObjectId
refs. Pattern below is an EXAMPLE — Users is always present; use however many real domain entities the project
needs (typically 2-4), named after the domain (e.g. Student, Invoice, Payment):
  Users({ _id, fullName, username:unique, email:unique, phone:unique, password, recoveryCodeHash })  // recoveryCodeHash = bcrypt hash of the 4-digit code
  [Entity1]({ _id, [domain fields], [computedField if needed] })
  [Entity2]({ _id, [domain fields], [dateField if relevant], [ref to another entity if implied] })
  [Entity3]({ _id, [domain fields], [computedField if relevant], [dateField if relevant], [refs to other entities if implied] })
Attributes naming another entity are FK-ref clues — YOU decide they're refs, add `ref`, set cardinality.
Computed fields (e.g. TotalPrice = Qty × UnitPrice) are calculated in the backend before saving — never trust the client.

=== BACKEND REQUIREMENTS ===
1. Use express, cors, mongoose, bcryptjs, express-session, dotenv, nodemon.
2. server.js: cors (origin = CLIENT_URL, credentials:true), JSON parser, session middleware, env validation
   (exit if MONGODB_URI or SESSION_SECRET missing), mongoose.connect() with success/failure logs, all routes registered, app.listen() with startup log.
3. One Mongoose model file per collection under /models.
4. Controller layer: all handler logic in /controllers (one file per resource: auth, [entity1..3], reports). Each
   exports named async handlers with try/catch, validation, Mongoose calls, status codes, JSON responses. /routes
   files are THIN: create router, map method+path to controller fn, attach middleware (requireAuth), export. No logic/DB in routes.
   Routes:
   - POST /api/auth/register  — validate fullName/email/phone/username/password server-side; hash password; reject duplicate
     username/email/phone (400); generate a random 4-digit recovery code, store only its bcrypt hash, return the plaintext code ONCE; start session.
   - POST /api/auth/login ; POST /api/auth/logout ; GET /api/auth/me (session user or 401)
   - POST /api/auth/recover/verify  — { username|email, recoveryCode }; bcrypt.compare vs recoveryCodeHash; generic error on mismatch; throttle attempts.
   - POST /api/auth/recover/reset   — { username|email, recoveryCode, newPassword }; re-verify, hash+save newPassword, issue a NEW 4-digit code returned once.
   - POST/GET /api/[entity1route] ; PUT /api/[entity1route]/:id ; DELETE /api/[entity1route]/:id
   - POST/GET /api/[entity2route] ; PUT /api/[entity2route]/:id ; DELETE /api/[entity2route]/:id
   - POST/GET /api/[entity3route] ; PUT /api/[entity3route]/:id ; DELETE /api/[entity3route]/:id
     (every DELETE = single record only — never the project/DB. EVERY domain entity exposes this same POST/GET/PUT/DELETE set; no INSERT-only entities.)
   - GET /api/reports/dashboard  — one aggregation: stat-card metrics (counts + ≥1 SUM/AVG), summary-block rows, and the chart TIME SERIES (ordered {date/label, value}).
   - GET /api/reports/<reportSlug>  — one per report; runs that report's MongoDB aggregation.
5. Mongoose model methods only — no raw/SQL queries.
6. Hash passwords + recovery codes with bcryptjs; compare with bcrypt.compare().
7. Correct status codes: 200/201/400/401/404/500.
8. Response shape, NO exceptions: success { data: ... }, failure { error: message } (try/catch on every route).
9. Protect all non-auth routes with a session-check middleware returning 401 if not logged in.

=== FRONTEND REQUIREMENTS ===
1. React + Vite. Install/configure: axios, react-router-dom, tailwindcss, lucide-react, shadcn/ui, recharts
   (dashboard chart), sonner (toasts). Render one global <Toaster/> in App.jsx.
2. Use shadcn/ui (Button, Input, Card, Table, Dialog, AlertDialog, Badge, Label, Tabs, Select, Alert, Toaster) for ALL UI — no manually-styled raw form elements.
2b. shadcn MUST be self-contained (no CLI assumed): either (a) generate every imported ui/ file in full
   (button, input, card, table, dialog, alert-dialog, badge, label, tabs, select, alert, sonner) plus deps
   (class-variance-authority, clsx, tailwind-merge, `cn` in src/lib/utils.js) and the `@/` alias in BOTH vite.config.js
   and jsconfig.json; or (b) fall back to plain Tailwind for any component that would be incomplete. Every import must resolve — no phantom imports.
3. lucide-react icons ONLY. No emoji.
3b. Auth via React Context (src/context/AuthContext.jsx): AuthProvider + useAuth() holding { user, loading } and
   login/logout/register/checkAuth (GET /api/auth/me). Wrap app in <AuthProvider> in App.jsx inside BrowserRouter;
   hydrate via /me on mount. ProtectedRoute/Navbar/Login/Register/Recover use useAuth(), not direct API calls.
3c. Theme (light/dark) app-wide on EVERY page incl. public: useTheme()/ThemeProvider { theme, toggleTheme } toggling
   the `dark` class on <html>; on first load use saved localStorage "theme" else OS prefers-color-scheme; persist to
   localStorage on toggle (localStorage allowed for theme only); apply theme before first paint to avoid flash.

Pages:
  a. LandingPage (/) PUBLIC: marketing showcase for [SYSTEM NAME]. DO NOT default to the same generic hero + about +
     3-card grid + "how it works" + footer recipe every time — pick a layout that fits the domain and the optional brief
     "Layout style" (split-screen, full-bleed statement, stat strip, editorial, side-panel, classic hero+cards, etc.) and
     make it look meaningfully different from the AI's default each run. Required behavior regardless of layout: system
     name + one-line value prop present; an obvious primary "Sign In" CTA (lucide ArrowRight) → /login; communicates what
     the system does and its core capabilities (managing [Entity1], recording [Entity2], tracking [Entity3], viewing
     Reports — expressed however the layout best supports); fully responsive; uses ONLY the CSS-variable palette + global
     font; lucide icons only, no emoji, no lorem-ipsum. Own minimal top bar (system name left; theme toggle + Sign In
     right). Not in ProtectedRoute, no Navbar, no protected API, both themes.
  a2. PUBLIC PAGES MUST LOOK DISTINCTIVE: Landing/Login/Register/Recover must look meaningfully different from one another
     and reflect the system's domain. NOT all the same centered card on a plain background. Pick layouts that suit the
     project (split-screen, full-bleed, editorial, stat strip, single-statement, classic card, side-panel, etc.) while
     still using the design tokens and respecting accessibility. Token-level coherence stays; visual sameness across the
     four public pages does NOT.
  b. LoginPage (/login): Username, Password (Eye/EyeOff toggle). Success → /dashboard. Failure → inline destructive Alert.
     Links: "Register" → /register, "Forgot password?" → /recover, "Back to home" → /.
  c. RegisterPage (/register) PUBLIC: Full Name, Email, Phone, Username, Password (+toggle), Confirm Password (+toggle), all
     required, validated client- AND server-side (see INPUT VALIDATION). On success the API returns a one-time 4-digit recovery
     code: show a "Save your recovery code" step (Dialog/Card) displaying it large/monospace, a one-line "shown once, save it"
     note, Copy (lucide Copy) + Download (.txt named <systemname>-recovery-code.txt) buttons, and a required "I've saved it —
     continue" confirm that logs in → /dashboard. Failure → destructive Alert. Links to /login and /. Not in ProtectedRoute, no Navbar.
  d. RecoverPage (/recover) PUBLIC: Step 1 Verify — Username|Email + 4-digit code (/^\d{4}$/), POST /recover/verify, generic
     error on mismatch. Step 2 Reset — New Password + Confirm (match, min 6), POST /recover/reset → returns a NEW one-time code,
     show the same "Save your recovery code" step, then → /login. "Back to Sign In" link. Not in ProtectedRoute, no Navbar.
  e. DashboardPage (/dashboard) PROTECTED, post-login/register landing. MUST present real stats, not just the chart:
     - Stat cards: responsive grid (grid-cols-2 md:grid-cols-4), ≥4 shadcn Cards of real metrics (totals per entity, today's
       counts, ≥1 SUM/AVG e.g. total revenue), each lucide icon + accent, human-formatted numbers.
     - Summary block: a compact shadcn Table or short list (~5 rows) complementing the cards (recent records or per-item totals).
     - ONE recharts LINE chart (LineChart) of a metric over time (time X axis, value Y), ResponsiveContainer, axes/grid/tooltip.
       No bar/pie/other. Line color is TREND-BASED, applied PER SEGMENT (not whole-line): for each pair (p[i-1] → p[i]),
       color the segment with --color-trend-up (#16A34A) if p[i].value >= p[i-1].value, else --color-trend-down (#DC2626).
       Reads as short green/red segments reflecting each step's direction. Implement as multiple two-point <Line> series or
       a per-segment stroke callback. Dots match each segment's own color.
     - All data from GET /api/reports/dashboard; show loading/error/empty states. Uses Navbar + PageWrapper.
  f. [Entity1]Page (/[entity1route]) INSERT/SELECT/UPDATE/record-DELETE: form fields per Entity1; auto-calc + show read-only any computed field; below: shadcn Table with per-row Edit (Pencil) + Delete (Trash2). Edit opens a pre-filled Dialog; Delete shows AlertDialog confirm then DELETEs that one record only (never project/DB).
  g. [Entity2]Page (/[entity2route]) INSERT/SELECT/UPDATE/record-DELETE: [Entity1] dropdown (from API) + remaining fields; date fields max=today; Table with per-row Edit (Pencil) + Delete (Trash2). Edit opens a pre-filled Dialog; Delete shows AlertDialog confirm then DELETEs that one record only (never project/DB).
  h. [Entity3]Page (/[entity3route]) INSERT/SELECT/UPDATE/record-DELETE: [Entity1] dropdown + fields; if qty-vs-stock applies,
     qty ≤ available stock (fetch stock on dropdown change); date fields max=today; Table with per-row Edit (Pencil) + Delete (Trash2).
     Edit opens a pre-filled Dialog; Delete shows AlertDialog confirm then DELETEs that one record only (never project/DB).
  EVERY domain entity page follows this same Create + Edit (Dialog) + Delete (AlertDialog) pattern — no INSERT-only pages.
  i. ReportsPage (/reports): shadcn Tabs, ONE TAB PER REPORT (those in brief, else the 2 designed), each rendering a shadcn Table with
     that report's columns and a filter control (default: date picker = today). Empty state w/ icon when no data. Each report has a
     Print button (lucide Printer, window.print()) with a print-only header (system name, report title, "Printed on: [datetime]");
     filters/nav get .no-print; table prints clean (see DESIGN @media print).
  j. Navbar (shared; all pages except Landing/Login/Register/Recover): links Dashboard, [Entity1], [Entity2], [Entity3], Reports,
     Logout; a light/dark toggle (Sun/Moon); active link visually distinct (accent underline/bg); Logout via useAuth().logout()
     (POST /api/auth/logout) then → /; collapses to hamburger (Menu) on mobile as a vertical dropdown that closes on link click.
5. ProtectedRoute: wrap all routes except / /login /register /recover; read { user, loading } from useAuth() — show loading state,
   redirect to /login if no user. Public pages never trigger an auth redirect.

=== INPUT VALIDATION (every form; client-side before the call AND mirrored server-side) ===
- Validate every required field before calling the API.
- Name fields: letters, spaces, hyphens, apostrophes only; trimmed; ≥2 chars/word. /^[A-Za-z]+([ '-][A-Za-z]+)*$/
- Email: /^[^\s@]+@[^\s@]+\.[^\s@]+$/
- Phone: exactly 10 digits, prefix 078/079/073/072. /^(078|079|073|072)\d{7}$/
- Recovery code: exactly 4 digits. /^\d{4}$/
- Rwandan plate: RLL NNN L — R + 2 letters + 3 digits + 1 letter; uppercase + normalize spaces; store "RAB 123 C".
  Normalized /^R[A-Z]{2} \d{3} [A-Z]$/ (space-tolerant /^R[A-Z]{2}\s?\d{3}\s?[A-Z]$/ after uppercasing). (Letter O optional to exclude.)
- Numbers: positive integers/decimals > 0. Dropdowns: a selection required. Dates: not future (HTML max=today's ISO).
- [Entity3 qty]: must not exceed selected [Entity1]'s available stock (fetch stock on dropdown change).
- Inline error text below each invalid field (red, small, shadcn). Disable Submit while in flight; reset form on success; no duplicate submits.

=== ERROR HANDLING ===
Wrap every API call in try/catch/finally. Server errors → destructive shadcn Alert above the form; success → green-styled
default Alert. Loader2 (spin) in the Submit button while loading. Network error → "Unable to connect to the server. Please try
again." Log raw errors with console.error; never expose raw error objects to the user.

=== TOAST NOTIFICATIONS (one per action result) ===
Use sonner: toast.success/error/info on the RESULT of EVERY action (in addition to inline Alerts): login, register, logout,
recovery-code generated ("Save your recovery code — it won't be shown again"), code copied/downloaded, recover verify failure,
password reset, create/update/delete on EVERY entity (success + failure), validation blocks, network errors. One toast per result,
auto-dismiss, dismissible, optional lucide icon, no emoji. <Toaster/> mounted once in App.jsx.

=== DESIGN & UI RULES ===
- Full palette as CSS variables (index.css/Tailwind): --color-bg, --color-surface, --color-accent, --color-text, --color-muted,
  --color-danger, --color-success. Use ONLY variables, never inline hex. Trend tokens (exact): --color-trend-up:#16A34A; --color-trend-down:#DC2626. (Coloring is PER-SEGMENT — see DashboardPage.)
- LIGHT & DARK MODE via `dark` class on <html> (Tailwind v3 darkMode:'class'). Define every token for BOTH themes (light on :root,
  dark under .dark). Background token exact: light --color-bg:#F6F8FF; dark --color-bg:#0A0A0A. Other tokens chosen to pair well and
  meet WCAG AA in both; trend tokens identical in both.
- Accent (--color-accent) MUST be ONE of: #094074 | #0D2149 | #003F91 — you pick the best for both backgrounds/contrast (a lighter
  tint allowed in dark mode). Accent used consistently for: active nav link, primary buttons, focus rings, table row highlights, badges.
- Theme toggle (Sun/Moon) in Navbar and the LandingPage top bar.
- No purple gradients on white. No emoji — lucide icons only, consistent size (16/18).
- Distinctive Google font applied globally (e.g. Outfit/Figtree/Space Grotesk — not generic Arial/Inter).
- Entity (authenticated) forms — the create/edit forms on the [Entity1/2/3] pages — use identical padding (p-6), field gap
  (gap-4), label style, input height. This uniformity applies ONLY to entity forms; the public auth pages (Login, Register,
  Recover) are exempt and may use any layout that suits the design (split-screen, side-panel, full-bleed, centered card,
  editorial, etc.) as long as they meet ACCESSIBILITY and use the design tokens + shadcn components. Authenticated pages
  share one page-wrapper (max-width + horizontal padding).
- Button variants consistent everywhere: default (primary), destructive (delete confirm), outline (cancel). Tables wrapped
  in overflow-x-auto. Entity form cards max-w-xl centered (full-width px-4 on mobile); public auth pages exempt. Responsive
  via sm:/md:/lg: only.
- @media print (index.css): `.no-print{display:none!important}` on nav/filters/buttons; `.print-only{display:none}` normally,
  `display:block` in print (the report header); report tables print plain black-on-white, no shadows/rounding, not clipped.

=== API INTEGRATION LAYER ===
src/api/axiosClient.js: baseURL = import.meta.env.VITE_API_URL || http://localhost:5000/api; withCredentials:true; response
  interceptor redirects to /login on 401.
authAPI.js: register, login, logout, getMe, recoverVerify, recoverReset.
[entity1..3]API.js: create/getAll/update/delete for every entity. reportsAPI.js: one fetcher per report + getDashboard().
Each fn is async, calls axiosClient, returns the { data } payload (response.data.data); throws so the caller's catch shows the
{ error } message. AuthContext wraps authAPI via useAuth(); auth UI uses useAuth(); entity/report pages call their api modules directly.

=== SESSION-BASED AUTH ===
express-session({ secret: process.env.SESSION_SECRET, resave:false, saveUninitialized:false, cookie:{ httpOnly:true } }) (secure+
sameSite in production). Session stores { userId, username }. Frontend axiosClient withCredentials on every request. AuthContext
calls /me on load (401 → /login). Users self-register; recovery via the 4-digit code flow. Idempotent startup seed (below).

=== REPORTS LOGIC (MongoDB aggregation ONLY) ===
One pipeline per report — implement those in the brief (name/shows/grouping/columns/filter, blanks defaulted) else design 2. Use
$match (default date filter = today), $lookup (join refs), $group (totals), $sort, $project (output columns). No raw queries/SQL.
The dashboard endpoint likewise aggregates the stat metrics, summary rows, and the time-series for the line chart.

=== CODE QUALITY ===
Clean, consistent, one responsibility per file/fn. Descriptive names (PascalCase components/models, camelCase vars/fns, UPPER_SNAKE
consts, files match default export). No dead code, no leftover console.log (console.error for real errors), no magic numbers/strings
(hoist to consts). Small focused fns; extract repeats. Layers stay separate (controllers=logic, routes=thin, models=schema; presentational
components, data/auth in api modules/AuthContext/hooks). async/await + try/catch throughout. Consistent module style; ordered imports.

=== SEEDING ===
Idempotent on startup (insert only when collection empty; restarts never duplicate). Seed one admin if Users empty: bcrypt password,
valid email + phone (078/079/073/072+7), recovery code from SEED_ADMIN_RECOVERY_CODE (stored hashed, a known 4-digit code so the
recovery flow is demoable); log only the username. Read SEED_ADMIN_USERNAME/PASSWORD/RECOVERY_CODE from env (labelled dev defaults
only). REQUIRED: seed a few valid sample rows per entity (respecting relationships/validation, some dated today) so tables/dashboard/
reports render non-empty. Seeding must not block registration.

=== REQUIRED ENV VARS ===
All secrets via process.env/dotenv, never hardcoded. backend .env.example (placeholder + comment each):
  PORT=5000 ; MONGODB_URI=mongodb://localhost:27017/[DATABASE_NAME] ; SESSION_SECRET=change-me-long-random ;
  CLIENT_URL=http://localhost:5173 (CORS origin) ; NODE_ENV=development ; SEED_ADMIN_USERNAME=admin ;
  SEED_ADMIN_PASSWORD=change-me ; SEED_ADMIN_RECOVERY_CODE=1234 (known 4-digit, stored hashed)
frontend .env.example: VITE_API_URL=http://localhost:5000/api
server.js validates MONGODB_URI + SESSION_SECRET at startup (clear console.error + exit if missing). Only .env.example is delivered.

=== DATA-FETCHING UI ===
Every data view has explicit loading (Loader2 spin or skeleton; disable dependent actions), error (destructive Alert + Retry;
console.error), and empty (centered lucide icon + message) states — never a blank screen. Fetch lists on mount; refresh affected list
after create/update/delete. Dependent dropdowns fetch on mount (loading/disabled until ready) and refetch on selection change. Network
failure message matches ERROR HANDLING.

=== TABLE FEATURES ===
shadcn Table in overflow-x-auto. Headers; zebra/hover using accent; loading/empty states. Client-side search box above each table;
sortable columns (header toggles asc/desc with chevron); pagination (or lazy load) past ~25 rows. Human-format values (dates, numbers/
currency with separators, booleans/enums as accent Badges). EVERY entity table has per-row Edit/Delete; Edit opens a pre-filled Dialog and Delete always goes through the AlertDialog confirmation before calling the record-level DELETE endpoint.

=== REUSABILITY ===
Build reusable pieces (PageWrapper, FormField = Label+Input+error, DataTable, ConfirmDialog wrapping AlertDialog, StateBlock for
loading/error/empty); pages compose them. Centralize validation regexes/helpers, formatting helpers, and route/option constants in one
module each. Reuse api modules + AuthContext; never duplicate axios or the /me check. A shared-rule change should touch ONE place.

COMPONENT DEFINITION RULE (prevents input-focus loss): EVERY React component — especially FormField, DataTable, ConfirmDialog,
PageWrapper, and any wrapper around Input/Textarea/Select — MUST be declared at MODULE TOP LEVEL (or its own file), NEVER inside
another component's function body and NEVER inline in JSX. A component declared inside a parent gets a new identity on every render,
so React unmounts/remounts the input on every keystroke — the input loses focus and the user can only type one char at a time. Do
NOT write `const FormField = (...) => ...` (or `function FormField`) inside a page component. Keep input identity stable: controlled
inputs always have a defined `value` (never undefined→defined, which remounts as uncontrolled→controlled); use stable keys from data
(record._id), never Math.random()/Date.now()/array index; declare static options/regexes at module scope (or useMemo) so refs are stable.

=== SECURITY ===
Passwords + recovery codes always bcrypt-hashed; never plaintext, logged, or returned (except the one-time code at register/reset).
Never send password/recoveryCodeHash to the client (select:false or projection; strip from returned user). Validate + sanitize ALL
input server-side; compute derived fields server-side. httpOnly session cookies (secure + sameSite in prod). CORS = CLIENT_URL only,
credentials:true (no wildcard). Generic auth/recovery errors; throttle login + recovery (4-digit code) attempts. Never expose stack
traces/DB internals. Guard NoSQL injection: typed/validated inputs via Mongoose methods, never queries built from raw bodies.

=== AUTHORIZATION (when roles are needed) ===
requireAuth enforces login on every non-auth route. If the domain implies permission levels, add a `role` field + requireRole(...roles)
middleware; protect sensitive/mutating endpoints, return 403 when a logged-in user lacks the role. Hide disallowed UI actions (role from
AuthContext) AND enforce on the backend (server is source of truth). If no role distinction is needed, use a single role and say so in the assumptions summary.

=== ACCESSIBILITY ===
Every input has a <Label> (htmlFor/id); icon-only buttons have aria-label. Errors associated (aria-describedby) and announced (role=
"alert"/aria-live), never color-only. Full keyboard operability: logical tab order, visible accent focus rings, Enter submits, Esc
closes dialogs; dialogs trap + restore focus. WCAG AA contrast in both themes. Semantic landmarks (nav/main/header/footer), alt text.
Respect prefers-reduced-motion.

=== PERFORMANCE ===
Backend: index queried/sorted/unique fields (username/email/phone); lean queries + projections; filter/aggregate in MongoDB not JS;
avoid N+1 ($lookup/populate); paginate large list/report endpoints (no unbounded results). Frontend: avoid needless re-renders/refetches;
fetch dependent data only on input change; useMemo/useCallback where it matters; debounce search; lazy-load heavy routes; clean up
effects (abort/ignore stale on unmount).

=== STRICT LIBRARY RULES ===
Backend only: express, cors, mongoose, bcryptjs, express-session, dotenv, nodemon (dev); express-rate-limit allowed solely for throttling.
Frontend only: react, react-dom, react-router-dom, axios, tailwindcss, shadcn/ui, lucide-react, sonner, recharts (dashboard chart),
and shadcn deps (class-variance-authority, clsx, tailwind-merge). MongoDB+Mongoose only for data. lucide icons only; recharts only
(dashboard, LINE chart). No other deps (no moment/lodash/Redux/Zustand/other UI or chart libs) — solve with the allowed stack or note in
assumptions. EXACT pinned versions in both package.json (no risky ^ majors): react-router-dom v6 (NOT v7); Tailwind v3 (NOT v4) with
matching postcss.config.js + tailwind.config.js + v3 @tailwind directives; Vite + @vitejs/plugin-react compatible with React 18.
Every import resolves to a listed, installed dep — no deprecated APIs, no phantom imports.

=== ARCHITECTURE ===
Backend layered per request: route (thin: path+middleware→controller) → controller (handling, validation, status, JSON) → model
(schema+data). Never merge layers; cross-cutting logic in /middleware. Single source of truth per concern. Frontend direction:
pages/components → hooks/AuthContext → api modules → axiosClient → backend (UI never calls axios directly). Stateless backend (only the
session store; no module-level mutable cache). API contract: noun resources, correct verbs/status; response shape NO exceptions —
{ data } success / { error } failure (lists {data:[...]}, single {data:{...}}); frontend reads response.data.data. Config injected via
env. Frontend/backend independently runnable, communicate only over HTTP (no shared imports). Files live in the folder matching their responsibility.

=== PROJECT README ===
Generate root [RootFolderName]/README.md in clean Markdown, accurate to the built app (real names/entities/routes): title + 1-paragraph
description; tech stack (React 18 + Vite, react-router-dom v6, Node/Express, MongoDB/Mongoose, Tailwind v3 + shadcn/ui + lucide-react,
recharts); features (auth + 4-digit recovery, dashboard with stats + chart, per-entity CRUD, reports with print); brief structure tree;
prerequisites (Node LTS, MongoDB); copy-pasteable setup/run for BOTH projects (npm install, cp .env.example .env, npm run dev; API :5000,
app :5173); env var table (no real secrets); seeded admin note (creds from SEED_ADMIN_* env, no real password printed); API reference
(method+path+purpose by resource); account recovery note (4-digit code, reset flow, seeded code from SEED_ADMIN_RECOVERY_CODE);
"Build Phases" — recap each phase (1-10) you followed, one truthful sentence each using real names. Accurate, no placeholder brackets, no boilerplate.

=== DELIVERABLES ===
Complete, immediately runnable code for every file. No TODOs, no incomplete functions, no hardcoded credentials.
RUNS-IMMEDIATELY: must run with ZERO edits after: (1) cd backend-project && npm install (2) cp .env.example .env (fill MONGODB_URI/
SESSION_SECRET) (3) start MongoDB (4) npm run dev → :5000 (5) cd ../frontend-project && npm install (6) cp .env.example .env (7) npm run
dev → :5173. Every API call → real route → real controller fn; every import resolves; package.json "dev" script each (nodemon / vite).
NO PER-FILE TRUNCATION: every file complete — never truncate/summarize a file or write "same as above". You MAY split across multiple
messages for length (continue on your own at clean file boundaries); that never licenses abbreviating a file.
Default is 3 domain entities — scale per-entity files (model/controller/routes/api/page each) up/down to the real count; keep shared files;
use REAL entity names in filenames (Student.js, student.controller.js, StudentPage.jsx), not "[Entity1]".

Root: README.md
Backend: package.json ; .env.example ; server.js ; config/db.js ; config/seed.js (idempotent admin+sample seed, called from server.js) ;
  models/User.js + one per entity ; controllers/{auth,[entity1..3],reports}.controller.js (reports incl. /dashboard handler) ;
  routes/{auth,[entity1..3],reports}.routes.js (reports = dashboard + per-report) ; middleware/requireAuth.js (+ requireRole.js only if roles needed)
Frontend: package.json (pinned: React 18, react-router-dom v6, Tailwind v3, recharts) ; vite.config.js (React plugin + `@/`→src) ;
  jsconfig.json (`@/*`) ; postcss.config.js (Tailwind v3 + autoprefixer) ; .env.example ; tailwind.config.js (v3, darkMode:'class') ;
  src/index.css (v3 directives + tokens + font + @media print) ; src/lib/utils.js (cn) ; src/components/ui/* (every imported shadcn comp,
  full) ; src/App.jsx (BrowserRouter + AuthProvider + routes + ProtectedRoute + global Toaster) ; src/context/AuthContext.jsx ;
  src/api/{axiosClient,authAPI,[entity1..3]API,reportsAPI}.js (reportsAPI incl. getDashboard) ; src/components/{Navbar,ProtectedRoute}.jsx ;
  src/pages/{LandingPage,LoginPage,RegisterPage,RecoverPage,DashboardPage,[Entity1..3]Page,ReportsPage}.jsx

=== FINAL OUTPUT RULES ===
- Work through WORKFLOW PHASES in order. Output: (1) assumptions summary (Phases 1-2); (2) Mermaid ERD (Phase 2); (3) every file, built in
  phase order, incl. README.md whose "Build Phases" recaps what you did (Phases 3-10).
- Entities/attributes/relationships are NOT given (beyond the brief) — design/infer them; don't ask to confirm unless the domain is unpickable.
- LandingPage (/) public, no session; LoginPage /login, RegisterPage /register, RecoverPage /recover all public. Post-login/register → /dashboard.
- No feature to delete the project, drop the DB, or bulk-wipe collections. Only deletion permitted = a single record on any entity via DELETE /api/[that entity's route]/:id — never multi-record or whole-collection.
- Do NOT end with similar-project suggestions or "next steps." Stop once assumptions, ERD, README, and all files are delivered.
