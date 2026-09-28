# GovTrace

GovTrace is a working browser-based product demonstration for government supply-chain traceability and digital product passports. The first release focuses on the Food / PDS rice journey: batch records, quality checks, inventory, handoffs, quantity reconciliation, exceptions, document integrity, internal records, and a public passport view.

## Run locally

Requires Python 3. Start a local static server from this folder:

```powershell
python -m http.server 4173
```

Open <http://127.0.0.1:4173>.

## Demonstration behavior

- Sample records are synthetic. Changes are saved in this browser's local storage.
- Create a transfer from a quality-passed batch, then record a lower received quantity to see an exception created while the original dispatch amount remains intact.
- Document registration calculates a SHA-256 digest in the browser. The file itself is not uploaded or retained.
- Public passport URLs show approved sample fields; copy the link from the passport page to reopen it.
- Blockchain, hosted document storage, authentication, tenant isolation, external integrations, and production APIs are not connected. The interface labels the ledger as unconfigured.

## Project files

- `index.html` — application entry point
- `app.css` — responsive interface styling
- `app.js` — local demonstration data and workflows
- `docs/` — product requirements and Phase 0 decision record
