# PaisaPilot v0.3.3

A local-first, zero-cloud-cost PWA for Android/Chrome.

## What's included
- Preserves v0.1: expense tracking, savings wins (realised vs potential), money moves, monthly baseline, surplus estimate, emergency-fund-first guidance, category breakdown, delete/reset.
- IndexedDB storage. On first run, migrates the `paisapilot_v1` localStorage dataset from v0.1 into IndexedDB without deleting the old localStorage copy.
- Encrypted backup/restore: PBKDF2-SHA-256 (310,000 iterations) derives an AES-256-GCM key. Password is never stored. Restore validates and asks before replacing current data.
- Salary, bonus and other actual credited income ledger. If no income is entered in a month, the baseline estimate is used.
- Monthly category budgets and spending-type budgets (Essential / Commodity, Discretionary, Luxury, Ad hoc / Unexpected, Fixed commitment, Other).
- Recovery savings target and tracker; transfers recorded as Money moves (Recovery savings).
- v0.2 records are retained when v0.3 runs at the same web origin and path. Encrypted v0.2 backups can be restored into v0.3.
- PWA manifest + service worker for offline app shell.
- Manual transaction and receipt details only; no OCR, bank connection, analytics, cloud sync or multi-device access.

## Run locally
For development, serve this folder from localhost (service workers require localhost or HTTPS):
`python3 -m http.server 8000`
Then open `http://localhost:8000`.

## Install on Android
1. Put the contents of this folder on a static HTTPS host you control (e.g. GitHub Pages). This hosts only the app shell; no financial records are uploaded.
2. Open the HTTPS site in Chrome on Android.
3. Menu (⋮) → **Install app** / **Add to Home screen**.
4. Open PaisaPilot from the home screen. Use Settings & backup to create encrypted backups regularly.

## Data and security
- Records are held in IndexedDB within the browser profile on this device.
- No server receives financial records. There is no cross-device sync.
- Browser data clearing, device loss or browser-profile reset can erase records. Keep encrypted backups somewhere safe.
- Anyone with access to your unlocked browser profile may be able to use the app. IndexedDB is not itself encrypted at rest. Backup encryption protects the exported backup file, not the live database.
- If you forget the backup password, the file cannot be recovered. Do not store your bank password, UPI PIN or card CVV here.

## Notes
- The surplus number is an estimate, not a live bank balance. It excludes transfers, card settlement timing, refunds and irregular cashflows unless manually represented.
- The app provides educational budgeting prompts, not regulated financial advice.
- No paid APIs or external JavaScript libraries are used.

## Beginner setup and migration
1. Back up v0.2 first (Settings & backup → Create encrypted backup).
2. Replace your existing hosted files with the contents of this v0.3 `paisapilot` folder **at the same HTTPS URL**. The browser should retain IndexedDB records.
3. Open/reload the site and confirm v0.3, existing expenses and savings are visible. If needed, restore your v0.2 encrypted backup.
4. Go to Salary & income and record the amount actually credited. Then Budgets to set limits for each spending type and category.
5. If you enter home-loan EMI as an expense, do not also include it under Settings → Fixed commitments.
6. In Recovery, enter your own target; add a Money move → Recovery savings whenever you actually set aside money.
7. Create a fresh encrypted v0.3 backup.

Important: A ZIP opened directly from Files is not a reliably installable PWA; serve it over HTTPS or localhost. Each browser/site origin has separate storage. The app does not connect to banks, verify salary deposits, or encrypt IndexedDB at rest.


## v0.3.3 validation patch
- Restore now overwrites the single IndexedDB state record transactionally instead of clearing the object store first.
- Service-worker activation only removes older PaisaPilot-prefixed caches; unrelated site caches are preserved.
- Added 192px and 512px PNG app icons for Android installability checks.

This is a hardening patch, not a substitute for testing on the target Android device. Create an encrypted backup before upgrading any existing installation.


## v0.3.3 dashboard reporting
- Dashboard defaults to the current month.
- Added period selector for week, month, quarter, year, and all-time views.
- Date anchor lets you inspect earlier or later reporting periods.
- Transaction records and existing IndexedDB data format remain unchanged.


## v0.3.3 custom date range
- Added a Custom date range dashboard option with independent From and To dates.
- Existing week, month, quarter, year, and all-time reporting options remain available.
- Default dashboard period remains Current month.
- Existing IndexedDB records and backup format are unchanged.
