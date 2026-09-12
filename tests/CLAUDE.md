# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Scope: the automated test suites. Three separate mechanisms live here, plus a fourth (script checkers) in
`/utils`. See the repository root `CLAUDE.md` for the full build/test command set.

## The three kinds of test

| Kind | Where | Runs via | What it can reach |
|---|---|---|---|
| Unit | `unit/src/test_*.cpp` (Catch2) | `ctest --preset <preset>-test` | Classes with no external dependencies |
| Benchmark | `BENCHMARK` blocks inside the same unit files | `ctest --preset <preset>-benchmark` | Same |
| Integration | `integration/config/plugins/integration-tests/data/tests/*.txt` | `ctest --preset <preset>-integration` | The whole running game |

Prefer a unit test when coverage would be equal - it is faster and far easier to debug.

## Unit tests (Catch2)

Both unit-test files and integration-test `.txt` files **must be registered in `tests/CMakeLists.txt`**
(`target_sources` and the `INTEGRATION_TESTS` list respectively); `utils/check_cmake.sh` fails CI otherwise.

Start from the template, which already carries the required GPL header and structure:

```bash
cp tests/unit/src/test_template.txt tests/unit/src/test_foo.cpp
```

Conventions (`unit/README.md` is the authority):

- One file per class, named `test_classToBeTested.cpp`; a subdirectory per class once a file gets cumbersome.
- **All test code lives in an anonymous `namespace { ... }`** so fixtures never leak between translation units.
- Prefer scenario language - `SCENARIO` / `GIVEN` / `WHEN` / `THEN` - over `TEST_CASE` / `SECTION`.
- `CHECK` / `CHECK_FALSE` for probing (execution continues on failure); `REQUIRE` / `REQUIRE_FALSE` for validity
  assertions that should abort the block.
- Include `es-test.hpp`, then only the header under test, then system headers.
- Helpers live in `unit/src/helpers/` (e.g. `datanode-factory.cpp` builds `DataNode`s from literal text - the
  usual way to test a `Load()` method) with headers in `unit/include/`.

**The hard limitation**: Endless Sky has no dependency injection, so anything touching `GameData`, SDL, or other
external state is not unit-testable. The binary is built with LTO, which lets *some* methods of dependency-heavy
classes be tested as long as the tested path never actually reaches the dependency. If a method calls into
`GameData`, it needs an integration test instead.

Running a subset - the test binary is plain Catch2, so its own filtering works:

```bash
ctest --preset <preset>-test                        # all unit tests
ctest --preset <preset>-test -V                     # with output
build/<preset>/Debug/EndlessSkyTests "Angle*"       # one Catch2 test by name
build/<preset>/Debug/EndlessSkyTests --list-tests
```

CTest registers exactly two tests here: `unit` and `benchmark` (the latter runs the binary with the `[!benchmark]`
tag). Per-test granularity comes from the Catch2 binary, not from `ctest -R`.

## Integration tests

These drive the real game binary through scripted input. The runner scripts (`IntegrationTests.cmake`,
`RunIntegrationTest.cmake`, `run_tests.sh`, `run_tests_headless.sh`) live here; the tests themselves are **game
data files**, written in the same DSL as `data/` and loaded as a plugin from `integration/config/plugins/`.

A test is a `test "Name"` node with `status`, `description`, and a `sequence` of steps - `inject` a savegame,
`call` a shared routine from `tests_common.txt`, `navigate`, `assert` on conditions:

```
test "Landing in a system with multiple planets"
	status active
	description "Test if a ship can land on different planets in a single system."
	sequence
		inject "Three Earthly Barges Save"
		call "Load First Savegame"
		navigate
			"travel destination" Mars
		call "Land"
		assert
			"flagship planet: Mars" == 1
```

The C++ side of this framework is `source/test/` (`Test`, `TestContext`, `TestData`).

```bash
ctest --preset <preset>-integration                        # Linux only
ctest --preset <preset>-integration-debug -N               # list tests (any OS)
ctest --preset <preset>-integration-debug -R <name>        # debug one test (any OS)
```

**Limitations**: the framework can supply commands and keyboard input only - no mouse input, and tests can only
check conditions the game already exposes. `retryable_issues.txt` lists known-flaky failure signatures.
