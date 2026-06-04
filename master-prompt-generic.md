=== PROJECT BRIEF (fill in the blanks; leave any blank to let the AI decide) ===
This prompt builds an app with the SAME shape and quality as the reference Stock Management System
(a catalogue item, an actor/location, and a movement record that links them, plus auth, dashboard and
reports) — but for WHATEVER DOMAIN YOU WRITE BELOW. It must NOT be a stock-management system; fill in a
different domain (e.g. a school, clinic, library, payroll, transport, etc.).

Project description: ____________________________________________________________
  (one or two sentences, e.g. "A clinic system to register patients and record their visits")

System name (optional):           ______________________________
Company/organization (optional):  ______________________________
Database name (optional):         ______________________________
Database type (optional):         ______________________________
  (relational/SQL  OR  document/NoSQL — blank = relational/SQL.)
Database engine (optional):       ______________________________
  (the specific engine; blank = pick a sensible default for the chosen type.
   SQL options: MySQL (default) / PostgreSQL / SQLite.   NoSQL options: MongoDB.
   If engine is given, it wins and implies the type.)

Entities (optional — leave blank to let the AI choose; aim for 3 domain entities + Users. List each entity
WITH its own attributes if you want; whatever you write is used VERBATIM — the AI only infers each
attribute's type/constraints and the relationships, and fills in anything you leave blank):
  Entity 1 (the "catalogue" item):     ______________________________
    its attributes: ______________________________
  Entity 2 (the "actor/place"):        ______________________________
    its attributes: ______________________________
  Entity 3 (the "movement/event" that references Entity 1 and Entity 2): ______________________________
    its attributes: ______________________________

Per-entity operations (optional):  ______________________________
  (default: full CRUD on all three. To restrict, name which are INSERT-ONLY and which are full CRUD.)

Layout style (optional):           ______________________________   (minimal / editorial / bold / corporate / …)

Data visibility (optional):        ______________________________
  (blank = per-user: each user sees only their own records, seed data belongs to admin;
   "shared" = one shared dataset; "admin sees all" = per-user but admin sees everyone.)

Reports required (optional — leave blank to let the AI design the right ones for the domain):
  Report 1 — Name: __________  Shows: __________  Group/total by: __________  Filter by: __________
  Report 2 — Name: __________  Shows: __________  Group/total by: __________  Filter by: __________
  Report 3 — Name: __________  Shows: __________  Group/total by: __________  Filter by: __________

=== INSTRUCTIONS TO THE AI ===
You are a senior full-stack developer. Build the complete application described in the PROJECT BRIEF
using React.js (frontend) and Node.js/Express.js (backend). Use the database chosen in the brief: honor the
Database engine if given; otherwise pick a sensible default for the Database type (relational/SQL -> MySQL,
document/NoSQL -> MongoDB); if both are blank, default to MySQL. For SQL engines, access the database
through its OFFICIAL Node.js driver with hand-written, parameterized queries and NO ORM (no Sequelize,
no Prisma). For MongoDB, you MUST use Mongoose (schemas + models) — it is the one allowed ODM. The brief is the only
required input. Any blank field and every [SQUARE BRACKET] below is a value YOU derive with a sensible
default; if a field is filled in, honor it exactly. When the brief names entities and/or their attributes,
treat them as AUTHORITATIVE — build exactly those entities with exactly those attributes (never rename,
drop, merge, or invent extra ones); you still infer each attribute's type/constraints and the
relationships, and you still fill in any entity or attribute left blank. Do not ask the human to complete
the brief or to confirm the entities, attributes or relationships — make the most reasonable assumption
and proceed.

Before writing code, output: (1) a short assumptions summary (system name, entities/attributes,
inferred relationships one line each, per-entity operations, the reports); (2) a Mermaid erDiagram showing
every entity, its attributes, the PRIMARY KEY and FOREIGN KEY of each table, and the relationships; and
(3) a Level-0 Context Diagram (a Mermaid flowchart) with ONE central process for the whole system, every
actor as an external entity, and labelled, arrowed data flows both into and out of the process.
Then build every file. Deliver README.md, ERD.md (the erDiagram) and CONTEXT-DIAGRAM.md (the Level-0
context diagram) at the project root.

=== HOW TO INFER THE MODEL ===
- Choose 3 domain entities plus Users (more/fewer only if the domain clearly needs it).
- [Entity3] is a "movement/event" that references [Entity1] and [Entity2] (like a transaction linking a
  product and a warehouse). The reference is a foreign-key column (SQL) or a stored id field (document DB)
  on B pointing to A, meaning A has-many B (one-to-many).
- If the domain needs a quantity/balance, compute it on the SERVER from the movement records
  (e.g. available = base + (increase events) - (decrease events)), and never let a "decrease" exceed
  what is available. Mirror that rule on the client.
- Unless data visibility is "shared", every domain entity also carries an owner reference to the User
  (a NOT-NULL indexed `owner_id` foreign-key column in SQL, or an indexed `ownerId` field in a document DB)
  set server-side to the creating user; all reads/writes and every aggregation filter by the owner
  (admin is unscoped only in "admin sees all").

=== BACKEND RULES ===
- Libraries: express, cors, bcryptjs, express-session, dotenv, nodemon, express-rate-limit, PLUS the
  data layer for the chosen DB: MySQL -> `mysql2` (promise pool, no ORM); PostgreSQL -> `pg` (Pool, no ORM);
  SQLite -> `better-sqlite3` (no ORM); MongoDB -> `mongoose` (schemas + models — the one allowed ODM).
- For MySQL specifically, config/db.js imports the driver and creates the connection by explicitly passing
  the host, user, password AND database (from env) — so the connection step is unambiguous — then exports
  the pool/connection for the data-access modules.
- Layered, never mixed: thin routes -> controllers (all logic, validation, status codes, JSON) -> models.
  For SQL, a model is a data-access module that exports plain functions (findAll/findById/create/update/
  remove) running the driver queries and returning plain objects. For MongoDB, a model is a Mongoose
  schema + model (schema definition only — no business logic). Either way, controllers hold the logic, the
  model holds no HTTP, and the routes dir holds NO validation or business logic — a route ONLY maps an HTTP
  method+path to a controller function and attaches middleware; EVERY validation lives in the controllers,
  never in /routes. Reusable checks (auth) live in /middleware.
- RESPONSE SHAPE (no exceptions): `{ data }` on success, `{ error: "message" }` on failure.
  Status codes: 200/201/400/401/404/500. Protect all non-auth routes with a session-check middleware (401).
- Auth endpoints: register, login, logout, me, recover/verify, recover/reset. Session management is via
  express-session with httpOnly cookies — NOT JWTs. Configure express-session({ secret: SESSION_SECRET,
  resave: false, saveUninitialized: false, cookie: { httpOnly: true } }) (also set cookie.secure + an
  appropriate sameSite in production). On successful register and login, start the session by storing
  { userId, username } on req.session and respond with `{ data: { user } }`. The session-check middleware
  (requireAuth) returns 401 when req.session has no userId; owner-scoping and aggregations use
  req.session.userId. GET /api/auth/me returns the session user, or 401. POST /api/auth/logout destroys
  the session (req.session.destroy) and clears the cookie.
  * register validates everything, hashes the password, rejects duplicate username/email/phone (400),
    generates a random 4-digit recovery code, stores ONLY its bcrypt hash, and returns the plaintext ONCE.
  * Never return password or recoveryCodeHash to the client: select them only when needed for
    bcrypt.compare (SQL: an explicit column list / a dedicated "find with secrets" query; Mongoose: mark
    them `select:false` and re-select with `.select('+password')`), and delete those fields from any user
    object you send back. Throttle login and recovery (4-digit codes are easy to brute-force).
- SQL queries go through the official driver using PARAMETERIZED queries (mysql2/pg/sqlite placeholders) —
  never build query strings by concatenating user input. MongoDB access goes through Mongoose model methods
  (find/findById/create/findByIdAndUpdate/deleteOne, aggregate). Compute derived fields server-side.
- On startup: connect to the DB once (a shared SQL pool, or a single `mongoose.connect`). For SQL engines,
  create the schema if it does not exist (run CREATE TABLE IF NOT EXISTS statements / a schema.sql) before
  seeding; for MongoDB, indexes are declared on the Mongoose schemas. Then seed.
- Seeding (idempotent, on startup): seed one admin from SEED_ADMIN_* env vars with a known recovery code,
  plus a few sample rows per entity (some dated today) owned by the admin. Never duplicate on restart;
  never log the password or code.
- Routes per entity: POST (create), GET (list), PUT/:id (update), DELETE/:id (delete one record only).
  An INSERT-ONLY entity gets only POST (+ GET if another form needs it as a dropdown). Deleting a
  referenced parent record returns 400 (no orphaned movement records) — check for referencing rows before
  deleting (and use RESTRICT foreign keys in SQL).
- Reports: one aggregation query each, filtered by owner + date range. SQL: a single query with JOINs,
  GROUP BY and SUM/COUNT, ORDER BY, selecting only the needed columns. MongoDB: one Mongoose aggregate()
  pipeline ($match owner + date range, $lookup to join, $group to total, $sort, $project). Default the date
  filter to today. Also one /dashboard aggregation.

=== FRONTEND RULES ===
- React 18 + Vite, react-router-dom v6, Tailwind CSS v3, shadcn/ui, Phosphor Icons (regular weight via a
  single IconContext.Provider), Sonner toasts (one global <Toaster/>), axios. Pin exact versions.
- Generate every shadcn component you import, in full, under src/components/ui/ (button, input, card,
  table, dialog, alert-dialog, badge, label, tabs, select, alert, sonner) with the cn() util and the
  `@/` alias in vite.config.js AND jsconfig.json. No phantom imports.
- Auth state via AuthContext (useAuth): it calls GET /api/auth/me on mount to hydrate the session and
  exposes login/logout/register. ProtectedRoute, Navbar, AuthPage and RecoverPage read from useAuth().
  axiosClient sets withCredentials:true on every request (so the session cookie is sent) and, on a 401,
  redirects to /login.
- Build reusable pieces (declared at module top level, never inside another component): PageWrapper,
  FormField (Label + Input + inline error), StateBlock (loading/error/empty), a confirm dialog, and a
  pagination helper. Pages compose these.
- Pages:
  * Navbar (shown on every PROTECTED page) — a single shared nav bar that lists a link to EVERY protected
    page: Dashboard, ONE link per entity page, and Reports. Each link MUST pair its own Phosphor icon with
    its label — every page's icon appears on the Navbar with NO exceptions (Dashboard, each [Entity], and
    Reports all have a distinct Phosphor icon). The active route is highlighted with the accent; the bar
    also shows the current user and a Logout action, and collapses to a mobile menu on small screens (the
    per-page icons stay visible).
  * LandingPage (/) — PUBLIC, its own minimal top bar (system name + Sign In), a hero with a primary
    "Get Started" (→ /register) and secondary "Explore More" (smooth-scroll to a #services section), and a
    capabilities section. Distinct layout; not wrapped in ProtectedRoute.
  * AuthPage (/login AND /register) — PUBLIC, one floating two-part card (accent showcase panel + form),
    segmented Sign In / Sign Up tabs derived from the URL (useLocation). Sign Up shows the one-time
    recovery code (copy + download + "I've saved it" before continuing).
  * RecoverPage (/recover) — PUBLIC, verify code -> set new password -> show the new one-time code.
  * DashboardPage (/dashboard) — PROTECTED, >=4 stat cards (counts + one SUM/AVG) and a recent-activity
    summary, from a single /api/reports/dashboard call. A "Quick Actions" card (REQUIRED) sits beside the
    summary (e.g. lg:grid-cols-3, summary lg:col-span-2, Quick Actions lg:col-span-1) and lists one button
    per full-CRUD entity (Phosphor icon + "New {Entity}" label + one-line description); clicking opens that
    entity's create form in a Dialog modal (reusing its {Entity}Form), and a successful create closes the
    modal and re-fetches /api/reports/dashboard so the stats refresh.
  * One page per entity — TWO cards in a responsive grid (lg:grid-cols-2): a "New {Entity}" card holding
    the create form, and an "All {Entity}s" card holding the searchable, sortable, paginated table (each
    card a shadcn Card with a CardTitle of exactly "New {Entity}" / "All {Entity}s"). Full-CRUD entities
    get a per-row Edit (Dialog) and Delete (AlertDialog); an INSERT-ONLY entity shows only the
    "New {Entity}" card.
  * ReportsPage (/reports) — Tabs (one per report), a filter control (default a date picker = today, or a
    daily/weekly/monthly selector), an empty state, and a Print button (window.print) with a print-only
    header/footer; print the FULL report, not just the current page.
- Every API call: try/catch/finally, inline shadcn Alert for server errors, a toast on every action
  result, a loading spinner in the submit button, and "Unable to connect to the server. Please try again."
  on network errors. Refresh the affected list after create/update/delete.
- React fundamentals to apply visibly: useState and useEffect in the pages that load/hold data; at least
  one CUSTOM hook beyond useAuth (e.g. a useFetch/use[Entity] data hook) declared either in its own file
  under src/hooks/ OR alongside the shared helpers in components/common.jsx, and actually used; explicit event handlers (onClick/onChange/onSubmit) with arguments passed where needed;
  list rendering with .map and stable keys.
- Responsive, mobile-first design on every page: Tailwind flex and grid utilities with responsive
  breakpoints (Tailwind sm/md/lg prefixes and/or CSS @media queries); layouts reflow cleanly from phone to desktop and stay readable; interactive
  elements keep visible hover and focus states.

=== VALIDATION (client AND server) ===
- Name fields: letters, spaces, hyphens, apostrophes only — /^[A-Za-z]+([ '-][A-Za-z]+)*$/
- Email: /^[^\s@]+@[^\s@]+\.[^\s@]+$/
- Phone (Rwanda): exactly 10 digits starting 078/079/073/072 — /^(078|079|073|072)\d{7}$/
- Recovery code: exactly 4 digits — /^\d{4}$/
- Rwandan plate (only if the domain needs it): /^R[A-Z]{2} \d{3} [A-Z]$/ after uppercasing.
- Numbers > 0 where required; integers >= 0 for counts; dropdowns must have a selection; dates not in the
  future (HTML max = today); a "decrease" movement must not exceed the available amount.
- Inline red error under each invalid field; disable submit while sending; reset the form on success.

=== DESIGN TOKENS (FIXED signature — keep identical across builds) ===
- Light mode only; all colors are CSS variables on :root.
- Page background: --color-bg: #F6F8FF.   Accent (FIXED): --color-accent: #003F91 — used for active nav,
  primary buttons, focus rings, highlights and badges.
- Signature shadow on every elevated surface (cards, modals, navbar):
  --shadow-accent: 0 4px 14px -4px rgba(0, 63, 145, 0.25);  (removed only in @media print).
- Font (FIXED): Google Font "Outfit" applied globally (weights 400/500/600/700); tabular-nums on all
  figures, which are shown in FULL with thousands separators (never compacted or abbreviated — no 1.2k or $1.2M);
  small uppercase muted table-column headers; slight negative letter-spacing on headings/big numbers.
- No emoji; Phosphor icons only at consistent sizes. No purple gradients on white.
- @media print: `.no-print` hides chrome; `.print-only` shows the report header/footer; tables print as
  plain black-on-white with borders and no shadows.

=== REQUIRED ENV VARS ===
- backend-project/.env.example always has: PORT=5000, NODE_ENV=development, SESSION_SECRET=change-me,
  CLIENT_URL=http://localhost:5173, SEED_ADMIN_USERNAME=admin, SEED_ADMIN_PASSWORD=change-me,
  SEED_ADMIN_RECOVERY_CODE=1234 — PLUS the connection vars for the chosen DB:
  * MySQL/PostgreSQL: DB_HOST=localhost, DB_PORT=3306 (Postgres 5432), DB_NAME=[DB], DB_USER=root,
    DB_PASSWORD=change-me.
  * MongoDB: MONGODB_URI=mongodb://localhost:27017/[DB].
  * SQLite: DB_FILE=./data/[DB].sqlite.
- frontend-project/.env.example: VITE_API_URL=http://localhost:5000/api.
- server.js validates the chosen DB's connection vars and SESSION_SECRET at startup and exits with a clear
  message if missing.
- Never commit real .env files; deliver only .env.example.

=== PROJECT STRUCTURE (use this exact layout; one file per entity, named after the REAL entity) ===
[ROOT_FOLDER_NAME]/                     ← choose a clear root folder name (or use one the human gives)
├── README.md                           ← project doc + "Build Phases" recap
├── ERD.md                              ← Mermaid erDiagram (entities, attributes, PK/FK, relationships)
├── CONTEXT-DIAGRAM.md                  ← Mermaid Level-0 context diagram (one process, actors, dataflows)
├── backend-project/
│   ├── package.json (pinned; "dev": nodemon)   ├── .env.example   ├── server.js
│   ├── config/ (db.js, seed.js   [+ schema.sql for SQL engines])
│   ├── models/ (User.js, [Entity1].js, [Entity2].js, [Entity3].js — data-access modules for SQL,
│   │            Mongoose schemas/models for MongoDB)
│   ├── controllers/ (auth, [entity1], [entity2], [entity3], reports .controller.js)
│   ├── routes/ (auth, [entity1], [entity2], [entity3], reports .routes.js)
│   └── middleware/ (requireAuth.js   [+ requireRole.js only if the domain needs roles])
└── frontend-project/
    ├── package.json (pinned) ├── .env.example ├── vite.config.js ├── jsconfig.json
    ├── postcss.config.js ├── tailwind.config.js ├── index.html
    └── src/
        ├── App.jsx ├── main.jsx ├── index.css
        ├── lib/ (utils.js, constants.js, validators.js, format.js   [+ a domain helper if needed])
        ├── hooks/ (useFetch.js or a use[Entity].js custom hook — OPTIONAL; the custom hook MAY instead
        │           live alongside the shared helpers in components/common.jsx)
        ├── context/ (AuthContext.jsx)
        ├── api/ (axiosClient.js, authAPI.js, [entity1]API.js, [entity2]API.js, [entity3]API.js, reportsAPI.js)
        ├── components/ (Navbar.jsx, ProtectedRoute.jsx, common.jsx,
        │                [Entity1]Form.jsx, [Entity2]Form.jsx, [Entity3]Form.jsx, ui/*)
        └── pages/ (LandingPage, AuthPage, RecoverPage, DashboardPage,
                    [Entity1]Page, [Entity2]Page, [Entity3]Page, ReportsPage)

=== EXAM RUBRIC ALIGNMENT (the build is graded against these — every item must be demonstrably present) ===
This prompt is practice for a national practical exam. On top of everything above, GUARANTEE the following
graded indicators appear in the delivered code (keep the advanced stack — these are additions, not
replacements):
- Database design (preliminary): the ERD shows every entity + attributes, a PRIMARY KEY per table, and the
  FOREIGN KEYS linking child to parent; the Level-0 context diagram shows one process, each actor as an
  external entity, and labelled arrowed dataflows in and out. Keep entity/relationship wording consistent
  with the brief.
- Physical schema (SQL): DDL that CREATEs each table with an explicit PRIMARY KEY and the FOREIGN KEY
  constraints that mirror the ERD (child table holds the parent's key). Tables created idempotently on
  startup.
- React.js setup: a real React project with react-router-dom and axios installed and used.
- Node.js setup: an Express project with cors and nodemon ("dev": nodemon) installed and used.
- React basics: components are declared functions that return JSX, are exported, and the root is mounted to
  the DOM; a Login form plus one input form per entity exist.
- UI navigation: routes configured, links created, navigation between pages works.
- Hooks: useState and useEffect imported and applied; at least one custom hook defined and used.
- Events & lists: event listeners with arguments where needed; a list-handling function; lists rendered.
- Tailwind: installed, configured, imported, used in JSX, with flexbox and responsive grid; mobile-first.
- Server/DB connection (MySQL): server.js sets up Express + cors, picks a port, creates the app and
  listens; the DB driver is imported and the connection is created passing host, user, password and
  database, with a clear success/failure message logged.
- REST CRUD: POST, GET, PUT and DELETE endpoints per entity, backed by INSERT, SELECT, UPDATE and DELETE
  statements (parameterized).

=== CODE QUALITY ===
Write simple, beginner-readable code: clear names, small focused functions, no clever one-liners, one
responsibility per file. Add a short comment ONLY where intent is not obvious (the recovery-code flow,
owner-scoping, each aggregation query, any computed balance, non-trivial validation). Do not over-engineer
or add features, options or error handling that the brief did not ask for.

=== DELIVERABLE ===
Every file complete and immediately runnable with zero edits after: backend `npm install` + copy
.env.example to .env + `npm run dev`; then frontend `npm install` + copy .env.example to .env +
`npm run dev` (API on :5000, app on :5173, the chosen database running and reachable, and its database
created — SQLite needs no server). Every frontend call maps to a real route,
every route to a real controller, every import to a real file. No placeholder TODOs, no hardcoded secrets.
Do NOT suggest deleting the project, dropping the database, or bulk-wiping tables/collections — the only
delete is a single record via DELETE /api/[entity]/:id. End the README with a "Build Phases" recap of the
steps you followed.
