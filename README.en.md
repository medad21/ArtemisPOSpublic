# Artemis — Retail POS & Inventory Management System

🌐 **[🇮🇷 فارسی](README.md) | [🇬🇧 English](README.en.md) | [🇩🇪 Deutsch](README.de.md) | [🇸🇦 العربية](README.ar.md)**

**Artemis** is a complete, self-hosted point-of-sale and shop management system, built from the ground up over two years of real, daily use in an actual retail store. It started as a way to avoid paying for — and dealing with the installation headaches of — expensive commercial POS software, and grew feature by feature into a full business management platform: sales, inventory, customers, staff, accounting, and printing, all in one place.

It runs as a single web app (usable from any browser, on any device) **and** as a native Android app, both talking to your own [Supabase](https://supabase.com) project — meaning **you own your data completely**. There's no monthly SaaS fee, no vendor lock-in, and no dependency on our servers to keep running.

**Repository:** https://github.com/medad21/ArtemisPOS
**Contact:** bagheryane@gmail.com

## 🧩 Built to be customized

Artemis isn't locked to one type of shop. The store this was built for also does print/copy-shop work on the side, so a **Services** module was added: instead of a plain item sale, each service (a color print, a binding job, a photocopy order...) can be broken down into its own cost components (paper, ink/toner, labor, etc.), so real profit margin per job is tracked separately from regular retail sales — with its own reporting, breakdown by service type and by day. That's the general idea behind the whole project: every module was added because a real, specific need came up, and the codebase is straightforward enough (plain HTML/JS, no build step) that anyone can open it and adapt a section to their own kind of business.

## ✨ Features

**Sales & Checkout**
- Fast sale entry with barcode scanning (camera-based, no extra hardware needed)
- Discounts, multiple payment methods (cash, card, card-transfer, on-credit/consignment)
- Colleague/wholesale sales for business-to-business transactions
- A separate **Services** module for job-based work (printing, repairs, custom orders...) with itemized cost-component breakdown and its own profit reporting

**Inventory**
- Full stock tracking with low-stock alerts
- Item categories, Kardex (in/out history) per item
- Generate and print scannable barcode labels for items that don't have one

**Returns & Exchanges**
- Partial or full returns with automatic stock and profit adjustment
- Item exchange (customer swaps for a different item), settling only the price difference — fully tracked, not a manual workaround

**Customers**
- Customer profiles, purchase history, debt tracking
- Loyalty/stars program with VIP and active-buyer tiers
- Bulk SMS to debtors, all customers, or any custom number — multi-provider (Kavenegar, MeliPayamak, SMS.ir, or any custom REST API via webhook)

**Multi-user**
- Manager and staff roles with separate logins
- Per-staff action tracking for accountability

**Accounting**
- Balance sheet, multiple cash/bank accounts
- Expense tracking, opening balance, automatic backups

**Printing**
- Regular printers (A4/A5/A6) and thermal receipt printers (58mm/80mm)
- Direct Bluetooth printing from the Android app (no OS print driver needed)
- Fully customizable invoice content (what shows on the receipt) and amount-in-words

**Everything else**
- 4 languages: Persian, English, German, Arabic (with full RTL support)
- Multi-currency
- Dark/light mode
- Purchase and check-payment reminders

## 📸 Screenshots

*(Add your own screenshots to `docs/screenshots/` with the filenames below — they'll show up here automatically.)*

| | |
|---|---|
| **Sales & Checkout**<br>![Sales](docs/screenshots/sales.png) | **Inventory Management**<br>![Inventory](docs/screenshots/inventory.png) |
| **Returns & Exchange**<br>![Returns](docs/screenshots/returns.png) | **Customer Club & Loyalty**<br>![Customer Club](docs/screenshots/customer-club.png) |
| **Reports & Balance Sheet**<br>![Reports](docs/screenshots/reports.png) | **Settings**<br>![Settings](docs/screenshots/settings.png) |

## 🚀 Getting Started

Artemis needs a [Supabase](https://supabase.com) project (their free tier is enough to get started) to store your data.

1. Create a free Supabase project.
2. Open the app (`index.html`) in a browser — you'll see a first-run setup screen.
3. In your Supabase project's SQL editor, run the table-creation script (the app can generate this for you once connected — see `Settings → Download SQL script`, or use the version in this repo).
4. Enter your Supabase Project URL and anon/public key into the setup screen.

That's it — your data lives entirely in your own Supabase project.

## 📱 Android App

Artemis is also packaged as a native Android app using [Capacitor](https://capacitorjs.com), with native features like camera-based barcode scanning, biometric login, and local notifications. See the `android/` folder and the GitHub Actions workflow in `.github/workflows/` for the build pipeline.

## 🛠 Tech Stack

- Single-file HTML/CSS/JS front end — no build step needed to run it in a browser
- [Supabase](https://supabase.com) (Postgres + Auth + Realtime + Edge Functions) as the backend
- [Capacitor](https://capacitorjs.com) for the Android app wrapper

## 📄 License

See [LICENSE](LICENSE) for usage terms.

## 📬 Contact

Questions, customization requests, or licensing inquiries: **bagheryane@gmail.com**

---

*Built by Ehsan Bagheryan.*

