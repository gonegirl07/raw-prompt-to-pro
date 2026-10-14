# Platform-specific prompt compilation

Select target UI, image model if known, and operation (new image, reference-based new view, or edit). Preserve one canonical intent lock, but compile a separate prompt for the destination. Do not assume a shared wrapper gives equivalent results.

These are editorial starting budgets, not vendor input limits or guarantees. Count the actual submitted text including wrappers/JSON. If the UI reports a limit, obey its measured character limit.

| Target | Starting length | Form and priorities |
|---|---|---|
| Flow / Nano Banana Pro | 100–180 words for master; 60–120 for reference follow-ups | Natural sentences, one scene/action/crop, lighting with protected skin tone; attach master image. Read flow-nano-banana.md. |
| Gemini image UI | 90–180 words | Describe the scene coherently; attach references with explicit roles; use local edit instructions for corrections. Verify model selection in UI rather than assuming Pro. |
| Grok web / Imagine | 60–110 words, aim around 700 characters when practical | Hero identity, outfit, action/crop, light. Plain text; no JSON or long negative list unless requested. This character target is a heuristic, not a documented cap; preserve locks even if longer. |
| ChatGPT image UI | 120–220 words when needed | Natural-language art direction; reference roles and precise edit constraints. Avoid repeating structured spec in submitted prompt. |
| Midjourney | Concise visual clauses | Add only requested/current supported parameters; reference syntax depends on selected version. Verify before claiming syntax. |
| Flux / local | Depends on model/workflow | Subject/action/material/light in priority order; use native settings for dimensions; separate negative field only if supported. |
| OpenClaw / Hermes / Codex / OhMyPi | Compile for their actual image tool | These are execution agents, not image models. Send the destination prompt in its tool schema; never send the entire brief/manifest as one render. |

For a named destination deliver that prompt first, with settings outside the copy block. Do not produce unused full-length variants. For an unspecified target give one general prompt and short routing notes.

Grok compression: remove wrappers, duplicate locks, camera numerics, explanations and negative lists first. Keep age supplied by user, identity, wardrobe, scene and requested action. If too long for the observed cap, report any unavoidable tradeoff instead of silently dropping hard locks. Do not promise that English always outperforms Vietnamese.

Multi-image web requests: one independent flat prompt per image. JSON is optional and only delivered when requested. Reference-based prompts explicitly require an attached master; without it, provide a text-only fallback and state that exact face continuity is unverified.

Read platform-safety.md for actual refusals. Do not treat ordinary fashion neckline details as an instruction to change the user's wardrobe preemptively.
