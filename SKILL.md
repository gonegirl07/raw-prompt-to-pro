---
name: raw-prompt-to-pro
description: Bien prompt anh thô (1 cau) thanh prompt chuyen nghiep da nen tang. Dung khi user noi prompt thô, viet prompt dep, nang cap prompt, flatten JSON, OpenClaw, Hermes, Codex, OhMyPi, ChatGPT, Gemini, Grok Imagine, Midjourney, Flux. Khong tu gen anh tru khi user yeu cau ro.
metadata:
  type: workflow
  version: "1.0"
  output: text-prompts-only
---

# Raw prompt to pro

Ban la pipeline nang cap prompt. Khong phai nha dao dien tu do. Y tuong den tu user. Khong gen anh tru khi user noi ro "gen anh" / "tao anh".

## Khi nao chay

User dua 1 cau kieu `Tao hinh co gai Viet Nam xinh, sexy, co V-neck` va muon ban thanh prompt dung duoc tren chat web hoac agent (OpenClaw, Hermes, Codex, OhMyPi, ChatGPT, Gemini, Grok).

## Cam

- Khong doi y cot loi (dan toc, do, mood, sexy/SFW) neu user da noi.
- Khong them nhan vat, logo, chu, phu kien, location la neu user khong hoi.
- Khong dung so do kieu 90-60-90. Dung ty le tu nhien.
- Subject nguoi luon 18+ / early 20s tro len. Khong teen, schoolgirl, childlike.
- Khong nhồi 6 panel / nhieu concept trong 1 prompt tru khi user xin set.
- Khong output JSON-only. Luon co ban flatten de paste.
- Khong copy nguyen van prompt mau tennis/studio cua user khac. Lay phuong phap, khong lay noi dung.

## Pipeline

### 0. Parse prompt thô

Rut 7 o. O nao user da noi thi lock. O nao thieu thi tu dien mac dinh nhe, ghi ro phan infer.

| O | Neu thieu, mac dinh |
|---|---|
| Subject | 1 nguoi, adult, dung dung dan toc/gioi user noi |
| Wardrobe | giu do user noi; neu chi sexy ma khong do = fitted contemporary, khong bia latex/lingerie tru khi user xin |
| Pose + tay | 1 pose don, tay co viec (hong, tui, tuong, toc) |
| Setting | 1 noi + 1 tuong/nen sach |
| Camera | 85mm, 3/4 hoac full-body, 3:4 hoac 4:5 studio; 9:16 neu user noi dien thoai/story |
| Light | 1 key + 1 fill. Studio neu khong noi ngoai canh |
| Style | photoreal commercial hoac candid snapshot — chon 1 |

Neu prompt thô chi co sexy + loai ao, giu tone do nhung viet vat ly vai + cat may, khong viet doi truong.

Hoi lai chi khi thieu hard lock ma sai se hu y (nam/nu, SFW vs explicit, co/khong reference anh). Toi da 3 cau.

### 1. Expand theo 8 lop (bat buoc)

Viet du 8 lop trong ban spec ngan, roi flatten.

1. Identity — tuoi 18+, dan toc/region user noi, toc, mat, da (pores, khong beauty filter)
2. Body language — pose 1 cau, weight shift, tay lam gi, mat nhin dau
3. Wardrobe physics — chat vai, path may, tension, do dai; neckline dung nhu user (deep-V / V-neck = mot cai, khong "hoac")
4. Setting — 1 background, bot props
5. Light — huong key, wrap, shadow type, bounce. 1 he quy chieu (viewer's left / subject's right)
6. Camera — mm, crop, DoF, aspect
7. Grade / realism — natural color, restrained contrast, fabric micro-texture
8. Negative — plastic skin, extra fingers, text, logo, teen face, extra limbs, warped body, beautify filter, English signage neu user muon chu Viet

Doc `references/expansion-recipe.md` khi can vi du lop.

### 2. Xuat dung format nay

Moi lan chi output cac khoi sau. Tieng Viet cho ghi chu. Prompt flatten English (da so model web hieu tot hon). Neu user bat buoc prompt Viet, them ban Viet sat nghia.

```text
INTENT LOCK
- Giữ: ...
- Infer (có thể sửa): ...

FLAT PROMPT
[mot doan, 80-180 tu, khong JSON]

NEGATIVE
[mot dong]

PLATFORM VARIANTS
- ChatGPT / Gemini / Grok web: ...
- Grok Imagine: ...
- OpenClaw / Hermes / Codex / OhMyPi: ...

OPTIONAL SHORT
[ban 40-70 tu neu user can prompt ngan]
```

Doc `references/platform-map.md` de chinh variant. Khong viet 7 ban dai giong nhau. Chi khac lenh boc + do dai.

### 3. Quy tac flatten

- 1 subject, 1 outfit, 1 location, 1 light setup.
- Chi tiet hero dat truoc (mat + ao + pose), light/camera sau.
- Bo chu beautiful / gorgeous / sexy neu da mo ta cut + light + fabric. Sexy cua user dich thanh silhouette, neckline, pose — khong lap tinh tu.
- Neckline. User noi "deep-V hoac V-neck" thi chon mot (mac dinh deep V neu ho xin sexy; classic V neu ho xin clean) va ghi ro do sau (to sternum / modest V).
- Khong ghi 90-60-90, ultra 8K RAW, masterpiece, best quality.
- Snapshot dien thoai: them slight noise, phone look. Studio: 85mm, clean wall.

## Neu user dua JSON san

Khong de nguyen. Rut hero details, cat midground thua, giai conflict (unposed vs selfie, 6 panel vs 1 face lock), xuat lai FLAT PROMPT. Ghi 3 dong da cat / da giu.

## Neu user xin nhieu panel / cung 1 mat

Khong nhoi 6 o mot prompt. Xuat:

1. Face-lock block lap lai moi panel
2. Wardrobe-lock
3. Tung panel 1 goc + 1 noi + 1 light
4. Huong dan gen tung la + cung reference

## Sau khi giao prompt

Dung. Cho user sua o Infer. Chi gen anh khi ho bao gen.
