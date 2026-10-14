# raw-prompt-to-pro

Agent skill: turn a raw image prompt into a professional photographer-style brief and paste-ready prompt for ChatGPT, Gemini, Grok, OpenClaw, Hermes, Codex, OhMyPi, Flux, and Midjourney.

The expanded prompt covers subject, face and skin, pose and hands, garment construction, textile physics, composition, lighting, camera angle, color science, finish, and negative constraints. It preserves user-locked details and labels safe inferences.

## Install

Copy this folder into your agent's skills directory so `skills/raw-prompt-to-pro/SKILL.md` sits at the expected path.

Keep `references/` next to it.

## Use

Give the agent a rough line such as:

> Tao hinh co gai Viet Nam xinh, sexy, co V-neck

It returns Intent lock, Flat prompt (English), Negative, and short platform wrappers. When the user provides JSON or asks for a structured brief, it can also return a `PRODUCTION SPEC` with `metadata`, `image_parameters`, and `raw_prompt`. It does not generate an image unless you ask.

For Google Flow video, dance, yoga, and cycling use motion transfer when a reference clip can be attached. Text-only motion samples are the fallback. See `references/motion-transfer.md`. The skill does not generate video unless you ask.

When a target platform is known, or a user reports a refusal, it also applies the platform safety reference. It keeps valid creative details, neutralizes sexualized wording that can create false positives, avoids repeating sensitive terms in negative prompts for stricter UIs, and produces a bounded `SAFE RETRY PROMPT` without attempting to bypass guardrails.

## Modes

With no command prefix, it uses AUTO mode: infer safe missing details and produce the final prompt.

Use `PROMPT:`, `PROMPT FORM:`, `PROMPT DETAIL:`, or `/prompt-form` to receive a fillable guided form first. The form follows the language of the raw prompt, so a Vietnamese input receives Vietnamese labels and explanations. Edit the form and send it back, or reply `giu tat ca goi y`, to finalize the professional prompt.

Use `JSON FORM:`, `JSON PROMPT FORM:`, `/json-form`, or a clear request to create a JSON form from the raw prompt when you want to fill the production-spec structure directly. JSON keys stay stable for parsing; labels, values, notes, and instructions follow the raw prompt language. The temporary form marks fields as `LOCKED`, `SUGGESTED`, `FILL`, `OPEN`, or `AUTO`. After you return the edited JSON, the agent normalizes it and produces the final production spec and flat prompt.
