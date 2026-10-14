# Flow / Nano Banana prompt workflow

Use for flow.google.com, Google Flow, or Nano Banana image work. Distinguish Flow's agent from its standard image/editor composer. Verify the model label for the actual generation; an agent can rewrite prompts or route models. Model names and UI locations can change.

## Compile a visual scene

Use natural sentences: image type + subject/action + visible wardrobe + crop/viewpoint + setting + light/finish. Start around 100–180 words for a new master image and 60–120 for a reference-based change; these are starting budgets, not platform limits. Do not force a long brief, JSON, all ten layers, numerical camera settings or repeated negative lists into the composer.

Keep locked details. Allocate detail by visibility: iris color matters in a face portrait; lip contour, wine rim and hand placement matter in a close-up. Preserve off-frame wardrobe through the reference, but do not demand a high slit be visible in a lips close-up. A slit needs a crop that includes the thigh/knee. Do not add body measurements or ethnicity.

Make lighting causal: warm candles in the background and soft neutral face light can preserve cool pale skin. Velvet absorbs light with soft pile highlights; silk has broad smooth sheen. Avoid conflicting warm skin instructions, waxy smoothing, excessive pores at distant framing and generic quality slogans.

Settings belong outside the pasted prompt: image/still mode, confirmed model, one output per shot and selected aspect ratio. Do not promise that textual resolution claims change export resolution. Flow branding/watermarks may remain despite no-text instructions.

## Same character across shots

1. Generate one master with a clearly visible face and requested appearance.
2. Inspect it before treating it as approved. Fix specific wardrobe/crop/color errors with a local edit that explicitly preserves identity.
3. Attach the same master to each subsequent shot using Add to Prompt/ingredients or the editor's current image. Say which image defines identity and what changes. A typed 'same woman' without a linked image is insufficient.
4. Generate one new action/view at a time. For a pouring shot specify bottle hand, glass placement and continuous stream; for a lips close-up specify the glass beside the lips and depth of focus.
5. Compare face shape, nose, lip contour, hair streak, skin and makeup; in a tight crop verify only visible traits. Do not claim exact continuity from text, session or seed.

Prefer new assets with the master as a reference for new shots. If using edit history, keep/export each chosen version and make later requests from the intended master rather than an accidental chained close-up. Local edits preserve the original in Flow history.

## Live optimization

Only generate/spend credits when authorized. A request to test on an already-funded Flow account authorizes modest task-relevant trials, not buying credits or unlimited experimentation. Start with a master, at most two targeted master corrections, then requested reference shots; retry each shot once for a concrete visible defect. Stop for a service/credit/access blocker and report it.

Record actual submitted prompt, visible model/settings, output and observed defects. Distinguish suggestions from tested findings. If previews are blank, inspect the downloaded media using the browser's supported media download; do not conclude the image itself is black. Keep user/project assets; do not delete failed runs without authorization.

## Sources (checked 2026-10-06)

- https://support.google.com/flow/answer/16729550?co=GENIE.Platform%3DDesktop&hl=en — image creation, editing/history, adding references; agent routing differs from standard composer.
- https://ai.google.dev/gemini-api/docs/image-generation — scene descriptions, reference-based image generation and iterative edits. API capabilities are not automatically Flow UI controls.

For the current burgundy-bar case and observed tests, read flow-burgundy-case.md. Treat it as an example, not a universal portrait template.

## Shape, garment construction and instruction clarity

When the user requests a fuller bust or another silhouette, state that shape explicitly and separately from neckline depth. Deep-V specifies a garment cut, not bust volume. Never add this body preference to unrelated requests.

Connect requested shape to visible clothing construction: a close-fitting bodice, princess seams, darts, under-bust shaping or fabric ease where appropriate. Use only construction compatible with the chosen garment. Avoid simultaneous requests for a fitted waist and loose blousing if a defined silhouette matters. Mentioning silk plus relaxed waist folds can yield a loose blouse even when 'fitted through the bust' is present.

Choose a pose/crop that reveals the relevant shape: arms clear of the bodice, a mild torso turn, head-to-waist or head-to-thigh framing. Do not confuse neckline coverage with body shape or automatically deepen the neckline to increase apparent bust volume.

For edits, separate invariants from changes: preserve identity/pose/scene/light; change only silhouette/fit/seams. Inspect collateral changes and do not promise an exact lock. If several variables are edited together, the output cannot isolate which phrase caused the improvement.

## Evidence and limits

Google's public guidance recommends explicit subject, composition, action, location, style and reference roles, with direct local edits. It does not expose a keyword parser, hidden prioritization weights, or a guarantee that shorter prompts are better. Our budgets are readability/iteration heuristics; retain useful detail and expand when complexity needs it.

Observed 2026-10-06: direct composer had its own settings (initially Nano Banana 2, x2, landscape), separate from agent settings. Verify the model/count/aspect ratio again after switching modes. In the emerald case, direct Nano Banana Pro at 3:4/x1 created a new woman successfully. A subsequent local fit edit produced visibly clearer bust/waist shaping with similar identity. This was one initial image and one edit, not a controlled multi-seed A/B test; do not infer moderation behavior or internal reasoning from it. Read flow-emerald-case.md for submitted prompts and defects.

Additional official sources checked 2026-10-06:
- https://deepmind.google/models/gemini-image/prompt-guide/ — visible-scene components, incremental detail and editing.
- https://blog.google/products-and-platforms/products/gemini/prompting-tips-nano-banana-pro/ — Pro prompting, reference roles, camera/light controls and consistency limitations.

## Beauty direction and identity preservation

For a new fictional character, translate the user's desired beauty direction into a few coherent visible traits plus grooming, makeup, expression and flattering light. Beauty is subjective; do not prescribe a universal face or add these preferences when not requested. Avoid contradictory stacks of unrelated beauty archetypes.

For an existing identity reference, distinguish styling enhancement from face redesign. Keeping eye/nose/jaw/lip structure while changing makeup, hair and light may yield only subtle changes. Do not promise that 'beautiful' or 'beauty editorial' overrides a strong identity lock. If the user wants a substantially different face, obtain or use their authorization for a new facial design and distinguish it from preserving the same person.

Live emerald test 2026-10-06: one local edit requested polished beauty styling, eyeliner/blush/lip finish, sleek bob, subtle smile and softened face light while explicitly preserving facial structure. Output stayed very close to the prior image; makeup/eye definition changes were subtle, with no clear transformation in hairstyle or smile. Do not label this a demonstrated large beauty improvement. This single edit cannot establish model-wide beauty defaults or the causal strength of individual phrases.
