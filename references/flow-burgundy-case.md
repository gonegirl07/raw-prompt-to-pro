# Flow burgundy bar case — 2026-10-06

Target: Flow in-app browser. Nano Banana Pro selected in image settings and visible in image editor. 3:4, one output. Live tests, not an A/B benchmark against other platforms. Flow agent rephrased the initial request; image editor accepted the local correction.

## Master prompt (recommended revision after inspection)

Create one photorealistic fashion photograph, portrait 3:4. A 29-year-old adult woman with cool porcelain-white skin, dark brown eyes with a subtle burgundy tint, deep red lipstick, and long straight silky black hair with a burgundy highlight streak. She wears a black velvet slip dress with a deep V neckline reaching the sternum and a high side slit, an open burgundy silk robe draped loosely over her shoulders, a black velvet choker and an antique silver ring. Frame from head to knees with the slit visible on one thigh: she leans against a nighttime bar, looks directly at the camera, and holds a stemmed glass of red wine. Dark wooden vinyl-record shelves behind her. Soft neutral face light and warm candlelight in the background; preserve cool pale skin, natural texture, velvet softness and silk sheen. Eye-level viewpoint, softly blurred background. One photograph, no text.

The combined revision above is not a separately tested text-to-image render. Tested sequence: initial master used 'deep V neckline and high slit' with a head-to-mid-thigh crop; output did not visibly deliver the deep neckline or slit. Local correction below improved visibility while retaining similar identity.

## Actual local correction submitted

Keep this exact woman, face, hair, makeup, bar and lighting. Refine only the dress and framing: a clearly defined deep V neckline reaching the sternum, and a high side slit visible on one thigh. Keep the black velvet slip dress and open burgundy silk robe. Widen the portrait framing to include her knees so the slit is visible; she still leans on the bar, looks at the camera, holds red wine, and wears the black velvet choker and antique silver ring. Preserve natural skin texture and fabric detail. One photorealistic fashion photograph.

## Actual pouring prompt (attach the corrected master)

Create one new photorealistic fashion photograph using the attached woman as the identity and wardrobe reference. Preserve her face, cool porcelain skin, burgundy-tinted brown eyes, deep red lipstick, straight black hair and burgundy streak, black velvet deep-V slit dress, open burgundy silk robe, velvet choker and antique silver ring. At the same candlelit bar with dark wooden vinyl shelves, show her in a three-quarter side view from head to upper thigh. She looks down at the glass as her right hand tilts a wine bottle; a continuous red-wine stream enters a stemmed glass standing on the counter, while her left hand steadies its base. Keep both hands and the stream visible. Soft neutral face light, warm background candles, realistic silk and velvet. Portrait 3:4, one image, no text.

Observed: recognizable continuity in face/hair/wardrobe, side-view pouring and visible stream. Glass was held by the stem rather than standing on the counter. This was an inferred staging detail, not a user's hard lock; no additional rerender needed. Platform watermark remained.

## Actual close-up prompt (attach the same corrected master)

Create one photorealistic close-up using the attached woman as the identity reference. Preserve her exact lip shape, nose, jaw, cool porcelain skin, deep red lipstick and straight black hair with the burgundy streak. Tight crop from the lower nose to collarbone: her closed red lips and a clear stemmed glass of red wine are the main subjects. She holds the glass beside her mouth, its thin rim just below her lower lip, without hiding the lips. Show the black velvet choker, a glimpse of burgundy silk and her antique silver ring if the hand enters the frame. Keep the candlelit vinyl bar softly blurred behind her. Soft neutral light on skin, warm candle reflections in the glass, natural lip texture, sharp lips and glass rim. Portrait 3:4. One photograph, no text.

Observed: sharp deep-red lips and wine rim, natural skin/lip texture, matching nose/lip contour, choker and burgundy hair streak. The crop includes a small part of the eye area rather than exactly beginning below the nose. Exact identity cannot be proved from a lower-face crop. Total: four generation/edit operations (initial, local master correction, pouring, close-up); no extra paid quality reruns. Actual credit charge was not inspected. Grok/Gemini guidance remains untested in this run.

## Reusable findings

- Align crop with garment feature visibility; wording alone did not make a slit visible.
- A narrow reference edit worked for neckline/crop without rebuilding the subject from scratch.
- Use the same image asset as a reference for each new shot.
- Blank browser preview and 'cannot load image' occurred even with downloadable valid images; loading retry solved the asset tile. Treat rendering/display failures separately from prompt quality.
- This run supports the workflow, not a claim of measured superiority over long prompts or other models.
