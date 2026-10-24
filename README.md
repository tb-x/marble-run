# Marble Run

A toy marble run on a wooden pegboard. Drag parts onto the board (ramps, a funnel, a trampoline, a bell, a
xylophone ramp), drop marbles from the blue hopper at the top, and watch them roll, bounce and ring their way down.
The goal on each board is to roll through the stars and land a marble in the purple cup at the bottom. The last board
is free play with as many parts as you like.

Eight boards, everything built in code (no picture files), synthesized sounds, and a small home-made 2D physics engine
(no extra library). Play it at [tb-x.github.io/marble-run](https://tb-x.github.io/marble-run/).

It works offline and can be added to the Home Screen: the first visit stores everything it needs, and later visits
start without internet.

## How it plays

- **Placing parts:** drag a part from the tray onto a square, or tap a part to pick it and then tap a square. A
  green ghost shows where it will go (red if the square is taken). The number on each part says how many are left.
- **Changing parts:** tap a part on the board to flip it the other way (ramps, xylophone and trampoline). Drag it to
  move it, or drag it off the board to put it back in the tray. The circle-arrow button puts every part back at once.
- **Dropping marbles:** the big green button (or tapping the hopper) drops a marble. Up to six can roll at once, each
  in its own colour.
- **The parts:**
  - Ramps send the marble sideways, off their low end.
  - The funnel catches a flying marble and drops it straight down.
  - The trampoline always throws the marble up and to one side.
  - The bell rings when hit.
  - The xylophone ramp plays a little tune as the marble rolls down its six coloured bars.
- **Stars and the cup:** stars a marble rolls through stay collected, so it never has to be done in one perfect run.
  The board is won when all the stars are collected and a marble lands in the cup. A marble in the cup before then
  just gets a happy clink and a nudge towards the stars.
- **Gentle failing:** marbles that miss drop into the trough at the bottom with a soft "bloop", and there's no
  limit. A marble that gets stuck fades away after three seconds.
- Some boards have parts already fixed in place (darker wood). They wiggle when tapped but don't move.
- Progress and the mute setting are remembered.

## Running it

```bash
python -m http.server 5232
```

Then open `http://localhost:5232/` (or use the `marble-run` entry in the root launch config).

## Tuning knobs

At the top of the script in `index.html`:

| Knob | What it does | Now |
| --- | --- | --- |
| `COLS`, `ROWS` | size of the pegboard in squares | 6 × 8 |
| `GRAVITY` | how fast marbles fall (squares per second²) | 16 |
| `MARBLE_R` | marble size (a square is 1) | 0.14 |
| `BOUNCE_MIN` | how hard the trampoline always throws | 6.2 (about 1¼ squares high) |
| `MAX_SPEED` | speed limit, so marbles can't pass through parts | 13 |
| `MAX_MARBLES` | marbles rolling at once | 6 |
| `STUCK_SECS` | how long a still marble waits before fading away | 3 s |
| `FRICTION` | speed lost along a surface | 0.0006 per physics step |
| `LEVELS` | each board: hopper column, cup column, stars, parts in the tray, fixed parts | 7 boards + free play |

## Checking the boards

The page has a hook, `window.__mr.simulate(level, parts)`, that runs one marble without drawing anything. During
development every board was brute-forced with it: every square and direction for one part, and every pair of parts.
These are the numbers of two-part solutions (2026-10-24):

| Board | Hopper → cup | Parts | Ways to solve |
| --- | --- | --- | --- |
| 1 | straight down | ramp | dropping alone works (learn the button) |
| 2 | column 1 → 4 | ramp, funnel | 3 with the ramp alone, many with the funnel |
| 3 | 4 → 1, one star | 2 ramps | 155 |
| 4 | 1 → 5 | trampoline, funnel | 144 |
| 5 | 1 → 4, one star | ramp, bell, funnel | 76 |
| 6 | 5 → 0, two stars | 3 ramps, funnel | 6 with two ramps, more with all four parts |
| 7 | 0 → 5, one star | 2 xylophones, funnel | 11 |

Run the same search again after changing the physics knobs, because they move every trajectory.

## Ideas for the next version

- Kids may want to place parts anywhere, not only on squares. Snapping keeps it tidy for now.
- More parts: a seesaw, a spinning wheel, a pipe that carries a marble across, a fan, a lift.
- A faint trail of the last marble's path could help kids see what to change.
