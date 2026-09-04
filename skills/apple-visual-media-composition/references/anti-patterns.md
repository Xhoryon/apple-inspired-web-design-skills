# Visual Media Anti-Patterns

This reference consolidates actual Batch 8 visual-media anti-patterns. The Skill project architecture does NOT have a single global anti-pattern file; this is the visual-media-specific consolidation. Cross-Skill duplicates are addressed in the main architecture remediation.

| Anti-pattern | Primary owner | Summary |
|---|---|---|
| Apple Media Template | this Skill | The claim that an Apple-like design requires a particular template (giant centered hero, 50/50 split, sticky cinematic). Reject. Apple public-site observations show rhythm alternation, not template uniformity. |
| Cinematic Overreach | this Skill | Using scroll-driven 3D, WebGL, or real 3D where simple static composition would suffice. Reject when asset / capability does not support the cinematic ambition. |
| Asset Bluffing | this Skill | Claiming or producing assets that the project does not actually possess. UNKNOWN ≠ MISSING — ask first. |
| Fake Viewpoint | this Skill | CSS `rotateY` on a flat image is NOT a new physical viewpoint. Sparse multi-view does NOT support continuous rotation. |
| WebGL Prestige | this Skill | Choosing WebGL because it is the most advanced option. Reject. Choose simplest faithful strategy. |
| Silent AI Asset Synthesis | this Skill + Capability Contract | Generating assets without explicit user permission. AI supplementation requires permission, capability gate, and fidelity gate. |
| AI Sequence Consistency Blindness | this Skill | Assuming twenty individually generated views automatically form a smooth rotation sequence. Review for continuity is required. |
| One-Way Scroll Cinematic | this Skill | Animations that play once on scroll without reversing when the user scrolls upward. Scrubbed cinematic state must reverse coherently. |
| Scroll Resets User State | this Skill | Resetting user-chosen variant / color / feature when scroll direction reverses. User-owned state is distinct from scroll-owned state. |
| Reduced-Motion Narrative Collapse | this Skill | Setting animation duration to 0 in reduced motion and destroying the feature ordering / explanation. Preserve narrative, not merely delete animation. |
| Marketing-Site-Inside-an-App | this Skill | Putting a cinematic entrance on every photo open in a professional editor. Reject. Working media avoids spectacle. |
| Preview / Inspection Collapse | this Skill | Treating Preview and Inspect as the same mode. Reject. Preview is a quick identity confirm; Inspect is fidelity-priority with zoom/pan. |
| Destructive Crop | this Skill | Cropping inspection images to fill the window because it looks cleaner. Reject. Inspect preserves content. |
| Visual Climax Everywhere | this Skill | Making every section the climax. Whole-experience intensity review. Reject. |

## Cinematic Scene Choreography anti-patterns (post-v2.0.0 amendment)

| Anti-pattern | Summary |
|---|---|
| **Direction Literalism** | Assuming vertical scroll must produce vertical visual motion. The amendment principle is: scroll direction is an input / progress signal, not a required visual-motion direction. Reject. |
| **Scroll Hijacking** | Replacing understandable page progression with trapped / custom scrolling. Reject endless-feeling pinned sections and wheel / trackpad interception that breaks expected page navigation. Bounded pinned storytelling is permitted. |
| **Opaque Video Overclaim** | Inferring cinematic / video semantics without perceptual evidence. A video file being available proves technical availability, not understanding of shots / content. Treat video as Opaque Media unless trustworthy semantic evidence exists. |
| **Metadata-to-Storyboard Hallucination** | Inferring shot structure from duration / frame metadata. "Video is 8 seconds, therefore 0–2 s is establish, 2–5 s rotation, 5–8 s detail." REJECT. Timeline meaning cannot be inferred from duration alone. |
| **Cross-Axis Confusion** | Horizontal presentation driven by vertical scroll may visually suggest horizontal swiping on touch contexts. If it misleads the user or the interaction model does not support it, avoid or provide a vertical editorial fallback. |
| **Hero State Graph Overscope** | Treating Hero as the entire cinematic architecture. Scene Choreography is the broader concept; Hero is one important use case. |

## Notes

- These are Skill / project-owned internal anti-patterns, not Apple terminology.
- The project-level `APPLE-EXPERIENCE-CAPABILITY-EVIDENCE-CONTRACT.md` provides the cross-cutting evidence rule; this reference does not duplicate it.
- Cross-Skill anti-patterns (Hover on Touch, Modal Matryoshka, Glass Everywhere) have their primary owners in other Skills; this reference does not own them.
