# Donation Item Inventory  (FutureReady Hack - Project #79, Safety & Community)

A beginner-friendly, zero-dependency Python 3.10+ terminal app to track donated items,
quantities and distribution records, with validation, JSON persistence, search,
summary analytics and exports.

## Quick start
```bash
python -m venv .venv && source .venv/bin/activate    # Windows: .venv\Scripts\activate
python main.py              # first launch creates data/donations.json
```
Then choose **9 - Load sample data** (32 donations + 39 distributions) to try everything.
Optional: `pip install matplotlib` for a PNG chart in exports.

## Features (mapped to the brief)
| Brief requirement | Where |
|---|---|
| Menu-driven terminal UI, ANSI colours | `inventory_app/ui.py` |
| Defensive input validation (re-prompts, `/cancel`) | `inventory_app/validation.py` |
| Core logic: stock = received - distributed, no overselling, expired items blocked | `inventory_app/logic.py` |
| JSON persistence, atomic writes, corrupt/missing-file recovery, audit log | `inventory_app/storage.py` |
| Keyword / category / status search | menu 4, `Inventory.search` |
| Executive summary: totals, averages, category table, monthly trend, top donors/recipients | menu 5, `inventory_app/reports.py` |
| Stretch: Markdown / text audit / CSV (+PNG) export | menu 7 |
| Stretch: warnings - expired, expiring <=14 days, low stock, unusually large donations | menu 6 (also shown at startup) |
| Stretch: bulk CSV import with row-by-row error log | menu 8 |

## Data model
- **Item** (one donation batch): id, name, category, quantity, unit, donor, date_received, expiry_date, min_stock
- **Distribution**: id, item_id, quantity, recipient, date
- Dates accept `YYYY-MM-DD`, `DD/MM/YYYY` or `DD-MM-YYYY`; stored as ISO.

## Project layout
```
main.py                     entry point (--data PATH to use another file)
inventory_app/              models, validation, logic, storage, importer, reports, ui
data/                       sample_donations.{csv,json}, sample_import_with_errors.csv
scripts/generate_sample_data.py   regenerate samples relative to any date
tests/test_app.py           python -m unittest discover -s tests
reports/                    exports and import error logs land here
```

## Suggested 3-minute demo
1. Launch -> banner + startup warnings. 2. `9` load sample. 3. `1` add a donation, deliberately type
`abc`, `-4`, `31/02/2026` to show validation. 4. `2` distribute more than stock to show the guard.
5. `5` summary, `6` warnings. 6. `8` import `data/sample_import_with_errors.csv` -> 2 added, 7 rejected with reasons.
7. `7` export; exit with `0`, relaunch, show data persisted.

## Ideas to extend
Tkinter/Textual front-end, edit/delete records, per-recipient reports, SQLite backend.
