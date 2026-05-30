=== PROJECT BRIEF (fill in the blanks; leave any blank to let the AI decide) ===
Project description: ____________________________________________________________
  (one or two sentences, e.g. "A school fee management system for a primary school"
   or "An inventory and sales tracker for a small electronics shop")

System name (optional):        ______________________________
Company/organization (optional): ______________________________
Database name (optional):      ______________________________
Entities to use (optional):    ______________________________
  (e.g. "Student, Invoice, Payment"; leave blank to let the AI choose)
Layout style (optional):       ______________________________
  (e.g. "minimal", "editorial", "playful", "corporate", "bold", "modern"; leave blank
   to let the AI choose an aesthetic that fits the domain. This is a nudge for the
   public pages — Landing/Login/Register/Recover — not a strict template.)

Reports required (optional — list each report; copy the block for more, leave blank to let the AI design 2):
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
You are a senior full-stack developer. Build the complete application described in the PROJECT BRIEF
above. The brief is the only required input; everything else is your responsibility to design.

Any field in the brief left blank, and every [SQUARE BRACKET] placeholder in the sections below, is a
value you must derive yourself with a sensible default. If a brief field is filled in, honor it exactly.
Do not ask the human to complete the brief or supply anything further unless the project description is
so vague that you genuinely cannot pick a domain; otherwise make the most reasonable assumption and proceed.

Design everything yourself from the brief:
1. Choose a clear system name, database name, and root folder name that fit the project (or use the
   values given in the brief).
2. Decide the entities (use those listed in the brief if given; otherwise invent a sensible set —
   default 3 domain entities plus Users, but use fewer or more if the project clearly needs it).
3. Decide each entity's attributes, their types, and constraints (required, unique, numeric, dates,
   dropdowns, computed, etc.).
4. ANALYZE the entities and attributes to infer all relationships yourself — which fields are
   foreign-key references, their direction, and cardinality (see "How to infer relationships" below).
5. Reports: if the brief lists reports, build EXACTLY those — one tab, one endpoint, and one MongoDB
   aggregation per report — honoring each report's name, columns, grouping, and filter, and filling any
   blank fields with sensible defaults. If the brief lists no reports, design two useful reports for the domain.
6. Map entities to routes, pages, and API endpoints following the patterns in this prompt.
7. Before writing code, output a brief assumptions summary: the system name, the entities/attributes
   chosen, the relationships inferred, and the reports to be built. Then proceed straight to the ERD and
   the code.

Treat every section below as the RULES and PATTERNS the generated app must follow. Wherever a section
references [Entity1], [field1], [report1slug], etc., substitute the specifics you designed above.

You are responsible for designing and drawing the complete Entity Relationship Diagram (ERD) yourself
before writing any code. Infer the entities, attributes, AND relationships from the brief. It is
entirely your job to analyze the domain, decide the entities and attributes, infer how they relate to
one another, and determine every relationship: which attributes are foreign-key references to other
entities, the direction of each reference, and the cardinality (one-to-one, one-to-many, many-to-many).
Do NOT ask the human to provide, draw, supply, or confirm the relationships or the ERD. Derive every
key, reference, and relationship by reasoning over the domain, then output the assumptions + ERD first
and only then proceed to build the full application.

How to infer relationships (do this analysis yourself):
- Treat any attribute whose name references another entity (e.g. ends in "Id"/"Ref", or is the
  singular/plural name of another entity in the list) as a foreign-key reference to that entity.
- A reference field on entity B pointing to entity A normally implies A has-many B (one-to-many),
  i.e. one A is referenced by many B records.
- If two entities each reference each other, or a join/linking entity sits between them, model it
  as many-to-many via the appropriate references.
- If an attribute is computed from others (e.g. a total), treat it as a derived field, not a relationship.
- Briefly state your reasoning for each relationship you infer (one line each) before the ERD,
  so the human can see how you derived the model from the attributes alone.

The ERD must show all entities, their attributes, primary keys (_id for MongoDB), foreign key
references, cardinality symbols (one-to-many, many-to-one), and all relationships between entities.

=== WORKFLOW PHASES (follow in this order) ===
Build the app by working through these phases in order. The detailed rule sections later in this
prompt define HOW to satisfy each phase; this is the sequence to follow. Do not begin a later phase's
code before the earlier analysis/design phases are done.

  Phase 1 — Analyze & Plan: read the PROJECT BRIEF, decide the system name/database/entities (or use
    what the brief gave), choose each entity's attributes/types/constraints, and design the required
    reports and the dashboard metrics. Output the brief assumptions summary.
  Phase 2 — Model & Relationships (ERD): analyze the attributes to infer all relationships
    (foreign keys, direction, cardinality), then output the relationship reasoning and the Mermaid ERD.
  Phase 3 — Backend Foundation: set up the backend project — package.json, server.js (cors, JSON,
    session, env validation, mongoose.connect, listen), config/db.js, .env.example, and the env vars.
  Phase 4 — Data Layer (Models): create one Mongoose model per collection, including the inferred
    refs, indexes, unique constraints, and hashed/sensitive-field handling.
  Phase 5 — Backend Logic (Controllers, Routes, Middleware, Auth): implement controllers (all logic,
    validation, { data }/{ error } responses), thin routes mapping to them, requireAuth (and
    requireRole if needed), session-based auth, registration + 4-digit recovery, and the report +
    dashboard aggregations. Add the idempotent seed (admin + sample data).
  Phase 6 — Frontend Foundation: scaffold the frontend — package.json (pinned versions), vite.config,
    jsconfig, postcss.config, tailwind.config (v3, darkMode 'class'), index.css (tokens incl. the
    exact bg/accent/trend colors + @media print), the self-contained shadcn ui components, lib/utils,
    axiosClient, AuthContext, and the theme (useTheme) setup.
  Phase 7 — Frontend API Layer: build the api modules (auth, per-entity, reports incl. getDashboard).
  Phase 8 — Frontend Pages & Components: build Navbar, ProtectedRoute, and the pages — Landing, Login,
    Register, Recover, Dashboard (stats + summary + trend line chart), the entity pages, and Reports
    (with print). Wire routing, validation, toasts, and loading/error/empty states.
  Phase 9 — Polish & Verify: apply design/accessibility/performance/reusability rules, confirm every
    import resolves and every API call maps to a real route/controller, and ensure the RUNS-IMMEDIATELY
    guarantee holds.
  Phase 10 — Documentation: write the root README.md, including the "Build Phases" recap described in
    the PROJECT README section.

=== DATABASE RULE (NON-NEGOTIABLE) ===
Use MongoDB with Mongoose ONLY. Never suggest, substitute, or fall back to MySQL, PostgreSQL,
SQLite, or any SQL/relational database under any circumstances, even if it seems simpler or the
schema looks relational. All data modeling, queries, and aggregations must be MongoDB/Mongoose.

Build a complete [SYSTEM NAME] for [COMPANY/ORGANIZATION NAME], a [BRIEF COMPANY DESCRIPTION],
using React.js (frontend) and Node.js/Express.js (backend) with MongoDB as the database.
([SYSTEM NAME], [COMPANY/ORGANIZATION NAME], and [BRIEF COMPANY DESCRIPTION] are derived by YOU
from the plain-English description unless the human stated them explicitly.)

=== PROJECT STRUCTURE (MANDATORY — USE THIS EXACT LAYOUT, AND ONLY THIS) ===
You MUST use exactly this folder structure — the same folders, the same file names, in the same
locations. Do NOT add, remove, rename, or relocate anything beyond scaling the per-entity files to your
real entity count (one model, one controller, one routes file, one API module, and one page per domain
entity, each named after the REAL entity, e.g. Student.js / student.controller.js / StudentPage.jsx).
The root folder name is FIXED and must be exactly: BENIMANA_Irakiza_Jean_Flaubert_National_Practical_Exam_2026/

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
    │                                   react-router-dom v6, Tailwind v3, recharts,
    │                                   Phosphor Icons; "dev" script (vite)
    ├── .env.example                  ← VITE_API_URL=http://localhost:5000/api
    ├── vite.config.js                ← React plugin + `@/` path alias → src
    ├── jsconfig.json                 ← editor `@/*` path mapping (matches vite)
    ├── postcss.config.js             ← Tailwind v3 + autoprefixer
    ├── tailwind.config.js            ← darkMode:'class', content globs, tokens
    ├── index.html                    ← Vite entry, inline theme-flash guard
    │
    └── src/
        ├── App.jsx                   ← BrowserRouter + <AuthProvider> +
        │                               <IconContext.Provider weight="regular"> +
        │                               routes + ProtectedRoute + global <Toaster />
        ├── main.jsx                  ← React render root
        ├── index.css                 ← @tailwind directives + CSS-variable palette
        │                               (light/dark, accent, trend tokens) + @media print
        │
        ├── lib/
        │   └── utils.js              ← cn() helper (clsx + tailwind-merge)
        │
        ├── context/
        │   └── AuthContext.jsx       ← AuthProvider + useAuth() hook + useTheme()
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
        │   │                           Sun/Moon theme toggle + Logout + mobile menu
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
            ├── LandingPage.jsx       ← PUBLIC: hero, features, theme toggle, Sign In
            ├── LoginPage.jsx         ← PUBLIC: login form, links to Register/Recover
            ├── RegisterPage.jsx      ← PUBLIC: full signup + one-time recovery code
            ├── RecoverPage.jsx       ← PUBLIC: verify code → reset password
            ├── DashboardPage.jsx     ← PROTECTED: ≥4 stat cards + summary block +
            │                           recharts LineChart with trend coloring
            ├── [Entity1]Page.jsx     ← full CRUD: create + table + edit/delete
            ├── [Entity2]Page.jsx     ← full CRUD: create + table + edit/delete
            ├── [Entity3]Page.jsx     ← full CRUD: create + table + edit/delete
            └── ReportsPage.jsx       ← Tabs per report + date filter + Print button

=== DATABASE: [DATABASE_NAME] ===
YOU design the entities and their attributes from the plain-English description (unless the human
listed them). The human does NOT need to supply any of this. Once you have the attributes, analyze
them and discover the relationships yourself (see "How to infer relationships" below), then add the
appropriate Mongoose `ref` fields, `_id` primary keys, and ObjectId references in your data model.

The shape below is an EXAMPLE PATTERN, not a fixed requirement. Users is always present. The
"[Entity1/2/3]" naming is just a template — use however many real, well-named domain entities the
project needs (typically 2-4) and name them after the actual domain (e.g. Student, Invoice, Payment):
  - Users({ _id, fullName, username: unique, email: unique, phone: unique, password,
            recoveryCodeHash })   // recoveryCodeHash = bcrypt hash of the 4-digit recovery code
  - [Entity1]({ _id, [domain fields you choose], [computedField if the domain needs one] })
  - [Entity2]({ _id, [domain fields you choose], [dateField if relevant],
                [attribute that references another entity, if the domain implies one] })
  - [Entity3]({ _id, [domain fields you choose], [computedField if relevant], [dateField if relevant],
                [attributes that reference other entities, if the domain implies them] })

Note: attributes that name or identify another entity are clues — YOU decide they are foreign-key
references, add the Mongoose `ref`, and set the cardinality. Nothing here pre-declares a `ref→`;
choosing the entities, attributes, AND relationships is all your analysis to make.

Computed fields (e.g. TotalPrice = Quantity × UnitPrice) must be calculated in the
backend before saving — never trust the client to send computed values.

=== ERD REQUIREMENTS (YOU DRAW THIS — DO NOT ASK THE HUMAN) ===
Before writing any code, FIRST list (one line per relationship) the relationships you inferred and
why, then output the full ERD yourself using a Mermaid erDiagram block showing:
- All collections as entities
- All fields with their types
- _id as PK on every entity
- The reference fields YOU determined, shown as FK relationships
- Correct cardinality notation (||--o{, }o--||, etc.) that YOU derived from the attributes
- Relationship labels (e.g. "has", "belongs to", "records")
The human supplies only a plain-English description (plus any optional overrides) — the entities,
attributes, and relationships are all designed/inferred by YOU. You determine every reference and
every cardinality yourself by analyzing the domain, the attributes, and the system's purpose.
Producing the assumptions summary, the relationship analysis, and the diagram is entirely your
responsibility.

=== BACKEND REQUIREMENTS ===
1. Use express.js, cors, mongoose, bcryptjs, express-session, dotenv, nodemon.
2. server.js must include: cors middleware (origin restricted to CLIENT_URL, credentials:true),
   JSON body parser, session middleware, env-var validation (exit if MONGODB_URI or SESSION_SECRET
   missing), mongoose.connect() with connection success/failure logs, all routes registered,
   and app.listen() with a startup console message.
3. Create one Mongoose model file per collection under /models.
4. Use a controller layer: put all handler logic in /controllers (one controller file per resource —
   e.g. auth.controller.js, [entity1].controller.js, [entity2].controller.js, [entity3].controller.js,
   reports.controller.js). Each controller exports named async handler functions containing the
   try/catch, validation, Mongoose calls, status codes, and JSON responses. The files under /routes
   are THIN — they only create the express.Router(), map each HTTP method + path to the matching
   controller function, attach middleware (e.g. requireAuth), and export the router. No business
   logic or database calls inline in route files. The routes below map to controller functions:
   - POST   /api/auth/register           — create a new user account, then start session
                                            (validate fullName, email, phone, username, password
                                             server-side; hash password; reject duplicate
                                             username/email/phone with a 400 + clear message;
                                             generate a random 4-digit recovery code, store ONLY its
                                             bcrypt hash in recoveryCodeHash, and return the plaintext
                                             code ONCE in the response so the user can save it — the
                                             plaintext code is never stored and never returned again)
   - POST   /api/auth/login              — validate credentials, start session
   - POST   /api/auth/logout             — destroy session
   - GET    /api/auth/me                 — return session user or 401
   - POST   /api/auth/recover/verify     — body: { username (or email), recoveryCode }; look up the
                                            user and bcrypt.compare the submitted 4-digit code against
                                            recoveryCodeHash; on match return 200 (allow proceeding to
                                            reset), on mismatch return 400 with a generic error. Apply
                                            rate limiting / attempt throttling (a 4-digit code is only
                                            10,000 combinations — lock or back off after a few failed
                                            attempts to prevent brute force)
   - POST   /api/auth/recover/reset      — body: { username (or email), recoveryCode, newPassword };
                                            re-verify the recovery code, then hash and save newPassword.
                                            Generate a NEW 4-digit recovery code, store its hash, and
                                            return the new plaintext code ONCE so the user can save it
                                            (the old code is invalidated). Validate newPassword the same
                                            way as registration
   - POST   /api/[entity1route]          — insert [Entity1]
   - GET    /api/[entity1route]          — get all [Entity1] records
   - PUT    /api/[entity1route]/:id      — update a [Entity1] record
   - DELETE /api/[entity1route]/:id      — delete a single [Entity1] RECORD
                                            (row-level only; NOT the project/database)
   - POST   /api/[entity2route]          — insert [Entity2]
   - GET    /api/[entity2route]          — get all [Entity2] records
   - PUT    /api/[entity2route]/:id      — update a [Entity2] record
   - DELETE /api/[entity2route]/:id      — delete a single [Entity2] RECORD
                                            (row-level only; NOT the project/database)
   - POST   /api/[entity3route]          — insert [Entity3]
   - GET    /api/[entity3route]          — get all [Entity3] records
   - PUT    /api/[entity3route]/:id      — update a [Entity3] record
   - DELETE /api/[entity3route]/:id      — delete a single [Entity3] RECORD
                                            (row-level only; NOT the project/database)
   EVERY domain entity must expose this same POST/GET/PUT/DELETE set — no
   "INSERT-only" entities. If you choose more or fewer entities, repeat the
   four-route pattern for each one.
   - GET    /api/reports/dashboard      — single aggregation powering DashboardPage: returns the
                                            stat-card metrics (counts + at least one SUM/AVG), the
                                            summary-block rows (e.g. recent records or per-item totals),
                                            and the chart series as a TIME SERIES (an ordered array of
                                            { date/label, value } points for the line graph), all
                                            computed via MongoDB aggregation over the real entities
   - GET    /api/reports/<reportSlug>    — one GET endpoint per report you build (one for each
                                            report listed in the PROJECT BRIEF, or for each of the
                                            two you designed if none were listed); each runs that
                                            report's MongoDB aggregation and returns its rows
                                            e.g. GET /api/reports/daily-sales-summary, etc.
5. All Mongoose queries use model methods — no raw string queries, no SQL.
6. Passwords hashed with bcryptjs on register/seed; compared with bcrypt.compare() on login.
7. Return correct HTTP status codes: 200, 201, 400, 401, 404, 500.
8. Response shape is consistent with NO exceptions: success returns { data: ... } JSON, and every
   try/catch failure returns { error: message } JSON (see ARCHITECTURE RULES).
9. Protect all non-auth routes with a session-check middleware that returns 401 if not logged in.

=== FRONTEND REQUIREMENTS ===
1. React + Vite. Install and configure: axios, react-router-dom, tailwindcss, @phosphor-icons/react,
   shadcn/ui, recharts (dashboard chart), and the shadcn toast system (use shadcn's Sonner integration
   — install `sonner` and add shadcn's `<Toaster />`). Render a single global <Toaster /> once at the
   app root (in App.jsx) so toasts are available on every page including the public ones.
2. Use shadcn/ui components (Button, Input, Card, Table, Dialog, AlertDialog, Badge, Label, Tabs,
   Select, Alert, Toaster/Sonner) for ALL UI elements — no plain HTML form elements styled manually.
2b. shadcn/ui setup MUST be self-contained and complete. Do NOT assume a shadcn CLI init step ran.
   Either (a) generate every shadcn component file you import, in full, under src/components/ui/
   (button.jsx, input.jsx, card.jsx, table.jsx, dialog.jsx, alert-dialog.jsx, badge.jsx, label.jsx,
   tabs.jsx, select.jsx, alert.jsx, sonner.jsx) together with their dependencies
   (class-variance-authority, clsx, tailwind-merge, and the `cn` util in src/lib/utils.js), and
   configure the `@/` path alias in BOTH vite.config.js and jsconfig.json; or (b) if any single
   component would end up incomplete, fall back to plain Tailwind for that one element. Every shadcn
   import must resolve to a file you actually generated — NO phantom imports.
3. Phosphor icons ONLY (@phosphor-icons/react), using the regular weight throughout via a single
   IconContext.Provider at the app root. No emoji, no other icon packs.
3b. Auth state via React Context: create src/context/AuthContext.jsx exporting an AuthProvider and a
   useAuth() hook. The provider holds { user, loading } and exposes login(), logout(), register(),
   and a refresh()/checkAuth() that calls GET /api/auth/me. Wrap the app in <AuthProvider> (in
   App.jsx, inside BrowserRouter). On mount the provider calls /api/auth/me once to hydrate the
   session. ProtectedRoute, Navbar, LoginPage, RegisterPage, and RecoverPage read auth state and
   actions from useAuth() instead of calling the auth API directly or duplicating the /me check.
3c. Theme state (light/dark) is app-wide and available on EVERY page, including the public ones.
   Provide it via a small useTheme() hook / ThemeProvider (or as part of AuthContext) holding
   { theme, toggleTheme }; it applies/removes the `dark` class on <html>. On first load, use the
   saved theme from localStorage (key e.g. "theme") if present, otherwise fall back to the OS
   prefers-color-scheme. Persist the user's choice to localStorage on every toggle so it survives
   page reloads. (localStorage is permitted for THIS theme preference only.) To avoid a flash of the
   wrong theme, apply the saved/OS theme to <html> as early as possible (e.g. a tiny inline script in
   index.html or before first paint). The Navbar toggle and the LandingPage top-bar toggle both use
   this hook.
3d. Wrap the app in <IconContext.Provider value={{ weight: 'regular' }}> (from @phosphor-icons/react)
   at the same level as <AuthProvider> and <Toaster>, so every Phosphor icon inherits the regular weight.
4. Build the following pages:

   a. LandingPage (route: /)  ← PUBLIC, no auth required
      - A marketing-style page that showcases [SYSTEM NAME] for [COMPANY/ORGANIZATION NAME].
        DO NOT default to the same generic hero + about + 3-card grid + "how it works" + footer
        recipe every time. Choose a layout that fits the domain and the optional "Layout style"
        from the brief — for example a split-screen (copy left, visual/stat right), a full-bleed
        statement page, a stat strip, an editorial long-form layout, a side-panel + content
        layout, a classic hero + feature cards, or another shape you design. The layout MUST
        look meaningfully different from the AI's default each run.
      - Required behavior (independent of the layout you choose):
          * The system name and a one-line value proposition for [COMPANY/ORGANIZATION NAME] are present
          * A primary CTA "Sign In" (Phosphor ArrowRight icon) that routes to /login is present and obvious
          * The page communicates what the system does for the organization and the core
            capabilities it offers (managing [Entity1], recording [Entity2], tracking [Entity3],
            and viewing Reports) — express these however the chosen layout best supports
            (cards, a list, callout blocks, sectioned copy, a stat strip, etc.)
          * Fully responsive (collapses gracefully on mobile), uses ONLY the CSS-variable palette
            and the global font, Phosphor icons only, no emoji, no placeholder lorem-ipsum text
          * This page must NOT be wrapped in ProtectedRoute and must NOT call any protected API
          * The shared Navbar is NOT rendered here; the LandingPage has its own minimal top bar
            containing the system name on the left and, on the right, a light/dark theme toggle
            (Phosphor Sun / Moon) plus a "Sign In" button
          * Both light and dark themes render correctly, using the design tokens throughout

   a2. PUBLIC PAGES MUST LOOK DISTINCTIVE
      The public pages (LandingPage, LoginPage, RegisterPage, RecoverPage) must look meaningfully
      different from one another and reflect the system's domain. They are NOT all the same
      centered card on a plain background. Pick layouts that suit the project — split-screen,
      full-bleed, editorial, stat strip, single-statement, classic card, side-panel, etc. — while
      still using the design tokens (palette, font, accent), respecting accessibility (contrast,
      focus, labels), and using shadcn/ui components for interactive elements. Coherence with the
      authenticated app's tokens stays; visual sameness across the four public pages does NOT.

   b. LoginPage (route: /login)
      - Fields: Username, Password with show/hide toggle (Eye / EyeSlash Phosphor icons)
      - On success: redirect to /dashboard
      - On failure: show inline shadcn Alert with the server error message
      - Includes a "Don't have an account? Register" link that routes to /register
      - Includes a "Forgot password?" link that routes to /recover
      - Includes a "Back to home" link (Phosphor ArrowLeft icon) that routes to /

   c. RegisterPage (route: /register)  ← PUBLIC, no auth required
      - Fields (all required): Full Name, Email, Phone Number, Username,
        Password with show/hide toggle (Eye / EyeSlash), Confirm Password with its own toggle
      - Validation (enforce client-side BEFORE the API call, and mirror it server-side):
          * Full Name: letters and single spaces only (allow hyphens and apostrophes for names
            like "O'Brien" / "Jean-Paul"); at least 2 characters per word; no digits or symbols;
            trim leading/trailing whitespace. Regex guide: /^[A-Za-z]+([ '-][A-Za-z]+)*$/
          * Email: must be a valid email format. Regex guide: /^[^\s@]+@[^\s@]+\.[^\s@]+$/
          * Phone Number: exactly 10 digits, numeric only, and must start with one of the
            allowed prefixes 078, 079, 073, or 072. Regex guide: /^(078|079|073|072)\d{7}$/
            Reject any other prefix with an inline error.
          * Username: required, no spaces, min 3 characters
          * Password: required, min 6 characters
          * Confirm Password: must exactly match Password
      - Inline validation error text below each invalid field (red, small, shadcn style)
      - On success: the API returns a one-time 4-digit recovery code. Do NOT immediately redirect.
        Instead show a "Save your recovery code" step (a shadcn Dialog or Card) that:
          * Displays the 4-digit code prominently (large, monospace, easy to read)
          * Explains in one short line that this code is needed to recover the account if the
            password is forgotten, is shown ONLY ONCE, and must be saved somewhere safe
          * Provides a "Copy code" button (Phosphor Copy icon) and a "Download code" button
            (Phosphor Download icon) that downloads a small .txt file containing the username and the
            recovery code (e.g. blob download named "<systemname>-recovery-code.txt")
          * Has a "I've saved it — continue" button (require this click, or a checkbox confirmation,
            before proceeding) that then logs the user into /dashboard
      - On failure (e.g. duplicate username/email/phone): show inline shadcn Alert
        (variant="destructive") with the server error message above the form
      - Includes an "Already have an account? Sign In" link that routes to /login
      - Includes a "Back to home" link (Phosphor ArrowLeft icon) that routes to /
      - This page must NOT be wrapped in ProtectedRoute; the shared Navbar is NOT rendered here

   d. RecoverPage (route: /recover)  ← PUBLIC, no auth required
      - Purpose: let a user who forgot their password regain access using their 4-digit recovery code
      - Step 1 — Verify: fields Username (or Email) + 4-digit Recovery Code (numeric, exactly 4 digits,
        validate /^\d{4}$/ client-side). Submit calls POST /api/auth/recover/verify. On mismatch show
        an inline shadcn Alert (variant="destructive") with a generic "Invalid details" message; on
        match advance to Step 2
      - Step 2 — Reset: fields New Password (show/hide toggle) + Confirm New Password (must match,
        min 6 characters, same rules as registration). Submit calls POST /api/auth/recover/reset
      - On successful reset: the API returns a NEW one-time 4-digit recovery code. Show the SAME
        "Save your recovery code" step as RegisterPage (display prominently, Copy + Download .txt,
        require a "I've saved it — continue" confirmation), then redirect to /login so the user can
        sign in with the new password
      - Includes a "Back to Sign In" link (Phosphor ArrowLeft icon) that routes to /login
      - This page must NOT be wrapped in ProtectedRoute; the shared Navbar is NOT rendered here

   e. DashboardPage (route: /dashboard)  ← PROTECTED, default landing page after login
      - This is where login and registration redirect on success
      - The Dashboard MUST present real statistics, not just the chart. Lead with a stats section,
        with the line graph as a secondary element below it.
      - Stat cards: a responsive grid (e.g. grid-cols-2 md:grid-cols-4) of AT LEAST FOUR shadcn Cards
        showing key metrics of the REAL entities you built — each card has a Phosphor icon, a short
        label, and the metric value (large, using the accent palette). Choose meaningful metrics for
        the domain, for example: total [Entity1] records, total [Entity2] records, total [Entity3]
        records, today's [Entity3] count, and at least one aggregated SUM/AVG (e.g. total revenue,
        average order value, total quantity). Numbers are human-formatted (separators, currency where
        relevant).
      - A small summary block in addition to the cards: a compact shadcn Table OR a short list giving
        a breakdown that complements the cards — e.g. a "recent activity" mini-table of the latest few
        [Entity3] records, or a per-[Entity1] totals breakdown (top items by count/total). Keep it
        brief (about 5 rows).
      - ONE chart using recharts, which MUST be a line graph (recharts LineChart): plot a meaningful
        metric over time for the domain (e.g. records created per day, or a running/aggregated total
        such as daily revenue over the last N days), with a time-based X axis and the value on the Y
        axis. Render responsively (ResponsiveContainer), and include axes, grid, tooltip, and a legend
        if multiple series. Do not use bar, pie/donut, or other chart types for this dashboard chart.
      - The line color is TREND-BASED, applied PER SEGMENT (not as a single color for the whole line).
        For each pair of consecutive points (p[i-1] → p[i]), color the segment between them with the
        "trend up" token (--color-trend-up: #16A34A) if p[i].value >= p[i-1].value, otherwise with the
        "trend down" token (--color-trend-down: #DC2626). The line therefore reads as a sequence of
        short green/red segments that reflect each step's direction, not one whole-line color computed
        from first-vs-last. Implementation in recharts: render the chart as multiple short <Line>
        series (one per segment, each two points long, stroke driven by the segment's trend token), or
        provide a per-segment stroke callback — whichever cleanly produces the same per-segment colors.
        Keep the dot/active-dot stroke consistent with each segment's own color. Both tokens are CSS
        variables defined per DESIGN & UI RULES — no inline hex.
      - All dashboard data comes from a single real aggregation endpoint GET /api/reports/dashboard
        (returns the stat-card metrics, the summary-block rows, and the chart time series); show
        loading/error/empty states
      - Uses the shared Navbar and PageWrapper like the other protected pages

   f. [Entity1]Page (route: /[entity1route])  ← INSERT, SELECT, UPDATE, and record-level DELETE
      - Form fields: [list all Entity1 fields with types and constraints]
      - [If computed field exists]: auto-calculate and display read-only below the inputs
      - Below form: shadcn Table showing all [Entity1] records with Edit (PencilSimple icon)
        and Delete (Trash icon) per row
      - Edit opens a pre-filled shadcn Dialog modal
      - Delete shows shadcn AlertDialog confirmation, then DELETEs that single record only
        (this removes one row from the collection — it must never delete the project or database)

   g. [Entity2]Page (route: /[entity2route])  ← INSERT, SELECT, UPDATE, and record-level DELETE
      - Form fields: [Entity1] dropdown (populated from API), [list remaining fields]
      - [dateField] must not be in the future (client-side: max = today's date)
      - Below form: shadcn Table with Edit (PencilSimple icon) and Delete (Trash icon) per row
      - Edit opens a pre-filled shadcn Dialog modal
      - Delete shows shadcn AlertDialog confirmation, then DELETEs that single record only
        (this removes one row from the collection — it must never delete the project or database)

   h. [Entity3]Page (route: /[entity3route])  ← INSERT, SELECT, UPDATE, and record-level DELETE
      - Form fields: [Entity1] dropdown, [list all fields with types and constraints]
      - [If quantity-vs-stock validation needed]: [Entity3 quantity] must not exceed available stock
      - [dateField] must not be in the future
      - Below form: shadcn Table with Edit (PencilSimple icon) and Delete (Trash icon) per row
      - Edit opens a pre-filled shadcn Dialog modal
      - Delete shows shadcn AlertDialog confirmation, then DELETEs that single record only
        (this removes one row from the collection — it must never delete the project or database)

   EVERY domain entity page follows this same Create + Edit (Dialog) + Delete (AlertDialog)
   pattern — there are no INSERT-only pages.

   i. ReportsPage (route: /reports)
      - Use shadcn Tabs component with ONE TAB PER REPORT — one tab for each report described in
        the PROJECT BRIEF (or the two you designed if none were listed)
      - Each tab shows that report's name as the tab label and renders its results in a shadcn Table
        with the columns specified for that report (or the columns you chose if left blank)
      - Each tab includes a filter control matching that report's "Filter by" (default: a date
        picker defaulting to today, unless the report specified a different filter)
      - If no data for the selected filter, show an empty state message with a Phosphor icon
      - Each report has a Print button (Phosphor Printer icon) that calls window.print(). The printed
        output includes a print-only header showing the system name, the report title, and
        "Printed on: [current datetime]". Filters, the Navbar, and other chrome are hidden in print
        (give them the .no-print class); the table prints clean. See the @media print rules in
        DESIGN & UI RULES.

   j. Navbar (shared layout component, rendered on all pages except Landing, Login, Register, and Recover)
      - Links: Dashboard, [Entity1 label], [Entity2 label], [Entity3 label], Reports, Logout
      - Includes a light/dark theme toggle button (Phosphor Sun / Moon icons) that switches the theme
        by toggling the `dark` class on <html> (see DESIGN & UI RULES)
      - Active link must be visually distinct (consistent accent color underline or background)
      - Logout calls logout() from useAuth() (which calls POST /api/auth/logout), clears context
        user state, and redirects to /
      - Responsive: collapses to hamburger (List icon from Phosphor) on mobile screens
      - Mobile menu opens as a vertical dropdown, closes on link click

5. Protected routes: wrap all routes except / (LandingPage), /login (LoginPage),
   /register (RegisterPage), and /recover (RecoverPage) in a ProtectedRoute component that reads
   { user, loading } from useAuth() — while loading show a loading state; if there is no user
   redirect to /login. The auth state comes from AuthContext (which calls GET /api/auth/me on mount),
   so ProtectedRoute does not call the API itself. The LandingPage, LoginPage, RegisterPage, and
   RecoverPage are public and must never trigger an auth redirect.

=== INPUT VALIDATION (enforce on every form) ===
- Every required field validated before the API call is made
- Name fields (e.g. Full Name): letters, spaces, hyphens, and apostrophes only — no digits or
  other symbols; trimmed; each word at least 2 characters. Regex: /^[A-Za-z]+([ '-][A-Za-z]+)*$/
- Email fields: must match a valid email format. Regex: /^[^\s@]+@[^\s@]+\.[^\s@]+$/
- Phone Number fields: exactly 10 numeric digits and MUST start with one of the allowed
  prefixes 078, 079, 073, or 072. Regex: /^(078|079|073|072)\d{7}$/ — reject all other prefixes
  and any non-numeric input with an inline error
- Recovery Code fields: exactly 4 numeric digits. Regex: /^\d{4}$/ — reject anything else inline
- Rwandan Plate Number fields: format is RLL NNN L (Rwandan civilian plates) — first letter is
  always R, followed by two more uppercase letters, then three digits, then one final uppercase
  letter. Display/store with single spaces (e.g. "RAB 123 C"). Accept input case-insensitively
  and normalize to uppercase before validating/saving; allow the spaces to be optional on input
  but normalize them in. Regex (normalized, spaced): /^R[A-Z]{2} \d{3} [A-Z]$/
  Equivalent space-tolerant guide: /^R[A-Z]{2}\s?\d{3}\s?[A-Z]$/ applied after uppercasing.
  (Optional refinement: the letter O is conventionally avoided on Rwandan plates; only enforce
  this if the [SYSTEM NAME] requires it, otherwise accept all A-Z to avoid rejecting valid plates.)
  Reject any value not matching the pattern with an inline error.
- Number fields: must be positive integers or decimals greater than zero
- Dropdown fields: must have a selection before submitting
- Date fields: must not be in the future — use HTML max attribute set to today's ISO date
- [Entity3 quantity field]: must not exceed the available stock of the selected [Entity1]
  (fetch available stock when [Entity1] dropdown selection changes)
- Show inline validation error text below each invalid field (red, small, using shadcn style)
- Disable the Submit button while the API request is in flight
- Reset the form to empty/default values after a successful submission
- Do not allow duplicate submissions by disabling the button immediately on click

=== ERROR HANDLING ===
- Every API call wrapped in try/catch/finally
- Display server error messages in a shadcn Alert component (variant="destructive") above the form
- Display success messages in a shadcn Alert (variant="default" with green styling) after submission
- Show CircleNotch icon (with className="animate-spin") from Phosphor inside the Submit button while loading
- If the server is unreachable (network error), show: "Unable to connect to the server. Please try again."
- Log raw errors with console.error for debugging — never expose raw error objects to the user

=== TOAST NOTIFICATIONS (every action must show one) ===
- Use the shadcn toast system (Sonner). Import `toast` from `sonner` and trigger a toast on the
  RESULT of EVERY user action throughout the app — never leave an action silent.
- Toasts are in addition to (not a replacement for) the inline Alert messages already specified;
  the inline Alert stays on the form, the toast gives transient global feedback.
- Fire a toast for each of the following, with a clear, human-readable message:
    * Login success ("Welcome back, {username}") and login failure (server error message)
    * Register success ("Account created successfully") and register failure (server error message)
    * Recovery code generated ("Save your recovery code — it won't be shown again") on register/reset
    * Recovery code copied ("Recovery code copied") and downloaded ("Recovery code downloaded")
    * Recovery verify failure ("Invalid details") and password reset success ("Password reset — sign in")
    * Logout ("You have been logged out")
    * Create [Entity1] / [Entity2] / [Entity3] success ("[Entity] added successfully") and failure
    * Update [Entity1] / [Entity2] / [Entity3] success ("[Entity] updated successfully") and failure
    * Delete [Entity1] / [Entity2] / [Entity3] success ("[Entity] deleted") and failure
    * Any client-side validation block that prevents submission (e.g. "Please fix the highlighted
      fields") — optional but recommended in addition to inline field errors
    * Network/server-unreachable errors ("Unable to connect to the server. Please try again.")
- Use the correct toast variant per outcome: toast.success(...) for successful actions,
  toast.error(...) for failures and network errors, toast(...) or toast.info(...) for neutral info.
- Each toast may include a Phosphor icon consistent with the app's icon set (no emoji).
- Toasts must auto-dismiss (default duration) and be dismissible; do not stack duplicates for a
  single action (only one toast per action result).
- The <Toaster /> is mounted once globally (App.jsx) so toasts work on public and protected pages.

=== DESIGN & UI RULES ===
- Define a full color palette using CSS variables in index.css or Tailwind config.
  Example variables: --color-bg, --color-surface, --color-accent, --color-text, --color-muted,
  --color-danger, --color-success, --shadow-accent. Use ONLY these variables — never hardcode hex values inline.
  Also define the dashboard trend tokens and the signature shadow with these exact values:
    --color-trend-up:   #16A34A;  (segment color when value rose vs the previous point)
    --color-trend-down: #DC2626;  (segment color when value fell vs the previous point)
    --shadow-accent:    0 4px 14px -4px rgba(0, 63, 145, 0.25);  (blue-tinted card/modal shadow — see SIGNATURE ELEMENT)
- LIGHT & DARK MODE: the app supports both themes via a `dark` class on the root <html> element
  (Tailwind v3 darkMode: 'class'). Define every palette token for BOTH themes — the light values
  on :root and the dark overrides under the `.dark` selector — so all colors come from tokens and
  switch automatically with the theme. The page background token MUST use these exact values:
    light mode  --color-bg: #F6F8FF;
    dark mode   --color-bg: #0A0A0A;
  Pick the remaining tokens (surface, text, muted, accent, etc.) to pair tastefully and meet the
  contrast rule in ACCESSIBILITY for BOTH themes (the trend tokens above stay the same in both).
- Theme control: provide a theme toggle (Phosphor Sun / Moon icons) in the Navbar that switches between
  light and dark by toggling the `dark` class on <html>. On first load, default to the user's OS
  preference (prefers-color-scheme), persist the user's choice to localStorage (key e.g. "theme") so
  it survives reloads, and read that saved value on subsequent loads (localStorage is allowed for the
  theme preference only). Public pages (Landing/Login/Register/Recover) render correctly in both themes too.
- SIGNATURE ACCENT (FIXED — do NOT choose): --color-accent is ALWAYS exactly #003F91. There is no
  choose-from-a-list rule — this single blue is the project's signature and must be identical in every
  build. In dark mode you MAY use a slightly lighter tint of #003F91 for contrast against the #0A0A0A
  background, but the light-mode base is exactly #003F91. The accent must be used consistently for:
  active nav link, primary buttons, focus rings, table row highlights, and badge backgrounds.
- No purple gradients on white backgrounds.
- No emoji — use only Phosphor icons (@phosphor-icons/react) with consistent sizing (size={16} or size={18}).
- SIGNATURE FONT (FIXED — do NOT choose): load the Google Font "Outfit" and apply it globally as the
  single app-wide typeface (headings, body, UI labels, numbers). Do NOT substitute or offer another
  font — Outfit is the project's signature. Import the weights you use (400/500/600/700) via Google
  Fonts in index.html or index.css, and set it as the default font-family in tailwind.config.js and
  index.css so every element inherits it.
- SIGNATURE ELEMENT (FIXED — must ALWAYS appear): every elevated surface — shadcn Cards (especially
  the dashboard stat cards), Dialog/AlertDialog modals, and the Navbar — uses the blue-tinted accent
  shadow defined above (--shadow-accent: 0 4px 14px -4px rgba(0, 63, 145, 0.25); where 0,63,145 is
  #003F91), NEVER a neutral gray shadow. Apply box-shadow: var(--shadow-accent) to those surfaces. In
  dark mode keep the same hue but raise the alpha for visibility (e.g. rgba(0, 63, 145, 0.45)). This
  soft blue glow is the project's one consistent visual quirk and must be present in every build; it is
  removed only inside @media print.
- Entity (authenticated) forms — the create/edit forms on the [Entity1/2/3] pages — use identical
  padding (p-6), gap between fields (gap-4), label style, and input height for visual coherence
  inside the working app. This uniformity rule applies ONLY to entity forms, NOT to the public
  auth pages. LoginPage, RegisterPage, and RecoverPage may use any layout that suits the design
  (split-screen, side-panel, full-bleed, centered card, editorial, etc.) as long as they meet the
  ACCESSIBILITY rules and use the design tokens (palette, font, accent, shadcn components).
- All authenticated pages use the same page wrapper class for consistent max-width and horizontal padding.
- shadcn/ui component variants must be consistent across the app:
    Primary action buttons: variant="default"
    Destructive actions (delete confirm): variant="destructive"
    Secondary/cancel buttons: variant="outline"
- Tables must be wrapped in overflow-x-auto for horizontal scrolling on small screens.
- Entity form cards: max-w-xl centered on desktop, full-width with px-4 on mobile. (Public auth
  pages are exempt — their layout is open per the rule above.)
- Responsive breakpoints must be handled with Tailwind prefixes: sm:, md:, lg: — no inline styles.
- The LandingPage uses the SAME palette, font, and shadcn components as the rest of the app so the
  public page and the authenticated app feel like one product. Its hero may use the accent color as
  a background or highlight, but still drawn from the CSS variables — never a hardcoded hex value.
- Print styles (in index.css): add an @media print block so reports print cleanly. At minimum:
  `.no-print { display: none !important; }` (apply .no-print to Navbar, filters, buttons, and other
  chrome); `.print-only { display: none; }` normally and `display: block;` inside @media print (used
  for the report's printed header); render report tables with plain borders and black-on-white text,
  remove shadows/rounded surfaces, and ensure the table is not clipped (no overflow hidden) when printing.

=== API INTEGRATION LAYER ===
src/api/axiosClient.js:
  - baseURL: import.meta.env.VITE_API_URL or http://localhost:5000/api
  - withCredentials: true (required for session cookies to be sent)
  - Response interceptor: if status 401, redirect to /login

src/api/authAPI.js         — register(), login(), logout(), getMe(), recoverVerify(), recoverReset()
src/api/[entity1]API.js    — create[Entity1](), getAll[Entity1]s(), update[Entity1](), delete[Entity1]()
src/api/[entity2]API.js    — create[Entity2](), getAll[Entity2]s(), update[Entity2](), delete[Entity2]()
src/api/[entity3]API.js    — create[Entity3](), getAll[Entity3]s(), update[Entity3](), delete[Entity3]()
src/api/reportsAPI.js      — one fetch function per report (e.g. getDailySalesSummary(), etc.)

Each API function is async, calls axiosClient, and returns the payload from the { data } envelope
(i.e. response.data.data), so callers receive the records/object directly. On failure the thrown
error carries the server's { error } message for the calling component's catch block to display.
Layering: AuthContext (src/context/AuthContext.jsx) wraps authAPI and exposes auth state + actions
via useAuth(); auth-related UI (Login/Register/Recover/ProtectedRoute/Navbar) uses useAuth() rather
than calling authAPI directly. Entity/report pages call their [entity]API/reportsAPI modules directly.

=== SESSION-BASED AUTH ===
- Backend: express-session({ secret: process.env.SESSION_SECRET, resave: false,
  saveUninitialized: false, cookie: { httpOnly: true } })
- Session stores: { userId, username }
- Frontend: axiosClient has withCredentials: true on every request
- On app load, the AuthContext provider calls GET /api/auth/me once to hydrate { user, loading };
  ProtectedRoute uses that state — if no user, redirect to /login
- Users can self-register via POST /api/auth/register (validated server-side, password hashed).
  Optionally still seed one admin user on server startup if the Users collection is empty, so the
  app is usable immediately (username + hashed password); seeding must not block registration.
- Account recovery: at registration the server generates a random 4-digit recovery code, stores only
  its bcrypt hash (recoveryCodeHash), and returns the plaintext code ONCE so the user can copy/download
  it. To recover a forgotten password the user submits username/email + the 4-digit code to
  /api/auth/recover/verify, then sets a new password via /api/auth/recover/reset, which issues a new
  4-digit code. Never store or log the plaintext code; throttle recovery attempts to resist brute force.

=== REPORTS LOGIC (MongoDB Aggregation ONLY) ===
Build one aggregation pipeline per report. If the PROJECT BRIEF listed reports, implement each one
to match its description (name, what it shows, grouping, columns, filter); if none were listed,
design two reports for the domain. Every report MUST be implemented with a
MongoDB aggregation pipeline (no raw queries, no SQL) — use $match for filtering (default the date
filter to today), $lookup to join referenced entities, $group to total/aggregate, $sort to order,
and $project to shape the output columns. The two patterns below are EXAMPLES of typical reports;
adapt them to whatever each described report actually needs:

[Example pattern — a "status / summary" report, e.g. Daily Stock Status]:
  Use a MongoDB aggregation pipeline:
  - $lookup to join the transaction entity into the item entity, filtered by the requested date
  - $group to sum quantities per item
  - $project to output the chosen summary columns (e.g. item, totals, computed remaining field)
  Output columns: the columns described for that report (default: pick the most useful ones)
  Filter: records where the date field equals the requested date (default: today)

[Example pattern — a "detailed list" report, e.g. Daily Transactions]:
  Use a MongoDB aggregation pipeline:
  - $match on the date field equal to the requested date
  - $lookup to populate referenced entity names
  - $sort by date descending
  Output columns: the columns described for that report (default: pick the most useful ones)
  Filter: records where the date field equals the requested date (default: today)

=== CODE QUALITY RULES ===
- Write clean, readable, consistently formatted code; one responsibility per file/function.
- Use descriptive names (no a, x, tmp, data2). Functions are verbs; components/models are PascalCase;
  variables/functions camelCase; constants UPPER_SNAKE; files match their default export's name.
- No dead code, no commented-out blocks, no console.log left in (console.error for real errors only).
- No magic numbers/strings — hoist them to named constants (e.g. a shared constants file or top of file).
- Keep functions small and focused; extract repeated logic into helpers rather than copy-pasting.
- Backend: controllers contain logic, routes are thin, models hold schema only — never mix layers.
- Frontend: components stay presentational where possible; data/auth logic lives in api modules,
  AuthContext, or small hooks — not duplicated inside JSX.
- Use async/await with try/catch everywhere (no unhandled promises, no .then chains mixed in).
- Consistent module style across the codebase (pick CommonJS or ESM on the backend and stay consistent;
  the frontend is ESM). Group and order imports (external, then internal) consistently.

=== SEEDING RULES ===
- On server startup, run an idempotent seed: only insert when the relevant collection is empty, so
  restarting the server never creates duplicates.
- Seed exactly one admin user if Users is empty: hash the password with bcryptjs, set a valid email
  and a valid phone (078/079/073/072 + 7 digits), and set the recovery code from the
  SEED_ADMIN_RECOVERY_CODE env var (a known 4-digit code, stored hashed) so a grader/demo can ALWAYS
  exercise the recovery flow with a known code — even though user-generated codes are shown only once.
  Print the seeded username (NEVER the password or recovery code) to the console once.
- Read seed credentials from environment variables (SEED_ADMIN_USERNAME, SEED_ADMIN_PASSWORD,
  SEED_ADMIN_RECOVERY_CODE); fall back to clearly-labelled defaults ONLY in development. Never
  hardcode real credentials in source.
- REQUIRED (not optional): seed a few sample rows per domain entity ([Entity1]/[Entity2]/[Entity3]) on
  first run so tables, the dashboard, and reports render non-empty immediately for demo value. The
  sample data must be valid (respect all relationships and validation rules) and idempotent (guarded by
  an empty check). Include at least some rows dated today so date-filtered reports/dashboard show data.
- Seeding must not block or interfere with normal registration.

=== REQUIRED ENVIRONMENT VARIABLES ===
- All secrets and environment-specific values come from process.env via dotenv — never hardcoded.
- backend-project/.env.example must list every variable with a safe placeholder value and a comment:
    PORT=5000
    MONGODB_URI=mongodb://localhost:27017/[DATABASE_NAME]
    SESSION_SECRET=change-me-to-a-long-random-string
    CLIENT_URL=http://localhost:5173        # used for CORS origin
    NODE_ENV=development
    SEED_ADMIN_USERNAME=admin
    SEED_ADMIN_PASSWORD=change-me
    SEED_ADMIN_RECOVERY_CODE=1234       # known 4-digit code for the seeded admin (stored hashed)
- frontend-project uses Vite env vars prefixed VITE_; provide a .env.example with:
    VITE_API_URL=http://localhost:5000/api
- server.js must validate that critical env vars (MONGODB_URI, SESSION_SECRET) are present at startup
  and exit with a clear console.error if any are missing.
- Never commit real .env files; only .env.example is delivered.

=== DATA-FETCHING UI RULES ===
- Every view that loads data has three explicit states: loading, error, and empty — never a blank screen.
- Loading: show a spinner (Phosphor CircleNotch with the animate-spin class) or skeleton rows in tables; disable
  actions that depend on the data until it arrives.
- Error: show a shadcn Alert (variant="destructive") with a friendly message and a Retry button that
  re-runs the fetch; log the raw error with console.error.
- Empty: show a centered empty-state with a Phosphor icon and a short message (e.g. "No records yet").
- Fetch lists on mount; after a successful create/update/delete, refresh the affected list (re-fetch or
  optimistically update) so the table always reflects current data.
- Dropdowns that depend on another entity fetch their options on mount and show a loading/disabled state
  until ready; refetch dependent data when a selection changes (e.g. available stock on [Entity1] change).
- Network failure messaging matches ERROR HANDLING ("Unable to connect to the server. Please try again.").

=== TABLE FEATURES ===
- All record tables use the shadcn Table, wrapped in overflow-x-auto for small screens.
- Provide, at minimum: column headers, zebra/hover row styling using the accent token, and the
  loading/empty states above.
- Client-side search/filter box above each table (filters the visible rows by key text columns).
- Sortable columns: clicking a header toggles asc/desc with a Phosphor chevron indicator (sort the
  data the table already has; for large sets prefer server-side sorting).
- Pagination (or virtualized/lazy loading) when a table can exceed ~25 rows — never render thousands
  of rows at once.
- Format values for humans: dates in a readable locale format, numbers/currency with separators,
  booleans/enums as shadcn Badges with the accent palette.
- EVERY entity table shows per-row Edit (PencilSimple) and Delete (Trash) actions; Edit opens a pre-filled
  shadcn Dialog modal, and Delete always goes through the AlertDialog confirmation before calling the
  record-level DELETE endpoint.

=== REUSABILITY RULES ===
- Don't repeat UI: build small reusable pieces (e.g. PageWrapper for max-width/padding, FormField that
  pairs Label + Input + inline error, DataTable for the shared table behavior, ConfirmDialog wrapping
  AlertDialog, StateBlock for loading/error/empty). Pages compose these rather than re-implementing them.
- COMPONENT DEFINITION RULE (prevents input-focus loss): EVERY React component — especially FormField,
  DataTable, ConfirmDialog, PageWrapper, and any small wrapper around Input/Textarea/Select — MUST be
  declared at MODULE TOP LEVEL (or in its own file), NEVER inside another component's function body and
  NEVER inside JSX. Defining a component inside a parent creates a brand-new component identity on
  every render, which makes React unmount and remount the input on every keystroke — the input loses
  focus and the user can only type one character at a time. Same rule for components passed via props:
  pass element trees (<MyForm />) or stable references, not freshly-declared functions. Do NOT create
  components inline with arrow functions inside render. Inside a page component you may declare local
  handlers/values, but NEVER `const FormField = (...) => ...` or `function FormField(...) {...}`.
- Keep input identity stable across renders: use stable keys derived from the data (e.g. record._id),
  never Math.random()/Date.now()/array index for inputs that re-render. Define static options arrays
  and validation regexes at module scope (or memoize with useMemo) so they don't get a new reference
  every render. Controlled inputs must always have a defined `value` (never undefined → defined, which
  remounts the input as uncontrolled→controlled).
- Centralize cross-cutting concerns: validation regexes/helpers in one module (shared by all forms),
  formatting helpers (date/number) in one module, route paths and option lists as named constants.
- Reuse the api modules and AuthContext everywhere; never duplicate axios calls or the /me check inline.
- A change to a shared rule (e.g. a validation regex or the accent color) should require editing ONE place.

=== SECURITY RULES ===
- Passwords and recovery codes are ALWAYS stored hashed with bcryptjs — never plaintext, never logged,
  never returned by any endpoint (except the one-time recovery code at register/reset).
- Never send the password hash or recoveryCodeHash to the client; exclude them in queries (select:false
  on the schema or explicit field projection) and strip them from any user object returned.
- Validate and sanitize ALL input server-side (do not trust client validation); reject malformed input
  with 400. Compute derived/computed fields server-side only.
- Use httpOnly session cookies; in production set cookie.secure=true and an appropriate sameSite value.
- CORS allows only the known CLIENT_URL origin with credentials:true — not a wildcard.
- Return generic auth/recovery errors (don't reveal whether a username/email exists); throttle login and
  recovery attempts to resist brute force (the 4-digit recovery code especially).
- Never expose raw error objects, stack traces, or DB internals to the client; log them server-side only.
- Guard against NoSQL injection: use Mongoose model methods with typed/validated inputs; never build
  queries from raw unvalidated request bodies.

=== AUTHORIZATION (apply when roles are needed) ===
- Authentication (is the user logged in?) is enforced on every non-auth route via requireAuth.
- If the domain implies different permission levels (e.g. an admin vs a regular user, or staff vs
  viewer), add a `role` field to Users and a requireRole(...roles) middleware; protect mutating or
  sensitive endpoints accordingly and return 403 (not 401) when a logged-in user lacks the role.
- Hide UI actions the current user isn't allowed to perform (read role from AuthContext) AND enforce the
  same rule on the backend — frontend hiding is convenience, the server is the source of truth.
- If the domain has no meaningful role distinction, a single authenticated role is sufficient — do not
  invent unnecessary roles, but say so in the assumptions summary.

=== ACCESSIBILITY RULES ===
- Every input has an associated <Label> (htmlFor/id); icon-only buttons have aria-label.
- Inline validation errors are programmatically associated with their field (aria-describedby) and
  announced (role="alert" or aria-live) so they aren't conveyed by color alone.
- Full keyboard operability: logical tab order, visible focus rings (use the accent token), Enter
  submits forms, Esc closes dialogs; modals/dialogs trap focus and return it on close (shadcn handles
  much of this — don't break it).
- Color contrast meets WCAG AA for text and interactive elements; never rely on color alone to convey
  state (pair with text/icon).
- Use semantic landmarks (nav, main, header/footer) and meaningful alt text on any images.
- Respect prefers-reduced-motion for spinners/animations.

=== PERFORMANCE RULES ===
- Backend: add indexes for fields queried/sorted/filtered often and for unique fields (username, email,
  phone). Use lean queries and projections to return only needed fields; do filtering/aggregation in
  MongoDB (aggregation pipeline), not in JS over large arrays. Avoid N+1 — use $lookup/populate.
- Paginate list and report endpoints that can grow large (limit + skip or cursor); never return unbounded
  result sets.
- Frontend: avoid unnecessary re-renders and refetches; fetch dependent data only when inputs change,
  memoize expensive derived values (useMemo) and stable callbacks (useCallback) where it matters, and key
  lists correctly. Debounce search inputs. Code-split/lazy-load heavier routes if the bundle grows.
- Clean up effects (abort in-flight requests / ignore stale responses on unmount) to prevent leaks and
  state-update-after-unmount warnings.

=== STRICT LIBRARY RULES ===
- Use ONLY the libraries specified in this prompt:
    Backend: express, cors, mongoose, bcryptjs, express-session, dotenv, nodemon (dev). A rate-limiter
      (e.g. express-rate-limit) is permitted solely to satisfy the throttling security rule.
    Frontend: react, react-dom, react-router-dom, axios, tailwindcss, shadcn/ui, @phosphor-icons/react, sonner,
      recharts (for the dashboard chart), and shadcn's own deps (class-variance-authority, clsx,
      tailwind-merge).
- MongoDB with Mongoose ONLY for data — never any SQL/relational DB or query builder (see DATABASE RULE).
- Icons: @phosphor-icons/react ONLY (regular weight via a single IconContext.Provider at the app root).
  No emoji, no other icon packs, no inline SVG icon sets.
- Charts: recharts ONLY (used solely on the DashboardPage), and the dashboard chart MUST be a line graph (LineChart).
- Do NOT introduce extra dependencies beyond those listed above (no moment, no lodash, no
  axios-alternative, no UI kit other than shadcn/ui, no charting lib other than recharts, no state
  library like Redux/Zustand — AuthContext + local state is sufficient). If a need seems to require
  another library, solve it with the allowed stack or note it in the assumptions summary instead of
  adding the dependency.
- EXACT VERSION PINNING (this prevents first-run breakage — do not skip):
    * Specify exact, mutually-compatible versions in BOTH package.json files (no ^ ranges that could
      pull an incompatible major).
    * react-router-dom v6 (NOT v7) — the routing API in this prompt assumes v6.
    * Tailwind CSS v3 (NOT v4) — v4 changes config and PostCSS setup significantly. Provide the
      matching postcss.config.js and tailwind.config.js for v3, and the v3 @tailwind directives in
      index.css. Be explicit and consistent: this is a Tailwind v3 project.
    * Use Vite + @vitejs/plugin-react versions compatible with React 18 and the Tailwind v3 toolchain.
    * Every import must resolve to a listed, installed dependency — no deprecated APIs, no phantom imports.

=== ARCHITECTURE RULES ===
- Strict separation of concerns. Backend follows a layered flow on every request:
    route (thin: path + middleware + maps to controller)
      → controller (request handling, validation calls, status codes, JSON responses)
      → model (Mongoose schema + data access)
  Never skip or merge layers: no DB calls in routes, no req/res handling in models, no business logic
  in route files. Reusable cross-cutting logic (auth checks, role checks) lives in /middleware.
- Single source of truth per concern: one place defines each model, each validation rule, each route
  path constant, each formatting helper. Other files import from it rather than redefining.
- Frontend follows a clear dependency direction:
    pages/components → hooks/AuthContext → api modules → axiosClient → backend
  UI never calls axios directly; data and auth logic live in api modules, AuthContext, or small hooks.
  Components receive data via props/context and stay as presentational as practical.
- Stateless backend: no server-side state beyond the session store; every request is self-contained so
  the app can run as multiple instances. Do not cache mutable data in module-level variables.
- Clear API contract: REST resources are nouns (/api/[entity]), HTTP verbs express the action, status
  codes are used correctly. RESPONSE SHAPE — NO EXCEPTIONS: every response is JSON, `{ data }` on
  success and `{ error }` on failure. Lists return { data: [...] }, single records { data: {...} }, and
  every failure { error: "message" }. The frontend api modules read response.data.data accordingly.
  Do not return bare arrays/objects or mix shapes across endpoints.
- Configuration is injected, not hardcoded: ports, URIs, secrets, and origins come from env vars
  (see REQUIRED ENVIRONMENT VARIABLES), so the same code runs in dev and prod unchanged.
- Frontend and backend are independently runnable projects that communicate only over the HTTP API —
  no shared imports across the backend-project/frontend-project boundary.
- Folder structure must match PROJECT STRUCTURE exactly; new files go in the layer folder that matches
  their responsibility. Each file has a single, clear responsibility.

=== PROJECT README ===
Generate a single root-level README.md (at BENIMANA_Irakiza_Jean_Flaubert_National_Practical_Exam_2026/README.md) written in clean Markdown,
accurate to the app you actually built (real system name, real entities, real routes). Include:
- Project title (the system name) and a one-paragraph description of what the app does and for whom.
- Tech stack: React 18 + Vite, react-router-dom v6, Node/Express, MongoDB/Mongoose, Tailwind v3 +
  shadcn/ui + Phosphor Icons (regular weight), recharts (dashboard chart).
- Features: a short bullet list (auth with registration + 4-digit recovery code, a dashboard with stat
  cards and a chart, per-entity CRUD as specified, reports with print, etc.) reflecting what was built.
- Project structure: a brief tree of backend-project/ and frontend-project/ showing the main folders.
- Prerequisites: Node.js (state a sensible LTS version) and a running MongoDB (local or Atlas URI).
- Setup & run instructions, copy-pasteable, for BOTH projects:
    * backend: cd backend-project, npm install, copy .env.example to .env and fill values, npm run dev
    * frontend: cd frontend-project, npm install, copy .env.example to .env, npm run dev
  Include the default dev URLs (API on :5000, app on :5173).
- Environment variables: a table or list of every variable from the .env.example files with a short
  description of each (do NOT put real secret values in the README).
- Seeded admin login: explain that an admin user is seeded on first run and where its credentials come
  from (the SEED_ADMIN_* env vars) — never print a real password in the README.
- API reference: a concise list of the endpoints (method + path + one-line purpose), grouped by resource.
- Account recovery: a short note on how the 4-digit recovery code works and how to reset a password,
  and that the seeded admin's code comes from SEED_ADMIN_RECOVERY_CODE so the flow can be demoed.
- Build Phases: a section titled "Build Phases" that recaps the WORKFLOW PHASES you actually followed
  to build this app. List each phase you went through (Phase 1 … Phase 10) with a short one-line brief
  of what was done in that phase, written truthfully for THIS project using the real entity/report
  names (e.g. "Phase 4 — Models: created Student, Invoice and Payment Mongoose schemas with the
  Invoice→Student and Payment→Invoice references and indexes"). Keep each line to one sentence; this
  is a concise recap of the process, not a re-listing of every file.
- Keep it accurate and free of placeholder brackets — substitute the real names/values you designed.
Do NOT include license/contribution boilerplate unless relevant; keep the README focused and truthful.

=== DELIVERABLES ===
Output complete, immediately runnable code for every file needed by the app you designed.
No placeholder TODOs. No incomplete functions. No hardcoded credentials.

RUNS-IMMEDIATELY GUARANTEE: the project must run with ZERO code modifications after this exact sequence:
  1. cd backend-project && npm install
  2. cp .env.example .env   (fill MONGODB_URI / SESSION_SECRET if needed)
  3. ensure MongoDB is running (local mongod or an Atlas URI in .env)
  4. npm run dev            (backend on http://localhost:5000)
  5. cd ../frontend-project && npm install
  6. cp .env.example .env
  7. npm run dev            (frontend on http://localhost:5173)
Every frontend API call must map to a real backend route; every route must map to a real controller
function; every import must resolve to a file or installed dependency. package.json scripts must include
the "dev" script for each project (backend uses nodemon; frontend uses vite).

NO PER-FILE TRUNCATION: every file you deliver must be complete — do not truncate a file, summarize
its contents, or write "same as above"/"unchanged" in place of real code. You MAY spread the files
across multiple messages if there is too much to fit in one (see FINAL OUTPUT RULES); that is about
length only and never licenses abbreviating any individual file.

The list below assumes the default 3 domain entities. If you chose a different number of entities,
scale the per-entity files up or down to match (one model, one controller, one routes file, one API
file, and one page per domain entity) — keep all the shared files (auth controller/routes, reports
controller/routes incl. the dashboard endpoint, axios, AuthContext, Navbar, ProtectedRoute, LandingPage,
LoginPage, RegisterPage, RecoverPage, DashboardPage, App, index.css, tailwind.config). Use the REAL
entity names you chose for the filenames (e.g. Student.js, student.controller.js, StudentPage.jsx),
not the literal "[Entity1]".

Root:
  0. BENIMANA_Irakiza_Jean_Flaubert_National_Practical_Exam_2026/README.md   (per the PROJECT README section)

Backend:
  1. backend-project/package.json   (exact pinned versions; "dev" script via nodemon)
  2. backend-project/.env.example
  3. backend-project/server.js
  4. backend-project/config/db.js
  5. backend-project/models/User.js
  6. backend-project/models/[Entity1].js
  7. backend-project/models/[Entity2].js
  8. backend-project/models/[Entity3].js
  9. backend-project/controllers/auth.controller.js
  10. backend-project/controllers/[entity1].controller.js
  11. backend-project/controllers/[entity2].controller.js
  12. backend-project/controllers/[entity3].controller.js
  13. backend-project/controllers/reports.controller.js  (includes the /dashboard aggregation handler)
  14. backend-project/routes/auth.routes.js
  15. backend-project/routes/[entity1].routes.js
  16. backend-project/routes/[entity2].routes.js
  17. backend-project/routes/[entity3].routes.js
  18. backend-project/routes/reports.routes.js  (dashboard + per-report endpoints)
  19. backend-project/middleware/requireAuth.js
  20. backend-project/config/seed.js   (idempotent admin + sample-data seeding, called from server.js)
      (plus backend-project/middleware/requireRole.js ONLY if the domain needs roles — see AUTHORIZATION)

Frontend:
  21. frontend-project/package.json   (exact pinned versions: React 18, react-router-dom v6,
      Tailwind v3, recharts, etc.; "dev" script via vite)
  22. frontend-project/vite.config.js   (React plugin + `@/` path alias → src)
  23. frontend-project/jsconfig.json    (`@/*` path mapping for editor/resolver)
  24. frontend-project/postcss.config.js (Tailwind v3 + autoprefixer)
  25. frontend-project/.env.example
  26. frontend-project/tailwind.config.js (Tailwind v3 config; darkMode: 'class'; content globs; theme tokens)
  27. frontend-project/src/index.css  (Tailwind v3 @tailwind directives + CSS variables + font import + @media print rules)
  28. frontend-project/src/lib/utils.js  (the `cn` helper using clsx + tailwind-merge)
  29. frontend-project/src/components/ui/*  (EVERY shadcn component you import, generated in full:
      button, input, card, table, dialog, alert-dialog, badge, label, tabs, select, alert, sonner —
      one file each under src/components/ui/, per the self-contained shadcn rule)
  30. frontend-project/src/App.jsx    (BrowserRouter + <AuthProvider> + routes + ProtectedRoute wrapper + global <Toaster />; includes <IconContext.Provider value={{ weight: 'regular' }}>)
  30b. frontend-project/src/main.jsx  (React render root that mounts <App />)
  31. frontend-project/src/context/AuthContext.jsx  (AuthProvider + useAuth hook + useTheme)
  32. frontend-project/src/api/axiosClient.js
  33. frontend-project/src/api/authAPI.js
  34. frontend-project/src/api/[entity1]API.js
  35. frontend-project/src/api/[entity2]API.js
  36. frontend-project/src/api/[entity3]API.js
  37. frontend-project/src/api/reportsAPI.js  (per-report fetchers + getDashboard())
  38. frontend-project/src/components/Navbar.jsx
  39. frontend-project/src/components/ProtectedRoute.jsx
  40. frontend-project/src/pages/LandingPage.jsx
  41. frontend-project/src/pages/LoginPage.jsx
  42. frontend-project/src/pages/RegisterPage.jsx
  43. frontend-project/src/pages/RecoverPage.jsx
  44. frontend-project/src/pages/DashboardPage.jsx  (>=4 stat cards + a summary table/list + one recharts LINE chart from /api/reports/dashboard)
  45. frontend-project/src/pages/[Entity1]Page.jsx
  46. frontend-project/src/pages/[Entity2]Page.jsx
  47. frontend-project/src/pages/[Entity3]Page.jsx
  48. frontend-project/src/pages/ReportsPage.jsx

=== FINAL OUTPUT RULES ===
- Work through the WORKFLOW PHASES in order. Output order: (1) a brief assumptions summary — the
  system name, entities/attributes, inferred relationships (one line each with reasoning), and the
  reports you will build (Phases 1-2); (2) the Mermaid ERD you drew from that analysis (Phase 2);
  (3) every file the app needs, built in phase order, including the root README.md whose "Build
  Phases" section recaps the phases you followed (Phases 3-10).
- You do NOT have to produce everything in a single response. It is fine to deliver the code across
  multiple messages — for example, finish the backend, then continue with the frontend in the next
  message. If you run out of room, stop at a clean file boundary and continue in the next message
  until everything is delivered. When splitting, keep going on your own; the human should not have to
  prompt "continue" between every file (though they may).
- Whatever the number of messages, each individual file must be delivered COMPLETE and runnable — do
  not truncate a file, do not summarize a file's contents, and do not write "same as above" or
  "unchanged" in place of real code. Splitting across messages is about length, never an excuse to
  abbreviate a file.
- Taken together, the delivered files must satisfy the RUNS-IMMEDIATELY GUARANTEE in DELIVERABLES with
  zero code edits.
- The entities, attributes, and relationships are NOT provided by the human (beyond the
  plain-English description and any optional overrides); you design and infer them yourself. Do not
  ask the human to supply or confirm them unless the description is too vague to pick a domain.
- The LandingPage (route /) is public and must render without any session; the LoginPage is at
  /login and the RegisterPage is at /register. Do not gate any of these three behind ProtectedRoute.
  After login/registration the user lands on /dashboard (protected).
- Do NOT suggest or include any feature for deleting the project, dropping the database, or
  bulk-wiping collections. The only deletion permitted is a single record on any entity, via
  DELETE /api/[that entity's route]/:id — never a multi-record or whole-collection wipe.
- Do NOT end by recommending similar projects, alternative project ideas, or "next steps you
  could build." Stop once the assumptions, ERD, README, and all the files are delivered.
