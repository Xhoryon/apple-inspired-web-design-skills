# Interactive Media Explorer

## Core model

```
User choice
    ↓
Persistent user media state
    ↓
Visual representation
    ↓
Optional explanation
```

## Possible state

- variant
- viewpoint
- feature
- mediaAsset
- annotation
- inspection target

## Examples

- choose product color
- choose material
- inspect feature
- choose configuration
- before / after comparison

## Hard rules

- Do NOT couple controls directly to random images without a coherent media-state model.
- The control / state / representation must remain aligned.
- User state persists across section transitions.
- Scroll-owned state (camera / scale) may reverse.

## User-owned vs scroll-owned state

### User-owned

Examples:
- chosen color
- chosen variant
- manually selected feature
- compare target
- user zoom state where appropriate

Should generally NOT be reset merely because scroll direction reverses.

### Scroll-owned

Examples:
- camera position
- scroll-linked rotation
- scroll-linked scale
- annotation reveal
- cinematic section progress

May reverse when scrubbed backwards.

Do not collapse these axes.

## Reference

- `references/media-role-architecture.md` — Media Role Contract
- `references/scroll-cinematic-storytelling.md` — reversibility
- `references/application-media-modes.md` — Browse / Preview / Inspect / Compare / Work