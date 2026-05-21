# KR3 Back-to-School Ops Dashboard

A celebratory internal dashboard for Ignite Reading staff. Tracks completed BTS work items and a fundraising-style thermometer showing hours saved per week against a 2,000 hrs/week goal for SY 26-27.

Live site: https://chris-geiser.github.io/kr3-bts-dashboard/

## What's here

```
kr3-bts-dashboard/
├── index.html        # The microsite (everything inline, vanilla JS)
├── README.md         # This file
└── sheet-spec.md     # Google Sheet column and tab structure
```

No build step, no dependencies. Open `index.html` in any browser and it works. In production it reads from a published Google Sheet (CSV) on every page load.

## How to update the data

The site pulls live from a Google Sheet. To add a new completed item:

1. Open the [KR3 BTS Tracking Sheet]([DATA NEEDED, paste link once sheet is created])
2. Add a row to the **Done Items** tab. Column definitions are in `sheet-spec.md`.
3. Update the **Sheet Last Updated** value on the **Config** tab.
4. Refresh the dashboard. No deploy needed.

### Writing a good Staff Summary

The Staff Summary is the text that appears on the back of each card. Two to three sentences. Plain language. Present tense. Focus on what changed and who it helps. No jargon, no acronyms unless universally understood.

Good: "Tutor schedules now generate automatically based on student needs and tutor availability. This eliminates hours of manual scheduling each week for School Champions."

Less good: "We shipped an automated scheduling algorithm leveraging the new constraint solver to optimize tutor-student matching at scale."

## How to set up from scratch

If you're recreating this from zero:

1. **Build the Google Sheet.** Two tabs: `Done Items` and `Config`. Use the exact column headers in `sheet-spec.md`.
2. **Publish each tab as CSV.** File > Share > Publish to web. Pick the tab, choose CSV format, click Publish, copy the URL. Do this for both tabs.
3. **Wire up the URLs.** Open `index.html`. Near the top of the `<script>` block find the CONFIGURATION section. Paste the two CSV URLs into `DONE_ITEMS_CSV_URL` and `CONFIG_CSV_URL`.
4. **Commit and push to `main`.** GitHub Pages will redeploy in a minute or two.

### Repo + Pages setup

1. Create a new public GitHub repo (suggested name `kr3-bts-dashboard`).
2. Push the three files in this directory to `main`.
3. In repo Settings > Pages, set source to `Deploy from a branch`, branch `main`, folder `/ (root)`.
4. Pages will publish to https://chris-geiser.github.io/kr3-bts-dashboard/.

## Local preview

```bash
cd "OKR Dashboard"
python3 -m http.server 8080
```

Then open http://localhost:8080. The site falls back to `SAMPLE_DATA` (hardcoded in the script) until you paste real CSV URLs, so you can preview everything before the sheet is live.

## Brand notes

Site follows the Ignite Reading brand style guide.

- **Font:** Roboto from Google Fonts (Light, Regular, Medium, Bold, Black, plus italics).
- **Colors:** Pink #ED017F, Yellow #FFC854, Blue #39C2D7, Purple #27004B / #514F9C. All colors live as CSS custom properties at the top of the inline `<style>` block, easy to tweak.
- **Accent treatment:** Treatment 2 (Dashed + Hand-Drawn). Dashed-circle SVG ring around the hours-saved total and scattered confetti SVG accents.
- **Voice register:** Internal comms, the warmest and most celebratory register of the brand. Conversational, joyful, no jargon.
- **Accessibility:** WCAG AA contrast on card fronts (white text on pink/blue/purple, dark text on yellow). Cards are keyboard-focusable and flip on Enter or Space. Reduced-motion preference is respected.
