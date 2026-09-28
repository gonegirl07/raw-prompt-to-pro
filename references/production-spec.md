# Production spec

Use this reference when the user provides JSON, asks for a structured prompt, or wants a reusable image brief. The JSON is a planning and interchange format; `raw_prompt` remains the canonical paste-ready prompt.

## Schema

```json
{
  "metadata": {
    "source_author": null,
    "context": "",
    "intent": "",
    "inference_notes": []
  },
  "image_parameters": {
    "medium": "",
    "style": "",
    "subject": {
      "demographics": "",
      "age": "adult",
      "body_type": "",
      "pose": "",
      "gaze_expression": "",
      "features": {
        "hair": "",
        "face": "",
        "skin": "",
        "hands": ""
      }
    },
    "wardrobe": {
      "items": [],
      "construction": "",
      "textile_physics": ""
    },
    "setting": {
      "location": "",
      "background": "",
      "props": []
    },
    "composition": {
      "framing": "",
      "subject_placement": "",
      "negative_space": "",
      "aspect_ratio": ""
    },
    "lighting": {
      "key": "",
      "fill_bounce_rim": "",
      "shadows": "",
      "highlight_behavior": ""
    },
    "camera": {
      "height": "",
      "viewpoint": "",
      "angle": "",
      "lens": "",
      "distance_crop": "",
      "aperture_depth_of_field": ""
    },
    "color_grade": {
      "palette": "",
      "white_balance": "",
      "contrast_saturation": "",
      "skin_tone": "",
      "finish": ""
    },
    "negative_prompt": []
  },
  "raw_prompt": ""
}
```

## Rules

- Keep the user's explicit values. Do not copy values from this template into a real prompt just because they are present here.
- Use `null`, an empty string, or omit a field when the user did not provide the value and a low-risk inference would be misleading.
- `source_author` is metadata, never an invented identity. Keep it `null` unless the user supplied it.
- `age` must stay adult for a human subject. Do not use teen, schoolgirl, childlike, or ambiguous age wording.
- Keep one subject, one wardrobe direction, one setting, one composition, and one coherent lighting setup unless the user requests a set.
- Put measurable camera values in the `camera` object only when they support the shot. Do not add arbitrary technical values to make the JSON look precise.
- `negative_prompt` must be conditional. Remove text, logo, jewelry, props, or other items from it when the user explicitly requests them.
- After writing the JSON, ensure `raw_prompt` contains the same locked details and does not introduce a contradictory lens, pose, color, or garment.
