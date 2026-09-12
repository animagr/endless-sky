# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Scope: the C++ game engine (~370 files, ~97k LOC). See the repository root `CLAUDE.md` for build commands and
the fork's relationship to upstream.

## Registering files with the build

**Every `.h` and `.cpp` under `source/` except `main.cpp` must be listed in `source/CMakeLists.txt`**, by path
relative to `source/` (e.g. `text/Format.cpp`), in the `target_sources(EndlessSkyLib PRIVATE ...)` block. Adding a
file without registering it is the single most common way to break the build; `utils/check_cmake.sh` is the CI job
that catches it.

`EndlessSkyLib` is an OBJECT library. `main.cpp` is deliberately excluded so the same objects can be linked into
both the game (`EndlessSky`) and the test binary (`EndlessSkyTests`).

## Architecture

**Startup** - `main.cpp` parses CLI flags, opens the SDL2/OpenGL window (`GameWindow`), starts
`GameData::BeginLoad()` on a `TaskQueue` (background threads), and shows `GameLoadingPanel` while loading runs.
`GameLoop()` then drives the SDL event loop at 60 fps.

**`GameData` - the universe, as global static state.** It owns a `Set<T>` for every kind of definition (ships,
outfits, systems, planets, governments, missions, fleets, ...), keyed by name, loaded from the text files under
`data/`. Almost everything reads from it: it has the highest fan-in in the codebase (109 files). Two consequences
that shape the design:

- Definitions are handed around as raw pointers into these `Set`s (`const Ship *`, `const System *`), stable for
  the process lifetime. Loading is two-phase - `BeginLoad()` / `FinishLoading()`, then `CheckReferences()` to
  report things referred to but never defined.
- A player's `GameEvent`s *mutate* the loaded universe (`GameData::Change()`). `GameData::Revert()` restores the
  pristine state, and must run when switching players, before the new player's events are applied.

**`UI` + `Panel` - the entire interface.** A `UI` is a stack of `Panel`s. Events are offered to panels top-down
until one handles them; drawing runs bottom-up, so panels can show through. Panels push/pop themselves
(`UI::Push`, `UI::Pop`); a popped panel is deleted at the start of the next `Step()`, so self-`Pop()` is safe.
`MenuPanel` is the main menu, `MainPanel` is the flight view; dialogs, shops and maps are all just panels.

**`Engine` - the simulation, one frame behind the draw.** Motion, collision and AI calculations run on a separate
thread, so the drawn state is always one 1/60s step behind what is being calculated. That lag is invisible but it
means engine state must not be read directly from drawing code - `Engine` hands the graphics thread a prepared
`DrawList`/`BatchDrawList`.

**`PlayerInfo` - everything that gets saved.** The pilot, ships, cargo, accounts, missions, conditions and visited
systems; it is both the save-file serializer and the mutable game state the panels act on.

**`AI`** (largest file, ~4.8k LOC) drives every non-player ship. Per-ship cached decisions live in
`source/ship/ShipAICache`; player fleet commands in `source/orders/`.

**`DataFile` / `DataNode` / `DataWriter` - the text format.** One parser serves game content, plugins, save files
and preferences. A `DataNode` is a line of whitespace-separated tokens plus children nested by indentation; quotes
group words, backticks group text containing quotes. Every loader is a `Load(const DataNode &node)` that walks
`node.Token(i)` / `node.Value(i)` and recurses into children. See `data/CLAUDE.md` for the content side.

**Conditions** (`ConditionsStore`, `ConditionSet`, `ConditionAssignments`, `ConditionEntry`) are the named
integer variables behind mission availability, triggers and the in-game test framework. Some are stored, some are
computed on access ("autoconditions" derived from game state).

### Subdirectories

| Path | Contents |
|---|---|
| `audio/` | `Audio`, `Music`, `Sound` - OpenAL playback, plus `player/` and `supplier/` |
| `comparators/` | Header-only sort predicates (`ByName`, `ByUUID`, `BySeriesAndIndex`, ...) |
| `image/` | `Sprite`, `ImageBuffer`, `ImageSet`, collision `Mask`s, async sprite loading |
| `orders/` | Player fleet command state |
| `shader/` | GL shader wrappers + `DrawList`/`BatchDrawList`; GLSL sources live in `/shaders` |
| `ship/` | Per-ship caches split out of the oversized `Ship` |
| `test/` | The *in-game* test framework driving `tests/integration` - not unit tests |
| `text/` | `Font`, `Format`, `WrappedText`, `Table`, `Utf8`, truncation/alignment |
| `windows/` | Windows-only: console attach, timer resolution, version checks, `WinApp.rc` |

Everything else sits flat in `source/`. The heaviest hubs are `GameData.h`, `Point.h`, `DataNode.h`, `Ship.h`,
`text/Format.h`, `PlayerInfo.h`.

## Style rules enforced by CI

`python ./utils/check_code_style.py` gates every PR and encodes the
[C++ Style Guide](https://github.com/endless-sky/endless-sky/wiki/C++-Style-Guide). It is a regex checker, so it
reports line/reason but cannot fix. The rules that actually bite:

- **Tabs for indentation**, and tabs *only* for indentation - never to align.
- **Hard wrap at 120 characters. Plain ASCII only. LF line endings.**
- **Allman braces** - `{` on its own line after `if`/`for`/`while`/`switch`; `try`/`do` keep `{` on the same line.
- **No space between a control keyword and its paren**: `if(condition)`, not `if (condition)`.
- **Whitespace around binary operators**, after commas, and none just inside parentheses.
- **One statement per line**; a statement starts a line; a semicolon ends it.
- **Reference/pointer binds right**: `const string &name`, `Ship *ship`.
- **GPL copyright header on every file**, in the exact existing shape (`/* Filename.h` ... GPLv3 paragraphs). Copy
  it from a neighbouring file. Multi-line `/* */` comments are allowed *only* for that header.
- **`#pragma once`** in headers, never include guards.
- **Include order**: a `.cpp` starts with its own header, then a blank line, then blocks of `"local"` and
  `<system>` includes separated by blank lines; **alphabetical within each block** (comparing basenames, so
  `"text/Format.h"` sorts as `Format.h`). Forward `class X;` declarations must also be alphabetical.
- `using namespace std;` goes in `.cpp` files (167 of them do) and **never** in a header.
- File-local helpers go in an anonymous `namespace { ... }` - 86 `.cpp` files do this.
- No `(void)` parameter lists.

Compiler flags add teeth: `-Wall -pedantic-errors -Wold-style-cast` (so `static_cast`, never C-style casts) and
`-fno-rtti` in release builds - no `dynamic_cast` or `typeid` in code that must ship.

Run the checker before considering C++ work done:

```bash
python ./utils/check_code_style.py     # needs: pip install regex
./utils/check_cmake.sh                 # every source file registered?
```
