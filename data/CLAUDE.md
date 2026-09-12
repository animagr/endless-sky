# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Scope: the game content - every ship, outfit, system, planet, mission, conversation and UI layout in Endless Sky,
as plain text. No build step: the game parses these files at startup. See `source/CLAUDE.md` for the parser side
and the repository root `CLAUDE.md` for commands.

## The data format

One DSL (`DataFile`/`DataNode`) serves game content, plugins, save files and preferences.

- A node is a line of **whitespace-separated tokens**; its **children are the lines indented one tab deeper**.
- Quotes group words into one token (`"Heavy Laser Turret"`); **backticks** group text that itself contains
  quotes (`` `He said "no."` ``).
- `#` starts a comment.
- The first token is the type (`ship`, `outfit`, `mission`, `system`, `event`, `conversation`, ...), usually
  followed by the name it is keyed under in the matching `GameData` `Set`.

```
ship "Aerie"
	sprite "ship/aerie"
	attributes
		category "Medium Warship"
		"cost" 3500000
	outfits
		"Sidewinder Missile Launcher" 2
```

Names are the linkage - a ship's `outfits` entries, a mission's planet, a system's `object` sprites are all
looked up by string across every file. There is no per-file scope and **load order is not meaningful**: define
things anywhere. Dangling references surface at startup via `GameData::CheckReferences()`, which is exactly what
the parse commands below check.

## Layout

- **Faction/region folders** (`human/`, `hai/`, `korath/`, `remnant/`, `pug/`, `quarg/`, `wanderer/`, `coalition/`,
  `drak/`, `avgi/`, `bunrodea/`, `gegno/`, `iije/`, `incipias/`, `kahet/`, `rulei/`, `sheragi/`, `successors/`,
  `vyrmeid/`) hold that group's ships, outfits, fleets, missions and storylines.
- **`_ui/`** - interface layouts, tooltips, help text, flight checks, landing messages. Edits here change the
  game's chrome rather than its universe.
- **`_deprecated/`** - definitions kept only so old saves and plugins still load. Don't extend these.
- **Root `.txt` files** are the cross-cutting definitions: `map systems.txt`, `map planets.txt`, `governments.txt`,
  `commodities.txt`, `effects.txt`, `hazards.txt`, `starts.txt`, `gamerules.txt`, `persons.txt`, `stars.txt`,
  `series.txt`, `substitutions.txt`, `dialog phrases.txt`, `formations.txt`, `harvesting.txt`, `categories.txt`,
  `globals.txt`, `confusions.txt`.

Art and audio referenced from here live outside this folder, in `/images` and `/sounds`; paths are relative and
extensionless (`sprite "ship/aerie"`).

## Checks that gate a content change

```bash
python ./utils/check_content_style.py         # needs: pip install regex
python ./utils/check_content_style.py -a      # attempt auto-correction (review the diff!)
"./Endless Sky.exe" -p                        # parse all data, report content errors
"./Endless Sky.exe" --parse-assets            # also load every image and sound
```

`check_content_style.py` is config-driven by `utils/contentStyle.json` (it also covers
`tests/integration/config/plugins/integration-tests/data/`), and enforces:

- **Tabs for indentation, indent increasing by at most one level per line**; LF endings; trailing empty line.
- The **GPL copyright header** at the top of every file, `#`-commented, in the exact existing wording - copy it
  from a neighbouring file.
- **ASCII only** - no curly quotes, no typographic apostrophes, no em dashes from a word processor.
- Prose hygiene inside quoted text: single spaces, no space before punctuation, space after punctuation, no
  doubled words, correct `a`/`an`, no whitespace at the start or end of a string.

CI additionally runs **codespell** over `data/` and the changelog (`.codespell.exclude` and
`.codespell.words.exclude` hold the allowlists), and **editorconfig-checker** over the tree.

## Writing content

Upstream's conventions for what content should look and read like are on the wiki
([Style Goals](https://github.com/endless-sky/endless-sky/wiki/StyleGoals),
[Quality Checklist](https://github.com/endless-sky/endless-sky/wiki/QualityChecklist),
[Creating Plugins](https://github.com/endless-sky/endless-sky/wiki/CreatingPlugins) for the full node reference) -
the repository itself documents the format only by example.

Because everything is keyed by name and merged at load, **a self-contained addition is usually better authored as
a plugin** (its own folder of data files loaded from the config directory) than as an edit to these files. For
this fork that also keeps upstream merges clean - see the root `CLAUDE.md`.
