# Fairway — A Golf Field Instrument

A process-first golf tracker. Five taps per hole. Built for an older eye in bright sun.
Records your round, records your practice, and computes a single **Trajectory Index** (0–100)
that rewards consistency and discipline over heroics.

---

## What's in this folder

```
index.html              — the entire app (HTML, CSS, JS in one file)
manifest.webmanifest    — tells Android/iOS this is an installable app
service-worker.js       — makes the app work offline (no signal at the course)
icon-192.png            — small app icon
icon-512.png            — large app icon
README.md               — this file
```

All your data lives on your phone only. There is no server, no account, no internet
required once installed. Anthropic never sees your scores.

---

## Installing on your phone

There are two paths. **Path A** (free hosting via GitHub Pages) takes ~10 minutes and
gives you the full installable app. **Path B** (a single-file fallback) takes 2 minutes
and works in any browser but won't quite feel native.

I recommend Path A. It's a one-time setup.

### Path A — Full PWA install via GitHub Pages (recommended)

1. **Create a free GitHub account** at https://github.com/signup if you don't have one.
2. On GitHub, click **+ ▸ New repository**. Name it `fairway` (or anything). Make it **public**. Click *Create*.
3. On the empty repo page, click **uploading an existing file**.
4. Drag all six files from this folder onto the upload area: `index.html`, `manifest.webmanifest`,
   `service-worker.js`, `icon-192.png`, `icon-512.png`, `README.md`.
5. Scroll down, click **Commit changes**.
6. Go to **Settings ▸ Pages** in the repo. Under *Source*, choose **Deploy from a branch**,
   branch **main**, folder **/ (root)**. Click *Save*.
7. Wait ~1 minute. The Pages section will show a URL like `https://yourusername.github.io/fairway/`.
8. **On your phone**, open that URL in **Chrome** (Android) or **Safari** (iPhone).
9. In Chrome: tap the **⋮** menu ▸ **Install app** (or *Add to Home Screen*).
   In Safari: tap **Share** ▸ **Add to Home Screen**.
10. Done. The Fairway icon is now on your home screen. Tap it — it opens full-screen,
    works offline, saves your rounds locally.

### Path B — Quick local fallback

If you want to try the app right now without setting up GitHub:

1. Email the `index.html` file to yourself.
2. Open the email on your phone.
3. Download the attachment.
4. Tap it to open in Chrome.
5. Use the app. You can bookmark it.

The downside: in this mode the service worker doesn't activate (browsers block local files
from doing that for security reasons), so if you lose signal mid-round the app *might* still
work (modern browsers cache aggressively) but it isn't guaranteed. Path A is more reliable.

---

## How to use it

### Round tab

1. Tap **Start New Round**, enter the course name and pick 9 or 18 holes.
2. On each hole, tap the following (in order on screen):
   - **Score** (Birdie+ / Par / Bogey / Double / Triple / +4↑)
   - **Fairway** (Hit / Safe Miss / Trouble) — skipped on par 3s
   - **Approach Distance** (< 50m / 50–100m / 100–150m / 150m+) — *always asked*
   - **Green** (GIR / Missed Short / Missed Long / Short-Sided)
   - **Miss Distance** (Just Off / Short Pitch / Way Off) — *only if green missed*
   - **Recovery** (On & Close / On, Lag Putt / Still Off) — *only if green missed*
   - **Putts** (1 / 2 / 3 / 4+)
   - **Decision** (Played Safe / Attacked)
   - Plus a Yes/No for penalty stroke.
3. Average load: about 6 taps per hole; missed-green holes ask for 8.
4. The strip at the top shows your running **Strokes**, **Grinder** score and **Process** score.
5. The par defaults to a typical pattern — tap **PAR 4 ▾** to cycle through 3 / 4 / 5 for the actual hole.
6. The **Next Hole** button moves you forward. You can go back with the **‹** arrow if you mis-tapped.
7. Hit **End Round** when finished — you get a full summary card.

### Short-game diagnostic fields (v3 additions)

Three of the eight per-hole fields above were added specifically to surface short-game leaks:

- **Approach Distance** answers *"from where?"* and powers a GIR-rate-by-distance breakdown
  in the Stats tab. Long-iron play vs wedge play finally separate cleanly.
- **Miss Distance** answers *"how badly did you miss?"* A "Just Off" approach is a different
  failure than a "Way Off" one — one is a chip away, the other is wedge distance control
  collapsing. The forensics attribute these differently.
- **Recovery** answers *"did the follow-up save it?"* — separating chip execution from
  putting under pressure. When the recovery is "On & Close" but the hole still ends in
  bogey, the leak is the next putt, not the chip.

The Round Detail chain narratives now read like *"Tee shot in play, but the approach from
50–100 m missed way off, got on but with a long lag putt, and the putting didn't bail you out.
Approach distance control was the leak."* That's diagnostic, not just descriptive.

### Practice tab

Pick a category, pick a drill (or "+ Custom"), enter your result and minutes spent. That's it.
No timers, no enforcement — just an honest log.

### Stats tab

- **Trajectory Index** (TI) — 0–100, rolling 5-round window. Needs 3+ rounds to calculate.
  - 40% process discipline
  - 40% damage control (doubles, 3-putts, penalties)
  - 20% strokes vs your baseline
- **Grinder Card bar chart** — per round, positive means the disciplined game won.
- **Process Quality line** — % per round, derived from your inputs.
- **Damage Events bars** — count of doubles + 3-putts + penalties per round; lower is better.
- **Practice Mix** — your last 28 days. Target is 70% inside the three short-game blocks.
- **Recent Rounds list** — tap any round to open its detail screen (see below).

In **Settings** at the bottom you can edit your **baseline strokes** (default 84) and export
all your data as a CSV file for safekeeping.

### Round Detail — "Where It Hurt"

Tap any round (on the Round tab landing, in the Recent Rounds list, or via the *See Where
It Hurt* button after finishing a round) to open its detail screen. It shows:

- **Three top-line stats** — Strokes, Grinder, Process.
- **Hole by Hole strip** — all 18 holes color-coded by score, with red rings on the three
  worst-grinder holes so you can see at a glance where the damage was.
- **Where It Hurt** — a damage report: counts of blown-up holes, three-putts, penalty
  strokes, trouble tee shots, short-sides, missed greens, plus rough stroke estimates.
- **Double Bogey Forensics** — for every double-or-worse hole, the app attributes the
  primary cause in the chain (tee / approach / short-side / putting / penalty) and writes
  a one-line narrative of how the hole came apart. A summary at the top shows the
  count by cause: *"6 holes blew up today. Where the chain started: 3 tee, 1 pen, 1
  s-side, 1 putt. Most of the damage today started with tee shot in trouble."*
- **What the Round Says** — 3–6 generated observations that pick out the patterns: *"You
  hit 10 greens but three-putted 2 of them. The iron play was there; the putting gave it
  back."*
- **Practice This Week** — up to 3 prescriptions that match symptoms to causes. Short-sides
  send you to on-course aim discipline, not chipping (chipping practice wouldn't fix it).
  Three-putts send you to lag-putting drills with specific protocols. Excessive trouble
  off the tee suggests benching the driver for a few rounds.
- A noise warning in amber: *"One round is one Tuesday."* Cross-check the patterns in the
  Stats tab over 5 rounds before changing your practice schedule.

### Where Doubles Come From — cross-round pattern (Stats tab)

The Stats tab also shows an aggregated forensics view: across your last 10 rounds, what
fraction of your doubles started with which cause? A clean 50%+ dominance of one cause
is the strongest single piece of practice guidance the app produces. Most amateurs have
one dominant leak; this tells you which.

---

## The Trajectory Index — what it actually rewards

> "You don't lower the handicap by aiming at it.
> You lower it by aiming at the player who shoots those scores."

The TI is deliberately weighted so that a higher-stroke round played with discipline can
*beat* a lower-stroke round played with chaos. That is the philosophy, encoded in math:

- **Process discipline (40%)** — leading indicator. Moves first.
- **Damage control (40%)** — middle indicator. Moves second.
- **Strokes (20%)** — lagging indicator. Moves last.

If the math rewarded strokes equally, the TI would just be your handicap with extra steps.
You already have a handicap. You don't need another.

Expect the TI to occasionally fall in a week where your strokes also fell. That is the
metric telling you the truth — you scored fewer because of luck, not because of the player
you are becoming. The next round will reveal which.

---

## Data export & backup

Stats tab ▸ **Export Data (CSV)** writes a file you can email to yourself or save to Google Drive.
Do this every few weeks. The data is otherwise only on this phone.

If you wipe the app or change phones, you'll lose history unless you exported. There is
no cloud sync by design — fewer moving parts.

---

## Troubleshooting

**The app doesn't install** — Make sure you're using Chrome (Android) or Safari (iPhone) and
that you're on the GitHub Pages URL, not the raw GitHub repo. The URL should look like
`https://yourname.github.io/fairway/`, ending in a slash.

**My data disappeared** — Most likely you cleared your browser data, or opened a different
URL. Browser localStorage is tied to the exact origin. Always open the app from your home
screen icon after installing.

**The Trajectory Index says "3 more rounds"** — You need 3 logged rounds (each at least
half-complete) before it computes. Play three rounds and tag them honestly; the number
appears.

**A button doesn't respond** — Tap firmly with a finger pad, not a fingertip. Capacitive
screens are temperamental with gloves; if you wear a glove on your left hand, tap with
your right.

---

That's everything. The app does one thing — it measures the player you're becoming, not
the player you've been. Use it for two months. Don't peek at the handicap. See what the
TI says.
