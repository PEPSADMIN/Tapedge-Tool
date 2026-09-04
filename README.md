# Tape Edge POC

Peps Tape Edge scanning POC — a mobile-first tool for logging tape-edge machine
production by scanning each mattress's MRP sticker QR code / SI No.

## Architecture

Single static HTML file, **no backend**:
[Peps_TapeEdge_Handover_files/peps_tape_edge_poc_v2_6.html](Peps_TapeEdge_Handover_files/peps_tape_edge_poc_v2_6.html)

- No server, no database, no localStorage. All state (reference data, scan log,
  targets) lives in in-memory JS variables and is lost on page refresh.
- Only external dependencies are two CDN scripts: [jsQR](https://github.com/cozmo/jsQR)
  (camera QR scanning) and [SheetJS/xlsx](https://sheetjs.com/) (Excel import/export).
- Ships with a built-in sample reference dataset (~16k SI No records) for demo use
  without uploading a file.

## Running it

Open the HTML file directly in a browser — no build step, no install. For camera
scanning to work, serve it over HTTPS or `localhost` (browsers block camera access
on plain `file://` and non-secure origins); manual SI No entry works everywhere.

## Shift model

A business day is two shifts:
- **Day Shift**: 08:00–20:00
- **Night Shift**: 20:00–08:00 (crosses midnight)

The Board tab shows Current Shift / Previous Shift / Total counts per machine and
brand, live, based on the device's local clock.

## Daily workflow

1. Upload the day's Excel export (SI No / Item Code / Description / Doc No columns)
   on the Scan tab, or use the built-in sample data for a demo.
2. Assign the device to a machine (TE-01…TE-11).
3. Scan or manually enter each SI No; confirm the match, then Save.
4. Duplicate and not-found scans are flagged for supervisor review instead of
   being silently accepted.
5. Export Excel (Brand-wise / Machine-wise / Variety-wise sheets) for any date
   range, or use the one-tap "Previous Day" export.

## Known limitation

**A page refresh wipes all data.** There's no server in this POC, so re-upload
the reference file and expect the scan log to reset every session (deliberate
per the current architecture, called out on the Guide tab in-app).
