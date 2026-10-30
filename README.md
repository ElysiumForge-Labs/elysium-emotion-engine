# elysium-emotion-engine

Pure-Python emotion-state engine for AI agents and companion models (**Elysium X 20** / Project NHE). Appraise a message, update persistent state, let it decay, and return a prompt block, style parameters, and a memory decision.

Part of [ElysiumForge Labs](https://github.com/ElysiumForge-Labs).

> **Independent research repository.** Models a *state that changes behaviour*; it makes no claim that an agent "really feels". Zero runtime dependencies. Not affiliated with, endorsed by, or sponsored by Kinetiq or Hyperliquid.

## Features

| Output | What you get |
| --- | --- |
| `prompt_modulator()` | System-prompt block describing the current state |
| `style()` | `warmth`, `energy`, `verbosity`, `temperature_delta`, `emoji_ok` |
| `memory_policy()` | Whether to store this turn, with tags and a salience score |

Design principles:

- **Functional, not claimed.** Emotion is data that accumulates, decays, has a slow mood, and persists across sessions — not vibes written into a static system prompt.
- **Transparent and swappable.** The default appraiser is a small word-list heuristic. Replace it with a classifier or LLM call without touching the rest of the engine.
- **Emotion shapes memory.** Charged moments are flagged for storage via `memory_policy()`.

## Stack

| Item | Value |
| --- | --- |
| Language | Python ≥ 3.9 |
| Runtime deps | None |
| Package name | `elysium-x20` (`src/elysium_x20`) |
| Version | 0.1.0 |
| Tests | pytest (optional `[dev]` extra) |
| License | MIT |

## Getting started

### Install

```bash
pip install -e .                 # from a clone
pip install -e ".[dev]"          # with pytest
```

### Quick start

```python
from elysium_x20 import EmotionEngine, Personality

engine = EmotionEngine(Personality(empathy=0.7))

engine.process_turn("I passed my exam!! so happy")
system = BASE_PROMPT + "\n\n" + engine.prompt_modulator("Nova")
reply = my_llm(system, user_msg)
engine.observe_reply(reply)

decision = engine.memory_policy()
if decision.store:
    store_memory(user_msg, reply, tags=decision.tags, salience=decision.salience)
```

See `examples/companion_loop.py` for a full stubbed loop. Assets under `assets/` illustrate architecture and demo output.

### Tests

```bash
pytest
```

## Layout

| Path | Role |
| --- | --- |
| `src/elysium_x20/` | Engine package |
| `examples/` | Companion loop stub |
| `tests/` | pytest suite |
| `assets/` | Architecture and demo figures |
| `scripts/` | Helper scripts |

## License

MIT.
