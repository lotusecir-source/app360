# Unveil Shadows · Revenue Assurance (Agentic AI)

By Lotus Cyber Security.

A web app for the EIKAM Revenue Assurance proposal. It replays **1 Jul – 15 Oct 2026** for a
Telangana-style distribution circle (6 sections around south-west Hyderabad, 60 DTs,
~4,500 services) and shows agents finding planted problems, explaining them with evidence, and
waiting for an officer to approve.

> All figures are sample data generated in the browser from a fixed seed (20261016). No real consumers,
> utility records, tariffs or loss figures are used.

## Screens
| Screen | What it shows |
|---|---|
| Command map | DTs coloured by 7-day unaccounted energy, category clusters, investigation pins |
| Live alerts | Time-slider replay of the alert stream, incl. data-quality noise |
| Investigations | Agent trace: hypotheses, approved checks, evidence IDs, recommendation, Approve / Reject / Need data |
| Energy balance | Daily energy in vs billed, hour × day residual heatmap per DT |
| Gruha Jyothi | Units histogram vs 200-unit limit, reader bunching, claim reconciliation, naive vs subsidy-aware "non-payers" |
| Meter health | Fault vs comms loss vs tamper, using work orders and diagnostics |
| Tickets + AI RAG | TF-IDF retrieval over past tickets with root causes and fixes |
| Outputs | Inspection orders, bill corrections, reconciliations, consumer alerts — only from approved cases |
| Scorecard | 14 planted scenarios (8 misconduct, 6 innocent look-alikes) vs agent proposals |
| Data + downloads | Download the dataset as CSV |

## Run locally
```bash
npm install
npm run dev        # http://localhost:3000
```

## Deploy to Vercel
**Option A – CLI (fastest)**
```bash
npm i -g vercel
vercel login
vercel --prod      # accept the detected Next.js settings
```

**Option B – GitHub import**
1. Push this folder to a new GitHub repository.
2. In Vercel: *Add New → Project → Import* the repository.
3. Framework preset: **Next.js** (auto-detected). No environment variables needed. Click **Deploy**.

The app is a static export (`output: 'export'` in `next.config.js`), so it also works on any static host.

## Project layout
```
app/              Next.js app router (layout, page, styles, icon)
components/App.tsx  All screens
components/charts.tsx  Lightweight SVG charts (no chart library)
lib/data.ts       Seeded synthetic data generator + planted scenarios
lib/cases.ts      Investigations: scripted agent traces computed from the data
lib/rag.ts        In-browser TF-IDF retrieval over past tickets
lib/csv.ts        CSV export
```

## Notes and assumptions
- Gruha Jyothi rule: zero bill when units ≤ 200 for enrolled domestic services; above the limit the full bill applies. Configurable in `lib/data.ts` — verify against current GOs.
- Tariffs and ₹/kWh values are illustrative, not TGERC-approved.
- Agent reasoning is scripted over computed evidence so results are reproducible offline. To phrase explanations with an LLM, add an API route that sends a case's evidence to your model and display the reply in the Investigations screen.
