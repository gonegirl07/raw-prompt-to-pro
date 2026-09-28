# Platform map

Cung mot FLAT PROMPT lam noi dung chinh. Chi doi vo boc, do dai va syntax khi platform can. Neu user yeu cau JSON hoac dua JSON dau vao, giu them PRODUCTION SPEC lam brief; khong thay the FLAT PROMPT bang JSON.

Neu user chi ro nen tang, hoac da bao prompt bi tu choi, doc them `references/platform-safety.md` truoc khi chon wrapper. Khi do uu tien safe wording va co the xuat `SAFE RETRY PROMPT`; khong lap lai negative co tu nhay cam tren UI co bo loc nhay.

## ChatGPT web / Gemini web / Grok web

Dat truoc 1 cau lenh:

`Create one still image. Follow the prompt exactly. Do not add text, caption, or extra people.`

Roi dan FLAT PROMPT + Negative.

Grok web / Imagine hieu photoreal + camera mm tot. Gemini hay them chu - nhan manh no text khi user khong yeu cau chu. ChatGPT hay lam dep da - nhan manh pores, natural texture, no beautify.

## Grok Imagine

Dung FLAT PROMPT day du neu prompt can chi tiet vat lieu va camera; dung OPTIONAL SHORT khi giao dien hoac token budget ngan. Aspect noi ro 4:5 hoac 9:16. Khong dua JSON vao prompt paste tru khi user yeu cau.

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

Giu FLAT. Strength cua natural language; dua subject, pose, fabric, light, camera va color len som. Neu dung JSON de quan ly workflow, map cac field sang prompt truoc khi gui model.
