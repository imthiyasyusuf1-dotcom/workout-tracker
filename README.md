# PPL Tracker — your personal training PWA

A free, installable app for your Push / Pull / Legs program. Logs every set, scores it with the
**Beat the Book** strength engine, tells you exactly what to load next, runs the 12-month phase plan
(Linear → Grinding → Conditioning) with a deload scheduler, and tracks your bodyweight from 77 → 88 kg.

Works offline, installs to your iPhone home screen, and syncs your Mac and iPhone for free via Firebase.

---

## 1. Put it online for free (GitHub Pages — ~5 min, one time)

A PWA needs an `https://` link to install on iPhone. GitHub Pages gives you one free, forever.

1. Make a free account at **github.com**.
2. Click **+** (top-right) → **New repository**. Name it `ppl-tracker`, set it **Public**, click **Create**.
3. On the new repo page click **uploading an existing file**.
4. Drag in **all the files** from this folder: `index.html`, `manifest.webmanifest`, `sw.js`,
   `icon-180.png`, `icon-192.png`, `icon-512.png`. (You don't need to upload this README.) Click **Commit changes**.
5. Go to **Settings → Pages**. Under *Build and deployment* → *Source* pick **Deploy from a branch**,
   choose **main** / **/(root)**, click **Save**.
6. Wait ~1 minute, refresh. Your link appears at the top:
   **`https://YOUR-USERNAME.github.io/ppl-tracker/`**

That link is your app. Open it on any device, anywhere.

> Alternative: drag this folder onto **app.netlify.com/drop** for an instant link instead.

---

## 2. Install on your iPhone

1. Open your GitHub Pages link in **Safari** (must be Safari, not Chrome, on iOS).
2. Tap the **Share** button (square with ↑).
3. Scroll down → **Add to Home Screen** → **Add**.
4. Open it from your home screen. It runs full-screen like a real app and works offline.

Do the same on your Mac (Safari: **File → Add to Dock**, or just bookmark the link).

---

## 3. Turn on Mac ↔ iPhone sync (free)

Your Firebase project is **already wired into the app** (`workout-tracker-d1021`). You just need to
switch on the database and its access rules once:

1. Go to **console.firebase.google.com** → open your **workout-tracker-d1021** project.
2. Left menu → **Build → Firestore Database** → **Create database**.
   Pick a location near you, choose **Start in production mode**, click **Enable**.
3. Open the **Rules** tab, replace everything with the block below, and click **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /trackers/imthi-ppl-77to88 {
      allow read, write: if true;
    }
  }
}
```

That's it. The app uses the sync code **`imthi-ppl-77to88`** on every device, so your Mac and iPhone
share the same data automatically and in real time. Log a set on your phone at the gym, and it's on
your Mac when you get home.

**Privacy note:** these rules let anyone who knows both your app URL *and* the exact sync code read/write
that one document. For a personal gym log that's fine. To change the code, open the app → ⚙ (gear) →
*Sync code* → type a new private value on **both** devices, and update the doc name in the rule above to match.

---

## 4. How to use it

- **Today** — your next workout, current phase, deload watch, bodyweight, and how many lifts you've
  "beaten the book" on this week. Tap **Start … workout**.
- **Train** — pick Push / Pull / Legs. Each lift shows *what to beat* and pre-fills the suggested
  weight. Type your two working sets; the badge lights up **✓ BEAT IT**, **▲ ADD WEIGHT**, or
  **✗ beat next time** live. Tap **Save session** when done.
- **Body** — log your weekly weigh-in, see the trend chart vs your goal, and the 12-month phase map.
- **History** — every session per exercise, with your all-time e1RM PR.

Blue/editable fields are yours; everything else is calculated.

### The engine
- **e1RM score** = weight × (1 + reps/30). One extra rep counts as progress just like adding weight —
  beat last session's score to "beat the book".
- **Add weight** triggers when your *lowest* working set hits the top of the rep range; the next load
  is pre-filled with your increment.
- **Phases:** Months 1–3 Linear (add often) · 4–9 Grinding (fractional plates, deload every 6–8 wks) ·
  10–12 Conditioning (maintain strength in a deficit). Set smaller increments per exercise in Phase 2
  via the inputs.

---

## 5. Backup & updates

- **Backup:** ⚙ → *Export JSON* saves all your data to a file. *Import JSON* restores it.
- **Update the app later:** re-upload a changed `index.html` to the repo. Bump the `CACHE` version in
  `sw.js` (e.g. `ppl-v2`) so phones pull the new version.
