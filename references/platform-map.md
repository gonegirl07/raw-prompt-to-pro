# Platform map

Cung mot FLAT PROMPT. Chi doi vo boc.

## ChatGPT web / Gemini web / Grok web

Dat truoc 1 cau lenh:

`Create one still image. Follow the prompt exactly. Do not add text, caption, or extra people.`

Roi dan FLAT PROMPT + Negative.

Grok web / Imagine hieu photoreal + camera mm tot. Gemini hay them chu — nhan manh no text. ChatGPT hay lam dep da — nhan manh pores, no beautify.

## Grok Imagine

FLAT PROMPT ngan hon (gap o OPTIONAL SHORT neu >160 tu). Aspect noi ro 4:5 hoac 9:16. Khong JSON.

## OpenClaw / Hermes / Codex / OhMyPi

Agent hay lo prompt. Boc:

```text
TASK: write-or-call image prompt only unless user asked to generate.
PROMPT:
[FLAT]
NEGATIVE:
[NEG]
RULES: one image, adult subject, no added logos, keep wardrobe.
```

Neu agent co tool gen, user phai noi "gen". Neu chi xin prompt, dung lai o text.

## Midjourney (neu user hoi)

Them `--ar 4:5 --stylize 100` (studio) hoac `--ar 9:16`. Negative thanh `--no plastic skin, text, logo, extra fingers`.

## Flux / local

Giu FLAT. Strength cua natural language; dua light + fabric len som.
