# CS 6300 &middot; Lab 2 Walkthrough

A code stepper for **Lab 2 — Automatic Emergency Braking** in CS 6300 Autonomous Racing at
Weber State University. It runs the AEB node one line at a time against the seven lidar scans
`check_lab2.py` actually grades: the code on the left with the executing line highlighted, the
scan and the beam being looked at in the middle, and every variable's value at that moment on
the right.

Live: **https://wsu-roboracer.github.io/cs6300-lab2-walkthrough/**

Built to the same pattern as the CS 1400 and CS 2420 steppers, and as its
[Lab 1 counterpart](https://github.com/wsu-roboracer/cs6300-lab1-walkthrough).

## Why this repo exists

The stepper ships with the [course site](https://github.com/wsu-roboracer/cs6300-course-site)
as `docs/_static/steppers/lab2.html`. This repo is a standalone public copy so it can be
iterated on and reviewed at its own URL before the change lands in the course site. The course
site is the destination; this repo is the workbench.

The only difference between the two copies: the footer links to the Lab 2a and Lab 2b pages
are relative paths on the course site and absolute URLs here.

## It computes, it does not assert

The page builds all seven of the grader's scans the same way `tests/check_lab2.py` builds them
— 1080 beams over 270°, `wall_ahead`, `hallway`, `object_beside`, `open_space`, and the same
`inf` every 7th beam and `nan` every 11th — then runs the real iTTC arithmetic over every
beam. Every number on screen is computed, including the verdict. At the 0.4 s threshold the
code pane shows, the result is:

| Scan | Speed | min iTTC | Must | Does |
|------|-------|----------|------|------|
| wall 0.5 m ahead | 2.5 m/s | 0.20 s | brake | brakes |
| the same wall with `inf` and `nan` | 2.5 m/s | 0.20 s | brake | brakes |
| wall 2.0 m ahead | 10 m/s | 0.20 s | brake | brakes |
| wall 1.0 m ahead | 0.25 m/s | 4.00 s | hold | holds |
| object 0.4 m to the left | 2.5 m/s | 5.11 s | hold | holds |
| hallway, walls 1.5 m each side | 1.0 m/s | 3.00 s | hold | holds |
| open space, every beam `inf` | 2.5 m/s | inf | hold | holds |

Tab 6 renders that table live, and the self-check at the bottom of the page fails loudly if
any row ever stops agreeing with the grader.

## How it is laid out

| Column | What it holds |
|--------|---------------|
| **The code** | `safety_node.py` with its TODOs written out, executing line highlighted, a yellow band under it carrying that line's arithmetic with this scan's real numbers. Every line is clickable. |
| **What the lidar sees** | The car from above, the scan's shape, and the sampled beams colored by what the current line knows about them. Under it, a table of those beams: index, angle, range, closing rate, iTTC. |
| **What the program is holding** | `self.speed`, the masks, the running minimum, and the current beam's five values. |

The theory is **not** on the page. Click any line and a box opens with what that line is for,
the math behind it, and what breaks without it — tagged `why`, `trap`, `math`, `syntax` or
`note`. Nineteen lines have one, and they are where the interesting content lives: why a
`nan` poisons `np.min` for the entire scan, why `cos` is the only line that knows a wall
beside you is harmless, why starting the `ittc` array at zeros brakes forever.

## The tabs

| Tab | Walks |
|-----|-------|
| 1 &middot; `__init__` | The three topics and the two pieces of state |
| 2 &middot; `odom_callback` | Two lines, and why the whole thing is dead without them |
| 3 &middot; `scan_callback` | The real walk: ranges, angles, closing rate, both masks, the division beam by beam, then `np.min` against the threshold |
| 4 &middot; The brake | Why zero speed is the brake, and why one message is not braking |
| 5 &middot; On the car | The three things that change for Lab 2b, each of which fails silently |
| 6 &middot; What is graded | The seven scenarios, computed, next to what the grader demands |

Stepping all 1080 beams would take an hour, so the division is walked over a handful chosen to
include the beams that decide the outcome: dead ahead, the smallest iTTC in the scan, the first
`inf`, the first `nan`, 90° to the side, and both edges. **Next beam &raquo;&raquo;** skips
between them.

## Controls

| Control | Action |
|---------|--------|
| **Tabs** | Switch which part of the node is being walked |
| **The grader's scans** | Re-run the walk against any of the seven scenarios |
| **&laquo; First** / **Last &raquo;** | Jump to the start or the end |
| **&lsaquo; Prev** / **Next &rsaquo;** | One line at a time |
| **&#9654; Play** | Auto-advance |
| **Next beam &raquo;&raquo;** | Skip to the next beam in the loop |
| **Click a line** | Open its popup; click again to close |

Arrow keys step, space plays, Home and End jump to the ends. `#scan`, `#brake`, `#car` and
`#graded` open a tab directly.

## Self-check

The page checks itself on load. Because the scans are generated and the arithmetic is really
run, the assertions are about behavior rather than formatting: at this threshold the code
brakes for exactly the three scans that demand it and holds for the four that forbid it, every
iTTC shown is range over closing rate, nothing behind the car is ever counted as closing, the
beam that decides the outcome is always among the beams the walk visits, all nineteen popups
land on the line they describe, and the scenario points add up to the 80 `check_lab2.py`
scores. Change the threshold and the page will tell you which checks you just broke.

## Files

```
cs6300-lab2-walkthrough/
├── index.html    # Everything: layout, CSS, code panes, line popups, scan generators, walk builders
└── README.md
```

No build step, no dependencies. Open `index.html` in any modern browser.

The scan generators are near the top (`wallAhead`, `hallway`, `objectBeside`, `openSpace`,
`withBadReadings`) and are deliberate transliterations of the grader's. `THRESHOLD` is one
constant. To edit a walk, find its `build*` function; to edit the theory, find the `info(...)`
call for that line. Line numbers are looked up by text through `L(key, needle)`, never typed
in.

The code shown is the Python skeleton. The C++ skeleton has the same structure, the same three
topics and the same arithmetic.

## Deploying

GitHub Pages serves the `main` branch root. Push to `main` and the live URL updates within
about a minute. No CI workflow needed.

## Embedding in Canvas

```html
<iframe
  src="https://wsu-roboracer.github.io/cs6300-lab2-walkthrough/"
  width="100%"
  height="950"
  frameborder="0">
</iframe>
```

## License

Course teaching material &mdash; free to use and adapt for educational purposes.
