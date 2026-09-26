# Artemis ERP — Inventory & Sales Management System

🌐 **[English](README.md) | [فارسی](README.fa.md) | [Deutsch](README.de.md) | [العربية](README.ar.md)**

**A full-stack business management and point-of-sale (POS) application for small businesses and retail stores, with a database-aware AI assistant and sales forecasting built in.**

`JavaScript` · `HTML/CSS` · `Supabase` · `PostgreSQL` · `Capacitor` · `Gemini API`

![Artemis dashboard](docs/screenshots/dashboard.png)

**At a glance:**
- Full retail POS/ERP (sales, inventory, returns, accounting, printing) — plus a database-aware AI assistant and heuristic sales forecasting, not just CRUD.
- Self-hosted on your own Supabase project (Postgres + Auth + Realtime + Edge Functions); ships as both a web app and a native Android app via Capacitor.
- Built and refined over two years of real, daily use in an actual store — not a tutorial project.

---

## Overview

**Artemis** started as a way to avoid paying for — and dealing with the installation headaches of — expensive commercial POS software for a real retail store, and grew feature by feature over two years of daily production use into a full business management platform: sales, inventory, customers, staff, accounting, printing, and an AI assistant that actually knows the store's data.

It runs as a browser-based web app **and** as a native Android app (via Capacitor), both backed by a self-hosted [Supabase](https://supabase.com) project — every deployment owns its own data completely.

> **Project status:** Active personal project, in daily production use. This repository is a public demo/portfolio version — see [Public Repository Notice](#public-repository-notice).

## Why Artemis?

The project was built around real operational problems, not as an academic CRUD exercise. Features were added when a specific, practical need appeared:

```text
Real business problem → design a workflow → implement it → test with real
sales → find edge cases → improve reliability → integrate with the rest
of the system
```

Concrete examples from this codebase: item exchanges that settle only the price difference instead of two full transactions, an offline sales queue that can't create duplicate invoices when connectivity drops mid-sale, a multi-provider SMS/printing layer instead of hard-coding one vendor, and a sales forecast that adjusts itself from real occasion-day history instead of a fixed hand-tuned multiplier.

---

## Core Features

**POS & Sales** — Barcode-scanning checkout, discounts, cash/card/transfer/credit payments, wholesale (colleague) sales, and a separate job-costing module for service work (e.g. print/copy jobs) with itemized cost-component breakdown.

**Inventory Management** — Stock tracking with low-stock alerts, categories, Kardex (in/out history) per item, and barcode label generation/printing for items that don't already have one.

**Returns & Exchanges** — Partial/full returns with automatic stock and profit adjustment; item exchanges that settle only the price difference, fully tracked rather than handled as a manual workaround.

**Customer Management** — Profiles, purchase history, debt tracking, and bulk SMS to debtors/all customers/custom numbers through a swappable provider layer (Kavenegar, MeliPayamak, SMS.ir, or any REST API via a generic webhook).

**Loyalty System** — Points-based loyalty with configurable earn/redeem rates and purchase-based customer tiers (regular / active buyer / VIP).

**Accounting & Reports** — Balance sheet, multiple cash/bank accounts, expense tracking, opening balance, daily/profit reports, and purchase/check-payment reminders.

**Printing** — A4/A5/A6 and thermal receipt printing (58mm/80mm), direct Bluetooth printing from the Android app, fully configurable invoice content, and amount-in-words.

**Multi-user** — Manager/staff roles with separate logins and per-staff action tracking.

**Localization** — Persian, English, German, Arabic, with full RTL support and multi-currency.

**Web + Android** — Single-file web app (no build step) plus a Capacitor-packaged Android app with camera barcode scanning, fingerprint login, and local notifications.

---

## AI-Powered Business Assistant

One of Artemis's central features is a chat assistant that's built around the store's actual data, not a generic chatbot bolted on the side.

### Two personas

- **Artemis** — a more formal assistant for financial and business queries.
- **Aria** — a lighter, conversational persona for quick day-to-day questions.

Each has its own color scheme, greeting style, and tone; switching between them is instant from the chat widget.

### A hybrid, three-layer architecture

1. **Local intent engine** — greetings, thanks, and other common patterns are recognized and answered directly from in-app logic, with no server round-trip.
2. **Direct database queries** — business questions ("how much did we sell today", "who are the top customers", "which products are low on stock") are answered by querying Supabase/PostgreSQL directly from the client.
3. **AI / server-side processing (Gemini)** — anything more open-ended is routed to a Supabase Edge Function (`gemini-handler`), keeping AI calls and any related secrets off the client entirely.

### Sales forecasting

The assistant can forecast the coming week or month from real sales history using a rule-based (heuristic) model — not a trained machine-learning model:

- Builds a **per-weekday baseline** from historical sales.
- Learns a real **occasion multiplier** for recurring events (e.g. a specific holiday) by comparing actual past sales on that occasion against the weekday baseline, weighted so recent occurrences count more than older ones — falling back to a manually configured multiplier when there isn't enough history yet.
- Assigns each forecast day a **confidence level** (high/medium/low/unclear) based on how much historical data backs it, and flags low-confidence days in the output instead of presenting every number with equal certainty.
- Can also break the forecast down by which items are expected to contribute to it.

### Assistant memory

Two levels of context: short-term conversation history for the current session, and a small set of longer-term facts persisted in the database so the assistant can carry useful context across sessions rather than starting cold every time.

### Sensitive data protection

Financial and inventory queries can require a separate access code before the assistant will expose them, on top of Supabase's own Row Level Security and role-based rules.

---

## Technical Architecture

```text
                          Artemis
                              |
             +----------------+----------------+
             |                                 |
        Web / Browser                     Android App
             |                                 |
             +----------------+----------------+
                              |
                           Supabase
                              |
       +----------------------+----------------------+
       |                      |                       |
   PostgreSQL              Auth (RLS)              Realtime
       |
   Edge Functions
       |
  Gemini AI Assistant · SMS Gateway · Invoice Scanner
```

**Frontend** — Single-file HTML5/CSS3/JavaScript, no build step, responsive and RTL-aware.
**Backend** — Supabase: PostgreSQL, Auth, Realtime, Edge Functions, Row Level Security.
**Mobile** — Capacitor-wrapped Android app with native device APIs.
**AI** — Google Gemini via a dedicated Edge Function, plus the local intent/forecasting logic described above.
**Automation** — GitHub Actions for Android builds.

---

## Offline & Synchronization

Sales can continue to be recorded when the connection drops. Pending sales are held in a local queue keyed by a client-generated ID; when connectivity returns, the queue is flushed against that ID so a connection that drops mid-sync can't create the same invoice twice.

---

## Security & Authorization

- Supabase Authentication and PostgreSQL Row Level Security (RLS)
- Role-based access rules (manager vs. staff)
- A separate access code gating sensitive assistant queries (sales, profit, inventory, financial data)
- Server-side Edge Functions for AI calls, SMS sending, and invoice scanning, so provider credentials never reach the client

> **Before deploying:** configure your own Supabase project. Never commit service-role keys, passwords, private API keys, or real production/customer data to a public repository.

---

## Screenshots

Full-size images are in [`docs/screenshots/`](docs/screenshots/).

| | |
|---|---|
| **POS / Sales**<br>![Sales](docs/screenshots/sales.png) | **Inventory**<br>![Inventory](docs/screenshots/inventory.png) |
| **Returns & Exchange**<br>![Returns](docs/screenshots/returns.png) | **Customers & Loyalty**<br>![Customer Club](docs/screenshots/customer-club.png) |
| **Reports & Balance Sheet**<br>![Reports](docs/screenshots/reports.png) | **Settings**<br>![Settings](docs/screenshots/settings.png) |
| **AI Assistant**<br>![Assistant](docs/screenshots/assistant.png) | **Android App**<br>![Android](docs/screenshots/android.png) |

---

## Getting Started

Artemis uses your own Supabase project for storage — you own your data completely.

1. Create a free [Supabase](https://supabase.com) project.
2. Open `index.html` in a browser — you'll see a first-run setup screen.
3. In your Supabase project's SQL editor, run the table-creation script (the app can generate it for you once connected, from **Settings → Download SQL script**).
4. Enter your Supabase Project URL and anon/public key in the setup screen.

No frontend build step is required.

## Android Build

The Android app is packaged with [Capacitor](https://capacitorjs.com). See the `android/` folder and the GitHub Actions workflow under `.github/workflows/` for the build pipeline. Native capabilities include camera barcode scanning, fingerprint authentication, local notifications, and direct Bluetooth thermal printing.

---

## Engineering Highlights

- Item-exchange settlement that reconciles only the price difference, not two offsetting full-price transactions
- Offline sales queue with client-generated IDs to prevent duplicate invoices on reconnect
- Three-layer assistant architecture (local intent → direct DB query → server-side AI) instead of routing every message through an LLM
- Sales forecasting with a learned, recency-weighted occasion multiplier instead of a static hand-tuned one
- Row Level Security and role-based access control throughout
- Swappable SMS and printing provider layers (no vendor lock-in for either)
- Full RTL and multi-language support (Persian, English, German, Arabic)
- Android packaging via Capacitor with native device integration

---

## Repository Structure

```text
ArtemisPOSpublic/
├── index.html
├── docs/
│   └── screenshots/
├── README.md
├── README.fa.md
├── README.de.md
└── README.ar.md
```

This public repository is a demonstration/portfolio version focused on showing the application and its architecture.

## Public Repository Notice

This is a public demo version of a private, actively-used production system. Do not commit — and this repository does not contain — service-role keys, private API keys, passwords, real customer information, real financial records, or other production credentials. For a real deployment, connect Artemis to your own Supabase project.

---

## Author

**Ehsan Bagheryan** — Full-stack developer focused on practical business applications, Supabase/PostgreSQL, offline-first workflows, and AI integration.

- Email: `bagheryane@gmail.com`
- GitHub: [@medad21](https://github.com/medad21)

## License

See [LICENSE](LICENSE) for usage terms.
