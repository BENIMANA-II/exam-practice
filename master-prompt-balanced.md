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
   This is a nudge for the PUBLIC pages — Landing, the Auth (Sign In/Sign Up) page, Recover — not a strict template.)

Data visibility (optional):      ______________________________
  (blank = per-user: each user sees only the records they create, and the seeded sample data belongs to the admin only.
   Write "shared" to let all users see one shared dataset, or "admin sees all" for per-user data where the admin can
   view/manage every user's records.)

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
             tailwind.config (v3), index.css (tokens incl. exact bg/accent colors + @media
             print), the self-contained shadcn ui components, lib/utils, axiosClient, AuthContext.
  Phase 7  — Frontend API Layer: api modules (auth, per-entity, reports incl. getDashboard).
  Phase 8  — Pages & Components: Navbar, ProtectedRoute, and the pages (Landing, Auth (Sign In/Sign Up at /login + /register), Recover,
             Dashboard, entity pages, Reports); wire routing, validation, toasts, and loading/error/empty states.
  Phase 9  — Polish & Verify: apply design/accessibility/performance/reusability rules; confirm every import
             resolves and every API call maps to a real route/controller; ensure the RUNS-IMMEDIATELY guarantee holds.
  Phase 10 — Documentation: write the root README.md, including the "Build Phases" recap.

=== DATABASE RULE (NON-NEGOTIABLE) ===
Use MongoDB with Mongoose ONLY. Never suggest, substitute, or fall back to MySQL, PostgreSQL, SQLite, or any
SQL/relational database under any circumstances — even if it seems simpler or the schema looks relational. All
data modeling, queries, and aggregations are MongoDB/Mongoose. Build with React.js (frontend) and
Node.js/Express.js (backend) with MongoDB as the database.

=== PROJECT STRUCTURE (MANDATORY — USE THIS EXACT LAYOUT, AND ONLY THIS) ===
You MUST use exactly this folder structure — the same folders, file names, and locations. Do NOT add,
remove, rename, or relocate anything beyond scaling the per-entity files to your real entity count (one
model, controller, routes file, API module, and page per domain entity, named after the REAL entity,
e.g. Student.js / student.controller.js / StudentPage.jsx). The root folder name is FIXED and must be
exactly: BENIMANA_Irakiza_Jean_Flaubert_National_Practical_Exam_2026/

BENIMANA_Irakiza_Jean_Flaubert_National_Practical_Exam_2026/
│
├── README.md                         ← project doc + Build Phases recap
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
        │   ├── Navbar.jsx            ← Dashboard + entity links + Reports +
        │   │                           user/role label + Logout + mobile menu
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
            ├── DashboardPage.jsx     ← PROTECTED: ≥4 stat cards + summary block
            ├── [Entity1]Page.jsx     ← full CRUD: create + table + edit/delete
            ├── [Entity2]Page.jsx     ← full CRUD: create + table + edit/delete
            ├── [Entity3]Page.jsx     ← full CRUD: create + table + edit/delete
            └── ReportsPage.jsx       ← Tabs per report + date filter + Print button

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
before saving — never trust the client to send computed values. Unless data visibility is "shared", every domain
entity ALSO carries an `owner` (ObjectId ref → User, required, indexed) set server-side to the creating user — see
DATA OWNERSHIP & VISIBILITY.

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
   - GET /api/reports/dashboard  — one aggregation returning the stat-card metrics (counts + at least one SUM/AVG)
       and the summary-block rows.
   - GET /api/reports/<reportSlug>  — one endpoint per report; runs that report's MongoDB aggregation.
5. Use Mongoose model methods only — no raw string queries, no SQL.
6. Hash passwords and recovery codes with bcryptjs; compare with bcrypt.compare().
7. Return correct HTTP status codes: 200, 201, 400, 401, 404, 500.
8. Response shape with NO exceptions: success returns { data: ... } JSON; every try/catch failure returns
   { error: message } JSON.
9. Protect all non-auth routes with a session-check middleware that returns 401 if not logged in.
10. Data scoping (default): scope every domain entity's create/list/update/delete AND the dashboard/report aggregations
   to the logged-in user as `owner` — create sets owner server-side, reads filter by owner, update/delete only touch the
   user's own records (404 otherwise); the "shared" and "admin sees all" modes adjust this. See DATA OWNERSHIP & VISIBILITY.

=== FRONTEND REQUIREMENTS ===
1. React + Vite. Install/configure: axios, react-router-dom, tailwindcss, @phosphor-icons/react, shadcn/ui,
   and sonner (toasts). Render a single global <Toaster /> once in App.jsx.
2. Use shadcn/ui components (Button, Input, Card, Table, Dialog, AlertDialog, Badge, Label, Tabs, Select, Alert,
   Toaster) for ALL UI — no manually-styled raw HTML form elements.
2b. shadcn setup MUST be self-contained (do NOT assume a CLI init ran). Either (a) generate every shadcn component
   you import, in full, under src/components/ui/ (button, input, card, table, dialog, alert-dialog, badge, label,
   tabs, select, alert, sonner) together with their deps (class-variance-authority, clsx, tailwind-merge, and the
   `cn` util in src/lib/utils.js), and configure the `@/` path alias in BOTH vite.config.js and jsconfig.json; or
   (b) if a component would be incomplete, fall back to plain Tailwind for that one element. Every shadcn import
   must resolve to a file you generated — NO phantom imports.
3. Phosphor icons ONLY (@phosphor-icons/react), using the regular weight throughout via a single IconContext.Provider
   at the app root. No emoji, no other icon packs.
3b. Auth state via React Context (src/context/AuthContext.jsx): export AuthProvider + a useAuth() hook holding
   { user, loading } and exposing login/logout/register and a checkAuth() that calls GET /api/auth/me. Wrap the app
   in <AuthProvider> in App.jsx inside BrowserRouter; hydrate the session via /me on mount. ProtectedRoute, Navbar,
   AuthPage and Recover read auth from useAuth() instead of calling the auth API directly.
3c. Wrap the app in <IconContext.Provider value={{ weight: 'regular' }}> (from @phosphor-icons/react) at the same level
   as <AuthProvider> and <Toaster>, so every Phosphor icon inherits the regular weight.

Pages:
  a. LandingPage (route /) — PUBLIC: a marketing showcase for [SYSTEM NAME]. DO NOT default to the same generic
     hero + about + 3-card grid + "how it works" + footer recipe every time — choose a layout that fits the domain
     and the optional brief "Layout style" (split-screen, full-bleed statement, stat strip, editorial, side-panel,
     classic hero + cards, etc.) and make it look meaningfully different from the AI's default each run. Required
     behavior independent of the chosen layout: the system name + a one-line value prop are present; two obvious hero CTAs — a primary "Get Started" (Phosphor ArrowRight)
     routing to /register (the AuthPage's Sign Up segment) and a secondary "Explore More" (variant="outline", Phosphor
     ArrowDown) that smooth-scrolls to the services/capabilities section (give it id="services"; href="#services" or
     scrollIntoView, respecting prefers-reduced-motion); the page communicates what the system does and its core
     capabilities in that services section (managing [Entity1], recording [Entity2], tracking [Entity3], viewing Reports — expressed
     however the layout best supports them); fully responsive; uses ONLY the CSS-variable palette and global font;
     Phosphor icons only, no emoji, no lorem-ipsum. Has its own minimal top bar (system name on the left; "Sign In"
     on the right). NOT wrapped in ProtectedRoute, no Navbar, no protected API.
  a2. PUBLIC PAGES MUST LOOK DISTINCTIVE: the public surfaces (Landing, the combined AuthPage, Recover) must look
     meaningfully different from one another and reflect the system's domain. They are NOT all the same centered
     card on a plain background. The AuthPage has a FIXED layout — the two-part floating card (showcase + form) in
     (b) below; Landing and Recover must each look distinct from it and from each other. Pick layouts that suit the
     project (split-screen, full-bleed, editorial, stat strip, single-statement, classic card, side-panel, etc.)
     while still using the design tokens and respecting
     accessibility. Coherence via tokens stays; visual sameness across the public surfaces does NOT.
  b. AuthPage (routes /login AND /register) — PUBLIC: LAYOUT (FIXED) — render the page as ONE floating card centered
     on the public background, elevated with the signature accent shadow + rounded corners (rounded-2xl, overflow-hidden,
     ≈max-w-4xl), DIVIDED INTO TWO PARTS side by side on md+ (grid md:grid-cols-2, equal height): a SHOWCASE part — a
     branded panel filled with --color-accent #003F91 (light text, AA contrast) showing the system name, a one-line value
     prop, and 3-4 domain capabilities as Phosphor icon + label (managing [Entity1], recording [Entity2], tracking
     [Entity3], viewing Reports); NO form fields here — and a FORM part holding the segmented control + the active form.
     Below md it stacks to one column (showcase becomes a compact header above the form, or hidden; form full-width, px-4).
     The page combines Sign In and Sign Up via a segmented control at the top of the FORM part
     (shadcn Tabs styled as a segmented pill — "Sign In" with the Phosphor SignIn icon, "Sign Up" with the
     UserPlus icon, the ACTIVE segment filled with --color-accent #003F91, exactly like the reference). /login opens
     with Sign In active, /register with Sign Up active; selecting a segment NAVIGATES to the matching route (derive the
     active segment from the path via useLocation, not local state alone) so the URL, back button, and deep links stay
     correct. Shared chrome: a "Back to home" (ArrowLeft) → /; NOT in ProtectedRoute; no Navbar. The segments swap the
     form below but each keeps its own fields/validation/submit:
     - Sign In segment (was LoginPage; POST /api/auth/login): Username, Password (Eye/EyeSlash show-hide toggle). On
       success → /dashboard; on failure → inline destructive shadcn Alert. A "Forgot password?" link → /recover (the
       Sign In ⇄ Sign Up switch is the segmented control, not a text link).
     - Sign Up segment (was RegisterPage; POST /api/auth/register): Full Name, Email, Phone Number, Username, Password (+toggle), Confirm
     Password (+toggle) — all required, validated client-side before the call AND mirrored server-side (see INPUT
     VALIDATION). On success the API returns a one-time 4-digit recovery code: do NOT redirect immediately — show a
     "Save your recovery code" step (a Dialog/Card) that displays the code large/monospace, explains in one line that
     it's needed to recover the account and is shown ONLY ONCE, provides Copy (Phosphor Copy) and Download (Phosphor
     Download → a .txt named "<systemname>-recovery-code.txt" containing username + code) buttons, and requires an
     "I've saved it — continue" confirmation before logging the user in → /dashboard. Failure → destructive Alert.
     (The shared AuthPage chrome — segmented toggle, "Back to home", not in ProtectedRoute, no Navbar — applies here.)
  c. RecoverPage (route /recover) — PUBLIC: lets a user who forgot their password regain access via the 4-digit code.
     Step 1 (Verify): Username|Email + 4-digit Recovery Code (validate /^\d{4}$/), POST /api/auth/recover/verify;
     generic "Invalid details" Alert on mismatch, advance on match. Step 2 (Reset): New Password (+toggle) + Confirm
     (must match, min 6), POST /api/auth/recover/reset → returns a NEW one-time code; show the same "Save your recovery
     code" step, then → /login. "Back to Sign In" link. Not wrapped in ProtectedRoute; no Navbar.
  d. DashboardPage (route /dashboard) — PROTECTED, the default landing after login/registration. MUST present real
     statistics — lead with the stats, summary below:
     - Stat cards: a responsive grid (grid-cols-2 md:grid-cols-4) of AT LEAST FOUR shadcn Cards showing real metrics
       of your entities (totals per entity, today's counts, and at least one aggregated SUM/AVG such as total revenue),
       each with a Phosphor icon and the accent color; numbers human-formatted (separators, currency where relevant).
     - A summary block: a compact shadcn Table or short list (~5 rows) complementing the cards — e.g. recent records
       or a per-item totals breakdown.
     - All data comes from GET /api/reports/dashboard; show loading/error/empty states. Uses the Navbar and PageWrapper.
  e. [Entity1]Page (route /[entity1route]) — INSERT, SELECT, UPDATE, and record-level DELETE: form fields per Entity1;
     auto-calculate and show read-only any computed field; below the form, a shadcn Table with per-row Edit (PencilSimple)
     and Delete (Trash). Edit opens a pre-filled Dialog; Delete shows an AlertDialog confirmation, then DELETEs that
     one record only (never the project/database).
  f. [Entity2]Page (route /[entity2route]) — INSERT, SELECT, UPDATE, and record-level DELETE: an [Entity1] dropdown
     (populated from the API) plus the remaining fields; date fields max = today; a Table with per-row Edit (PencilSimple)
     and Delete (Trash). Edit opens a pre-filled Dialog; Delete shows an AlertDialog confirmation, then DELETEs that
     one record only (never the project/database).
  g. [Entity3]Page (route /[entity3route]) — INSERT, SELECT, UPDATE, and record-level DELETE: an [Entity1] dropdown +
     all fields; if a quantity-vs-stock rule applies, the quantity must not exceed the selected [Entity1]'s available
     stock (fetch stock when the dropdown changes); date fields max = today; a Table with per-row Edit (PencilSimple) and
     Delete (Trash). Edit opens a pre-filled Dialog; Delete shows an AlertDialog confirmation, then DELETEs that one
     record only (never the project/database).
  EVERY domain entity page follows this same Create + Edit (Dialog) + Delete (AlertDialog) pattern — no INSERT-only pages.
  h. ReportsPage (route /reports): shadcn Tabs, ONE TAB PER REPORT (those in the brief, or the two you designed), each
     rendering its results in a shadcn Table with that report's columns and a filter control (default: a date picker =
     today). Empty state with a Phosphor icon when no data. Each report has a Print button (Phosphor Printer, window.print())
     with a print-only header (system name, report title, "Printed on: [datetime]") and a print-only footer "Generated by
     [current user label]" (same label as the Navbar (i): "System Admin" for the admin, the user's role if role-based, else
     the full name); filters/nav get the .no-print class; the table prints clean (see DESIGN @media print).
  i. Navbar (shared; rendered on all pages except Landing, the AuthPage (/login + /register), and Recover): links Dashboard, [Entity1], [Entity2],
     [Entity3], Reports, Logout. Shows the logged-in user's identity (useAuth().user): their full name, or "System Admin" if
     they are the seeded admin account (identified via a role/isAdmin field or SEED_ADMIN_USERNAME); if the app is role-based
     (a `role` field — see AUTHORIZATION) also show the role beside the name (small accent Badge). Define the "current user
     label" once — "System Admin" for the admin, else the role when role-based, else the full name — reused by the Reports
     print footer (h). The active link is visually distinct (accent
     underline/background); Logout calls useAuth().logout() (POST /api/auth/logout) then → /; collapses to a hamburger
     (List) on mobile as a vertical dropdown that closes on link click.
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
in a green-styled default Alert. Show CircleNotch (animate-spin) inside the Submit button while loading. On network error show
"Unable to connect to the server. Please try again." Log raw errors with console.error; never expose raw error objects.

=== TOAST NOTIFICATIONS (one per action result) ===
Use sonner: trigger toast.success/error/info on the RESULT of EVERY action (in addition to inline Alerts) — login,
register, logout, recovery-code generated ("Save your recovery code — it won't be shown again"), code copied/downloaded,
recover-verify failure, password reset, create/update/delete on EVERY entity (success + failure), validation blocks, and
network errors. One toast per result, auto-dismiss, dismissible, optional Phosphor icon, no emoji. <Toaster/> mounted once in App.jsx.

=== DESIGN & UI RULES ===
- Define a full color palette as CSS variables (index.css or Tailwind config): --color-bg, --color-surface,
  --color-accent, --color-text, --color-muted, --color-danger, --color-success, --shadow-accent. Use ONLY these
  variables — never hardcode hex inline. Signature shadow (exact value):
  --shadow-accent: 0 4px 14px -4px rgba(0, 63, 145, 0.25);
- COLOR PALETTE: define every token on :root (the app is light-mode only — no dark theme, no theme toggle). The
  background token MUST be exactly --color-bg: #F6F8FF. Choose the other tokens to pair tastefully and meet WCAG
  AA contrast.
- SIGNATURE ACCENT (FIXED — do NOT choose): --color-accent is ALWAYS exactly #003F91 (no choose-from-a-list rule;
  this single blue is the project's signature, identical in every build). Use the accent consistently for:
  active nav link, primary buttons, focus rings, table row highlights, and badge backgrounds.
- No purple gradients on white. No emoji — Phosphor icons only, consistent sizing (size 16 or 18).
- SIGNATURE FONT (FIXED — do NOT choose): load the Google Font "Outfit" and apply it globally as the single app-wide
  typeface (headings, body, UI, numbers); do NOT substitute another font. Import weights 400/500/600/700 via Google
  Fonts and set Outfit as the default font-family in tailwind.config.js + index.css. Type treatment (gives
  the UI its polish): body 400, labels/nav links 500, headings + metric values 600-700; slight negative
  letter-spacing on headings (~-0.02em) and large stat numbers (~-0.03em); font-variant-numeric: tabular-nums
  on all numeric values (stat figures + numeric table columns); table column headers small (~0.78rem) UPPERCASE
  muted (--color-muted), ~0.04em letter-spacing, weight 600.
- SIGNATURE ELEMENT (FIXED — must ALWAYS appear): every elevated surface — shadcn Cards (especially dashboard stat
  cards), Dialog/AlertDialog modals, and the Navbar — uses the blue-tinted accent shadow var(--shadow-accent)
  (0 4px 14px -4px rgba(0, 63, 145, 0.25), where 0,63,145 = #003F91), NEVER a neutral gray shadow. This soft blue
  glow is the project's one consistent visual quirk, present in every build; removed only inside @media print.
- Entity (authenticated) forms — the create/edit forms on the [Entity1/2/3] pages — use identical padding (p-6),
  field gap (gap-4), label style, and input height for visual coherence inside the working app. This uniformity
  rule applies ONLY to entity forms; the public auth pages are exempt from it — the AuthPage uses its FIXED two-part
  floating card (showcase + form; see Pages b) and Recover may use any layout that suits the design (split-screen,
  side-panel, full-bleed, centered card, editorial, etc.) — as long as they meet ACCESSIBILITY and use the design
  tokens + shadcn components. All AUTHENTICATED pages share one
  page-wrapper class (consistent max-width + horizontal padding). Button variants stay consistent everywhere:
  default (primary), destructive (delete confirm), outline (cancel). Tables wrapped in overflow-x-auto. Entity
  form cards max-w-xl centered (full-width with px-4 on mobile); public auth pages are exempt. Responsive via
  Tailwind sm:/md:/lg: only.
- @media print rules (in index.css): `.no-print { display:none !important }` on nav/filters/buttons; `.print-only`
  hidden normally and shown in print (the report header); report tables print plain black-on-white, no shadows/rounding, not clipped.

=== API INTEGRATION LAYER ===
src/api/axiosClient.js: baseURL = import.meta.env.VITE_API_URL or http://localhost:5000/api; withCredentials:true; a
  response interceptor that redirects to /login on 401 — BUT exempt the /api/auth/me hydration call and the public
  routes (/, /login, /register, /recover) so an unauthenticated visitor on a public page is never bounced to /login
  (checkAuth swallows that 401 and sets user=null).
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
(totals), $sort, and $project (output columns). No raw queries, no SQL. Unless data visibility is "shared", begin every
report pipeline AND the dashboard pipeline with an owner $match scoping to the current user (admin unscoped only in
"admin sees all" mode — see DATA OWNERSHIP & VISIBILITY). The dashboard endpoint likewise aggregates the
stat-card metrics and the summary-block rows.

=== CODE QUALITY ===
Clean, readable, consistently formatted; one responsibility per file/function. Descriptive names (PascalCase
components/models, camelCase variables/functions, UPPER_SNAKE constants, files match their default export). No dead code,
no commented-out blocks, no leftover console.log (console.error for real errors only), no magic numbers/strings (hoist to
named constants). Small focused functions; extract repeats into helpers. Keep layers separate (controllers hold logic,
routes are thin, models hold schema; components stay presentational, data/auth live in api modules/AuthContext/hooks).
async/await with try/catch throughout. Consistent module style; ordered imports (external then internal).
Comments: add brief, purposeful comments ONLY where logic is non-obvious or a complex decision was made — e.g. the
recovery-code hashing / one-time-code flow, re-selecting a select:false field before bcrypt.compare, server-side
computed-field calculation, the owner-scoping / data-visibility filter, each MongoDB aggregation pipeline (one line on
what it computes), the segmented Auth routing via useLocation, and any non-trivial validation/regex. Explain the WHY,
not the obvious WHAT; one short line each. They double as presentation/explanation hints, so keep them sparse — do NOT
narrate every line or comment self-explanatory code.

=== SEEDING ===
Run an idempotent seed on startup (insert only when the collection is empty, so restarts never duplicate). Seed one admin
if Users is empty: bcrypt the password, set a valid email and phone (078/079/073/072 + 7 digits), and set the recovery code
from SEED_ADMIN_RECOVERY_CODE (stored hashed — a known 4-digit code so the recovery flow is always demoable); log only the
username (never the password or code). Read SEED_ADMIN_USERNAME / SEED_ADMIN_PASSWORD / SEED_ADMIN_RECOVERY_CODE from env
(labelled dev defaults only; never hardcode real credentials). REQUIRED: also seed a few valid sample rows per entity
(respecting relationships and validation, with some rows dated today) so tables, the dashboard, and reports render non-empty
on first run. Unless data visibility is "shared", set each seeded row's `owner` to the seeded admin so the demo data is the
ADMIN's only and is NOT visible to other registered users (see DATA OWNERSHIP & VISIBILITY). Seeding must not block or
interfere with normal registration.

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
Every data view has explicit loading (CircleNotch animate-spin or skeleton rows; disable dependent actions), error (destructive Alert
with a Retry button; console.error the raw error), and empty (centered Phosphor icon + short message) states — never a blank
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
projection; strip them from any returned user). If you use select:false, the login and recover controllers MUST
explicitly re-select the field (e.g. .select('+password')) before bcrypt.compare, or auth/recovery always fails. Validate and sanitize ALL input server-side; compute derived fields
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

=== DATA OWNERSHIP & VISIBILITY ===
By DEFAULT every domain record is OWNED by its creator, and each user sees and manages ONLY their own records. The optional
"Data visibility" brief field selects the mode:
- (blank) PER-USER (default): add an `owner` field (ObjectId ref → User, required, indexed) to EVERY domain entity. Create
  sets owner = the logged-in user (req.session.userId) SERVER-SIDE (never trust a client-sent owner). EVERY
  list/get/update/delete AND every dashboard/report aggregation filters by owner = the current user (add owner to the
  $match). A user may only update/delete a record they own — treat another user's record as not found (404). The seed
  assigns ALL sample rows to the seeded admin, so the demo data is visible to the ADMIN ONLY and never leaks to other users.
- "shared": all users share ONE dataset — omit the owner scoping (records are global) and the seeded data is visible to
  everyone. You MAY still store owner/createdBy for audit, but do NOT filter by it.
- "admin sees all": PER-USER for regular users, BUT the admin is a super-user — when the logged-in user is the admin, skip
  the owner filter ({ owner: userId } for regular users, {} for the admin) so the admin views/manages every user's records;
  the admin still owns the seeded rows.
Owner is always set and enforced SERVER-SIDE; the { data }/{ error } shape and every other rule are unchanged. Identify the
admin as the Navbar rule does (a role/isAdmin field or SEED_ADMIN_USERNAME). State the chosen mode in the assumptions summary.

=== ACCESSIBILITY ===
Every input has an associated <Label> (htmlFor/id); icon-only buttons have aria-label. Validation errors are programmatically
associated (aria-describedby) and announced (role="alert" / aria-live), never conveyed by color alone. Full keyboard
operability: logical tab order, visible accent focus rings, Enter submits, Esc closes dialogs; dialogs trap and restore
focus. WCAG AA contrast. Use semantic landmarks (nav, main, header/footer) and meaningful alt text. Respect
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
for throttling. Frontend ONLY: react, react-dom, react-router-dom, axios, tailwindcss, shadcn/ui, @phosphor-icons/react, sonner,
and shadcn's deps (class-variance-authority, clsx, tailwind-merge). MongoDB + Mongoose only for
data. Phosphor icons only (regular weight via a single IconContext.Provider at the app root). Do NOT add any other dependency (no
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
Generate a single root BENIMANA_Irakiza_Jean_Flaubert_National_Practical_Exam_2026/README.md in clean Markdown, accurate to the app you built (real names, entities,
routes). Include: project title + a one-paragraph description; tech stack (React 18 + Vite, react-router-dom v6, Node/Express,
MongoDB/Mongoose, Tailwind v3 + shadcn/ui + Phosphor Icons (regular weight)); a short features list (auth with registration + 4-digit
recovery, a dashboard with stat cards and a summary, per-entity CRUD, reports with print); a brief structure tree; prerequisites
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
  package.json (pinned: React 18, react-router-dom v6, Tailwind v3) ; vite.config.js (React plugin + `@/`→src) ;
  jsconfig.json (`@/*`) ; postcss.config.js (Tailwind v3 + autoprefixer) ; .env.example ; tailwind.config.js (v3) ;
  index.html (Vite HTML entry) ; src/index.css (v3 directives + tokens + font + @media print) ; src/main.jsx (React render root) ; src/lib/utils.js (cn) ;
  src/components/ui/* (every shadcn component you import, in full) ; src/App.jsx (BrowserRouter + AuthProvider + routes +
  ProtectedRoute + global Toaster; includes <IconContext.Provider value={{ weight: 'regular' }}>; /login + /register both render AuthPage) ; src/context/AuthContext.jsx ; src/api/{axiosClient,authAPI,[entity1..3]API,
  reportsAPI}.js (reportsAPI includes getDashboard) ; src/components/{Navbar,ProtectedRoute}.jsx ;
  src/pages/{LandingPage,AuthPage,RecoverPage,DashboardPage,[Entity1..3]Page,ReportsPage}.jsx  (AuthPage renders at BOTH /login and /register)

=== FINAL OUTPUT RULES ===
- Work through the WORKFLOW PHASES in order. Output: (1) the assumptions summary (Phases 1-2); (2) the Mermaid ERD (Phase 2);
  (3) every file, built in phase order, including README.md whose "Build Phases" recaps what you did (Phases 3-10).
- Entities, attributes, and relationships are NOT provided (beyond the brief) — design and infer them yourself; don't ask
  the human to confirm them unless the domain is unpickable.
- LandingPage (/) is public with no session; the combined AuthPage renders at BOTH /login and /register, and RecoverPage /recover — all public
  too. After login/registration the user lands on /dashboard.
- Do NOT include any feature to delete the project, drop the database, or bulk-wipe collections. The only deletion permitted
  is a single record on any entity via DELETE /api/[that entity's route]/:id — never multi-record or whole-collection.
- Do NOT end with similar-project suggestions or "next steps." Stop once the assumptions, ERD, README, and all files are delivered.
