# Micro Frontend UI App — Generation Prompt

You are an expert frontend architect. Generate a complete, production-ready **Micro Frontend UI Application** based on the following specification. Scaffold every file with real, working code — no placeholders.

---

## Application Overview

Build a Micro Frontend architecture with:
- **1 Shell (Host) App** — the container that loads all remotes
- **3 Remote Micro Frontend Apps** — independently deployable UI modules
- **1 Shared Component Library** — common UI components and utilities

---

## Tech Stack

| Layer | Technology | Version |
|---|---|---|
| Framework | React + TypeScript | `react@18.3.1`, `typescript@5.4.5` |
| Module Federation | Webpack Module Federation Plugin | `webpack@5.91.0` |
| Styling | Tailwind CSS | `tailwindcss@3.4.3` |
| State Management | Zustand | `zustand@4.5.2` |
| Routing | React Router | `react-router-dom@6.23.1` |
| Build Tool | Webpack + ts-loader | `webpack-cli@5.1.4`, `ts-loader@9.5.1` |
| Package Manager | npm workspaces (monorepo) | `npm@10+` |
| Testing | Jest + React Testing Library | `jest@29.7.0`, `@testing-library/react@15.0.6` |
| API Layer | Axios with interceptors | `axios@1.7.2` |
| Charts | Recharts | `recharts@2.12.7` |
| Concurrency | Concurrently | `concurrently@8.2.2` |

---

## Project Structure

Generate the following monorepo layout:

```
microfrontend-app/
├── package.json                  # root workspace config
├── .gitignore                    # root gitignore
├── .eslintrc.js                  # shared ESLint config
├── .prettierrc                   # shared Prettier config
├── .github/
│   └── workflows/
│       └── ci.yml                # CI/CD pipeline
├── packages/
│   ├── shell/                    # Host app (port 3000)
│   │   ├── src/
│   │   │   ├── App.tsx
│   │   │   ├── bootstrap.tsx
│   │   │   ├── index.ts
│   │   │   ├── routes/
│   │   │   │   └── AppRoutes.tsx
│   │   │   ├── store/
│   │   │   │   └── useGlobalStore.ts
│   │   │   ├── components/
│   │   │   │   ├── Navbar.tsx
│   │   │   │   ├── Sidebar.tsx
│   │   │   │   └── Layout.tsx
│   │   │   └── remotes/
│   │   │       └── RemoteWrapper.tsx
│   │   ├── public/index.html
│   │   ├── webpack.config.js
│   │   ├── tsconfig.json
│   │   ├── .env
│   │   └── package.json
│   │
│   ├── remote-dashboard/         # Remote 1 — Dashboard (port 3001)
│   │   ├── src/
│   │   │   ├── App.tsx
│   │   │   ├── bootstrap.tsx
│   │   │   ├── index.ts
│   │   │   ├── pages/
│   │   │   │   └── Dashboard.tsx
│   │   │   └── components/
│   │   │       ├── StatsCard.tsx
│   │   │       ├── ChartWidget.tsx
│   │   │       └── RecentActivity.tsx
│   │   ├── public/index.html
│   │   ├── webpack.config.js
│   │   ├── tsconfig.json
│   │   ├── .env
│   │   └── package.json
│   │
│   ├── remote-users/             # Remote 2 — User Management (port 3002)
│   │   ├── src/
│   │   │   ├── App.tsx
│   │   │   ├── bootstrap.tsx
│   │   │   ├── index.ts
│   │   │   ├── pages/
│   │   │   │   ├── UserList.tsx
│   │   │   │   └── UserDetail.tsx
│   │   │   └── components/
│   │   │       ├── UserTable.tsx
│   │   │       ├── UserCard.tsx
│   │   │       └── UserForm.tsx
│   │   ├── public/index.html
│   │   ├── webpack.config.js
│   │   ├── tsconfig.json
│   │   ├── .env
│   │   └── package.json
│   │
│   ├── remote-settings/          # Remote 3 — Settings (port 3003)
│   │   ├── src/
│   │   │   ├── App.tsx
│   │   │   ├── bootstrap.tsx
│   │   │   ├── index.ts
│   │   │   ├── pages/
│   │   │   │   └── Settings.tsx
│   │   │   └── components/
│   │   │       ├── ProfileSettings.tsx
│   │   │       ├── ThemeToggle.tsx
│   │   │       └── NotificationSettings.tsx
│   │   ├── public/index.html
│   │   ├── webpack.config.js
│   │   ├── tsconfig.json
│   │   ├── .env
│   │   └── package.json
│   │
│   └── shared/                   # Shared library (not served)
│       ├── src/
│       │   ├── index.ts
│       │   ├── components/
│       │   │   ├── Button.tsx
│       │   │   ├── Modal.tsx
│       │   │   ├── Spinner.tsx
│       │   │   └── ErrorBoundary.tsx
│       │   ├── hooks/
│       │   │   ├── useFetch.ts
│       │   │   └── useDebounce.ts
│       │   ├── utils/
│       │   │   ├── formatDate.ts
│       │   │   └── apiClient.ts
│       │   └── types/
│       │       └── index.ts
│       ├── tsconfig.json
│       └── package.json
```

---

## Root Config Files

### `.gitignore`
Generate a `.gitignore` covering:
- `node_modules/` (all packages)
- `dist/` build outputs
- `.env` files (except `.env.example`)
- IDE folders: `.vscode/`, `.idea/`
- OS files: `.DS_Store`, `Thumbs.db`
- Coverage reports: `coverage/`

### `.eslintrc.js`
Generate a shared ESLint config with:
- `@typescript-eslint/recommended` rules
- `react-hooks` plugin rules
- `import/order` rule for consistent imports
- No `any` rule enforced

### `.prettierrc`
Generate Prettier config with:
- `singleQuote: true`
- `semi: true`
- `tabWidth: 2`
- `printWidth: 100`
- `trailingComma: 'es5'`

---

## Shell App Requirements

- Load all three remotes dynamically using `React.lazy` + `Suspense`
- Define routes:
  - `/` → Dashboard (remote-dashboard)
  - `/users` → User Management (remote-users)
  - `/settings` → Settings (remote-settings)
- Render `<Navbar>` and `<Sidebar>` in a persistent `<Layout>` wrapper
- Expose a global Zustand store that remotes can consume via shared singleton
- Show a fallback `<Spinner>` while remotes load
- Show `<ErrorBoundary>` if a remote fails to load

---

## Webpack Module Federation Config (per app)

For the **shell** `webpack.config.js`:
- Act as `host`
- Register all three remotes with their URL and exposed module name
- Share: `react`, `react-dom`, `react-router-dom`, `zustand` as singletons with `requiredVersion` pinned

For each **remote** `webpack.config.js`:
- Act as `remote`
- Expose the main page component (e.g. `./Dashboard`, `./UserList`, `./Settings`)
- Share the same singleton dependencies as the shell with matching `requiredVersion`

---

## Shared Library Requirements

Generate fully implemented, typed components:

- `<Button>` — variants: `primary`, `secondary`, `danger`; sizes: `sm`, `md`, `lg`
- `<Modal>` — controlled open/close, title, children, footer actions
- `<Spinner>` — centered loading indicator
- `<ErrorBoundary>` — catches render errors, shows fallback UI
- `useFetch<T>` hook — handles loading, error, data states with Axios
- `useDebounce<T>` hook — delays value updates
- `apiClient` — Axios instance with base URL from env, request/response interceptors, and auth token injection from `Authorization: Bearer <token>`
- Shared TypeScript types: `User`, `ApiResponse<T>`, `PaginatedResponse<T>`

---

## Remote: Dashboard

Generate a working dashboard page with:
- 4 `<StatsCard>` components showing mock KPIs (Users, Revenue, Orders, Growth)
- A `<ChartWidget>` using `recharts` (BarChart or LineChart) with mock data
- A `<RecentActivity>` list showing the last 5 mock activity entries
- Reads user info from the global Zustand store (set by shell)

---

## Remote: User Management

Generate a working user management page with:
- `<UserTable>` — paginated table (10 rows/page) with columns: Name, Email, Role, Status, Actions
- `<UserForm>` — create/edit user modal form with validation (name, email, role)
- `<UserCard>` — card view for mobile
- CRUD operations wired to mock API calls via `apiClient`
- Filter by role and status

---

## Remote: Settings

Generate a working settings page with:
- `<ProfileSettings>` — edit name, email, avatar upload placeholder
- `<ThemeToggle>` — light/dark mode toggle that persists to localStorage and updates Tailwind class on `<html>`
- `<NotificationSettings>` — toggle switches for email, push, and SMS notifications
- All changes save to the global Zustand store

---

## Authentication & Security

Generate a complete auth layer in the shell:

- `useAuthStore` (Zustand) — stores `accessToken`, `user`, `isAuthenticated`
- `<ProtectedRoute>` component — wraps routes, redirects to `/login` if unauthenticated
- `LoginPage` — simple email/password form, calls mock `/api/auth/login`, stores JWT in memory (not localStorage)
- `apiClient` interceptor — injects `Authorization: Bearer <token>` header on every request
- `apiClient` interceptor — on 401 response, clears auth store and redirects to `/login`
- Token refresh stub — `refreshToken()` function ready to wire up to a real endpoint
- All remotes receive auth state from the shell via the shared Zustand store singleton

---

## CI/CD Configuration

Generate a GitHub Actions workflow at `.github/workflows/ci.yml` that:
1. Triggers on push to `main` and on pull requests
2. Runs on `ubuntu-latest` with Node 20
3. Caches `node_modules` using `actions/cache`
4. Installs dependencies with `npm ci`
5. Runs ESLint across all packages
6. Runs Jest tests across all packages and uploads coverage report
7. Builds all packages with `npm run build --workspaces`
8. On push to `main` only: stub deploy step per package (echo the deployment command)

---

## Environment & Configuration

Generate `.env` and `.env.example` files for each app:

```
# shell/.env
REACT_APP_DASHBOARD_URL=http://localhost:3001
REACT_APP_USERS_URL=http://localhost:3002
REACT_APP_SETTINGS_URL=http://localhost:3003
REACT_APP_API_BASE_URL=http://localhost:8080/api

# remote-dashboard/.env / remote-users/.env / remote-settings/.env
REACT_APP_API_BASE_URL=http://localhost:8080/api
```

---

## Root package.json Scripts

```json
{
  "scripts": {
    "start": "concurrently \"npm run start --workspace=packages/shell\" \"npm run start --workspace=packages/remote-dashboard\" \"npm run start --workspace=packages/remote-users\" \"npm run start --workspace=packages/remote-settings\"",
    "build": "npm run build --workspaces",
    "test": "npm run test --workspaces",
    "lint": "eslint 'packages/*/src/**/*.{ts,tsx}'",
    "format": "prettier --write 'packages/*/src/**/*.{ts,tsx}'"
  }
}
```

---

## Code Quality Rules

- All components must be typed with TypeScript interfaces — no `any`
- Use functional components with hooks only — no class components
- Each component file must have a named export and a default export
- CSS via Tailwind utility classes only — no inline styles
- No hardcoded strings — use constants or config files
- All async operations must handle loading and error states
- ESLint and Prettier must pass with zero errors before build

---

## Output Instructions

1. Generate every file listed in the project structure with complete, working code
2. Start with root config files: `package.json`, `.gitignore`, `.eslintrc.js`, `.prettierrc`
3. Then generate `webpack.config.js` for shell and each remote
4. Then generate the shared library (`packages/shared/`)
5. Then generate shell app including auth layer (`packages/shell/`)
6. Then generate each remote in order: dashboard → users → settings
7. Then generate `.github/workflows/ci.yml`
8. After all files, provide a **Getting Started** section with:
   - `npm install` command
   - `npm start` command
   - Browser URLs for each app
   - How to add a new remote micro frontend
   - How to swap mock auth for a real identity provider

Begin generating now.
