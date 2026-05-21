# OKR Dashboard Microsite: Implementation Spec

## What This Is

A complete brief for building a celebratory internal dashboard microsite for Ignite Reading staff. The site tracks progress on KR3 (Back-to-School Ops) by showing completed BTS work items and a fundraising-style thermometer tracking hours saved per week toward a 2,000 hr/week goal.

The site is a single `index.html` file hosted on GitHub Pages. It reads data from a published Google Sheet (CSV). No build tools, no frameworks, no server-side code.

## Step 1: Create the GitHub Repo

Create a new public repo on GitHub:

- **Org/Owner**: Ask Chris which GitHub org or personal account to use
- **Repo name**: `okr-dashboard` (or ask Chris for preference)
- **Description**: "KR3 Back-to-School Ops Dashboard — tracking hours saved for SY 26-27"
- **Initialize with**: README.md
- **Branch**: `main`
- **GitHub Pages**: Enable from Settings → Pages → Source: Deploy from branch → `main` / root

The repo should contain at minimum:
```
okr-dashboard/
├── index.html          # The microsite
├── README.md           # Setup instructions (sheet URL config, how to update)
└── sheet-spec.md       # Google Sheet structure documentation
```

## Step 2: Google Sheet Structure

Create a markdown file (`sheet-spec.md`) documenting the sheet structure. Chris will create the actual Google Sheet manually and populate it from Jira Product Discovery.

### Tab 1: "Done Items"

| Column | Description | Example |
|--------|-------------|---------|
| Summary | Title of the completed work (from JPD Summary field) | "Automated tutor schedule builder" |
| Staff Summary | 2-3 sentence plain-language description of what shipped and why it matters. Written for all staff, no jargon. | "Tutor schedules now generate automatically based on student needs and tutor availability. This eliminates hours of manual scheduling each week." |
| H/W/P | Hours per week per person saved (numeric) | 1.5 |
| # Impacted | Count of employees impacted (numeric) | 200 |
| H/W Saved | Total hours per week saved. Calculation: H/W/P × # Impacted (numeric) | 300 |
| Requested By | Who requested this work | "Ops Team, School Champions" |
| Team | Team(s) that completed the work | "Product & Engineering" |
| Date Completed | When the work was marked Done (YYYY-MM-DD) | 2026-05-15 |

### Tab 2: "Config"

A simple two-column key/value layout:

| Key | Value | Description |
|-----|-------|-------------|
| Goal | 2000 | Target hours per week to save |
| Banner Title | KR3 Back-to-School Ops | Main headline |
| Banner Subtitle | Resolve top BTS blockers — save 2,000 hrs/week for staff in SY 26-27. | Subtitle text |
| Sheet Last Updated | 2026-05-21 | Date the sheet was last refreshed from JPD |

### Publishing the Sheet

Instructions for Chris to include in the README:
1. Open the Google Sheet
2. File → Share → Publish to web
3. Select "Done Items" tab → CSV format → Publish. Copy the URL.
4. Select "Config" tab → CSV format → Publish. Copy the URL.
5. Paste both URLs into the `index.html` file where indicated (clearly marked `SHEET_URL` constants at the top of the script).

## Step 3: Build the HTML Microsite

### Technical Requirements

- Single `index.html` file, everything inline (CSS and JS, no external files except Google Fonts)
- Load Roboto from Google Fonts (all weights: Light, Regular, Medium, Bold, Black, and their italic variants)
- No JavaScript frameworks. Vanilla JS only.
- Fetch published Google Sheet CSVs, parse them, render the UI
- Responsive: works on desktop (primary) and mobile
- CSS custom properties for all brand colors (easy to tweak)
- Accessible: proper contrast ratios, semantic HTML, keyboard-navigable flip cards

### Brand Design System

**Font**: Roboto (Google Fonts)
- H1: Roboto Black
- H2: Roboto Bold
- H3: Roboto Medium
- Body: Roboto Regular

**Color Palette** (use CSS custom properties):
```css
:root {
  /* Primary brand colors */
  --pink-base: #ED017F;
  --pink-light: #F18DC1;
  --pink-lighter: #F3CBE0;
  --pink-bg: #FDF5F5;

  --yellow-base: #FFC854;
  --yellow-light: #FAD078;
  --yellow-lighter: #FCE7C8;
  --yellow-bg: #FFFAEE;

  --blue-base: #39C2D7;
  --blue-light: #72CDDF;
  --blue-lighter: #C7E9F0;
  --blue-bg: #F2F9FC;

  --purple-base: #27004B;
  --purple-mid: #514F9C;
  --purple-light: #9A98CB;
  --purple-bg: #F7F4FA;

  --grey-base: #3D3C3C;
  --grey-mid: #999999;
  --grey-light: #EAEAEA;
  --white: #FFFFFF;
}
```

**Accessibility note**: Never use pink text on purple backgrounds. Never use blue text on white backgrounds (per brand guide).

**Visual personality**: Joyful, Rigorous, Compassionate, Bold, Visionary. This is an internal comms piece, so use the warmest, most celebratory register of the brand. Emojis are welcome. Think "dazzling burst of positivity" with "colorful sparkles."

**Circle treatments**: The brand uses two styles. For this site, use Treatment 2 (Dashed + Hand-Drawn), which is warm, approachable, and used for internal comms. Implement as SVG decorative elements (dashed circles, hand-drawn style rings) placed as accents around the thermometer or as background flourishes.

### Layout

```
┌─────────────────────────────────────────────────────┐
│                   TOP BANNER                         │
│   KR3 Back-to-School Ops                            │
│   Resolve top BTS blockers — save 2,000 hrs/week... │
└─────────────────────────────────────────────────────┘
┌────────────┬────────────────────────────────────────┐
│            │                                        │
│ THERMOMETER│         DONE & RELEASED                │
│   (25%)    │                                        │
│            │   ┌──────┐  ┌──────┐  ┌──────┐        │
│  Current:  │   │Card 1│  │Card 2│  │Card 3│        │
│  850 hrs   │   │(pink)│  │(blue)│  │(yel) │        │
│            │   └──────┘  └──────┘  └──────┘        │
│  ████░░░░  │                                        │
│  ████░░░░  │   ┌──────┐  ┌──────┐  ┌──────┐        │
│  ████░░░░  │   │Card 4│  │Card 5│  │Card 6│        │
│            │   │(purp)│  │(pink)│  │(blue)│        │
│  Goal:     │   └──────┘  └──────┘  └──────┘        │
│  2,000 hrs │                                        │
│            │                                        │
└────────────┴────────────────────────────────────────┘
```

### Top Banner

- Full width
- Background: `--purple-base` (#27004B)
- Text: white, Roboto Black for headline, Roboto Regular for subtitle
- Headline and subtitle pulled from Config tab (with hardcoded fallbacks)
- Optional: subtle SVG sparkle/dot accents in pink and yellow (small, decorative, not distracting)
- Padding: generous (at least 2rem vertical)

### Left Column: Thermometer (25% width)

A vertical fundraising-style thermometer showing progress toward the hours-saved goal.

**Design**:
- Vertical bar with rounded ends (pill shape)
- Background track: `--grey-light` (#EAEAEA)
- Fill: gradient from `--pink-base` at bottom to `--yellow-base` at top as it fills
- The fill height is proportional to (total H/W Saved) / (Goal from Config)
- If total exceeds goal, fill is 100% and show a "Goal exceeded!" celebration message
- Animated fill on page load (CSS transition, ~1.5s ease-out)

**Labels**:
- Above the thermometer: "Hours Saved Per Week" as a header
- Current total displayed prominently (large number, bold) with "of 2,000 hrs/week" below it
- Percentage shown (e.g., "42% of goal")
- At the very bottom of the thermometer: "0" label
- At the top: goal number label

**Decoration**:
- Hand-drawn dashed circle SVG accent around or near the current total number
- Celebratory emoji when past 50% (🔥), 75% (🎉), 100% (🏆)

**Separation**: A subtle vertical rule or 1-2rem padding gap between the thermometer column and the main content area.

**Responsive (mobile)**: Thermometer moves above the cards, displays as a horizontal bar instead.

### Main Area: Done & Released Cards (75% width)

**Section header**: "Done & Released 🎉" in Roboto Bold, dark text

**Card grid**: CSS Grid, 3 columns on desktop, 2 on tablet, 1 on mobile. Gap of ~1.5rem between cards.

**Cards are sorted by Date Completed, newest first.**

#### Flip Card: Front

- **Background**: Rotate through brand colors in this order: Pink (#ED017F), Blue (#39C2D7), Yellow (#FFC854), Purple (#514F9C). The first card is pink, second is blue, third is yellow, fourth is purple, fifth wraps back to pink, etc.
- **Text color**: White for pink, blue, and purple backgrounds. Dark (`--grey-base` #3D3C3C) for yellow backgrounds (contrast).
- **Title**: The Summary field, displayed in Roboto Bold, large (1.25-1.5rem). Should be the visual focus.
- **Bottom stats bar**: Three metrics displayed as compact chips or a horizontal row at the bottom of the card:
  - ⏱️ H/W/P: `{value}` hrs/person
  - 👥 # Impacted: `{value}` people
  - 💪 H/W Saved: `{value}` hrs/week
- **Flip affordance**: A small "tap to see details →" or a subtle flip icon in the corner. Should be discoverable but not dominate.
- **Card aspect**: Roughly 3:4 or square. Consistent height across the row (use min-height).
- **Border radius**: 12-16px for a friendly feel.
- **Shadow**: Subtle box-shadow for depth.

#### Flip Card: Back

- **Background**: White or very light tint of the front color (use the `-bg` variant, e.g., `--pink-bg` #FDF5F5 for a pink card)
- **Text**: `--grey-base` (#3D3C3C), Roboto Regular, comfortable reading size
- **Content**:
  - The Staff Summary text (2-3 sentences describing the work in plain language)
  - If the summary is long, the content area should scroll (overflow-y: auto, max-height set)
- **Thank you section** (at the bottom, always visible):
  - A warm line like "Requested by {Requested By}. Completed by {Team}. Thank you! 🙏"
  - Slightly smaller text, medium weight
  - This should be pinned to the bottom of the card back, not pushed off by long summaries
- **Flip affordance**: "← flip back" or a return icon

#### Flip Animation

- CSS 3D transform (`perspective`, `rotateY(180deg)`)
- Triggered on click (not hover, since this should work on touch devices too)
- Smooth transition (~0.6s)
- Both sides use `backface-visibility: hidden`
- Keyboard accessible: cards should be focusable (`tabindex="0"`) and flip on Enter/Space

### Celebratory Design Touches

- **Confetti-style SVG accents**: Small dots, sparkles, or stars in brand colors scattered lightly in the background of the main area. Keep it subtle (opacity ~0.1-0.15) so it doesn't compete with content.
- **Emojis**: Use in section headers and stat labels as noted above. Keep them functional, not excessive.
- **Animated counter**: When the page loads, the total hours number in the thermometer should count up from 0 to the actual total (a simple JS counter animation over ~2 seconds).
- **Hand-drawn SVG elements**: A few dashed-circle or squiggly-line decorations (2-3 max) placed as accents. These mirror the brand's Treatment 2 style.

### Empty State

If the sheet has no data rows (or the fetch fails):
- Thermometer shows 0
- Main area shows a friendly message: "Big things are coming! We're working on BTS improvements that will save the team thousands of hours. Check back soon. 🚀"
- If fetch fails specifically, show: "Couldn't load data. Check that the Google Sheet is published and the URLs are correct."

### Data Integration (JavaScript)

**Constants at the top of the `<script>` block** (clearly marked for easy editing):
```javascript
// ========================================
// CONFIGURATION: Update these URLs after publishing your Google Sheet
// ========================================
const DONE_ITEMS_CSV_URL = 'YOUR_PUBLISHED_CSV_URL_FOR_DONE_ITEMS_TAB';
const CONFIG_CSV_URL = 'YOUR_PUBLISHED_CSV_URL_FOR_CONFIG_TAB';
```

**CSV Parsing**:
- Write a simple CSV parser (no library needed). Handle quoted fields with commas.
- Parse "Done Items" CSV into an array of objects keyed by column header.
- Parse "Config" CSV into a key/value object.
- Convert numeric fields (H/W/P, # Impacted, H/W Saved, Goal) to numbers.

**Rendering**:
1. Fetch both CSVs on page load (use `fetch` with `Promise.all`)
2. Parse Config first, update banner text and goal
3. Parse Done Items, sort by Date Completed descending (newest first)
4. Sum all H/W Saved values for the thermometer
5. Render the thermometer with animated fill
6. Render flip cards in the grid
7. Start the counter animation for the total hours number

**Error handling**: If either fetch fails, show the error empty state. If one succeeds and the other fails, show what you can with a subtle error banner.

## Step 4: Create the README

The README should cover:

1. **What this is**: One-paragraph description of the dashboard
2. **Live site**: Link to the GitHub Pages URL (https://{org-or-user}.github.io/okr-dashboard/)
3. **How to update the data**:
   - Open the Google Sheet (include link or placeholder)
   - Add/edit rows in the "Done Items" tab when new BTS items are marked Done in JPD
   - Update the "Sheet Last Updated" date in the Config tab
   - The site refreshes automatically on each page load (no deploy needed)
4. **How to set up from scratch** (for future reference):
   - Create a Google Sheet following `sheet-spec.md`
   - Publish both tabs as CSV
   - Update the two URL constants in `index.html`
   - Push to GitHub, enable Pages
5. **Brand guidelines**: Note that the site follows Ignite Reading's brand style guide (Roboto, brand colors, Treatment 2 circle accents)

## Voice & Tone Guidance

This site is an internal communication for all Ignite Reading staff. Follow these rules:

**Tone**: Warm, approachable, professional. Conversational and joyful (this is the "internal comms" register from the brand voice guidelines, the warmest and most celebratory). Think of the CEO's "Yay! You're here!" energy from the Staff Culture Guide.

**Writing style**:
- Concise and clear. Say it in fewer words.
- No em dashes. Use commas, periods, or parentheses instead.
- No jargon. Write for a high school graduate. No "epic," "sprint," "deployment," "shaping," etc.
- Present tense.
- Specific numbers, not vague claims.
- Use "[DATA NEEDED]" as a placeholder rather than making anything up.

**Card descriptions (Staff Summary)**: These are written by Chris in the Google Sheet, not generated by the site. The site just displays them. But the README should include guidance that summaries should be 2-3 sentences, plain language, focused on what changed and who it helps.

**Thank-you language on card backs**: Warm and genuine. "Requested by {names}. Completed by {team}. Thank you! 🙏" Keep it simple.

## Sample Data for Testing

Include this sample data hardcoded as a fallback (or in a comment) so the site can be tested before the real Google Sheet is connected:

```javascript
const SAMPLE_DATA = {
  config: {
    "Goal": 2000,
    "Banner Title": "KR3 Back-to-School Ops",
    "Banner Subtitle": "Resolve top BTS blockers — save 2,000 hrs/week for staff in SY 26–27.",
    "Sheet Last Updated": "2026-05-21"
  },
  items: [
    {
      "Summary": "Automated tutor schedule builder",
      "Staff Summary": "Tutor schedules now generate automatically based on student needs and tutor availability. This eliminates hours of manual scheduling each week for School Champions.",
      "H/W/P": 1.5,
      "# Impacted": 200,
      "H/W Saved": 300,
      "Requested By": "Ops Team, School Champions",
      "Team": "Product & Engineering",
      "Date Completed": "2026-05-15"
    },
    {
      "Summary": "One-click session rescheduling",
      "Staff Summary": "When a session needs to move, staff can now reschedule with a single click instead of navigating multiple screens. Saves time and reduces scheduling errors.",
      "H/W/P": 0.75,
      "# Impacted": 350,
      "H/W Saved": 262.5,
      "Requested By": "Tutor Educators, Program Managers",
      "Team": "Product & Engineering",
      "Date Completed": "2026-05-10"
    },
    {
      "Summary": "Bulk student roster import",
      "Staff Summary": "School Champions can now upload an entire student roster from a spreadsheet instead of adding students one by one. A process that took hours now takes minutes.",
      "H/W/P": 2.0,
      "# Impacted": 150,
      "H/W Saved": 300,
      "Requested By": "District Champions, Implementation Team",
      "Team": "Product & Engineering",
      "Date Completed": "2026-05-01"
    }
  ]
};
```

## Acceptance Criteria

1. A new GitHub repo exists with Pages enabled and the site is live at the Pages URL
2. `index.html` loads, displays the sample data, and all flip cards work (click to flip, click to flip back, keyboard accessible)
3. The thermometer animates and shows correct totals from the data
4. The two CSV URL constants are clearly marked for easy replacement
5. Colors match the brand palette exactly (hex values above)
6. Font is Roboto loaded from Google Fonts
7. Layout is responsive (desktop 3-col grid, tablet 2-col, mobile 1-col with horizontal thermometer)
8. README includes setup instructions and the sheet spec
9. No external JS dependencies (vanilla only, no React/Vue/jQuery)
10. Cards rotate through Pink → Blue → Yellow → Purple backgrounds
11. All text on card fronts passes WCAG AA contrast (white on pink/blue/purple, dark on yellow)
12. Empty state displays correctly when no data rows exist
