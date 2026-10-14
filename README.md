# Legal Case Management System

React application with a local Node server, SQLite persistence, authenticated sessions, and role-based access for administrators, advocates, clerks, and clients.

## Run locally

Install Node.js 24 or newer. From this folder:

```powershell
npm ci
npm run dev
```

Open http://localhost:3000. Development runs the API and Vite on the same port.

For the production build:

```powershell
npm run build
npm start
```

The server listens on the local computer only. Set `PORT` to choose another port.

## Demo accounts

The login page includes shortcuts for these seeded accounts:

| Role          | Email                              | Password     |
| ------------- | ---------------------------------- | ------------ |
| Administrator | m.okonjo@harborlaw.example         | Admin@123    |
| Advocate      | d.reyes@harborlaw.example          | Advocate@123 |
| Clerk         | p.raghunathan@harborlaw.example    | Clerk@123    |
| Client        | helena.vaughn@vaughnretail.example | Client@123   |

Passwords can be changed in Settings. Suspension revokes existing sessions. Recovery requests appear in administrator user management, where an administrator can reset the account password.

## Data and workflows

Records persist in `.data/legal.sqlite`. Uploaded document bytes are encrypted using `.data/vault.key`; back up the entire `.data` folder together while the server is stopped. `LCM_DATA_DIR` selects a separate data directory. Old browser-local demo records are not automatically imported.

The API checks permissions and ownership for each operation. Clients see their own matters and billing; advocates see assigned matters. Database export and reset are administrator-only. Password hashes and file encryption keys are never included in exports.

Seed documents contain metadata only because original files were not provided. Upload an actual file to enable download and verification. Case deletion removes its related hearings, documents, invoices, and payment requests.

Payments use bank or UPI transfer references. A client reference remains pending until staff confirms receipt; staff can also record a confirmed receipt directly. This app does not process card transactions or verify transfers with a bank. Reports and CSV exports use the selected date range; financial totals group invoices by issue date and show their confirmed collections to date.

## Verification

```powershell
npm test
npm run typecheck
npm run lint
npm run build
npm audit
```

Regression tests use isolated temporary databases and cover access controls, sessions, record validation, document bytes, payment balances, persistence, and deletion cascades.
