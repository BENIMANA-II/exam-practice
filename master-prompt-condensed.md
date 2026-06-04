=== PROJECT BRIEF (fill in the blanks; leave any blank to let the AI decide) ===
Project description: ______________  (1-2 sentences, e.g. "A school fee management system for a primary school")
System name (optional): ______________
Company/organization (optional): ______________
Database name (optional): ______________
Entities to use (optional): ______________  (e.g. "Student, Invoice, Payment"; blank = AI chooses. You MAY give each entity WITH its attributes, e.g. "Student(fullName, regNo, class); Invoice(student, amount, dueDate, status); Payment(invoice, amount, paidOn)" — used VERBATIM; the AI only infers types/constraints/relationships and fills blanks.)
Per-entity operations (optional): ______________  (default: full CRUD on every entity. To restrict, name INSERT-ONLY entities (create form only, no edit/delete) vs full-CRUD ones, e.g. "insert-only: Product, Warehouse; full CRUD: StockTransaction".)
Layout style (optional): ______________  (e.g. "minimal"/"editorial"/"playful"/"corporate"/"bold"/"modern"; blank = AI picks. Nudge for the PUBLIC pages, not a strict template.)
Data visibility (optional): ______________  (blank = per-user: each user sees only records they create + seeded data belongs to admin only. "shared" = all users see one dataset; "admin sees all" = per-user but admin views/manages everyone's records.)
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
types, constraints (required, unique, numeric, date, dropdown, computed) — if the brief lists an entity's
attributes use EXACTLY those (verbatim — never rename/drop/add), inferring only types/constraints, and design
attributes yourself only for entities left blank; (4) all relationships, inferred
from the attributes (see below); (5) reports — exactly those in the brief (one tab + endpoint + aggregation
each, honoring name/columns/grouping/filter, blanks defaulted), else design 2 for the domain; (6) routes,
pages, endpoints per the patterns here. Before any code, output a brief ASSUMPTIONS SUMMARY (system name,
entities/attributes, inferred relationships, reports) then the ERD, then build.

Infer relationships yourself (the human may name entities/attributes but not relationships): treat any attribute naming another entity (ends in
Id/Ref, or another entity's name) as a foreign-key ref; a ref on B → A means A has-many B (one-to-many);
mutual refs or a join entity = many-to-many; a value computed from other fields is a derived field, not a
relationship. State one line of reasoning per relationship before the ERD. The ERD (Mermaid erDiagram) must
show all entities, attributes with types, _id as PK on each, ref fields as FK relationships, correct
cardinality (||--o{, etc.), and relationship labels. Do NOT ask the human to draw or confirm it. DELIVER it as a file too — ERD.md at the project root, a Markdown file whose body is the Mermaid erDiagram in a ```mermaid fenced block (renders on GitHub/VS Code) — and show the same diagram inline before the code; keep ERD.md consistent with the models you build.

=== WORKFLOW PHASES (follow in order; don't start a later phase's code before earlier design phases are done) ===
1 Analyze & Plan: decide names/entities/attributes, reports, dashboard metrics; output assumptions summary.
2 Model & Relationships: infer relationships; output reasoning + Mermaid ERD.
3 Backend Foundation: package.json, server.js (cors, JSON, session, env validation, mongoose.connect, listen), config/db.js, .env.example.
4 Models: one Mongoose model per collection with inferred refs, indexes, unique constraints, sensitive-field handling.
5 Backend Logic: controllers (all logic, validation, {data}/{error}), thin routes, requireAuth (+requireRole if needed), session auth, register + 4-digit recovery, report + dashboard aggregations, idempotent seed (admin + sample data).
6 Frontend Foundation: package.json (pinned), vite.config, jsconfig, postcss.config, tailwind.config (v3), index.css (tokens incl. exact bg/accent colors + @media print), self-contained shadcn ui, lib/utils, axiosClient, AuthContext.
7 Frontend API Layer: api modules (auth, per-entity, reports incl. getDashboard).
8 Pages & Components: Navbar, ProtectedRoute; pages Landing, Auth (Sign In/Sign Up at /login + /register), Recover, Dashboard, entity pages, Reports; wire routing, validation, toasts, loading/error/empty states.
9 Polish & Verify: apply design/a11y/performance/reusability rules; confirm every import resolves and every API call maps to a real route/controller; ensure RUNS-IMMEDIATELY holds.
10 Documentation: root README.md incl. the "Build Phases" recap.

=== DATABASE RULE (NON-NEGOTIABLE) ===
MongoDB + Mongoose ONLY. Never substitute or fall back to MySQL/PostgreSQL/SQLite or any SQL/relational DB,
even if simpler or the schema looks relational. All modeling, queries, and aggregations are MongoDB/Mongoose.
Build with React.js (frontend) + Node.js/Express.js (backend) + MongoDB.

=== PROJECT STRUCTURE (MANDATORY — USE THIS EXACT LAYOUT, AND ONLY THIS) ===
Use EXACTLY this folder structure — same folders, file names, and locations; add/rename/relocate nothing
beyond scaling the per-entity files to your real entity count (one model, controller, routes, API module,
and page per entity, named after the REAL entity, e.g. Student.js). Root folder name is FIXED:
BENIMANA_Irakiza_Jean_Flaubert_National_Practical_Exam_2026/

BENIMANA_Irakiza_Jean_Flaubert_National_Practical_Exam_2026/
│
├── README.md                         ← project doc + Build Phases recap
├── ERD.md                            ← the Mermaid erDiagram (entities, fields, _id PKs, FKs, cardinality)
│
├── backend-project/                  ← Node + Express + Mongoose
│   ├── package.json                  ← pinned versions, "dev" script (nodemon)
│   ├── .env.example                  ← documents all env vars (no real secrets)
│   ├── server.js                     ← app entry: cors, JSON, session, env check,
│   │                                   mongoose.connect, routes, app.listen
│   │
│   ├── config/
│   │   ├── db.js                     ← mongoose connection wrapper
│   │   └── seed.js                   ← idempotent admin + sample data seed
│   │                                   (called from server.js on startup)
│   │
│   ├── models/                       ← one Mongoose schema per collection
│   │   ├── User.js                   ← fullName, username, email, phone,
│   │   │                               password (hashed), recoveryCodeHash
│   │   ├── [Entity1].js              ← e.g. Student.js
│   │   ├── [Entity2].js              ← e.g. Course.js
│   │   └── [Entity3].js              ← e.g. Enrollment.js
│   │
│   ├── controllers/                  ← ALL business logic lives here
│   │   ├── auth.controller.js        ← register, login, logout, me,
│   │   │                               recoverVerify, recoverReset
│   │   ├── [entity1].controller.js   ← create, list, update, delete handlers
│   │   ├── [entity2].controller.js   ← create, list, update, delete handlers
│   │   ├── [entity3].controller.js   ← create, list, update, delete handlers
│   │   └── reports.controller.js     ← /dashboard aggregation + per-report
│   │                                   aggregation pipelines
│   │
│   ├── routes/                       ← THIN: only routers + middleware mapping
│   │   ├── auth.routes.js            ← auth paths → auth.controller fns
│   │   ├── [entity1].routes.js       ← entity1 paths → entity1.controller fns
│   │   ├── [entity2].routes.js       ← entity2 paths → entity2.controller fns
│   │   ├── [entity3].routes.js       ← entity3 paths → entity3.controller fns
│   │   └── reports.routes.js         ← /dashboard + per-report endpoints
│   │
│   └── middleware/
│       ├── requireAuth.js            ← session check, returns 401 if not logged in
│       └── requireRole.js            ← (only if the domain needs roles → 403)
│
└── frontend-project/                 ← React 18 + Vite + Tailwind v3 + shadcn/ui
    ├── package.json                  ← pinned exact versions: React 18,
    │                                   react-router-dom v6, Tailwind v3,
    │                                   Phosphor Icons; "dev" script (vite)
    ├── .env.example                  ← VITE_API_URL=http://localhost:5000/api
    ├── vite.config.js                ← React plugin + `@/` path alias → src
    ├── jsconfig.json                 ← editor `@/*` path mapping (matches vite)
    ├── postcss.config.js             ← Tailwind v3 + autoprefixer
    ├── tailwind.config.js            ← content globs, tokens
    ├── index.html                    ← Vite entry
    │
    └── src/
        ├── App.jsx                   ← BrowserRouter + <AuthProvider> +
        │                               <IconContext.Provider weight="regular"> +
        │                               routes + ProtectedRoute + global <Toaster />
        ├── main.jsx                  ← React render root
        ├── index.css                 ← @tailwind directives + CSS-variable palette
        │                               (accent tokens) + @media print
        │
        ├── lib/
        │   └── utils.js              ← cn() helper (clsx + tailwind-merge)
        │
        ├── context/
        │   └── AuthContext.jsx       ← AuthProvider + useAuth() hook
        │
        ├── api/                      ← only this folder talks to axios
        │   ├── axiosClient.js        ← baseURL, withCredentials, 401 interceptor
        │   ├── authAPI.js            ← register, login, logout, getMe,
        │   │                           recoverVerify, recoverReset
        │   ├── [entity1]API.js       ← create, getAll, update, delete
        │   ├── [entity2]API.js       ← create, getAll, update, delete
        │   ├── [entity3]API.js       ← create, getAll, update, delete
        │   └── reportsAPI.js         ← getDashboard() + one fetcher per report
        │
        ├── components/
        │   ├── Navbar.jsx            ← MINIMAL UNDERLINE TOP-BAR: brand + Dashboard + entity links
        │   │                           + Reports + user/role label + Logout + mobile dropdown
        │   ├── ProtectedRoute.jsx    ← reads useAuth(); redirects to /login if no user
        │   │
        │   └── ui/                   ← shadcn components generated in full (NOT npm)
        │       ├── button.jsx
        │       ├── input.jsx
        │       ├── card.jsx
        │       ├── table.jsx
        │       ├── dialog.jsx
        │       ├── alert-dialog.jsx
        │       ├── badge.jsx
        │       ├── label.jsx
        │       ├── tabs.jsx
        │       ├── select.jsx
        │       ├── alert.jsx
        │       └── sonner.jsx
        │
        └── pages/                    ← one file per screen
            ├── LandingPage.jsx       ← PUBLIC: hero (Get Started + Explore More), #services section
            ├── AuthPage.jsx          ← PUBLIC: Sign In + Sign Up on one page (segmented control),
            │                           rendered at BOTH /login and /register
            ├── RecoverPage.jsx       ← PUBLIC: verify code → reset password
            ├── DashboardPage.jsx     ← PROTECTED: ≥4 stat cards + summary + Quick Actions
            ├── [Entity1]Page.jsx     ← full CRUD: create + table + edit/delete
            ├── [Entity2]Page.jsx     ← full CRUD: create + table + edit/delete
            ├── [Entity3]Page.jsx     ← full CRUD: create + table + edit/delete
            └── ReportsPage.jsx       ← Tabs per report + date filter + Print button

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
Unless data visibility is "shared", every domain entity ALSO carries an `owner` (ObjectId ref → User, required, indexed) set server-side to the creating user — see DATA OWNERSHIP & VISIBILITY.

=== BACKEND REQUIREMENTS ===
1. Use express, cors, mongoose, bcryptjs, express-session, dotenv, nodemon.
2. server.js: cors (origin = CLIENT_URL, credentials:true), JSON parser, session middleware, env validation
   (exit if MONGODB_URI or SESSION_SECRET missing), mongoose.connect() with success/failure logs, all routes registered, app.listen() with startup log.
3. One Mongoose model file per collection under /models.
4. Controller layer: all handler logic in /controllers (one file per resource: auth, [entity1..3], reports). Each
   exports named async handlers with try/catch, validation, Mongoose calls, status codes, JSON responses. /routes
   files are THIN: create router, map method+path to controller fn, attach middleware (requireAuth), export. No validation/logic/DB in routes — all validation lives in the controllers.
   Routes:
   - POST /api/auth/register  — validate fullName/email/phone/username/password server-side; hash password; reject duplicate
     username/email/phone (400); generate a random 4-digit recovery code, store only its bcrypt hash, return the plaintext code ONCE; then start the session → { data: { user } }.
   - POST /api/auth/login (validate, start session → { data: { user } }) ; POST /api/auth/logout (destroy the session + clear the cookie) ; GET /api/auth/me (return the session user, or 401)
   - POST /api/auth/recover/verify  — { username|email, recoveryCode }; bcrypt.compare vs recoveryCodeHash; generic error on mismatch; throttle attempts.
   - POST /api/auth/recover/reset   — { username|email, recoveryCode, newPassword }; re-verify, hash+save newPassword, issue a NEW 4-digit code returned once.
   - POST/GET /api/[entity1route] ; PUT /api/[entity1route]/:id ; DELETE /api/[entity1route]/:id
   - POST/GET /api/[entity2route] ; PUT /api/[entity2route]/:id ; DELETE /api/[entity2route]/:id
   - POST/GET /api/[entity3route] ; PUT /api/[entity3route]/:id ; DELETE /api/[entity3route]/:id
     (every DELETE = single record only — never the project/DB. By DEFAULT every domain entity exposes this same POST/GET/PUT/DELETE set. EXCEPTION — the optional "Per-entity operations" brief field: an INSERT-ONLY entity gets ONLY POST (omit PUT/DELETE; keep GET only if another form needs it as a dropdown source); full-CRUD entities keep the complete set. State each entity's ops in the assumptions summary.)
   - GET /api/reports/dashboard  — one aggregation: stat-card metrics (counts + ≥1 SUM/AVG) and summary-block rows.
   - GET /api/reports/<reportSlug>  — one per report; runs that report's MongoDB aggregation.
5. Mongoose model methods only — no raw/SQL queries.
6. Hash passwords + recovery codes with bcryptjs; compare with bcrypt.compare().
7. Correct status codes: 200/201/400/401/404/500.
8. Response shape, NO exceptions: success { data: ... }, failure { error: message } (try/catch on every route).
9. Protect all non-auth routes with requireAuth — a session-check middleware that returns 401 if req.session has no userId (the user is not logged in).
10. Data scoping (default): scope every entity's create/list/update/delete AND the dashboard/report aggregations to the logged-in user as `owner` (create sets it server-side, reads filter by it, update/delete only touch own records → 404 otherwise); "shared" and "admin sees all" modes adjust this. See DATA OWNERSHIP & VISIBILITY.

=== FRONTEND REQUIREMENTS ===
1. React + Vite. Install/configure: axios, react-router-dom, tailwindcss, @phosphor-icons/react, shadcn/ui,
   sonner (toasts). Render one global <Toaster/> in App.jsx.
2. Use shadcn/ui (Button, Input, Card, Table, Dialog, AlertDialog, Badge, Label, Tabs, Select, Alert, Toaster) for ALL UI — no manually-styled raw form elements.
2b. shadcn MUST be self-contained (no CLI assumed): either (a) generate every imported ui/ file in full
   (button, input, card, table, dialog, alert-dialog, badge, label, tabs, select, alert, sonner) plus deps
   (class-variance-authority, clsx, tailwind-merge, `cn` in src/lib/utils.js) and the `@/` alias in BOTH vite.config.js
   and jsconfig.json; or (b) fall back to plain Tailwind for any component that would be incomplete. Every import must resolve — no phantom imports.
3. Phosphor icons ONLY (@phosphor-icons/react), regular weight throughout via a single IconContext.Provider at the app root. No emoji.
3b. Auth via React Context (src/context/AuthContext.jsx): AuthProvider + useAuth() holding { user, loading } and
   login/logout/register/checkAuth (GET /api/auth/me). Wrap app in <AuthProvider> in App.jsx inside BrowserRouter;
   hydrate via /me on mount. ProtectedRoute/Navbar/AuthPage/Recover use useAuth(), not direct API calls.
3b2. Wrap the app in <IconContext.Provider value={{ weight: 'regular' }}> (from @phosphor-icons/react) at the same level as <AuthProvider> and <Toaster>, so every Phosphor icon inherits the regular weight.

Pages:
  a. LandingPage (/) PUBLIC: marketing showcase for [SYSTEM NAME]. FIXED LAYOUT (same shape every run, do NOT vary) —
     a centered-hero, card-based Modern Minimal SaaS page: centered hero (system name + one-line value prop + both CTAs,
     center-aligned) above a responsive grid of capability cards on a clean light background, Soft-UI shadows (soft,
     diffused via var(--shadow-accent); no hard borders/gray shadows) and a SINGLE accent color (--color-accent) as the
     only chromatic emphasis. Minimal + modern; brief "Layout style" may tune tone, not this structure. Required behavior: system
     name + one-line value prop present; two hero CTAs — primary "Get Started" (Phosphor ArrowRight) → /register (Sign Up
     segment) and secondary "Explore More" (outline, Phosphor ArrowDown) that smooth-scrolls to the services section
     (id="services"; href="#services" or scrollIntoView, respect prefers-reduced-motion); communicates what the system
     does and its core capabilities in that services section (managing [Entity1], recording [Entity2], tracking [Entity3], viewing
     Reports — expressed however the layout best supports); fully responsive; uses ONLY the CSS-variable palette + global
     font; Phosphor icons only, no emoji, no lorem-ipsum. Own minimal top bar (system name left; Sign In right). Not in
     ProtectedRoute, no Navbar, no protected API.
  a2. PUBLIC PAGES MUST LOOK DISTINCTIVE: Landing, the combined AuthPage, and Recover must look meaningfully different from one another
     and reflect the system's domain. NOT all the same centered card on a plain background. The AuthPage has a FIXED layout — the
     two-part floating card (showcase + form) in (b); Landing has its own FIXED centered-hero card layout from (a)
     (hero + card grid, distinct from the AuthPage's single floating card); Recover must look distinct from BOTH — pick a
     layout for Recover that suits the project (split-screen, full-bleed, editorial, stat strip, single-statement, side-panel, etc.) while
     still using the design tokens and respecting accessibility. Token-level coherence stays; visual sameness across the
     public surfaces does NOT.
  b. AuthPage (routes /login AND /register) PUBLIC: LAYOUT (FIXED) — ONE floating card centered on the public background,
     elevated with the signature accent shadow + rounded corners (overflow-hidden, ~max-w-4xl), DIVIDED INTO TWO PARTS side
     by side on md+ (grid md:grid-cols-2): a SHOWCASE part — a branded panel filled with --color-accent #BE123C (light text,
     AA contrast) showing the system name, a one-line value prop, and 3-4 domain capabilities as Phosphor icon + label
     (managing [Entity1], recording [Entity2], tracking [Entity3], viewing Reports), NO form fields — and a FORM part holding
     the segmented control + active form. Below md stacks to one column (showcase → compact header above the form, or hidden;
     form full-width px-4).
     The page combines Sign In + Sign Up via a segmented control at the top of the FORM part
     (shadcn Tabs styled as a segmented pill — "Sign In" w/ Phosphor SignIn icon, "Sign Up" w/ UserPlus icon, ACTIVE segment
     filled with --color-accent #BE123C, like the reference). /login opens Sign In active, /register Sign Up active; selecting
     a segment NAVIGATES to the matching route (active segment derived from path via useLocation, not local state alone). Shared
     chrome: "Back to home" (ArrowLeft) → /; not in ProtectedRoute; no Navbar. Segments swap the form below, each with its own logic:
     - Sign In (was LoginPage; POST /api/auth/login): Username, Password (Eye/EyeSlash toggle). Success → /dashboard. Failure →
       inline destructive Alert. "Forgot password?" → /recover (the Sign In ⇄ Sign Up switch is the segmented control, not a link).
     - Sign Up (was RegisterPage; POST /api/auth/register): Full Name, Email, Phone, Username, Password (+toggle), Confirm Password (+toggle), all
     required, validated client- AND server-side (see INPUT VALIDATION). On success the API returns a one-time 4-digit recovery
     code: show a "Save your recovery code" step (Dialog/Card) displaying it large/monospace, a one-line "shown once, save it"
     note, Copy (Phosphor Copy) + Download (.txt named <systemname>-recovery-code.txt) buttons, and a required "I've saved it —
     continue" confirm that logs in → /dashboard. Failure → destructive Alert. (Shared AuthPage chrome above applies.)
  c. RecoverPage (/recover) PUBLIC: Step 1 Verify — Username|Email + 4-digit code (/^\d{4}$/), POST /recover/verify, generic
     error on mismatch. Step 2 Reset — New Password + Confirm (match, min 6), POST /recover/reset → returns a NEW one-time code,
     show the same "Save your recovery code" step, then → /login. "Back to Sign In" link. Not in ProtectedRoute, no Navbar.
  d. DashboardPage (/dashboard) PROTECTED, post-login/register landing. MUST present real stats:
     - Stat cards: responsive grid (grid-cols-2 md:grid-cols-4), ≥4 shadcn Cards of real metrics (totals per entity, today's
       counts, ≥1 SUM/AVG e.g. total revenue), each Phosphor icon + accent, numbers in full with thousands separators (never compacted/abbreviated — no 1.2k or $1.2M).
     - Summary block: a compact shadcn Table or short list (~5 rows) complementing the cards (recent records or per-item totals).
     - Quick Actions card (REQUIRED): summary + a "Quick Actions" card side by side (e.g. lg:grid-cols-3, summary lg:col-span-2, Quick Actions lg:col-span-1). The card lists one button per full-CRUD entity (Phosphor icon + "New [Entity]" label + one-line description); clicking opens that entity's create form in a Dialog modal (reusing its [Entity]Form), and a successful create closes the modal + re-fetches /api/reports/dashboard so stats/summary update immediately.
     - All data from GET /api/reports/dashboard; show loading/error/empty states. Uses Navbar + PageWrapper.
  e. [Entity1]Page (/[entity1route]) INSERT/SELECT/UPDATE/record-DELETE: TWO cards in a responsive grid (lg:grid-cols-2) — a "New [Entity1]" card holding the create form (fields per Entity1; auto-calc + show read-only any computed field), and an "All [Entity1]s" card holding a searchable, paginated shadcn Table with per-row Edit (PencilSimple) + Delete (Trash) (each a shadcn Card with CardTitle exactly "New [Entity1]" / "All [Entity1]s"). Edit opens a pre-filled Dialog; Delete shows AlertDialog confirm then DELETEs that one record only (never project/DB).
  f. [Entity2]Page (/[entity2route]) INSERT/SELECT/UPDATE/record-DELETE: TWO cards in a responsive grid (lg:grid-cols-2) — a "New [Entity2]" card holding the create form ([Entity1] dropdown from API + remaining fields; date fields max=today), and an "All [Entity2]s" card holding a searchable, paginated Table with per-row Edit (PencilSimple) + Delete (Trash) (each a shadcn Card with CardTitle exactly "New [Entity2]" / "All [Entity2]s"). Edit opens a pre-filled Dialog; Delete shows AlertDialog confirm then DELETEs that one record only (never project/DB).
  g. [Entity3]Page (/[entity3route]) INSERT/SELECT/UPDATE/record-DELETE: TWO cards in a responsive grid (lg:grid-cols-2) — a "New [Entity3]" card holding the create form ([Entity1] dropdown + fields; if qty-vs-stock applies,
     qty ≤ available stock — fetch stock on dropdown change; date fields max=today), and an "All [Entity3]s" card holding a searchable, paginated Table with per-row Edit (PencilSimple) + Delete (Trash) (each a shadcn Card with CardTitle exactly "New [Entity3]" / "All [Entity3]s").
     Edit opens a pre-filled Dialog; Delete shows AlertDialog confirm then DELETEs that one record only (never project/DB).
  By DEFAULT every domain entity page follows this same two-card ("New [Entity]" + "All [Entity]s") Create + Edit (Dialog) + Delete (AlertDialog) pattern. EXCEPTION — per the "Per-entity operations" brief field: an INSERT-ONLY entity's page shows ONLY the "New [Entity]" card (no edit/delete, no "All [Entity]s" table card unless a read is needed elsewhere); only full-CRUD entities get the "All [Entity]s" card with the Edit-dialog + Delete-confirm table.
  h. ReportsPage (/reports): shadcn Tabs, ONE TAB PER REPORT (those in brief, else the 2 designed), each rendering a shadcn Table with
     that report's columns and a filter control (default: date picker = today). Empty state w/ a Phosphor icon when no data. Each report has a
     Print button (Phosphor Printer, window.print()) with a print-only header (system name, report title, "Printed on: [datetime]") and a
     print-only footer "Generated by [current user label]" (same label as Navbar (i): "System Admin" for admin, the user's role if role-based, else full name);
     filters/nav get .no-print; table prints clean (see DESIGN @media print).
  i. Navbar — a MINIMAL UNDERLINE TOP-BAR (shared; all pages except Landing, the AuthPage (/login + /register), and Recover). This edition's nav is a slim,
     flat, full-width top bar (NO card surface, NO shadow, just a 1px bottom hairline border): brand at the left, text links in the middle, user identity +
     Logout at the right. Links: Dashboard, [Entity1], [Entity2], [Entity3], Reports, Logout. The ACTIVE link is marked by a thick accent underline bar beneath
     it (inactive links show a faint accent underline on hover) — underline only, never a filled background. Shows the logged-in user's identity (useAuth().user):
     full name, or "System Admin" if the seeded admin (identified via role/isAdmin or SEED_ADMIN_USERNAME); if role-based (a `role` field — see AUTHORIZATION)
     also show the role beside the name (small accent Badge). Define the "current user label" once — "System Admin" for admin, else role when role-based, else
     full name — reused by the Reports print footer (h). Logout via useAuth().logout() (POST /api/auth/logout + clears context) then → /; collapses to a
     hamburger (List) on mobile as a vertical dropdown that closes on link click.
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
default Alert. CircleNotch (animate-spin) in the Submit button while loading. Network error → "Unable to connect to the server. Please try
again." Log raw errors with console.error; never expose raw error objects to the user.

=== TOAST NOTIFICATIONS (one per action result) ===
Use sonner: toast.success/error/info on the RESULT of EVERY action (in addition to inline Alerts): login, register, logout,
recovery-code generated ("Save your recovery code — it won't be shown again"), code copied/downloaded, recover verify failure,
password reset, create/update/delete on EVERY entity (success + failure), validation blocks, network errors. One toast per result,
auto-dismiss, dismissible, optional Phosphor icon, no emoji. <Toaster/> mounted once in App.jsx.

=== DESIGN & UI RULES ===
- Full palette as CSS variables (index.css/Tailwind): --color-bg, --color-surface, --color-accent, --color-text, --color-muted,
  --color-danger, --color-success, --shadow-accent. Use ONLY variables, never inline hex. Signature shadow (exact): --shadow-accent:0 4px 14px -4px rgba(190,18,60,0.25).
- COLOR PALETTE: define every token on :root (the app is light-mode only — no dark theme, no theme toggle). Background token exact:
  --color-bg:#FFF5F7. Other tokens chosen to pair well and meet WCAG AA.
- SIGNATURE ACCENT (FIXED — do NOT choose): --color-accent is ALWAYS exactly #BE123C (no choose-from-a-list; this single rose is the
  project's signature, identical every build). Accent used consistently for: active nav link, primary buttons, focus rings, table row highlights, badges.
- No purple gradients on white. No emoji — Phosphor icons only, consistent size (16/18).
- SIGNATURE FONT (FIXED — do NOT choose): load the Google Font "Outfit" and apply it globally as the single app-wide typeface
  (headings, body, UI, numbers); no substitutes. Import weights 400/500/600/700 and set Outfit as the default font-family in tailwind.config.js + index.css.
  Type treatment (the polish): body 400, labels/nav 500, headings + metric values 600-700; slight negative letter-spacing on headings (~-0.02em) and large stat numbers (~-0.03em); tabular-nums on all numeric values; table headers small (~0.78rem) UPPERCASE muted, ~0.04em tracking, weight 600.
- SIGNATURE ELEMENT (FIXED — must ALWAYS appear): every elevated surface — shadcn Cards (esp. dashboard stat cards), Dialog/AlertDialog
  modals, and the Navbar — uses the rose-tinted accent shadow var(--shadow-accent) (0 4px 14px -4px rgba(190,18,60,0.25), 190,18,60 = #BE123C),
  NEVER neutral gray. This soft rose glow is the project's one consistent visual quirk, present every build; removed only inside @media print.
- Entity (authenticated) forms — the create/edit forms on the [Entity1/2/3] pages — use identical padding (p-6), field gap
  (gap-4), label style, input height. This uniformity applies ONLY to entity forms; the public auth pages are exempt — the
  AuthPage uses its FIXED two-part floating card (showcase + form; see Pages b) and Recover may use any layout that suits the
  design (split-screen, side-panel, full-bleed, centered card, editorial, etc.) as long as they meet ACCESSIBILITY and use the
  design tokens + shadcn components. Authenticated pages
  share one page-wrapper (max-width + horizontal padding).
- Button variants consistent everywhere: default (primary), destructive (delete confirm), outline (cancel). Tables wrapped
  in overflow-x-auto. Entity form cards max-w-xl centered (full-width px-4 on mobile); public auth pages exempt. Responsive
  via sm:/md:/lg: prefixes and/or CSS @media (min-width/max-width) queries.
- @media print (index.css): `.no-print{display:none!important}` on nav/filters/buttons; `.print-only{display:none}` normally,
  `display:block` in print (the report header); report tables print plain black-on-white, no shadows/rounding, not clipped.

=== API INTEGRATION LAYER ===
src/api/axiosClient.js: baseURL = import.meta.env.VITE_API_URL || http://localhost:5000/api; withCredentials:true on every request (so the session cookie is sent); a response
  interceptor redirects to /login on 401 — BUT exempt the /api/auth/me hydration call + public routes (/, /login, /register, /recover) so a visitor on a public page is never bounced (checkAuth swallows that 401, sets user=null).
authAPI.js: register, login, logout, getMe, recoverVerify, recoverReset.
[entity1..3]API.js: create/getAll/update/delete for every entity. reportsAPI.js: one fetcher per report + getDashboard().
Each fn is async, calls axiosClient, returns the { data } payload (response.data.data); throws so the caller's catch shows the
{ error } message. AuthContext wraps authAPI via useAuth(); auth UI uses useAuth(); entity/report pages call their api modules directly.

=== SESSION-BASED AUTH ===
Session management is via express-session with httpOnly cookies — NO JWTs. Configure express-session({ secret: SESSION_SECRET, resave:false,
saveUninitialized:false, cookie:{ httpOnly:true } }) (set cookie.secure + sameSite in production). On register/login, start the session by storing
{ userId, username } on req.session (add `role` only if the domain uses roles) and return { data: { user } }. requireAuth returns 401 when
req.session has no userId; req.session.userId is the source of truth for owner-scoping + aggregations. Frontend sets withCredentials:true on every
axiosClient request so the session cookie is sent; logout() calls POST /api/auth/logout (destroys the session) and clears context. AuthContext
calls /me on load to hydrate the session (401 → /login). Users self-register; recovery via the 4-digit code flow. Idempotent startup seed (below).

=== REPORTS LOGIC (MongoDB aggregation ONLY) ===
One pipeline per report — implement those in the brief (name/shows/grouping/columns/filter, blanks defaulted) else design 2. Use
$match (default date filter = today), $lookup (join refs), $group (totals), $sort, $project (output columns). No raw queries/SQL.
Unless data visibility is "shared", begin every report pipeline AND the dashboard pipeline with an owner $match scoping to the current user (admin unscoped only in "admin sees all" mode — see DATA OWNERSHIP & VISIBILITY).
The dashboard endpoint likewise aggregates the stat metrics and summary rows.

=== CODE QUALITY ===
Clean, consistent, one responsibility per file/fn. Descriptive names (PascalCase components/models, camelCase vars/fns, UPPER_SNAKE
consts, files match default export). No dead code, no leftover console.log (console.error for real errors), no magic numbers/strings
(hoist to consts). Small focused fns; extract repeats. Layers stay separate (controllers=logic, routes=thin, models=schema; presentational
components, data/auth in api modules/AuthContext/hooks). async/await + try/catch throughout. Consistent module style; ordered imports.
Comments: brief, purposeful, ONLY where logic is non-obvious / a complex decision was made — e.g. recovery-code hashing & one-time-code flow, re-selecting a select:false field before bcrypt.compare, server-side computed fields, the owner-scoping/visibility filter, each aggregation pipeline (one line on what it computes), segmented Auth routing via useLocation, non-trivial validation/regex. Explain the WHY not the WHAT, one short line each; they double as presentation hints so keep them sparse — never narrate every line or comment self-explanatory code.

=== SEEDING ===
Idempotent on startup (insert only when collection empty; restarts never duplicate). Seed one admin if Users empty: bcrypt password,
valid email + phone (078/079/073/072+7), recovery code from SEED_ADMIN_RECOVERY_CODE (stored hashed, a known 4-digit code so the
recovery flow is demoable); log only the username. Read SEED_ADMIN_USERNAME/PASSWORD/RECOVERY_CODE from env (labelled dev defaults
only). REQUIRED: seed a few valid sample rows per entity (respecting relationships/validation, some dated today) so tables/dashboard/
reports render non-empty. Unless data visibility is "shared", set each seeded row's `owner` to the seeded admin so the demo data is the ADMIN's only and is NOT visible to other users (see DATA OWNERSHIP & VISIBILITY). Seeding must not block registration.

=== REQUIRED ENV VARS ===
All secrets via process.env/dotenv, never hardcoded. backend .env.example (placeholder + comment each):
  PORT=5000 ; MONGODB_URI=mongodb://localhost:27017/[DATABASE_NAME] ; SESSION_SECRET=change-me-long-random ;
  CLIENT_URL=http://localhost:5173 (CORS origin) ; NODE_ENV=development ; SEED_ADMIN_USERNAME=admin ;
  SEED_ADMIN_PASSWORD=change-me ; SEED_ADMIN_RECOVERY_CODE=1234 (known 4-digit, stored hashed)
frontend .env.example: VITE_API_URL=http://localhost:5000/api
server.js validates MONGODB_URI + SESSION_SECRET at startup (clear console.error + exit if missing). Only .env.example is delivered.

=== DATA-FETCHING UI ===
Every data view has explicit loading (CircleNotch animate-spin or skeleton; disable dependent actions), error (destructive Alert + Retry;
console.error), and empty (centered Phosphor icon + message) states — never a blank screen. Fetch lists on mount; refresh affected list
after create/update/delete. Dependent dropdowns fetch on mount (loading/disabled until ready) and refetch on selection change. Network
failure message matches ERROR HANDLING.

=== TABLE FEATURES ===
shadcn Table in overflow-x-auto. Headers; zebra/hover using accent; loading/empty states. Client-side search box above each table;
sortable columns (header toggles asc/desc with chevron); pagination (or lazy load) past ~25 rows. Human-format values (dates, numbers/
currency in full with thousands separators (never compacted/abbreviated — no 1.2k or $1.2M), booleans/enums as accent Badges). Every FULL-CRUD entity table has per-row Edit/Delete (INSERT-ONLY entities per the "Per-entity operations" field have none, and usually no table); Edit opens a pre-filled Dialog and Delete always goes through the AlertDialog confirmation before calling the record-level DELETE endpoint.

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
Never send password/recoveryCodeHash to the client (select:false or projection; strip from returned user). If you use
select:false, login + recover controllers MUST re-select the field (e.g. .select('+password')) before bcrypt.compare, or auth always fails. Validate + sanitize ALL
input server-side; compute derived fields server-side. Use httpOnly session cookies signed with a strong SESSION_SECRET (cookie.secure + sameSite in prod); never store the password or recovery hash in the session. CORS = CLIENT_URL only with credentials:true (no wildcard). Generic auth/recovery errors; throttle login + recovery (4-digit code) attempts. Never expose stack
traces/DB internals. Guard NoSQL injection: typed/validated inputs via Mongoose methods, never queries built from raw bodies.

=== AUTHORIZATION (when roles are needed) ===
requireAuth enforces login on every non-auth route. If the domain implies permission levels, add a `role` field + requireRole(...roles)
middleware; protect sensitive/mutating endpoints, return 403 when a logged-in user lacks the role. Hide disallowed UI actions (role from
AuthContext) AND enforce on the backend (server is source of truth). If no role distinction is needed, use a single role and say so in the assumptions summary.

=== DATA OWNERSHIP & VISIBILITY ===
By DEFAULT every domain record is OWNED by its creator and each user sees/manages ONLY their own records. The optional "Data visibility" brief field selects the mode:
- (blank) PER-USER (default): add an `owner` (ObjectId ref → User, required, indexed) to EVERY domain entity. Create sets owner = the logged-in user (req.session.userId) SERVER-SIDE (never trust a client-sent owner). EVERY list/get/update/delete AND every dashboard/report aggregation filters by owner = current user (owner in the $match). A user may only update/delete records they own — another user's record is 404. The seed assigns ALL sample rows to the seeded admin, so the demo data is the ADMIN's only and never leaks to other users.
- "shared": all users share ONE dataset — omit owner scoping (records global), seeded data visible to everyone (may store owner/createdBy for audit but don't filter by it).
- "admin sees all": PER-USER for regular users, BUT the admin is a super-user — when the logged-in user is the admin, skip the owner filter ({ owner: userId } for regular users, {} for the admin) so the admin views/manages everyone's records; admin still owns the seeded rows.
Owner is always set + enforced SERVER-SIDE; the { data }/{ error } shape and all other rules are unchanged. Identify the admin as the Navbar rule does (role/isAdmin or SEED_ADMIN_USERNAME). State the chosen mode in the assumptions summary.

=== ACCESSIBILITY ===
Every input has a <Label> (htmlFor/id); icon-only buttons have aria-label. Errors associated (aria-describedby) and announced (role=
"alert"/aria-live), never color-only. Full keyboard operability: logical tab order, visible accent focus rings, Enter submits, Esc
closes dialogs; dialogs trap + restore focus. WCAG AA contrast. Semantic landmarks (nav/main/header/footer), alt text.
Respect prefers-reduced-motion.

=== PERFORMANCE ===
Backend: index queried/sorted/unique fields (username/email/phone); lean queries + projections; filter/aggregate in MongoDB not JS;
avoid N+1 ($lookup/populate); paginate large list/report endpoints (no unbounded results). Frontend: avoid needless re-renders/refetches;
fetch dependent data only on input change; useMemo/useCallback where it matters; debounce search; lazy-load heavy routes; clean up
effects (abort/ignore stale on unmount).

=== STRICT LIBRARY RULES ===
Backend only: express, cors, mongoose, bcryptjs, express-session, dotenv, nodemon (dev); express-rate-limit allowed solely for throttling.
Frontend only: react, react-dom, react-router-dom, axios, tailwindcss, shadcn/ui, @phosphor-icons/react, sonner,
and shadcn deps (class-variance-authority, clsx, tailwind-merge). MongoDB+Mongoose only for data. Phosphor icons only (regular weight via a single IconContext.Provider at the app root).
No other deps (no moment/lodash/Redux/Zustand/other UI or chart libs) — solve with the allowed stack or note in
assumptions. EXACT pinned versions in both package.json (no risky ^ majors): react-router-dom v6 (NOT v7); Tailwind v3 (NOT v4) with
matching postcss.config.js + tailwind.config.js + v3 @tailwind directives; Vite + @vitejs/plugin-react compatible with React 18.
Every import resolves to a listed, installed dep — no deprecated APIs, no phantom imports.

=== ARCHITECTURE ===
Backend layered per request: route (thin: path+middleware→controller) → controller (handling, validation, status, JSON) → model
(schema+data). Never merge layers; cross-cutting logic in /middleware. Single source of truth per concern. Frontend direction:
pages/components → hooks/AuthContext → api modules → axiosClient → backend (UI never calls axios directly). Stateless backend (no server
state beyond the session store; no module-level mutable cache). API contract: noun resources, correct verbs/status; response shape NO exceptions —
{ data } success / { error } failure (lists {data:[...]}, single {data:{...}}); frontend reads response.data.data. Config injected via
env. Frontend/backend independently runnable, communicate only over HTTP (no shared imports). Files live in the folder matching their responsibility.

=== PROJECT README ===
Generate root BENIMANA_Irakiza_Jean_Flaubert_National_Practical_Exam_2026/README.md in clean Markdown, accurate to the built app (real names/entities/routes): title + 1-paragraph
description; tech stack (React 18 + Vite, react-router-dom v6, Node/Express, MongoDB/Mongoose, Tailwind v3 + shadcn/ui + Phosphor Icons (regular weight));
features (auth + 4-digit recovery, dashboard with stats + summary, per-entity CRUD, reports with print); brief structure tree;
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

Root: README.md ; ERD.md (the Mermaid erDiagram in a ```mermaid fenced block)
Backend: package.json ; .env.example ; server.js ; config/db.js ; config/seed.js (idempotent admin+sample seed, called from server.js) ;
  models/User.js + one per entity ; controllers/{auth,[entity1..3],reports}.controller.js (reports incl. /dashboard handler) ;
  routes/{auth,[entity1..3],reports}.routes.js (reports = dashboard + per-report) ; middleware/requireAuth.js (+ requireRole.js only if roles needed)
Frontend: package.json (pinned: React 18, react-router-dom v6, Tailwind v3) ; vite.config.js (React plugin + `@/`→src) ;
  jsconfig.json (`@/*`) ; postcss.config.js (Tailwind v3 + autoprefixer) ; .env.example ; tailwind.config.js (v3) ; index.html (Vite entry) ;
  src/index.css (v3 directives + tokens + font + @media print) ; src/main.jsx (React render root) ; src/lib/utils.js (cn) ; src/components/ui/* (every imported shadcn comp,
  full) ; src/App.jsx (BrowserRouter + AuthProvider + routes + ProtectedRoute + global Toaster; includes <IconContext.Provider value={{ weight: 'regular' }}>; /login + /register both render AuthPage) ; src/context/AuthContext.jsx ;
  src/api/{axiosClient,authAPI,[entity1..3]API,reportsAPI}.js (reportsAPI incl. getDashboard) ; src/components/{Navbar,ProtectedRoute}.jsx ;
  src/pages/{LandingPage,AuthPage,RecoverPage,DashboardPage,[Entity1..3]Page,ReportsPage}.jsx  (AuthPage renders at BOTH /login and /register)

=== FINAL OUTPUT RULES ===
- Work through WORKFLOW PHASES in order. Output: (1) assumptions summary (Phases 1-2); (2) Mermaid ERD (Phase 2), delivered both inline and as the root ERD.md file; (3) every file, built in
  phase order, incl. README.md whose "Build Phases" recaps what you did (Phases 3-10).
- Entities/attributes/relationships are NOT given (beyond the brief) — design/infer them; don't ask to confirm unless the domain is unpickable.
- LandingPage (/) public, no session; the combined AuthPage at BOTH /login and /register, and RecoverPage /recover — all public. Post-login/register → /dashboard.
- No feature to delete the project, drop the DB, or bulk-wipe collections. Only deletion permitted = a single record on any entity via DELETE /api/[that entity's route]/:id — never multi-record or whole-collection.
- Do NOT end with similar-project suggestions or "next steps." Stop once assumptions, ERD, README, and all files are delivered.
