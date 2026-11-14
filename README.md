# Invoice Report Generator V2

A browser-based converter for SAP dispatch Excel exports. The source workbook is processed locally in the browser; it is not sent to an application server.

## Open and use

1. Extract the ZIP file.
2. Open `index.html` in a current version of Google Chrome or Microsoft Edge.
3. Keep an internet connection available when opening the page. The Excel formatting/processing library is loaded from the pinned jsDelivr CDN URL in the HTML file.
4. Select the SAP raw `.xlsx`, `.xls`, or `.xlsm` export.
5. Select **Generate Excel report** and review the preview and any validation warnings.
6. Select **Download formatted Excel (.xlsx)**.

The download action serializes the workbook, reopens it in the browser, and checks cached formula results, grand totals, number formats, hidden calculation columns, and merged invoice cells before starting the download. Excel is also configured to recalculate formulas when the file is opened.

## Applied rules

- State/address: uppercase; trailing `(New)` suffix removed, matching the supplied final-format example.
- Party: trailing `LIMIT` and `LIMITED` are treated as equivalent; trailing `LIMIT` is displayed as `LIMITED`.
- Vehicle: recognized Indian registration numbers are normalized with a hyphen before the final four digits.
- Material: spacing and bracket variations are normalized; different rates remain separate lines.
- GST: inferred from `Net Sales` against quantity × basic rate when available. The supplied sample resolves to 18% on all 70 lines.
- FOR: `Freight Inclusive` is the freight rate; freight amount is freight rate × total invoice quantity; remarks are `FOR/transporter`.
- Non-FOR: `SELF BY PARTY` becomes `EX BY PARTY`; another transporter becomes `EX BY transporter name`.
- Invoice order: source order is preserved. The supplied reference workbook has a different custom order that cannot be inferred reliably from the raw SAP fields.
- Conflicting invoice party/state/vehicle, mixed FOR/non-FOR rows, multiple transporters, or conflicting FOR freight rates are rejected rather than silently guessed.

## Compatibility

- Designed for current Chrome and Edge desktop browsers.
- Requires internet access to load `xlsx-js-style@1.2.0` from jsDelivr.
- Files up to 50 MB are accepted.
- Output is `.xlsx`; the raw workbook is not modified.
