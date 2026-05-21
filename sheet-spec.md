# Google Sheet Structure

The dashboard reads from a Google Sheet with two tabs. Header rows must match these names exactly (the JavaScript looks up fields by column name).

## Tab 1: Done Items

One row per completed BTS work item. Sorted by `Date Completed`, newest first, in the UI.

| Column | Description | Example |
|---|---|---|
| Summary | Title of the completed work (from JPD Summary field). Appears as the card front title. | Automated tutor schedule builder |
| Staff Summary | Two to three sentence plain-language description of what shipped and why it matters. Written for all staff, no jargon. Appears on the card back. | Tutor schedules now generate automatically based on student needs and tutor availability. This eliminates hours of manual scheduling each week. |
| H/W/P | Hours per week per person saved. Numeric. | 1.5 |
| # Impacted | Count of employees impacted. Numeric. | 200 |
| H/W Saved | Total hours per week saved. Equals H/W/P × # Impacted. Numeric. | 300 |
| Requested By | Who requested this work. Free text. | Ops Team, School Champions |
| Team | Team or teams that completed the work. | Product & Engineering |
| Date Completed | When the work was marked Done in JPD. YYYY-MM-DD format. | 2026-05-15 |

### Header row to paste

```
Summary	Staff Summary	H/W/P	# Impacted	H/W Saved	Requested By	Team	Date Completed
```

## Tab 2: Config

Two columns of key/value pairs plus an optional description column. The site reads `Key` and `Value` only.

| Key | Value | Description |
|---|---|---|
| Goal | 2000 | Target hours per week to save |
| Banner Title | KR3 Back-to-School Ops | Main headline on the page |
| Banner Subtitle | Resolve top BTS blockers, save 2,000 hrs/week for staff in SY 26-27. | Subtitle text under the headline |
| Sheet Last Updated | 2026-05-21 | Date the sheet was last refreshed from JPD |

### Header row to paste

```
Key	Value	Description
```

## Publishing the sheet

For each tab, separately:

1. Open the Google Sheet.
2. File > Share > Publish to web.
3. In the dropdown, pick the specific tab (Done Items, then Config).
4. Set format to **Comma-separated values (.csv)**.
5. Click Publish, confirm, and copy the URL Google gives you.
6. Paste the URL into `index.html` at the top of the `<script>` block (`DONE_ITEMS_CSV_URL` and `CONFIG_CSV_URL`).

Anyone with the link can read the published CSV but not the underlying sheet, so this is safe for an internal-but-not-secret tracker. If the sheet contains anything sensitive, do not use Publish to web.

## Updating cadence

Refresh the Sheet whenever a JPD item moves to Done. The site picks up changes on the next page load. There is no caching layer.
