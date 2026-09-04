# Apple Experience Skills

Professional Apple-inspired and Apple-platform-aware experience-design Skills for AI coding agents.

This project is an independent system for reasoning about interaction, controls, pointer behavior, motion, navigation, direct manipulation, modality, Liquid Glass, adaptive structure, feedback, keyboard and focus, text editing, and visual media. It teaches platform-appropriate decisions rather than superficial Apple imitation.

**License:** [PolyForm Noncommercial License 1.0.0](./LICENSE)

## What it is

The repository contains 13 primary Apple Experience Skills and the seven established Web companion Skills they reference. Each Skill uses provider-neutral Markdown frontmatter and progressively disclosed references so an agent can load the relevant guidance instead of the whole system for every task.

## Primary Skills

| Skill | Responsibility |
|---|---|
| `apple-experience-design` | Routes work by platform, input, capability, and behavior. |
| `apple-control-states` | Defines control states, transitions, cancellation, loading, and destructive behavior. |
| `apple-pointer-interaction` | Covers pointer-to-target relationships, hover, shared highlights, and retargeting. |
| `apple-motion-physics` | Covers spring response, interruption, retargeting, and reduced-motion adaptation. |
| `apple-navigation-spatial` | Defines navigation hierarchy, back semantics, spatial continuity, and scroll context. |
| `apple-direct-manipulation` | Covers drag, swipe, pinch, gesture conflicts, and alternative input. |
| `apple-modality-overlays` | Chooses and governs sheets, popovers, menus, alerts, and dismissal behavior. |
| `apple-liquid-glass-fidelity` | Covers advanced native and Web-approximate Liquid Glass behavior and fallback. |
| `apple-adaptive-structure` | Reorganizes application anatomy across platform, width, window, and input context. |
| `apple-feedback-response` | Defines status, progress, outcomes, recovery, haptics, and multimodal feedback. |
| `apple-keyboard-focus-commands` | Covers focus topology, keyboard traversal, shortcuts, commands, and restoration. |
| `apple-text-input-editing` | Covers text input, IME composition, selection, editing actions, and paste policy. |
| `apple-visual-media-composition` | Plans media roles, heroes, cinematic sequences, inspection, and asset feasibility. |

The Web companion Skills remain available for page composition, visual foundations, components, baseline Web Liquid Glass, motion, routing, and auditing. They are dependencies of the broader system, not additional Apple Experience primary Skills.

## Core principles

- **Platform appropriateness over superficial fidelity.** Preserve the interaction intent and adapt or abandon behavior that does not belong on the target platform.
- **System primitive first.** Prefer native platform behavior when it already satisfies the intent.
- **Capability and evidence honesty.** Code, runtime observation, metadata, generation, and visual inspection are distinct evidence types.
- **Accessibility is cross-cutting.** Essential meaning cannot depend on color, haptics, motion, or audio alone.
- **Interaction continuity matters.** State, focus, navigation, and direct manipulation must remain coherent through interruption and adaptation.
- **Liquid Glass is not generic glassmorphism.** On the Web it remains an approximation and must degrade safely.
- **Asset reality before cinematic ambition.** Media composition must match the assets and inspection capabilities actually available.

The binding project-level rules are documented in the [Capability & Evidence Contract](./APPLE-EXPERIENCE-CAPABILITY-EVIDENCE-CONTRACT.md) and [Accessibility Contract](./APPLE-EXPERIENCE-ACCESSIBILITY-CONTRACT.md).

## Installation and use

Clone the repository, then copy the Skill directories and cross-cutting contracts into the Skills location supported by your agent runtime:

```bash
git clone https://github.com/Xhoryon/apple-experience-skills.git
mkdir -p ~/.agents/skills
cp -R apple-experience-skills/skills/apple-* ~/.agents/skills/
cp apple-experience-skills/APPLE-EXPERIENCE-*-CONTRACT.md ~/.agents/skills/
```

Agent runtimes use different Skills directories. Replace `~/.agents/skills/` with the configured location for your runtime. Install the complete set when you want cross-Skill routing; install an individual Skill only when its companion references and named dependencies are also available.

Start with `apple-experience-design` for cross-platform interaction work. A focused task may invoke an owning Skill directly.

## License

Licensed under the [PolyForm Noncommercial License 1.0.0](./LICENSE). Noncommercial use, modification, and redistribution are permitted under its terms. Commercial use requires separate permission.

## Disclaimer

Independent project. Not affiliated with or endorsed by Apple Inc. Apple product and platform names are trademarks of Apple Inc. This repository provides design guidance, not official Apple APIs, specifications, or implementation guarantees.

## Releases

- [v2.0.0 release notes](./RELEASE-NOTES-v2.0.md)
- [v1.1 release notes](./RELEASE-NOTES-v1.1.md)
- [v1.0 release notes](./RELEASE-NOTES-v1.0.md)
