# Week 4 — Mediroza General Hospital Penetration Test

Cybersecurity coursework project (Networkwalks Academy — Cyber IT Diploma, Batch B082) — a full black-box penetration test against an authorized training target, chaining several real vulnerabilities from initial access through to critical data exposure.

## ⚠️ Disclaimer

This assessment was conducted only against a target explicitly authorized for security testing by Networkwalks, in a controlled educational environment. These techniques must never be applied to any system without explicit written permission from the owner.

## Target

- **Target:** `https://medirozahospital.com`
- **Engagement type:** Full black-box penetration test
- **Duration:** 3 days
- **Authorization:** Written authorisation granted by client via Networkwalks

## Objective

Three trainer-defined milestones:
1. Attack the website and locate 3 confidential patient PDF lab reports
2. Crack the encryption on all 3 retrieved files
3. Find the critical data exposure on the client server

## Summary of Findings

| # | Finding | Severity |
|---|---|---|
| 1 | Username enumeration on the login page | Medium |
| 2 | SQL injection — authentication bypass | Critical |
| 3 | Confidential patient PDFs accessible after bypass | Critical |
| 4 | Weak, crackable PDF passwords | High |
| 5 | Sensitive metadata left in a patient PDF | Medium |
| 6 | Publicly exposed, unauthenticated database backup | Critical |
| 7 | Confidential staff salary and shareholder data exposed | Critical |

**Overall Risk: CRITICAL**

## Attack Chain

```
Username enumeration ("admin" confirmed valid)
        ↓
SQL injection (admin'--) → full authentication bypass
        ↓
Access to patient portal → 3 confidential PDF reports downloaded
        ↓
PDF passwords cracked via dictionary attack
        ↓
Hidden metadata in one PDF (left by IT admin "j.malik") reveals /old/ path
        ↓
Exposed, unauthenticated database backup found at /old/
        ↓
Full staff salary + shareholder ownership data exposed
```

## Methodology

1. **Reconnaissance** — explored the site manually, tested the login form's behaviour
2. **Vulnerability identification** — probed authentication for weaknesses
3. **Exploitation** — bypassed login via SQL injection, retrieved and cracked the PDFs
4. **Metadata analysis** — used `exiftool` on decrypted files to look for hidden clues
5. **Follow-on discovery** — followed the metadata clue to an exposed backup file
6. **Documentation** — this report, with evidence for every step

## Tools Used

- Web browser (manual testing)
- Networkwalks Hash Calculator — PDF hash extraction
- Networkwalks Password Cracker — dictionary attack on PDF hashes
- `qpdf` — PDF decryption
- `exiftool` — metadata analysis
- Claude (AI assistant) — converting raw SQL dump data into readable tables

## Repo Structure

```
├── README.md
├── report/
│   └── Mediroza_My_Own_Report.docx      # full write-up with screenshots
├── data/
│   └── Mediroza_M3_Exposed_Data_Summary.xlsx   # full staff & shareholder data
└── screenshots/
    └── (evidence screenshots for each finding)
```

## A Note on Sensitive Data

The staff and shareholder data exposed in Finding 7 is real-format synthetic lab data (fictional names, national IDs, and salaries created for this training exercise). Full detail is kept in the accompanying spreadsheet rather than pasted directly into this README. PDF passwords recovered during Finding 4 are documented in the full report but intentionally left out of this public README.

## Key Takeaways

- A single unsanitised login field led to a complete chain from "no access" to "full internal data exposure" — small vulnerabilities compound fast
- Weak, common passwords (`123456`, `password`, `!@#$%^&`) provide essentially no real protection once a file is exposed at all
- Internal notes and metadata should never be left inside files that leave an organisation — this single leftover comment was the pivot point for the entire second half of the attack chain
- Backup files should never be stored inside a publicly reachable web directory, regardless of how "hidden" or "forgotten" the path seems

## Author

Salwa — [github.com/salwaibrahim00](https://github.com/salwaibrahim00)
