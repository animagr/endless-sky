# codemapy summary

- Root: `C:\Claude\Testing\endless-sky`
- Generated: `2026-09-12T15:31:04.096945+00:00`
- Git commit: `722016003fcc`
- Files: 529
- LOC: 104463
- Internal dependencies: 2661
- External references: 1039
- Symbols: 3582
- Dependency cycles: 0

## Languages

- C Header: 258 files
- C++: 258 files
- Shell: 7 files
- Python: 4 files
- C++ Header: 2 files

## Directory Overview

- `source/`: 481 files, 96849 loc (C Header)
- `tests/`: 39 files, 6166 loc (C++)
- `utils/`: 9 files, 1448 loc (Shell)

## Entry Points

- `source/main.cpp` (defines main())
- `tests/unit/src/test_main.cpp` (defines main())
- `utils/check_code_style.py` (__main__ guard)
- `utils/check_content_style.py` (__main__ guard)
- `utils/check_copyright.py` (__main__ guard)

## Top Hubs

- `source/GameData.h`: fan-in 109, fan-out 6, 173 loc
- `source/Point.h`: fan-in 79, fan-out 0, 116 loc
- `source/DataNode.h`: fan-in 78, fan-out 0, 83 loc
- `source/Ship.h`: fan-in 64, fan-out 18, 689 loc
- `source/text/Format.h`: fan-in 61, fan-out 0, 163 loc
- `source/PlayerInfo.h`: fan-in 55, fan-out 14, 529 loc
- `source/Color.h`: fan-in 50, fan-out 0, 54 loc
- `source/System.h`: fan-in 48, fan-out 6, 246 loc
- `source/Screen.h`: fan-in 48, fan-out 1, 55 loc
- `source/UI.h`: fan-in 45, fan-out 2, 95 loc

## Largest Files

- `source/AI.cpp`: 4754 loc
- `source/PlayerInfo.cpp`: 4736 loc
- `source/Ship.cpp`: 4306 loc
- `source/Engine.cpp`: 2752 loc
- `source/Mission.cpp`: 1609 loc
- `source/PreferencesPanel.cpp`: 1440 loc
- `source/ShopPanel.cpp`: 1286 loc
- `source/MapPanel.cpp`: 1256 loc
- `source/OutfitterPanel.cpp`: 1232 loc
- `source/System.cpp`: 1068 loc

## Dependency Cycles

- None detected

## Symbols by Kind

- function: 3456
- enum: 71
- struct: 18
- method: 17
- class: 16
- namespace: 2
- type: 2

## External References

- `string`: 149
- `vector`: 127
- `algorithm`: 105
- `map`: 77
- `cmath`: 69
- `memory`: 52
- `set`: 49
- `cstdint`: 36
- `utility`: 35
- `filesystem`: 23
- `functional`: 23
- `list`: 22
- `sstream`: 19
- `limits`: 18
- `stdexcept`: 14
- `cassert`: 13
- `mutex`: 13
- `optional`: 12
- `SDL2/SDL.h`: 11
- `cstring`: 10

## Documentation Files

- `README.md` (30 loc)
- `credits.txt` (532 loc)
- `keys.txt` (35 loc)
- `license.txt` (553 loc)
- `.github/pull_request_template.md` (36 loc)
- `data/categories.txt` (48 loc)
- `data/commodities.txt` (1168 loc)
- `data/confusions.txt` (98 loc)
- `data/dialog phrases.txt` (148 loc)
- `data/effects.txt` (498 loc)
- `data/formations.txt` (272 loc)
- `data/gamerules.txt` (37 loc)
- `data/globals.txt` (36 loc)
- `data/governments.txt` (2094 loc)
- `data/harvesting.txt` (536 loc)
- ... and 225 more

## Project Metadata Files

- `CMakeLists.txt`: project-metadata, 337 loc, 16437 bytes
- `CMakePresets.json`: project-config, 974 loc, 23477 bytes
- `source/CMakeLists.txt`: project-metadata, 499 loc, 9323 bytes
- `tests/CMakeLists.txt`: project-metadata, 113 loc, 5767 bytes

## Other Files

- `.png`: 6278 files, 252964862 bytes
- `.jpg`: 657 files, 95107900 bytes
- `.wav`: 244 files, 39702136 bytes
- `.mp3`: 3 files, 6485596 bytes
- `(no extension)`: 7 files, 712585 bytes
- `.bmp`: 2 files, 180360 bytes
- `.ico`: 1 file, 103528 bytes
- `.jpeg`: 1 file, 79352 bytes
- `.xml`: 1 file, 53982 bytes
- `.yml`: 9 files, 33941 bytes
- ... and 11 more extensions

## Artifact Guide

- `context.json`: full scan data - files, imports, dependency edges, cycles, per-file symbol counts
- `symbols.json`: per-file definitions plus an `index` mapping each defined name to its locations
- `hubs.json`: modules ranked by fan-in / fan-out
- `manifest.json`: generation metadata, artifact byte sizes, and `git_commit` for staleness checks
- `report.html`: visual file tree, treemap, dependency graph, and insights (entry points, hubs, cycles, per-file symbols) for humans
