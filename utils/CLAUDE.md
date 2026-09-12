# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Scope: the standalone checker and release scripts. These are the "single script checkers" referred to in
`tests/README.md` - the cheapest tests in the project, and the ones CI fails on most often. All paths below are
relative to the repository root; every script expects to be run from there.

## Checkers (these gate every PR)

| Script | Checks | Needs |
|---|---|---|
| `check_code_style.py` | C++ formatting, copyright headers, include order (`source/`, `tests/unit/`) | `pip install regex` |
| `check_content_style.py` | Game-data formatting and prose (`data/`, integration-test data) | `pip install regex` |
| `check_cmake.sh` | Every source/test file is registered in the matching `CMakeLists.txt` | bash |
| `check_shaders.sh` | GLSL in `shaders/` compiles as **both** OpenGL 3.0 (`#version 130`) and OpenGL ES 3.0 (`#version 300 es`) | `glslang-tools` |
| `check_copyright.py` | The Debian-format `copyright` file parses strictly | `pip install python-debian` |

```bash
python ./utils/check_code_style.py
python ./utils/check_content_style.py
python ./utils/check_content_style.py -a      # auto-correct where possible - review the diff
./utils/check_cmake.sh
./utils/check_shaders.sh
```

Notes that matter when using them:

- **Only `check_content_style.py` can fix anything** (`-a`/`--auto-correct`, and not for every rule). The C++
  checker reports line and reason only.
- `check_content_style.py` is driven by `contentStyle.json` - data roots, indentation rules, the copyright header
  regex, and every prose regex live there, not in the script. `--config-help` documents the schema; `--files` /
  `--add-files` narrow the scan to specific paths. Exit code **4** means "ran fine, found errors" (distinct from
  1 = unknown error, 2 = bad config, 3 = bad option).
- `check_shaders.sh` prepends a `#version` line to a temp copy of each shader, so a shader must be written
  *without* its own version directive and must be valid under both GL and GLES profiles. `.gl` / `.gles`
  suffixed files are checked against only their own profile.
- `check_cmake.sh` walks `source/`, `tests/unit/` and the integration-test data folder and greps the
  `CMakeLists.txt` files for each filename. Exit 1 = missing source entry, 2 = missing test entry.

## Release / packaging scripts

- **`set_version.sh <version>`** - stamps a version string (or a commit hash, which it renders differently) across
  the files that carry it.
- **`cd_update_versions.sh`** - CI's pre-build step: sets the version to the current commit hash and splices a
  build stamp (`hash`, UTC build time, last author and subject) into `credits.txt`. Run in every CI build job, so
  don't be surprised by `credits.txt` churn in CI logs.
- **`build_appimage.sh <build-dir>`** - packages a Linux AppImage; `OUTPUT` and `ARCH` env vars control naming.
- **`vcpkg_bootstrap.cmake`** - pulled in by the CMake configure step when `-DES_USE_VCPKG=ON` builds the
  dependencies from source.

## Content tool

**`korath-cipher.py`** is not a checker - it implements the in-universe Korath language cipher (Indonesian text
in on stdin, Exile and Efreti renderings out) for writing Korath dialogue. Runs on Python 2.7+.

## Editing these scripts

Per `.editorconfig`, `*.py` here uses **tab** indentation and `*.sh` uses **2 spaces** - the opposite of the usual
defaults, and editorconfig-checker enforces it in CI. The Python scripts use the third-party `regex` module, not
the stdlib `re`, for variable-width lookbehind; keep that import.
