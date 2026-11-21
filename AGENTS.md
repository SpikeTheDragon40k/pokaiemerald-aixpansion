# AGENTS.md — guide for AI coding agents

This file is for LLM coding agents (Claude Code, Codex, Copilot, Cursor, Aider, …) and the people driving them. Read it before changing code. Humans are welcome to read it too.

This repository is an **independent project**: a GBA ROM-hack base that started as a fork of RHH's `pokeemerald-expansion` (itself built on [pret's `pokeemerald`](https://github.com/pret/pokeemerald) decompilation) and no longer syncs with it. It accepts AI-assisted contributions under the rules in [`CONTRIBUTING.md`](CONTRIBUTING.md#ai-assisted-contributions-policy). **Only open PRs and issues on this repository**, never on RHH's: they ban LLM-assisted work.

## Golden rules

1. **Prove it works.** Build the ROM and run the test suite before you say a change is done. If you could not run them, say so plainly; never claim they pass.
2. **Write tests first or alongside.** Every new or changed battle mechanic needs tests in `test/`. Never delete, skip, or loosen an existing test to make it pass. If a test looks wrong, explain why in the PR instead.
3. **Be minimally invasive.** Change as little vanilla code as possible. Put new behaviour behind a config in `include/config/` when users might want the old behaviour. Do not change the save layout (`include/global.h`, `SaveBlock1/2`) without discussion.
4. **Don't invent mechanics.** Pokémon mechanics differ by generation. Base behaviour on the actual games (Bulbapedia, Showdown source, research threads) and cite the source in the PR. Use the generation configs (`B_* GEN_LATEST`, etc.) instead of hardcoding one generation.
5. **Follow [`docs/STYLEGUIDE.md`](docs/STYLEGUIDE.md).** 4-space indent, Allman braces, `PascalCase` functions, `camelCase` locals, `gGlobals`, `sStatics`, `UPPER_SNAKE` constants.
6. **Keep diffs small and focused.** One feature or fix per PR. No mass reformatting, no drive-by renames.

## Build

Toolchain: `arm-none-eabi-gcc`, `binutils-arm-none-eabi`, `libnewlib-arm-none-eabi`, `libpng-dev`, `python3`. See [`INSTALL.md`](INSTALL.md).

```bash
make -j$(nproc)                        # Emerald ROM (pokeemerald.gba)
make firered -j$(nproc)                # FireRed variant
make leafgreen -j$(nproc)              # LeafGreen variant
make -j$(nproc) UNUSED_ERROR=1 DEPRECATED_ERROR=1   # what CI uses: unused/deprecated warnings are errors
```

## Test

```bash
make check -j$(nproc)                  # whole suite (slow: a few minutes)
make check TESTS="Spikes"              # only tests whose name starts with "Spikes"
make pokeemerald-test.elf TESTS="Spikes"   # test ROM you can open in mGBA to watch a test
```

Tests run headless in `mgba-rom-test-hydra`, which the Makefile builds. CI (`.github/workflows/build.yml`) runs all three builds, `make release` and `make check` on every PR: all must pass. In this repository's CI, unused or deprecated code is an error and a **skipped test counts as a failure**, so check that your `ASSUME`s hold with the default configs.

### Writing tests

Read [`docs/tutorials/how_to_testing_system.md`](docs/tutorials/how_to_testing_system.md). Copy the style of a nearby file in `test/battle/` (`move_effect/`, `ability/`, `hold_effect/`, `ai/`, …). The shape is:

```c
#include "global.h"
#include "test/battle.h"

SINGLE_BATTLE_TEST("Stun Spore inflicts paralysis")
{
    GIVEN {
        ASSUME(GetMoveEffect(MOVE_STUN_SPORE) == EFFECT_PARALYZE);
        PLAYER(SPECIES_WOBBUFFET);
        OPPONENT(SPECIES_WOBBUFFET);
    } WHEN {
        TURN { MOVE(player, MOVE_STUN_SPORE); }
    } SCENE {
        ANIMATION(ANIM_TYPE_MOVE, MOVE_STUN_SPORE, player);
        STATUS_ICON(opponent, paralysis: TRUE);
    }
}
```

- Start test names with the thing tested ("Stun Spore …") so `TESTS=` can filter them.
- Use `ASSUME(...)` for data the test relies on (move effects, config values), so hackers who change data get skipped tests, not false failures.
- Test both the positive case and the edge cases (immunities, abilities that block it, double battles, generation configs).
- Use `Wobbuffet` as the neutral filler species, like the existing tests do.

## Where things live

| What | Where |
| --- | --- |
| Feature configs (generation behaviour, toggles) | `include/config/*.h` |
| Constants (moves, abilities, items, species, effects) | `include/constants/` |
| Move data | `src/data/moves_info.h`, effects in `src/data/battle_move_effects.h` |
| Ability / item / hold-effect data | `src/data/abilities.h`, `src/data/items.h`, `src/data/hold_effects.h` |
| Species data | `src/data/pokemon/` |
| Trainers | `src/data/trainers.party` (processed by `tools/trainerproc`) |
| Battle engine | `src/battle_main.c`, `src/battle_util.c`, `src/battle_script_commands.c`, `src/battle_end_turn.c`, … |
| Battle scripts (assembly macros) | `data/battle_scripts_1.s`, macros in `asm/macros/battle_script.inc` |
| Battle AI | `src/battle_ai_*.c`, docs in `docs/tutorials/ai_*.md` |
| Battle strings | `include/constants/battle_string_ids.h`, `src/battle_message.c` |
| Overworld scripts and maps | `data/maps/`, `data/scripts/` (edit maps with Porymap, not by hand) |
| Tests | `test/` |
| How-to guides | `docs/tutorials/` (e.g. `how_to_new_move.md`, `how_to_new_pokemon.md`) |

Generated files (anything under `build/`, `*.gbapal`, `*.4bpp`, `*.lz`, generated `*.json.txt` outputs) must not be hand-edited or committed.

## Working on this codebase

- This is C for a 16.78 MHz ARM7 with 256 KB of EWRAM and 32 KB of IWRAM. Avoid floats (use `Q_4_12` / `uq4_12_t` fixed point from `fpmath.h`), large stack arrays, and dynamic allocation in hot paths.
- Use the right integer sizes (`u8`, `u16`, `s32`, …) and existing enums. Don't widen save-block fields.
- Search for an existing helper before writing one (`grep -rn "Func" src include`). The battle engine has many: `GetBattlerAbility`, `GetBattlerHoldEffect`, `IsBattlerAlive`, `GetMoveEffect`, …
- When adding a move, ability or item, follow the matching how-to guide in `docs/tutorials/` and add it everywhere it must be registered (constants, data table, battle script, strings, AI scoring if relevant).
- Update [`docs/`](docs/) when behaviour or configs change. New `.md` files must be added to `docs/SUMMARY.md` (CI checks this).

## Before you open a PR

- [ ] `make -j$(nproc)` builds with no new warnings
- [ ] `make firered -j$(nproc)` still builds if you touched shared code
- [ ] `make check -j$(nproc)` passes; no test removed, skipped or weakened
- [ ] New tests cover the new behaviour, including edge cases
- [ ] Diff is focused and follows the style guide
- [ ] PR description fills in the **AI assistance** section of the template, written honestly, with sources for any game mechanic
- [ ] The PR targets `master` of **this repository**
