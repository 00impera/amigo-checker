# AMIGO Draw Generator — `amigo-checker.html`

Standalone HTML page that simulates an AMIGO draw (12 numbers out of
28: 7 "blue" + 5 "yellow"), running Python directly in the browser via
**PyScript** (Pyodide, runs locally, no server / backend).

## What it does

1. On page load, the Python engine (Pyodide) is downloaded through
   `core.js` / `core.css` from `pyscript.net`. While that's loading,
   the button is disabled and shows "Loading...".
2. Once the engine is ready, the button becomes active:
   **"Generate draw"**.
3. On click, the Python function `generate_amigo_draw()`:
   - starts from a pool of 28 numbers (1–28)
   - picks **7 unique random numbers** → the "blue" group
   - from the remaining 21 numbers, picks **5 unique random numbers**
     → the "yellow" group
   - each group is displayed **sorted in ascending order** (not in
     the actual "draw order", which is random anyway)
4. The result is written to the page, with blue numbers colored
   `#0099ff` and yellow numbers colored `#ffcc00`.

## Why the code looks the way it does

- **`random.sample(numbers, 7)`** — guarantees 7 distinct numbers,
  no repeats, from the pool.
- **`remaining = [...]`** — removes the numbers already picked, so
  the second `random.sample` (for yellow) can't repeat a number
  already drawn for blue.
- **`create_proxy(amigo_run)`** — a Pyodide technical requirement:
  without it, the Python function bound to the click event gets
  automatically destroyed as soon as the script finishes running
  once, and the first click would throw an error
  (`borrowed proxy was automatically destroyed`).
- **`<script type="py">`** — the current PyScript syntax
  (2024.1.1+); the old `<py-script>` + `pyscript.interpreter.run(...)`
  syntax is deprecated and no longer works with `core.js`.

## What it does NOT do

- It does not connect to real AMIGO results (no ticket checking,
  no payout calculation) — it's just an independent random
  generator, for fun / testing.
- It does not keep a history of generated draws — every click is
  independent, nothing is saved.

## How to run it

Open the file directly in a browser (double-click, or
`file:///.../amigo-checker.html`), or host it statically
(e.g. GitHub Pages). It needs an internet connection on first load
to download `core.js`/`core.css` and the Pyodide runtime.
