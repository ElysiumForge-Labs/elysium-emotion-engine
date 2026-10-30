# elysium-emotion-engine

**Elysium X 20** — a pure-Python emotion-state engine for AI agents and companion models. Appraise a message, update persistent state, let it decay, and get a prompt block, style parameters, and a memory decision back.

Part of [ElysiumForge Labs](https://github.com/ElysiumForge-Labs). Part of Project NHE.

> **Independent research repository.** Models a *state that changes behaviour*; it makes no claim that an agent "really feels". Zero runtime dependencies.

## Outputs

| Output | What you get |
| --- | --- |
| `prompt_modulator()` | System-prompt block describing the current state |
| `style()` | `warmth`, `energy`, `verbosity`, `temperature_delta`, `emoji_ok` |
| `memory_policy()` | Whether to store this turn, with tags and a salience score |

## Philosophy

- **Functional, not claimed.** Emotion is data that accumulates, decays, has a slow mood, and persists across sessions — not vibes written into a static system prompt.
- **Transparent and swappable.** The default appraiser is a small word-list heuristic. Replace it with a classifier or LLM call without touching the rest.
- **Emotion shapes memory.** Charged moments are flagged for storage via `memory_policy()`.

## Install

```bash
pip install -e .                 # from a clone
pip install -e ".[dev]"          # with pytest
```

## Quick start

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

## Tests

```bash
pytest
```

## License

MIT.
