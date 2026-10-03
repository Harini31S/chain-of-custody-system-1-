# Digital Forensics – Chain of Custody Management System
College DBMS mini-project. Flask + SQLite + vanilla HTML/CSS/JS. Runs fully offline.

## Setup
```
pip install -r requirements.txt
python app.py
```
Open http://127.0.0.1:5000 (a pre-populated `forensics.db` is included; delete it to re-seed automatically).

## Demo logins (password for all seeded users: `Password@123`)
| Role | Emails |
|---|---|
| Admin (10) | admin1@forensic.local … admin10@forensic.local |
| Investigator (15) | investigator1 … investigator15 @forensic.local |
| Forensic Analyst (15) | analyst1 … analyst15 @forensic.local |
| Evidence Custodian (10) | custodian1 … custodian10 @forensic.local |

## Data
50 users, 50 cases, 150 evidence items, ~500 custody logs.
Every 20th evidence item (e.g. EVD-0020) is deliberately tampered so **Verify Integrity** shows *Integrity Failed*; the rest show *Integrity Maintained*.

### Real-world calibration (NCRB)
The case dataset is calibrated using **real published statistics** from the National Crime Records
Bureau (NCRB), Government of India — "Crime in India" annual reports:
- **Case-type split** — 15 Email Phishing / 9 Mobile / 9 USB / 9 Laptop / 8 Hard Disk (instead of a flat
  10-each split), reflecting NCRB's 2021 finding that Fraud was the dominant cyber-crime motive (60.8%);
  "Email Phishing" is our closest match to online/financial fraud.
- **Year spread** — cases are dated across 2021 / 2022 / 2023 in a 13 / 16 / 21 ratio, matching the real
  national case-volume growth (52,974 → 65,893 → 86,420 cases, source: NCRB).
- **What's still simulated** — the evidence *device-type* breakdown (Mobile vs USB vs Hard Disk vs
  Laptop evidence items) is NOT published anywhere by NCRB or any public source (it's internal
  forensic-lab custody data), so that distribution remains a reasonable simulated assumption, stated
  explicitly here for transparency.

## Structure
```
chain_of_custody/
├── app.py          # Flask API, SQLite schema, seeding, reports (CSV/PDF)
├── forensics.db    # Prepopulated database
├── requirements.txt
├── README.md
└── static/  index.html · style.css · app.js
```
## Notes
Integrity: MD5/SHA-256 are stored at collection time; verification recomputes them from the stored evidence content and compares.
Passwords are hashed (werkzeug). Demo only – change `secret_key` for any real use.
