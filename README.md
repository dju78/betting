# Aldanat Care FC Match Predictor

A single page prediction game for the Aldanat Care FC charity fixture, **Supported Living v Residential**, Wednesday 26 August, kick off 2pm, Harwich and Parkeston Football Ground, in aid of **Cardiac Risk in the Young**.

Live: https://aldanat-match-predictor.vercel.app

## What it does

Entrants call the full time score, optionally name the first goalscorer, and pledge a voluntary donation. After the whistle the organiser enters the result and the table settles itself.

| Outcome | Points |
| --- | --- |
| Exact full time score | 5 |
| Right result, wrong score | 2 |
| Goal difference bonus on top of a right result | 1 |
| First goalscorer | 3 |

Tie break: closest to the correct total goals, then earliest entry.

## Not a betting product

There is no stake, no prize pot and no payout. Entry is free, a pledge of zero is allowed, and pledges are voluntary donations payable whether the prediction is right or wrong. That design keeps the activity outside the Gambling Act 2005. Adding a cash prize or a compulsory entry fee would make it a lottery or prize competition and bring licensing conditions with it, so do not change the prize model without taking advice.

No payment is processed by this application. Pledges are collected at the desk by cash or card, or given direct on the CRY donation page.

## Running it

No build step, no dependencies, no framework. Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
```

Deploy by pointing any static host at the repository root. The live copy runs on Vercel with no build command and no output directory.

## Two pages

| Page | Who it is for |
| --- | --- |
| `/` (`index.html`) | Entrants. Enter a name, call the score, choose a pledge. |
| `/admin` (`admin.html`) | The organiser at the desk. Behind a desk code set at the top of the file. |

The desk console holds the sheet: add, edit and delete entries, record who has actually paid and by which route, watch pledged against received, close entries, publish the result, print the sheet, and export a CSV that pastes straight into the Pledge Log tab of the reconciliation workbook.

Change `DESK_CODE` in `admin.html` before matchday. It deters idle tampering by anyone who wanders past the tablet. It is not security, because the code sits in the page source, so do not treat it as protecting anything that matters.

## Storage, and the one thing to understand before matchday

The app writes to `window.storage` if the host provides it, and otherwise falls back to `localStorage` on the device. **A static deployment has no shared backend, so each device keeps its own separate copy of the sheet.**

The event is therefore run on a desk device model: one tablet or laptop at the collection desk holds the master record, entries are keyed in as pledges are taken, and the result is published from that device. The public link is still useful for reading the fixture, the rules and the scoring, and for entrants to compose a prediction before handing it in.

If a genuinely shared live table is needed for a future fixture, add a small API route backed by a key value store and replace the shim at the top of `index.html`. The rest of the application reads and writes through that one interface, so nothing else needs to change.

Other known limitations, all managed in the organiser pack:

- No login. Entries are on trust and keyed on the name typed, so two people with the same name overwrite each other.
- Simultaneous saves resolve last write wins.
- The screenshot taken when entries close, not the app, is the authoritative record if anything is disputed.

## Repository contents

```
index.html                                     the entrant page
admin.html                                     the organiser desk console
docs/Match-Predictor-Rules-and-Privacy-Notice.docx   participant facing, circulate with the link
docs/Match-Predictor-Organiser-Pack.docx            roles, run sheet, money controls, compliance, forms
docs/Match-Predictor-Pledge-Reconciliation.xlsx     pledge log, cash count, reconciliation, remittance
docs/Match-Predictor-Flyer-A4.pdf                   print ready flyer with QR code
```

The documents carry placeholders in square brackets for the legal entity, organiser contact and CRY charity number. Fill these before circulating.

## Data protection

No personal data is committed to this repository and none should be. Entries live only on the device that took them and are deleted within 30 days of the reconciliation, signed off at Appendix B of the organiser pack. The privacy notice must be circulated with the link, not after it.
