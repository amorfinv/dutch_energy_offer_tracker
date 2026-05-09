# Dutch Energy Offer Tracker

A single-file, no-build web app for comparing Dutch electricity & gas offers side-by-side using your real consumption data. Tracks rate snapshots over time, calculates BTW-inclusive monthly costs, and lets you import usage data from Home Assistant.

Built as one self-contained web app. All data is stored locally in your browser — nothing leaves your machine.

<img width="1331" height="1042" alt="energy-offer-tracker-demo" src="https://github.com/user-attachments/assets/0f23f33a-1fca-4df1-8c34-c899794f81b3" />

## Features

- **Compare offers** with electricity (off-peak / on-peak), gas, network, delivery, and energiebelasting fields, all BTW-included.
- **Real usage** via two paths:
  - Manual entry of monthly off-peak / on-peak / gas values.
  - CSV import from Home Assistant's energy dashboars.
- **Snapshots & history** — capture rate changes over time, compare any two snapshots side-by-side, and edit a specific snapshot in place when you need to correct a value.
- **Charts** — monthly usage overview, per-offer timeline of monthly cost and electricity rates, and a dedicated gas-rate history chart.
- **Comparison view** — pick any subset of offers, see stacked bar charts of monthly cost broken down by electricity vs. gas.
- **CSV export/import** for both offers and usage data, so you can back up or move between machines.
- **Local-only storage** — everything is persisted in your browser. No server, no telemetry, no account.

## Getting started

Download `energy-offer-tracker.html` and open it in your browser.

> **Back up your data.** Everything lives in `localStorage`, so clearing site data or switching browsers will wipe it. Click **Export CSV** (offers) and **Export Usage** (usage data) every once in a while and keep the files somewhere safe.

## Usage walkthrough

1. **Add your current plan.** Use the `+` button in the header, fill in the rate fields (all incl. BTW), and pin it as your current plan.
2. **Add competing offers.** Same form, leave it unpinned. The comparison and sort helpers will show how each one stacks up against your current plan.
3. **Bring in real consumption** (optional but recommended) by clicking the 📊 icon:
   - **Manual:** add monthly entries one at a time.
   - **Home Assistant:** export your monthly data as CSV and use **Upload Usage Data**. New months are merged automatically; if a month already exists you'll be prompted per-month to keep the existing values or override them.
4. **Snapshot rate changes** with the ↻ button on any offer. Each snapshot is timestamped and labeled.
5. **Edit a snapshot** with ✎. If the offer has more than one snapshot, you'll be asked which one to edit; your changes replace it in place rather than appending.
6. **History** view (📊 button on any offer) shows the timeline of monthly cost + electricity rates, plus a separate gas-rate chart.

## Home Assistant CSV format

The app accepts Home Assistant's energy-dashboard CSV export directly. You'll need the entity IDs for off-peak electricity, on-peak electricity, and gas — set them once in the usage panel and they're remembered.

A simpler "month, offPeakKwh, onPeakKwh, gasM3, days" CSV is also accepted and is what the app produces when you click **Export Usage**.

## Calculation model

For each offer, the monthly cost is calculated as:

- **Energy cost** = usage × rate (rates entered include BTW and energy tax)
- **Fixed costs** = annual network + delivery costs ÷ 12
- **Vermindering** = annual energy-tax reduction ÷ 12 (subtracted)

When real usage is available it's used directly; otherwise the offer's annual estimates are split evenly across the year.

The 2026 energiebelasting reference table is included in the *Tax brackets* panel for reference only — it is not used in calculations because the entered rates already include tax.

## Roadmap

A large TODO is to add **hourly consumption data** so the tracker can do a proper **dynamic electricity analysis** — comparing fixed-rate offers against dynamic (EPEX/day-ahead) tariffs using your actual hour-by-hour usage profile rather than a flat off-peak / on-peak split. This means importing hourly Home Assistant exports, ingesting day-ahead price curves, and computing what each offer would have cost over the same period.

## Tech

- HTML, vanilla CSS, React 18 (UMD), Babel-standalone, Chart.js 4
- No bundler, no package manager, no backend
- Storage: `localStorage` keys `energy-offers-v8`, `energy-usage-v1`, `energy-manual-usage-v2`

## Disclaimer

This is a personal-use tracker, not financial advice. Double-check any switching decision against the supplier's actual contract terms — energy pricing in the Netherlands has plenty of fine print this app doesn't model (variable vs. fixed terms, dynamic pricing, sign-up bonuses, end-of-contract penalties, etc.).

## License

MIT
