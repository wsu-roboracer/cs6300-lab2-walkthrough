# CS 6300 &middot; Lab 2 Walkthrough

Interactive ten-step walkthrough of **Lab 2 — Automatic Emergency Braking** for CS 6300
Autonomous Racing at Weber State University. It reads the AEB skeleton a piece at a time,
then shows what changes when the same node drives a real RoboRacer.

Live: **https://wsu-roboracer.github.io/cs6300-lab2-walkthrough/**

## Why this repo exists

The walkthrough ships with the [course site](https://github.com/wsu-roboracer/cs6300-course-site)
as `docs/_static/steppers/lab2.html`. This repo is a standalone public copy so it can be
iterated on and reviewed at its own URL before the change lands in the course site. The
course site is the destination; this repo is the workbench. Same arrangement as
[cs6300-lab1-walkthrough](https://github.com/wsu-roboracer/cs6300-lab1-walkthrough).

The only difference between the two copies: the footer links to the Lab 2a and Lab 2b pages
are relative paths on the course site and absolute URLs here.

## The ten steps

Steps are tagged by phase — concept, code, simulator, car — so it is clear when the reader
has stopped reading code and started running it.

| Step | What it covers |
|------|----------------|
| 1 &middot; Why iTTC | Distance alone does not tell you whether to brake |
| 2 &middot; The skeleton | Sixty lines, eight of them the student's |
| 3 &middot; Wire it up | One publisher, two subscriptions |
| 4 &middot; Speed | `odom_callback` remembering current speed |
| 5 &middot; Beam angles | Every reading in the `LaserScan` array points somewhere |
| 6 &middot; iTTC | Range over closing speed, for every beam at once |
| 7 &middot; Bad data | Real scans contain `inf` and `nan` |
| 8 &middot; Brake | Publish zero speed, and keep publishing it |
| 9 &middot; In the simulator | Lab 2a: prove it without anything to break |
| 10 &middot; On the car | Lab 2b: one topic changes, and everything else gets harder |

Each step pairs the explanation with the relevant slice of the skeleton, so the code on
screen is only ever the part under discussion. The code shown is the Python skeleton; a C++
skeleton with the same structure ships beside it in the lab.

## Controls

| Control | Action |
|---------|--------|
| **Step tabs** | Jump straight to any step |
| **&laquo; First** / **Last &raquo;** | Jump to the start or the end |
| **&lsaquo; Prev** / **Next &rsaquo;** | One step at a time |
| **&#9654; Play** | Auto-advance through the steps |

Arrow keys step forward and back.

## Files

```
cs6300-lab2-walkthrough/
├── index.html    # Everything: layout, CSS, step content, figures
└── README.md
```

No build step, no dependencies. Open `index.html` in any modern browser. The step content
lives in the `STEPS` array in the inline script — editing a step means editing one object.

Light and dark are both supported: the palette follows `prefers-color-scheme`, with a
`data-theme` attribute override.

## Deploying

GitHub Pages serves the `main` branch root. Push to `main` and the live URL updates within
about a minute. No CI workflow needed.

## Embedding in Canvas

```html
<iframe
  src="https://wsu-roboracer.github.io/cs6300-lab2-walkthrough/"
  width="1040"
  height="900"
  frameborder="0"
  style="max-width: 100%;">
</iframe>
```

## License

Course teaching material &mdash; free to use and adapt for educational purposes.
