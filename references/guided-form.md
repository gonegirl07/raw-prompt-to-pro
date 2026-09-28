# Guided prompt form

Use this reference when the user starts with `PROMPT:`, `PROMPT FORM:`, `PROMPT DETAIL:`, or `/prompt-form`. The purpose is to turn vague words into selectable visual decisions without forcing the user to know photography vocabulary.

## Language rule

Render the entire form in the dominant language of the user's raw prompt. This includes headings, field labels, explanations, option names, suggested values, status labels, and the instruction for how to reply. If the raw prompt is Vietnamese, use Vietnamese with diacritics. If it is English, use English. For mixed-language input, use the language of the main descriptive sentence and preserve proper nouns, product names, camera specifications, and user-locked strings.

The internal states can be localized. For example:

| Internal state | Vietnamese display |
|---|---|
| LOCKED | DA CHOT |
| SUGGESTED | GOI Y |
| FILL | DIEN THEM |
| OPEN | DE MO |
| AUTO | DE AGENT TU CHON |

Do not show an English-only form to a Vietnamese-speaking user. Keep the final image prompt in English by default for model compatibility only when the user has not requested the input language; the guided form itself must always follow the input language.

## User flow

1. Read the raw idea and lock the explicit details.
2. Show only high-impact choices that are missing or ambiguous.
3. Pre-fill low-risk defaults as `AUTO` or `SUGGESTED`.
4. Let the user edit values or say `giu tat ca goi y`.
5. Convert the completed form through the normal 10-layer pipeline.

Do not ask a long sequence of separate questions. Use one compact form. A blank optional field means `AUTO`; `OPEN` means leave that attribute unspecified so the image model can choose; `FILL` means the user wants to decide it.

## Beauty direction choices

Use these as directions, not fixed face identities. They should guide the face, expression, makeup, lighting and styling consistently.

| Choice | Visual translation |
|---|---|
| natural commercial | balanced adult facial proportions, expressive eyes, softly defined brows, naturally shaped lips, restrained makeup, realistic pores and neutral skin tone |
| soft elegant | calm gaze, gentle expression, softly sculpted cheeks, delicate makeup, smooth wrap light and refined low saturation |
| sharp editorial | defined cheekbones, structured brows, controlled expression, cleaner geometry, directional key light and stronger shape contrast |
| cute adult | bright eyes, soft cheeks, relaxed smile, rounded but mature cues, fresh styling and light color palette; never childlike |
| sensual fashion | deliberate gaze, relaxed lips, one-hip weight shift, fitted silhouette, controlled neckline and elegant shadow; non-explicit |
| custom | ask the user for the desired eyes, brows, nose, lips, jaw, hair, makeup and expression only if those details materially matter |

Do not silently turn `natural commercial` into a specific ethnicity, eye color, lip thickness, nose shape or hair color. Those remain `AUTO` unless the user chooses them.

## Suggested form

```text
PROMPT DETAIL FORM

RAW IDEA:
[paste the raw prompt]

LOCKED
- Subject: [value or OPEN]
- Wardrobe: [value or OPEN]
- Mood: [value or OPEN]
- Setting: [value or OPEN]

SUBJECT
- Adult age band: [AUTO / FILL]
- Region or ethnicity: [AUTO / FILL / OPEN]
- Beauty direction: [natural commercial / soft elegant / sharp editorial / cute adult / sensual fashion / custom]

FACE AND HAIR
- Eyes: [AUTO / FILL / OPEN]
- Brows: [AUTO / FILL / OPEN]
- Nose: [AUTO / FILL / OPEN]
- Lips: [AUTO / FILL / OPEN]
- Face shape or jaw: [AUTO / FILL / OPEN]
- Hair style and color: [AUTO / FILL / OPEN]
- Skin and makeup: [AUTO / FILL / OPEN]

POSE AND COMPOSITION
- Body stance and weight shift: [AUTO / FILL]
- Hands and fingers: [AUTO / FILL]
- Head angle, gaze and expression: [AUTO / FILL]
- Shot size: [AUTO / FILL]
- Subject placement and negative space: [AUTO / FILL]

WARDROBE AND TEXTILE
- Garment: [LOCKED / FILL]
- Color and palette: [LOCKED / FILL / AUTO]
- Fit and neckline: [AUTO / FILL / OPEN]
- Material: [AUTO / FILL / OPEN]
- Weave, knit, seam, fold and tension: [AUTO / FILL / OPEN]

SCENE, LIGHT AND CAMERA
- Location and background: [AUTO / FILL / OPEN]
- Key light direction and softness: [AUTO / FILL]
- Fill, bounce, rim and shadow: [AUTO / FILL]
- Camera height and viewpoint: [AUTO / FILL]
- Angle, lens and crop: [AUTO / FILL]
- Aspect ratio: [AUTO / FILL]

COLOR AND CONSTRAINTS
- White balance and color grade: [AUTO / FILL]
- Contrast and saturation: [AUTO / FILL]
- Must keep: [values]
- Must avoid: [values]
- Text, logo, jewelry, props: [AUTO / INCLUDE / EXCLUDE]
```

## Finalization rules

- Preserve every `LOCKED` value exactly in the final prompt.
- Translate `SUGGESTED` or `AUTO` into concrete visual language and list the main choices in `Infer`.
- Do not add a detail marked `OPEN`.
- If a user fills only some fields, keep the remaining fields at `AUTO` and continue unless an omission would change the concept.
- If a user chooses `custom` beauty direction, ask no more than three targeted questions when needed: facial feature direction, expression, and hair/makeup direction.
- After finalization, use the normal `FLAT PROMPT`, `NEGATIVE`, platform variants, and optional short output.

## Vietnamese rendering example

For a Vietnamese raw prompt such as `Tạo ảnh một cô gái xinh đẹp mặc áo đỏ trong studio`, render the form with labels like:

```text
MẪU PROMPT CHI TIẾT

Ý TƯỞNG GỐC:
Tạo ảnh một cô gái xinh đẹp mặc áo đỏ trong studio

ĐÃ CHỐT
- Chủ thể: một phụ nữ trưởng thành
- Trang phục: áo màu đỏ
- Bối cảnh: studio

HƯỚNG VẺ ĐẸP:
[tự nhiên thương mại / thanh lịch mềm mại / sắc sảo editorial / dễ thương trưởng thành / thời trang quyến rũ / tự mô tả]

KHUÔN MẶT VÀ TÓC
- Mắt: [ĐỂ AGENT TỰ CHỌN / ĐIỀN THÊM / ĐỂ MỞ]
- Mũi: [ĐỂ AGENT TỰ CHỌN / ĐIỀN THÊM / ĐỂ MỞ]
- Môi: [ĐỂ AGENT TỰ CHỌN / ĐIỀN THÊM / ĐỂ MỞ]
- Kiểu tóc và màu tóc: [ĐỂ AGENT TỰ CHỌN / ĐIỀN THÊM / ĐỂ MỞ]

TRANG PHỤC VÀ CHẤT LIỆU
- Kiểu áo và độ ôm: [ĐỂ AGENT TỰ CHỌN / ĐIỀN THÊM]
- Chất liệu: [ĐỂ AGENT TỰ CHỌN / ĐIỀN THÊM / ĐỂ MỞ]
- Dệt, đan, đường may, nếp gấp và độ căng: [ĐỂ AGENT TỰ CHỌN / ĐIỀN THÊM / ĐỂ MỞ]

Bạn có thể điền các mục muốn khóa, hoặc trả lời: "Giữ tất cả gợi ý".
```
