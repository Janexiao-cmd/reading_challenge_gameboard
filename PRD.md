# Romanceopoly 2027: Product Requirements Document

> Living document. Update it whenever requirements, rules, or decisions change, and add a line to the Changelog at the bottom.

**Owner:** Jane · **Status:** Draft v0.27 · **Last updated:** 2026-11-24

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

### Events

Each Event is a seasonal space holding 4 prompts. Its season and definition show in a tooltip (ⓘ on the season chip in the location modal, and on the board pin tooltip).

| Event | Season | Definition |
|---|---|---|
| **Polar Plunge** | Winter | Start the year by jumping into the cold. We are going into deep hibernation, snowed in, cabins, and warmth. |
| **Picnic Games** | Spring | Bring your blankets and let's set up on the lawn. Spring is for fake dating, gardens, and romcoms. |
| **Summer Regatta** | Summer | Two teams on the water and a storm rolling in. Summer is for beaches, bodyguards and books that keep you up past midnight. |
| **Rivalry Week** | Fall | The last match of the year. Rivals, masks, a bonfire and the holidays coming in fast. Bring your mud boots. |

Events are extra credit only.

- **Seasons:** Winter = January to March, Spring = April to June, Summer = July to September, Fall = October to December. The app uses the current month on your device.
- **Season lock:** off season, a book can't be started (Reading, Complete and DNF are disabled while its status is "To read"). Title, author, cover, rating and notes can still be added. A book already started in season can still be finished or marked DNF after the season ends.
- **Access:** Events are reached by clicking them. The dice don't land on them.

### Clubs

Each Club is a space holding 4 prompts, extra credit only, reached by clicking (not by dice). Its definition shows in a tooltip (ⓘ on the Club chip in the location modal, and on the board pin tooltip).

| Club | Definition |
|---|---|
| **Grave Diggers** | For readers who like their love interests dangerous and their books with a content warning page a mile long. Read at your own comfort level and swap freely. |
| **Stargazers** | Space operas, cyborgs, the end of the world, and people who got pulled off Earth and fell in love on the way. |
| **Tea Society** | Low stakes, warm drinks, and a main character who is probably knitting something. For the weeks when you want a book to be your comfort place. |
| **Zine Club** | Indie romance, the way it actually gets found: the character art that stopped your scroll, the small press you love, the book written by two friends. |

**Rule:** finish all 4 prompts in a club to become that club's **Club President** 👑. It is a badge only and unlocks nothing. The app shows a Club President chip in the modal, a gold ring and label on the pin, and a "Club Presidents x/4" line in the progress card.

### Study Sessions

- 12 monthly sessions (January to December). Each has a prompt and an **assigned book**, defined in `data.js`.
- Extra credit only. They're not on the board or the dice, and open from the Progress card.
- Opening a session offers a "Use assigned book" button that fills in the title and author. The player can also read a different book.
- Not season-locked: any session can be started at any time. (Assumption from the existing app; say so if sessions should open only in their month.)

### Night School 🌙

> "Missing a dark romance department? After hours, any department can go dark. Pick any department except General Studies and swap its prompts for the Night School ones. It keeps its map locations, but your classes are at night. If you choose this as your major, use the name of the map area as your team."

- Night School is a **mode**, not a separate set of locations. It has 8 prompts (5 Core, 3 Honors), matching the 8 locations in a department.
- **Any number** of departments (not General Studies) can go dark, each using the same 8 Night School prompts.
- Prompts swap in order: a department's Nth Core location gets Night School Core N, and the same for Honors.
- A dark department keeps its locations, its Core and Honors tiers, and its place in the degree (major, minor or elective). Books already logged stay with their location.
- It can be turned on or off at any time under Night School in the "Elect my degree" modal. Dark locations show a 🌙 Night School chip.
- If a dark department is the major, the Progress card shows the department name as the player's team.

### Awards

| Award | How it's earned | Status |
|---|---|---|
| 🎓 **Graduate** | Finish your 20 prompts | Built |
| ★ **Honors Graduate** | Finish all 3 Honors prompts in any department (General Studies counts) | Built |
| 👑 **Club President** | Finish all 4 prompts in a club. One for every club you finish | Built |
| 🏆 **Varsity** | Finish all 4 prompts in all four seasonal events (16 prompts) | Built |
| 🏅 **Match Week Champion** | Log at least one entry during a match week your team wins | Not built: rules for match weeks, teams and winning are undefined |
| 💎 **Summa Cum Laude** | Earn Graduate, Honors Graduate, Club President and Varsity in the same year | Built (at least one Club President counts) |

Awards show as chips on the Progress card: gold when earned, grey when not. Hover a chip for how to earn it. The graduation banner changes for Honors Graduate and Summa Cum Laude. Awards are independent of each other (Honors Graduate doesn't require Graduate), except Summa Cum Laude.

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
| Other spaces | Clubs and Events (4 prompts each), Study Sessions (12 monthly assigned reads, below the map, extra credit). Night School is a prompt-swap mode (see below), not a separate set of locations. |

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

### Layout

- **Intro:** a collapsible "How it works" bar sits under the roll bar (design option B). It is open on a first visit, and remembers if the player collapses it. Full text: "Choose your own 2027 romance reading challenge with Romanceopoly! Every building on this map is a reading prompt, and you get to build your own class schedule for the year. Roll the dice or pick your prompts one by one, then finish 20 prompts and graduate with the Class of 2027."
- **Roll bar:** a compact bar sits at the top of the page, under the header and above the intro (and stays in view while scrolling). It holds the dice, Roll Core die, Roll Honors die, a one-line "token on …" status, and Open location and Pass space.
- **Main view:** the game board on the left, shown in full (not cropped) at 100% zoom. It only scrolls inside its card when zoomed in. The sidebar on the right has one card, **Progress**.
- **Journey log** lives in a drawer that slides in from the right. A "📜 Journey log" button in the header opens it. It shows all saved entries (up to 60).
- **Progress** shows only what applies to the player: total out of 20, then General Studies, Major, Minor and Electives; Honors rows for General Studies, the major and the minor; an Awards chip row; each Club (👑 when President); each Event with its season. A **🎓 Elect my degree** button and a **📚 Study Sessions x/12** button sit on this card.
- **Elect my degree** opens a modal to choose the major and minor, with Night School in its own section below them.
- **Study Sessions** opens a modal with the 12 monthly sessions. There is no section below the map any more.


### Implemented

- **Board:** pins with status styling (todo, reading, partial, complete, DNF, honors, group). Hover tooltips. Zoom slider. Optional dotted path overlay. Animated token.
- **Dice:** separate Core and Honors rolls with animated movement. Roll buttons disable when no open buildings of that tier remain.
- **Building modal:** prompt, status selector, title, author, start and end dates, half-star rating, notes, and cover image (upload or paste, resized to 320 px JPEG). Tabs for group spaces. "Use assigned book" for Study Sessions and Night School. "Place my token here" and "Clear this building".
- **Progress:** header summary, Core progress bar, per-region bars with badges, journey log (last 12 shown, 60 kept).
- **Persistence:** `localStorage` under key `romanceopoly-2027-v2`.
- **Undo:** an "↶ Undo" button in the header reverses the last roll or status change (one step only, and it resets on page reload).
- **Backup:** JSON export and import, plus full reset. After every 3 completed books since the last export, a reminder toast offers "Export now".
- **Rules modal** with the two rule toggles (text predates the degree rules).
- **Department tooltips:** an ⓘ icon after the department name in the sidebar progress list and the location modal chip. The definition shows on hover over the icon only.
- **Degree tracker:** "Elect my degree" modal with Major and Minor dropdowns (any time, changeable) and Night School checkboxes. Progress rows for General Studies, Major, Minor and Electives, and a 20-book total. Graduation and honors banners.
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
- Honors turns a normal graduation into an **honors graduation**. Honors graduation is now the **Honors Graduate** award: finish all 3 Honors prompts in any department (see Awards). This replaces the earlier rule (all Honors in major and minor).

**Degree rules (not yet built)**

- [ ] **Match Week Champion:** define a match week (dates? which weeks?), how teams work (is the team your major's map area?), who wins a week, and how an entry is tied to a match week.
- [ ] Summa Cum Laude says "in the same year". The app only covers 2027, so this isn't checked separately.
- [ ] Rename "building" to "location" in the code (UI text and the Rules modal are done).

**Other**

- [ ] Final prompts and assigned books in `data.js` (Study Sessions and Night School). Confirm they're complete.
- [ ] Is a mobile or tablet layout good enough? It hasn't been tested.
- [ ] Hosting on **GitHub Pages** (chosen). Repo: `Janexiao-cmd/reading_challenge_gameboard` (public). Everything now lives in the repo root, so Pages can serve branch `main`, folder `/ (root)`. Still to do: commit, push and switch on Pages in the repo settings. Prompts, board art, `PRD.md` and the `.numbers` file will all be public (approved).
- [ ] Phone and tablet layout needs checking, since friends will use it on phones.
- [ ] Cover images and the `.numbers` source file shouldn't be published. Confirm what goes online, and check board art and prompt text are OK to share.

## 7. Technical notes

- **Stack:** plain HTML, CSS, and JS. No framework and no build step.
- **Files:**
  - `index.html`: UI, styles, and game logic (single file)
  - `data.js`: building and prompt data (`window.BUILDINGS`)
  - `board.webp`: board artwork
  - `Romance Reading Prompts.numbers`: source spreadsheet for `data.js`
  - `package.json`: `dev` script only (`npx serve . -l 8000`)
- **Fonts:** Google Fonts (Fraunces, Nunito), so first load needs a network connection.
- **Run:** `npm run dev` from the repo root, then open http://localhost:8000. Node.js is required, and `npm` must be run from the folder holding `package.json` (the repo root).
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
| 2026-11-24 | Club President is a badge only | Jane's rule |
| 2026-11-24 | Night School is a prompt-swap mode; any number of departments, changeable any time | Jane's rule |
| 2026-11-24 | App will be shared with friends (hosted online, each player's data in their own browser) | Jane's choice |
| 2026-11-24 | Backups stay Export/Import, with a reminder every 3 completed books | Jane's choice; no accounts or backend |
| 2026-11-24 | Undo covers the last action only | Jane's choice; simple and covers most mistakes |
| 2026-11-24 | Project files live in the repo root (no `romanceopoly-2027/` subfolder) | Lets GitHub Pages serve the repo directly |
| 2026-11-24 | Prompts, board art and the repo contents may be public | Jane approved |
| 2026-11-24 | Host on GitHub Pages | Free, and the repo is already on GitHub |
| 2026-11-24 | Sidebar order: Progress, Roll the dice, Journey log; degree election and Study Sessions move into modals | Jane's layout request |
| 2026-11-24 | Roll the dice becomes a compact top bar; Journey log moves to a drawer | Jane's design request |
| 2026-11-24 | Intro uses design option B (collapsible "How it works") | Jane's pick |
| 2026-11-24 | Awards added: Graduate, Honors Graduate (all 3 Honors in any department), Club President, Varsity, Match Week Champion, Summa Cum Laude | Jane's rules |
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
- **2026-11-24:** Added the four Events with seasons and definitions, shown as tooltips (v0.10).
- **2026-11-24:** Event season lock and click-only access (v0.11).
- **2026-11-24:** Added Clubs with definitions and the Club President rule (v0.12).
- **2026-11-24:** Club President confirmed as a badge only (v0.13).
- **2026-11-24:** Night School redefined as a department prompt-swap mode and built (v0.14). The separate Night School grid was removed.
- **2026-11-24:** Documented Study Sessions from the existing data and app behavior (v0.15).
- **2026-11-24:** Decided to share the app with friends, so hosting and phone layout are needed (v0.16).
- **2026-11-24:** Backup approach decided: keep Export/Import and add an export reminder (v0.17).
- **2026-11-24:** Added one-step Undo for rolls and status changes (v0.18).
- **2026-11-24:** Chose GitHub Pages for hosting (v0.19).
- **2026-11-24:** Moved all files to the repo root and approved public sharing (v0.20).
- **2026-11-24:** Reworked the layout: Progress first and personal, "Elect my degree" modal, Study Sessions modal (v0.21).
- **2026-11-24:** Added the intro text at the top of the page (v0.22).
- **2026-11-24:** Removed "spin the wheel" from the intro text (v0.23).
- **2026-11-24:** Compact roll bar at the top, Journey log in a drawer, intro design options in `design-options/` (v0.24).
- **2026-11-24:** Intro switched to collapsible design option B (v0.25).
- **2026-11-24:** Board is no longer cropped at 100% zoom (v0.26).
- **2026-11-24:** Added awards, with Honors Graduate redefined as all 3 Honors in any department (v0.27).
