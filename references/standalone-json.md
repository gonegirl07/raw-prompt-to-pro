# Standalone JSON prompts for multiple images

Use this reference when the user asks for `n` image prompts, wants to copy each JSON directly into an image platform, or reports that a multi-image JSON was rendered as a triptych, collage, contact sheet, or other multi-panel layout.

## Core decision

Treat `n` requested images as `n` independent render requests. The primary deliverable must be one self-contained JSON object per image. Do not make a root `image_prompt_set` object with an `images` array the only copy-paste artifact.

A manifest or optional index may be included after the standalone objects, but it must not be required to interpret any individual object. Each object must work when copied alone into a fresh chat or image-generation request.

## Required properties of each object

Use a compact, model-readable object with these fields as appropriate:

```json
{
  "image_id": "01_example",
  "task": "Generate exactly one standalone still image on one continuous full-frame canvas.",
  "output": {
    "image_count": 1,
    "layout": "single full-frame image",
    "aspect_ratio": "4:5",
    "no_collage": true,
    "no_triptych": true,
    "no_text": true
  },
  "character_lock": {
    "identity": "all hard-locked age, adult status, region or ethnicity, and name details",
    "appearance": "all hard-locked face, hair, skin, and makeup details",
    "personality": "the requested behavioral and emotional direction"
  },
  "wardrobe_lock": "all hard-locked garment, neckline, layer, and accessory details",
  "scene": "one location and one action for this image",
  "composition": "shot size, placement, crop, and background relationship",
  "lighting": "key, fill, shadow, and highlight behavior",
  "camera": "height, viewpoint, angle, lens, depth of field, and aspect ratio",
  "color_grade": "palette, white balance, contrast, skin handling, and finish",
  "identity_reference": {
    "use_attached_image": false,
    "instruction": "optional character-reference instruction"
  },
  "prompt": "one complete paste-ready prompt with all essential locks repeated",
  "negative_prompt": ["image-specific and global exclusions"]
}
```

The exact field names may be adapted, but the object must remain valid JSON and must not contain unresolved references such as `same character defined in another object`, `use the global lock above`, or `see image 1` when the object is intended to be pasted alone. Repeat the essential identity, wardrobe, setting, and output constraints inside `prompt` even when they also appear in structured fields.

## Single-image guardrails

Put a positive output instruction near the beginning and end of `prompt`:

```text
Generate exactly one standalone still image on one continuous full-frame canvas. Use only this single scene. Output one image only.
```

```text
One image, one scene, one continuous frame. Do not create a triptych, diptych, collage, contact sheet, storyboard, grid, split screen, inset panels, multiple frames, or alternate views.
```

These instructions reduce accidental panel layouts but are not deterministic platform controls. If the platform exposes an image-count or layout setting, set image count to `1` and single-image layout.

## Face and character continuity

Text-only locks, a shared session, and a repeated seed can improve similarity but cannot guarantee an identical face across independent generations. When exact continuity matters:

1. Render the first image and select an approved master image with the desired face.
2. Attach that master image to every later request as a character or identity reference when the platform supports it.
3. Tell the model to preserve facial identity, face shape, eyes, brows, nose, lips, skin tone, age, hair part, and makeup while creating the new scene.
4. Tell the model that the reference is for identity only; do not place the reference image inside the output and do not copy its pose unless requested.
5. Keep the same model/version, wardrobe lock, makeup, and relevant settings. Reuse a seed only when the platform supports it; never promise deterministic identity from a seed alone.

Recommended optional field:

```text
"identity_reference": {
  "use_attached_image": true,
  "reference_role": "character identity only",
  "instruction": "Preserve the exact facial identity, face shape, eye shape, brows, nose, lips, skin tone, age, hairstyle, hair part, and makeup from the attached approved reference image. Create the new scene described by this JSON. Do not show the reference image inside the output and do not copy its old pose."
}
```

If no reference image is available, state that continuity is an approximation and keep the full character lock inline.

## Platform length handling

- ChatGPT and Gemini: use the full standalone JSON when the interface accepts structured prompt text. The object can include structured fields plus a detailed `prompt`.
- Grok or any interface with a shorter input limit: emit a compact standalone JSON containing `image_id`, `task`, `output`, `prompt`, and `negative_prompt`. Preserve the essential face, wardrobe, scene, camera, and lighting details inside `prompt`; remove explanatory metadata first.
- OpenClaw, Hermes, Codex, and OhMyPi: use one standalone object per tool call or wrap one object's `prompt` and `negative_prompt`; do not pass a multi-image manifest as one generation request.

## Multi-image delivery checklist

- `n` images means `n` independent objects and `n` separate render requests.
- No root `images` array is required for the user to generate the images.
- Every object contains one scene and one output image.
- Every object repeats the hard-locked character and wardrobe details.
- Each object has its own negative prompt plus any shared exclusions needed for that scene.
- Face continuity instructions and reference-image workflow are explained whenever the user asks for the same character across images.
