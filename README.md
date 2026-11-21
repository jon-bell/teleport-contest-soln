# NetHack 5.0.0 — C to JavaScript port

A JavaScript port of **NetHack 5.0.0_Release**, written for the Teleport Coding
Challenge. It aims at exact parity with the C original: the same random-number
draws in the same order, and the same 24x80 screen at every input boundary.

## Running it

Requires **Node >= 22** (the port uses JSON import attributes).

```bash
# score against the public sessions
node frozen/ps_test_runner.mjs sessions/

# or via the contest script, which also overlays the judge-owned fixture
bash frozen/score.sh

# interactive playability check
bash frozen/play.sh
```

There is no build step and there are no dependencies. `js/` imports nothing
outside itself — no Node builtins, no filesystem, no network — so the same
modules run in Node and in a browser. To play in a browser, open `index.html`.

## Layout

```
js/              the port: ES modules, one per area of the C source
js/dat_bundle.js NetHack's data files (special-level Lua, data.base, help text)
                 as a module
frozen/          judge-owned fixture: ISAAC64, the terminal model, storage and
                 the scoring runner; overwritten at scoring time, do not edit
sessions/        the public recorded sessions
```

`js/isaac64.js`, `js/terminal.js` and `js/storage.js` are copies of the frozen
originals and are replaced from `frozen/` on every scoring run.

## Structure

Functions keep their C names, control flow and call order, and comments cite
the C they correspond to (`C ref: save.c:62`), including the places where C has
a bug that the port reproduces deliberately.
