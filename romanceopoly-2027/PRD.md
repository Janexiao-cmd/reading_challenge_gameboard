# Romanceopoly 2027: Product Requirements Document

> Living document. Update it whenever requirements, rules, or decisions change, and add a line to the Changelog at the bottom.

**Owner:** Jane · **Status:** Draft v0.9 · **Last updated:** 2026-11-24

---

## 1. Overview

Romanceopoly 2027 is a personal, single-player reading challenge for romance novels, played on a Monopoly-style campus board. Each "building" on the board is a reading prompt. The reader rolls a die, moves a token around the board, reads a book that fits the prompt, and logs it. Progress is tracked by region, by tier (Core vs. Honors), and by bonus categories.

## 2. Goals

- Make a year-long reading challenge feel like a game: a board, dice, a token, and badges.
- Give a single place to log each book: title, author, cover, dates, rating, notes.
- Show progress at a glance: pin colors on the board, per-region bars, a journey log.
- Work with zero setup: no account, no backend, and data stays in the user's browser.

## 3. Non-goals (for now)

- Multiplayer or social features.
- Accounts, cloud sync, or a backend.
- Book search or metadata lookup APIs (titles and covers are entered by hand).
- Native mobile app (responsive web only).

## 4. User

One reader (Jane) who plays on desktop, and possibly on a phone or tablet. Comfortable with a simple browser app. Wants the game to be charming, and doesn't want to maintain it.

## 5. Game structure

The game is framed as going to university. The reader earns a "degree" by reading **20 books**.

### Graduation requirements

| # | Requirement | How it's completed | Books |
|---|---|---|---|
| 1 | **General Studies** | Read 5 General Studies Core prompts | 5 |
| 2 | **Major** | Declare one department (genre) and read all 5 of its Core prompts | 5 |
| 3 | **Minor** | Declare a second department and read all 5 of its Core prompts | 5 |
| 4 | **Electives** | Read any 5 Core prompts from the 3 departments not chosen as major or minor | 5 |
| | **Graduate** | Complete 1 to 4 | **20** |

### Departments (genres)

Each has a definition shown in a tooltip. An ⓘ icon follows the department name (sidebar progress list and location modal header), and the tooltip appears only when hovering the icon.

| Department | Definition |
|---|---|
| **Ruinous Arts** | Historical romance through London ballrooms and a trip outside of England. Let's fall in love with a grumpy duke or get ruined by a spy in a beautiful maze. |
| **Special Collections** | Where romance history lives: the clinch covers, the spinner racks full of romance. Explore the decades and how romance changes. Any subgenre counts here. |
| **Unnatural Sciences** | Everything that goes bump in the night and then catches feelings: fated mates, vampire brotherhoods. If it has fangs, claws, tentacles or a soul bond, but it's still in the real world, you can find it here. |
| **Applied Mythology** | Bargains you should have read the fine print on, heirs to thrones nobody asked for, tournaments, rebellions and the occasional dragon with opinions about your love life. |
| **Current Affairs** | Contemporary romance set in the real world, group chats and small-town gossip included. Enroll here for the boy next door and the bad boy that will ruin your sleep. |

General Studies is the required foundation, not a major option. It intentionally has no tooltip.

### Terminology

- **Location** replaces "building" in all user-facing text, and eventually in code. (The code and `data.js` still say "building" for now.)
- **Department** = romance genre.

### Board elements

| Element | Description |
|---|---|
| Board | `board.webp` (2000×1336 px) with clickable pins placed by pixel coordinates |
| Locations | Reading prompts, defined in `data.js` (generated from `Romance Reading Prompts.numbers`) |
| Departments | General Studies plus the five above |
| Tiers | **Core** (counts toward graduation) and **Honors** (extra credit, gold-ringed pins) |
| Other spaces | Clubs and Events (4 prompts each), Study Sessions (12 monthly assigned reads) and Night School (8 reads). Their role under the degree rules is still open. |

### Dice and movement rules (implemented)

1. Start on *Start Here*.
2. **Core die** moves the token clockwise, counting only open Core locations in the active departments (General Studies, then major and minor, then electives). **Honors die** counts only open Honors locations in General Studies, the major and the minor.
3. Landing on a location means reading a book that fits its prompt.
4. Status flow: To read → Reading → Complete or DNF. Completing stamps today's date.
5. Must finish, DNF, or pass the current space before rolling again (toggleable).
6. Completed locations are skipped when moving (toggleable).
7. **Lap bonus:** passing Start Here earns a free pick of any open location.
8. Clubs and Events are not reachable by dice. The player clicks them any time.
9. The graduation banner shows at 20 degree books. The honors banner needs all Honors in the major and minor as well.
10. Any location can be clicked at any time to read ahead.

## 6. Functional requirements

### Implemented

- **Board:** pins with status styling (todo, reading, partial, complete, DNF, honors, group). Hover tooltips. Zoom slider. Optional dotted path overlay. Animated token.
- **Dice:** separate Core and Honors rolls with animated movement. Roll buttons disable when no open buildings of that tier remain.
- **Building modal:** prompt, status selector, title, author, start and end dates, half-star rating, notes, and cover image (upload or paste, resized to 320 px JPEG). Tabs for group spaces. "Use assigned book" for Study Sessions and Night School. "Place my token here" and "Clear this building".
- **Progress:** header summary, Core progress bar, per-region bars with badges, journey log (last 12 shown, 60 kept).
- **Persistence:** `localStorage` under key `romanceopoly-2027-v2`.
- **Backup:** JSON export and import, plus full reset.
- **Rules modal** with the two rule toggles (text predates the degree rules).
- **Department tooltips:** an ⓘ icon after the department name in the sidebar progress list and the location modal chip. The definition shows on hover over the icon only.
- **Degree tracker:** Degree card with Major and Minor dropdowns (any time, changeable), progress rows for General Studies, Major, Minor and Electives, and a 20-book total. Graduation and honors banners.
- **Dice hard limit:** Core die lands on General Studies first, then major and minor, then electives. Honors die covers Honors locations in General Studies, the major and the minor.
- **Dev server:** `npm run dev` serves the static files on port 8000.

### Known gaps and open questions

**Decisions on degree rules (2026-11-24)**

- **Dice order (hard limit):** dice land only on General Studies first, then on major and minor locations. Electives open up only once both are complete.
- Major and minor can be changed at any time after declaring, and progress in each department is kept.
- Major and minor can be declared at any time, without finishing General Studies first.
- The player starts in General Studies, then moves to other departments, prioritizing the major and minor first.
- One book counts toward one requirement only (revisit later).
- Clubs, Events, Study Sessions and Night School are extra credit only. Details to come from Jane.
- Honors turns a normal graduation into an **honors graduation**. **Honors graduation requires completing every Honors location in the declared major and minor departments**, on top of the 20 books.

**Degree rules (not yet built)**

- [ ] Clubs, Events, Study Sessions and Night School: details to come from Jane.
- [ ] Rename "building" to "location" in the code (UI text and the Rules modal are done).

**Other**

- [ ] Final prompts and assigned books in `data.js` (Study Sessions and Night School). Confirm they're complete.
- [ ] Is a mobile or tablet layout good enough? It hasn't been tested.
- [ ] Cover images are stored as base64 in `localStorage`, which could hit the storage limit with many books. Export is the only backup.
- [ ] No undo for an accidental roll or status change.
- [ ] Decide whether the app will be hosted anywhere, or only run locally.

## 7. Technical notes

- **Stack:** plain HTML, CSS, and JS. No framework and no build step.
- **Files:**
  - `index.html`: UI, styles, and game logic (single file)
  - `data.js`: building and prompt data (`window.BUILDINGS`)
  - `board.webp`: board artwork
  - `Romance Reading Prompts.numbers`: source spreadsheet for `data.js`
  - `package.json`: `dev` script only (`npx serve . -l 8000`)
- **Fonts:** Google Fonts (Fraunces, Nunito), so first load needs a network connection.
- **Run:** `cd romanceopoly-2027 && npm run dev`, then open http://localhost:8000. Node.js is required, and `npm` must be run from this folder.
- **Pin positions** are hard-coded in the `RAW` array in `index.html`, as pixel coordinates on the 2000×1336 board image.
- **Regenerating data:** `data.js` is generated from the Numbers file. Edit the spreadsheet or `data.js`, and keep both in sync.

## 8. Decisions log

| Date | Decision | Why |
|---|---|---|
| 2026-11-24 | Static site with no backend; data in `localStorage` | Simplest thing that works for a single player |
| 2026-11-24 | Added `package.json` with a `dev` script | So `npm run dev` works as a one-command start |
| 2026-11-24 | Game framed as a university degree: General Studies + major + minor + electives = 20 books | Jane's rules |
| 2026-11-24 | "Building" becomes "location"; "department" = genre | Jane's terminology |
| 2026-11-24 | Dice use a hard limit: General Studies, then major and minor, then electives | Keeps the degree order clear |
| 2026-11-24 | Honors graduation = all Honors locations in major and minor | Jane's rule |
| 2026-11-24 | Major and minor can be changed any time | Jane's rule |
| 2026-11-24 | General Studies gets no tooltip | Jane's choice |
| 2026-11-24 | Honors die can reach General Studies Honors; they don't count toward honors graduation | Jane's rule |
| 2026-11-24 | One book counts toward one requirement (for now) | Simplest rule; revisit later |
| 2026-11-24 | Honors = honors graduation; Clubs, Events, Study Sessions, Night School = extra credit | Jane's rules |
| 2026-11-24 | Track requirements in `PRD.md` in the repo | Keeps context next to the code and in git history |

## 9. Changelog

- **2026-11-24:** PRD created from the existing app (v0.1).
- **2026-11-24:** Added degree rules, department definitions and terminology (v0.2). Added department tooltips to the app.
- **2026-11-24:** Recorded decisions on declaring major and minor, dice priority, extra credit and honors graduation (v0.3). Tooltips now use an ⓘ icon.
- **2026-11-24:** Dice priority decided: hard limit (v0.4).
- **2026-11-24:** Honors graduation decided: all Honors in major and minor (v0.5).
- **2026-11-24:** Major and minor changeable any time (v0.6).
- **2026-11-24:** General Studies stays without a tooltip (v0.7).
- **2026-11-24:** Built the degree tracker, major/minor declaration, dice hard limit, banners and new Rules text (v0.8).
- **2026-11-24:** Honors die now reaches General Studies Honors locations (v0.9).
