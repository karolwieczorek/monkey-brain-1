# monkey-brain

`monkey-brain` is a minimal experimental repository for executing simple PC commands and validating their effects using high-frequency visual feedback within the control loop.

## Premise

The core idea is:

> Execute simple PC commands and validate them at **<10 frames/s**.

Instead of relying on dense, continuous state access, the system acts in small steps:
1. issue a simple command,
2. observe the screen or environment,
3. validate whether the expected change happened,
4. continue or recover.

This makes the setup intentionally slow, simple, and robust enough for coarse interaction loops.

## Why

Many desktop automation systems assume:
- direct access to application state,
- high-frequency feedback,
- brittle selectors or APIs,
- full determinism.

`monkey-brain` explores a different regime:
- **commands are simple**
- **feedback is frame-based and iterative**
- **validation is visual and outcome-level**: it checks simple visible effects, such as whether a dialog appeared, text showed up, or a slider moved in the intended direction, rather than precise internal UI state
- **separation of levels**: strategic planning lives above, while `monkey-brain` is responsible only for primitive PC actions like clicking, typing, or sliding

This is closer to an agent that “pokes” the machine, checks the result, and then decides the next move.

## Design constraints

- **High observation rate**: a practical success criterion is primitive-action validation below 10 frames per second on consumer hardware
- **Simple commands only**: click, double click, drag, slide, type, press key, press key combination, scroll, wait, etc.
- **Closed-loop operation**: every action should be followed by validation
- **Fail-soft behavior**: when uncertain, retry, back off, or return to a known state
- **Minimal assumptions**: avoid depending on internal application APIs

## Example loop

```
higher-level system sends primitive command
→ monkey-brain enters a frame-based execution flow
→ across successive frames it applies primitive actions, observes, and validates state changes
→ as long as nothing unexpected is detected, the flow continues
→ it returns success when the expected effect is reached and validated
→ it returns failure as soon as an unexpected state is detected
```
