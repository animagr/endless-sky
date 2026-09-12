# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## This is a fork - keep it mergeable with upstream

This checkout is **`animagr/endless-sky`**, a personal fork of the upstream game. Git is wired accordingly:

| Remote | URL | Role |
|---|---|---|
| `origin` | `https://github.com/animagr/endless-sky.git` | The fork. Push here. `master` tracks `origin/master`. |
| `upstream` | `https://github.com/endless-sky/endless-sky.git` | The official repo. Pull from here; never push. |

**The goal for this fork is to stay compatible with upstream so that upstream changes can keep being merged in.**
There is no intention to contribute changes back the other way. Practical consequences for any work done here:

- **Prefer additive and isolated changes** over edits threaded through existing code. New files, new data
  definitions, and new `data/` content as a **plugin** all merge cleanly; rewrites of `AI.cpp` or `PlayerInfo.cpp`
  will conflict with every upstream release.
- **Don't reformat, rename, or reorganize anything you weren't asked to change.** Drive-by cleanups are the main
  source of merge pain in a long-lived fork, and they buy nothing here.
- **Keep upstream's style checks passing** (see below) even though no upstream reviewer will see the code. Staying
  inside their formatting rules is what keeps diffs small and merges mechanical.
- **Keep fork-specific work on its own branches and in its own commits**, so a conflicted merge can be reasoned
  about one change at a time.
- Sync regularly rather than in big batches:
  ```bash
  git fetch upstream
  git merge upstream/master        # or: git rebase upstream/master
  git push origin master
  ```
- Note that **upstream's `docs/CONTRIBUTING.md` forbids AI-generated or AI-assisted contributions.** Work done
  here with Claude is fine for the fork, but it must not be submitted upstream as a pull request.

Several things in this checkout are additions made in the fork, tracked here but absent from upstream. None of
them exist in any upstream path, so they never conflict when merging `upstream/master`:

- **`wiki/`** - a snapshot of upstream's community wiki (56 pages, commit `ef5f768`, 2026-09-07), which is where
  the game-data format is actually specified. See the note at the end of this file.
- **`.codemapy/`** - a generated code map. `.codemapy/summary.md` is a fast orientation to hubs, entry points and
  file sizes.
- **`Endless Sky Guide.md`** - a player-facing strategy guide written against this checkout (version 0.11.3). It
  is about *playing* the game, not developing it; don't treat it as project documentation, and note that its
  numbers are pinned to the source as of when it was written.
- **`Endless Sky Codebase Overview.html`** - an architecture tour of this codebase for a reader new to it.

## What this is

Endless Sky is a 2D sandbox space-exploration game: C++ (~97k LOC under `source/`) on SDL2 + OpenGL, with the
entire universe - ships, outfits, systems, missions, dialogue, UI layouts - defined in plain text files under
`data/` rather than compiled in. GPLv3; art under assorted permissive licenses tracked in `copyright`.

## Build

CMake with presets (requires CMake 3.21+ for the preset workflow; 3.19 minimum otherwise). Dependencies come from
the system, or from vcpkg with `-DES_USE_VCPKG=ON`. On **Windows** the presets are `clang-cl`, `mingw`, and
`mingw32`; also available are `linux`, `linux-gles`, `linux-armv7`, `macos`, `macos-arm`.

```bash
cmake --preset <preset>                                    # configure (once)
cmake --build --preset <preset>-debug                      # build game + tests
cmake --build --preset <preset>-debug --target EndlessSky  # build only the game
cmake --list-presets                                       # see them all
```

The executable lands in `build/<preset>/Debug/` (or `Release/` for `<preset>-release`). Full platform-by-platform
dependency instructions are in [docs/readme-developer.md](docs/readme-developer.md).

## Test

```bash
ctest --preset <preset>-test                     # unit tests (Catch2)
ctest --preset <preset>-benchmark                # benchmarks
ctest --preset <preset>-integration              # integration tests (Linux only)
ctest --preset <preset>-integration-debug -N     # list integration tests (any OS)
ctest --preset <preset>-integration-debug -R <name>   # run/debug one (any OS)
```

CTest exposes only `unit` and `benchmark` as targets; to run a **single unit test**, filter with the Catch2 binary
directly: `build/<preset>/Debug/EndlessSkyTests "Angle*"` (`--list-tests` to browse). See
[tests/CLAUDE.md](tests/CLAUDE.md).

## Lint and validate

There is no formatter - the checkers report, and only the content one can fix. Run the ones matching what you
touched; CI runs all of them.

```bash
python ./utils/check_code_style.py        # C++ style/headers/includes   (pip install regex)
python ./utils/check_content_style.py     # data file style/prose        (pip install regex)
./utils/check_cmake.sh                    # every source file registered in CMakeLists
./utils/check_shaders.sh                  # GLSL valid as GL 3.0 and GLES 3.0 (needs glslang-tools)
```

Data changes are additionally validated by running the game itself:

```bash
"./Endless Sky.exe" -p                                            # parse data, report content errors
"./Endless Sky.exe" -p --config "tests/integration/config"        # parse integration-test data
"./Endless Sky.exe" --parse-assets                                # also load every image and sound
```

Other useful runtime flags: `-r <path>` (resources dir), `-c <path>` (config dir), `-d` (debug features),
`--tests` (list in-game tests), `--test <name>`, `--rng-seed <seed>`.

## Layout

| Path | What | Detail |
|---|---|---|
| `source/` | The C++ engine | [source/CLAUDE.md](source/CLAUDE.md) |
| `data/` | The whole game universe, as text | [data/CLAUDE.md](data/CLAUDE.md) |
| `tests/` | Unit, benchmark and integration suites | [tests/CLAUDE.md](tests/CLAUDE.md) |
| `utils/` | Style checkers and release scripts | [utils/CLAUDE.md](utils/CLAUDE.md) |
| `shaders/` | GLSL sources; C++ wrappers live in `source/shader/` | |
| `images/`, `sounds/` | Assets referenced by extensionless relative path from `data/` | |
| `docs/` | `readme-developer.md` (build), `CONTRIBUTING.md` (upstream's policies) | |
| `overlays/`, `steam/`, `installer/`, `resources/`, `icons/` | Packaging and per-platform release config | |
| `copyright`, `credits.txt`, `changelog` | Asset licensing, credits, release history | |

## Cross-cutting conventions

- **`.editorconfig` is authoritative and enforced in CI** (editorconfig-checker). Notably: **ASCII** and **LF**
  everywhere; **tabs** in C++, data files, `CMakeLists.txt`, shaders, JSON *and Python*; **2 spaces** in shell,
  YAML, XML and the changelog.
- **Every file carries the GPLv3 header** in its language's comment style, checked by both style scripts. Copy it
  from a neighbouring file rather than retyping it.
- **Two `CMakeLists.txt` files must be kept in sync by hand**: every new file under `source/` (except `main.cpp`)
  goes in `source/CMakeLists.txt`, and every new file under `tests/unit/` plus every new integration-test `.txt`
  goes in `tests/CMakeLists.txt`.
- **The data format is specified in `wiki/`, not in the source tree.** The repository documents the content
  format only by example; upstream's wiki is the actual reference, and a snapshot of it is checked in here.

## The `wiki/` snapshot

`wiki/` is a copy of <https://github.com/endless-sky/endless-sky/wiki> (56 pages, upstream commit `ef5f768`,
2026-09-07), flattened to plain Markdown so it can live in this fork's history. **It is upstream's community
documentation, not work authored in this fork** - treat it as a vendored reference and don't edit the pages.

Reach for it before guessing at content syntax. The pages that carry the node reference the source tree lacks:

| Page | Covers |
|---|---|
| `DataFormat.md` | The token/indentation grammar itself |
| `CreatingMissions.md`, `CreatingOutfits.md`, `CreatingShips.md` | The three biggest node vocabularies |
| `MapData.md` | Systems, planets and how the galaxy map is defined |
| `LocationFilters.md`, `Player-Conditions.md` | The filter syntax and the condition namespace from the root file |
| `CreatingPlugins.md` | Plugin layout, and `ImageFormats.md` / `SpriteData.md` for the sprite filename rules |
| `C++-Style-Guide.md` | The rules `utils/check_code_style.py` enforces mechanically |

It is a snapshot, so it drifts. To refresh it:

```bash
rm -rf wiki && git clone --depth 1 https://github.com/endless-sky/endless-sky.wiki.git wiki && rm -rf wiki/.git
```
