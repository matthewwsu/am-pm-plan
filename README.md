# AM/PM Master Plan

A single-file HTML study tracker for breaking into asset management / portfolio management at the VP level. Covers a 30-day intensive sprint, four FINRA/NASAA licensing paths, and full CFA Level I prep through February 2027.

Built for myself. Syncs across laptop + phone via Firebase. Editable if you want to fork it for your own use.

---

## What this is

One file: `am_pm_master_plan_v4.html`. Open it in a browser and you get:

- **30-day AM/PM sprint** — a day-by-day curriculum hitting industry structure, portfolio theory, factor investing, private markets, allocator perspective, interview drills, and two IC memo deliverables
- **Licensing section** — exam mechanics and weekly study plans for SIE, Series 65, Series 7, Series 63, and Series 66, plus reference cards for Series 3/79/24/52/31, CAIA, and FRM
- **Full CFA Level I plan** — week-by-week topic schedule across 36 weeks covering all 10 topics with takeaway questions for each
- **Check-off progress** — day-level checkboxes, per-bullet sub-checkboxes with progress counters, and a three-level progress bar at the top
- **Textarea deliverables** — 47 prompts where you write your own answers; autosaves as you type
- **Toggleable sample answers** — click "Show sample answer" below any deliverable to reveal a model answer in a tan box, for self-calibration
- **225 explainer links** — each concept-level bullet links to Investopedia or the relevant official body
- **Cloud sync via Firebase** — signed-in users get their data synced across devices; works offline via localStorage fallback
- **Export + reset** — pull all answers to a .txt file; reset everything if you want to start over

---

## Quick start

### If you just want to run it locally (no sync)

1. Download `am_pm_master_plan_v4.html`
2. Open it in any browser
3. Start clicking boxes and typing answers. Everything saves to localStorage automatically.

That's it. You don't need Firebase unless you want cross-device sync.

### If you want cloud sync across devices

Follow `FIREBASE_SETUP.md`. Summary:

1. Create a Firebase project at [console.firebase.google.com](https://console.firebase.google.com)
2. Enable Firestore Database and Google Sign-In authentication
3. Create a Web app inside your project and copy the config object
4. Paste the config into the `FIREBASE_CONFIG` block near the bottom of the HTML file
5. Set Firestore security rules to restrict each user to their own document:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /plans/{userId} {
         allow read, write: if request.auth != null && request.auth.uid == userId;
       }
     }
   }
   ```
6. Add your hosting domain (e.g., `yourname.github.io`) to Firebase Authentication → Settings → Authorized domains
7. Deploy the HTML to GitHub Pages (or anywhere else that serves static files)

Once set up, "Sign in with Google" appears in the top-right. Sign in and your progress follows you across devices.

---

## How to use it

### Day by day

Work through one day at a time. Each day has:

- **Primary reading list** — canonical sources. Read before you try the deliverable.
- **Learning bullets with sub-checkboxes** — the specific concepts to walk away with. Check each as you internalize it. The `(X/N)` counter next to each learning box shows progress.
- **Deliverable textarea** — a prompt asking you to synthesize, analyze, or take a position. Write your answer in the box. It autosaves.
- **"Show sample answer" button** — after you write yours, click to see a model answer. Compare. If your answer hit the same key points, you're solid. If the sample had 3 things you didn't include or didn't know, those are your study gaps.
- **Day-level checkbox** — the big circle on the left. Mark the whole day complete when you're done.

### Licensing path

I chose to build out all four licensing paths. The overview cards for each exam are reference info — mechanics, fees, question counts, content weights. Not checkable. The weekly study cards below each overview have the actual learning items with sub-checkboxes and takeaway questions.

My personal sequencing:
- Month 1: 30-day sprint + SIE (parallel)
- Month 2: Series 65 (RIA path, no sponsor required)
- Months 3–9: CFA L1 prep
- When sponsored: Series 7 + 63 (or 66 if already have 7)
- Feb 22–28, 2027: Sit CFA L1

### CFA L1

The plan breaks 36 weeks (June 2026 → Feb 2027) into topic-specific weekly cards:
- Weeks 1–3: Quant Methods
- Weeks 4–8: FRA (heaviest weight)
- Weeks 9–10: Corporate Issuers
- Weeks 11–12: Economics
- Weeks 13–15: Equity Investments
- Weeks 16–18: Fixed Income (hardest — extra time)
- Weeks 19–20: Derivatives
- Weeks 21–22: Alternative Investments
- Weeks 23–25: Portfolio Management
- Weeks 26–28: Ethics (deep, and it's the tiebreaker)
- Weeks 29+: Second pass + mocks + final polish

Each week has learning items with explainer links, primary readings, and takeaway questions with sample answers.

---

## Design philosophy

- **Hyphens over em-dashes.** Personal preference in my writing voice. The file uses `&mdash;` in HTML where em-dashes appear visually, but I avoid them in prose I write.
- **Reference info is not a task.** Cards that tell you exam mechanics, topic weights, or licensing decision criteria don't have checkboxes. Only learning/doing tasks do.
- **Sample answers are calibration, not cheat sheets.** The goal is to let you self-check after you've written your own answer. If you just toggle the sample before writing, you're wasting the exercise.
- **Position deliverables acknowledge they're opinions.** For "take a view" prompts (AI bubble, 60/40, US exceptionalism), sample answers are framed as "one defensible view" with explicit counter-arguments. They're structural templates, not handed-down truth.
- **Personal prompts have framework guidance, not fake stories.** Day 26 "Why AM?" doesn't have a fabricated personal story — it has a framework for what makes a strong answer so you can write your own in your actual voice.

---

## File structure

```
am_pm_master_plan_v4.html     Main file (~280KB). Everything is in here.
FIREBASE_SETUP.md             Step-by-step Firebase setup guide.
README.md                     This file.
```

Legacy files from earlier iterations (kept for reference):
- `am_pm_30_day_plan.md` — original markdown version of the 30-day sprint
- `am_pm_master_plan.html` — first HTML version, localStorage only
- `am_pm_master_plan_cloud.html` — first Firebase cloud sync attempt
- `am_pm_master_plan_v2.md` / `v3.md` — intermediate markdown versions

---

## Progress at a glance

Top bar shows three progress indicators:
- **Days complete** (out of 30 sprint days plus licensing + CFA cards)
- **Learning items checked** (across all sub-bullets)
- **Deliverables written** (textareas with any content)

Sync status dot on the right: green = cloud synced, yellow = local-only, gray = signed out.

---

## Data model

State shape (same in localStorage and Firestore):
```json
{
  "checks": { "day1": true, "sie-w1": true, ... },
  "learn":  { "1.1": true, "1.2": true, ... },
  "answers": { "d1": "...", "d21": "...", ... }
}
```

Storage key: `am_pm_plan_state_v2` (local). Firestore path: `plans/{uid}`.

---

## Customizing

The file is single-file HTML with inline CSS and JS. No build step. To modify:

- **Add a day**: copy an existing `<div class="day">` block, increment the `data-day` attribute, and edit content.
- **Add a sample answer**: after a `<textarea>` + `<div class="save-indicator">`, add:
  ```html
  <button class="reveal-btn">Show sample answer</button>
  <div class="sample-answer">
    <span class="sample-answer-label">Sample answer</span>
    <p>Your content here.</p>
  </div>
  ```
- **Change colors / fonts**: edit CSS variables at the top of the `<style>` block (`--accent`, `--paper`, `--ink`, etc.)
- **Add cards**: place them anywhere inside `<section id="...">`. Progress bar counts all day-level checkboxes automatically.

---

## Known limitations

- **Single user per device** for localStorage mode. Firebase mode is per-account.
- **No real-time collaboration** — last-write-wins if you edit the same answer simultaneously on two devices.
- **Firebase free tier** should be more than sufficient for a personal tracker (Spark plan: 50K reads/day, 20K writes/day). You'll never come close.
- **Explainer links** were generated programmatically using the Investopedia `/terms/{letter}/{term}.asp` URL pattern. ~99% work but a dead link now and then is possible. Google the term, it's always findable.
- **Current market data in sample answers** (BlackRock Q1 2026 figures, AQR 2025 returns, April 2026 PC stress) will go stale. Treat as point-in-time anchors, not live data.
---

## License

Personal project. Fork it, modify it, share it. Attribution appreciated but not required.
