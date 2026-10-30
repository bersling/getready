# Get Ready

A morning routine timer for getting to school on time. One task at a time,
a ring that drains as the minutes run out, a beep when you're overdue, and a
big green **Fertig** button. All on-screen text is German.

## Run it

Open `index.html` in a browser. No build, no server needed. On a tablet or
phone, add it to the home screen so it opens full-screen.

Tap the sunrise screen once to start. That tap is what allows sound to play,
so don't skip it.

## How the timing works

The morning has a hard window, **07:10 to 07:55**, and every task has a
**time budget** (minutes). The budgets fill the window exactly.

- A task starts the moment the previous one is ticked (at the earliest 07:10).
- Its deadline is "late enough that every task still open after it gets its
  full budget before 07:55". So:
  - **Done early?** The saved time is added to the next task
    (the hint shows e.g. `bis 07:28 · 8 min gespart 🎉`).
  - **Late?** The lost time comes out of the next task, so the morning still
    ends at 07:55.
  - Ticking a later task ahead of time frees its budget for the current one.

## What it does

- A checklist on the side shows every task with its budget. Tap any item to
  tick it off (or untick it), in any order. The big timer always shows the
  first task that isn't ticked yet.
- **Drag and drop** an item to reorder the routine (mouse or finger). A plain
  tap still ticks. The order is remembered across days.
- A second, smaller **Eltern** checklist below it holds untimed to-dos
  (Znüni, water bottle; swim stuff on swim days; rain gear on rainy days).
  They count: the morning is only finished when both lists are ticked. Once
  the kid's list is done, the middle shows the parents' next open to-do
  ("noch 2 Sachen · für die Eltern") and **Fertig** ticks that.
- **Weather**: the header shows the current temperature and an icon, fetched
  from [Open-Meteo](https://open-meteo.com) (free, no key). If rain or snow is
  forecast between 07:00 and 17:00 (probability of 40% or more, or any
  precipitation), the header says when it starts (`🌧️ 9° · Regen ab 10:00`)
  and a **Regensachen** item (Schirm, Stiefel, Regenhose, Regenjacke) is added
  to the parents' list. Location and thresholds live in the `WEATHER`
  constant at the top of the script; the default is Zürich (the personal
  link below can set another spot). Without
  internet the chip stays hidden and no rain item is shown.
- **School buddy**: on the days she walks to school with someone, the header
  shows `👭 heute mit <Name>` and the finish screen says
  "Viel Spass in der Schule mit <Name>!". Set via the personal link below.
- Background colour tells the state at a glance: blue while on track, yellow
  in the last minute, pulsing red when overdue.
- Beeps every 5 seconds while overdue, plays a chime when the morning starts,
  and a happy jingle when a task is marked done.
- Progress is saved in the browser for the day, so a reload or an accidental
  close doesn't lose it. It resets automatically the next day.
- **Fertig** ticks the task shown in the middle. **Hoppla, rückgängig** unticks
  the one ticked most recently, on either list. **Neu starten** clears the day.
- The finish screen says "Hier ist dein Sticker!", shows a sticker emoji and "Viel Spass in der Schule!" below. It hands out
  a sticker emoji. There are 108 of them, given out in a fixed order, one new
  one per morning, so none repeats for over three months.

## Routine

| Budget | Task                 |
| ------ | -------------------- |
| 13 min | Milch trinken        |
| 8 min  | Zähne putzen         |
| 4 min  | Haare kämmen         |
| 10 min | Anziehen             |
| 5 min  | Schuhe und Jacke     |
| 3 min  | Aufs WC gehen        |
| 2 min  | Tschüss sagen        |

Parents: Znüni vorbereiten, Wasserflasche füllen, Badesachen einpacken
(swim days), Regensachen (rainy days).

Badesachen shows every Friday by default. If swimming is only on some
Fridays, list the dates in the personal link below.

## Personal settings

This repo and the GitHub Pages site are public, so names, swim dates and the
location are not in the code. They go in the hash of a link, which the
browser never sends to a server, and the app remembers them on the device:

```
https://bersling.github.io/getready/#buddy=Name&buddyDays=1,4,5&swim=2026-08-28,2026-09-11&lat=47.4&lon=8.5
```

| Key         | Meaning                                                    |
| ----------- | ---------------------------------------------------------- |
| `buddy`     | who she walks to school with                               |
| `buddyDays` | on which weekdays (1 = Monday ... 5 = Friday)              |
| `swim`      | swim days, `YYYY-MM-DD`, comma separated                   |
| `lat`,`lon` | where to get the weather for                               |

Open the link once on the device (or add it to the home screen). Opening a
link with a new hash replaces all saved settings, so always send the full set.
Keep the real link out of this repo; `personal-link.txt` is git-ignored for
that.

To change it, edit `START`, `END` and the `TASKS` array at the top of the
script in `index.html`. If the budgets don't add up to the window, they're
scaled to fit (with a console warning). Keep each task's `id` stable;
progress and the saved drag order are keyed by it.

The parents' list is the `PARENTS` array right below. Add `days: [5]` to an
item to show it only on certain weekdays (0 = Sunday, 5 = Friday), or
`dates: ["2026-11-06", ...]` for certain dates only.

The stickers are the `STICKERS` array; add more to the end.

## Practice run

To try it outside the real morning window, set the clock via the URL:

```
index.html?at=07:04            # pretend it is 07:04, real speed
index.html?at=07:04&speed=20   # same, but 20x faster (whole morning in ~3 min)
index.html?at=07:26:30         # seconds work too
```

Practice runs keep their progress separately from the real morning (the drag
order and sticker counter are shared).
