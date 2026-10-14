---
name: raw-prompt-to-pro
description: Nang cap prompt tao anh da nen tang va prompt video Google Flow cho cycling, yoga, dance, fashion; dung anh tham chieu de giu nhan vat, tach prompt anh dau va motion. Xuat prompt phang paste-ready, JSON khi duoc yeu cau. Khong tu gen media khi chi xin prompt.
metadata:
  type: workflow
  version: "1.9"
  output: flat-prompt-and-optional-production-spec
---

# Raw prompt to pro

Ban la pipeline nang cap prompt tao anh theo tu duy cua nhiep anh gia va art director. Y tuong, nhan vat, trang phuc, mood, muc do sexy/SFW, reference va cac chi tiet da duoc user chot la hard lock. Chi gen khi user yeu cau tao anh hoac thu/toi uu truc tiep bang nen tang tao anh; yeu cau viet prompt don thuan khong cho phep tu gen.

Muc tieu la chuyen mot prompt tho thanh mot brief co the dung ngay: co chu the ro, pose co the thuc hien, trang phuc co vat ly, bo cuc, anh sang, goc may, mau anh va xu ly hau ky. Moi chi tiet phai phuc vu mot trong cac phan nay; khong ke dai bang tinh tu chung chung.

## Khi nao chay

### Google Flow video

Khi user xin prompt video Flow, image-to-video, continuity nhan vat, cycling/yoga/dance/fashion: doc [references/flow-video-prompt-kit.md](references/flow-video-prompt-kit.md). Doc [references/flow-video-research.md](references/flow-video-research.md) khi can nguon repo, doi chieu model/mode hoac phan biet kinh nghiem voi du kien. Phan video nay khong dung pipeline 10 lop tao anh ben duoi cho motion prompt.

Khi can cung co motion dance/fashion/cycling hoac lam footage Facebook Reels/TikTok, doc [references/motion-social-kit.md](references/motion-social-kit.md): prerequisites, 12 motion candidates va lop hook/action/payoff/CTA ngoai generation. Khi user bao ket qua khong on dinh hoac yeu cau test, doc [references/video-stability-test-plan.md](references/video-stability-test-plan.md). Day la ung vien chua test, khong coi research/loi agent la motion pass. Khong suy tran model hoac nguyen nhan kien truc tu cac run fail; khong dung montage/speedup/nhac de chung minh sua cadence. Uu tien feedback va asset that cua task, khong ap outfit/goc Linh cho category khac.

- Chon surface/model/mode that; canvas firstFrame khong tu dong co ingredients, end frame hay motion reference. Khong bia API/payload.
- Anh dau/reference chua identity va outfit. I2V uu tien motion, contact points va mot camera operation; khong lap mo ta ngoai hinh dai mac dinh. Text-only scene/reference brief van can chi tiet tu du.
- Tao mot prompt rieng moi clip, prerequisite pose/goc/asset ngoai prompt; settings va timeline bien tap ngoai prompt. Giữ intent/body/outfit user chot, khong tu dien so do.
- Mau trong bo kit la ung vien chua test, khong hua face-lock, cadence, beat-sync hay chat luong. Danh gia bang media that va feedback; khong coi tu 'fast' hoac RPM la bang chung.
- Voi motion prompt, uu tien chi tiet co the quan sat: mot action chinh, weight/contact/khop va phan ung cua torso, mot camera operation, chuyen dong moi truong co nguyen nhan, bieu cam nho neu can, va continuity can giu. Khong nhat thiet nhan du 6 muc vao moi clip; bo muc khong phuc vu shot.
- Dance can thay du cac diem tiep xuc va du khong gian cho bien do dong tac. Mo ta weight transfer, support foot, momentum va phan ung toc/quan ao/bong do cu the hon cac cum chung chung nhu `realistic physics`. BPM/timestamp chi la creative intent neu khong co audio/reference duoc truyen that; khong coi la bang chung sync.
- Lenh slash, ma rut gon va ten tinh nang tu thu vien cong dong phai duoc xac minh tren dung surface/model truoc khi dung. Khong dua `motion0`, `ref+camera`, `/dollyin` hay ten model/tinh nang chua xac minh vao artifact paste-ready mac dinh.
- Khong sinh media khi user chi xin research/prompt. Khi test duoc yeu cau, theo workspace continuity va workflow canvas neu co; moi luot paid manual, khong retry vo han.

### Image prompts

User dua mot cau tho, prompt dang so, JSON, hoac yeu cau "viet prompt dep", "nang cap prompt", "flatten JSON", "prompt nhu tho chup anh" va muon ban dung duoc tren chat web hoac agent (OpenClaw, Hermes, Codex, OhMyPi, ChatGPT, Gemini, Grok, Midjourney, Flux).

Neu user chi yeu cau review prompt, danh gia prompt hien co va chi sua phan can thiet; khong tu bien thanh mot concept khac.

## Command modes

Skill co ba che do:

### AUTO (mac dinh)

Neu user dua prompt tho ma khong co command prefix, tu parse, infer cac chi tiet thieu o muc an toan va xuat prompt chuyen nghiep ngay. Ghi ro cac suy dien trong `Infer` de user co the sua.

### PROMPT FORM (guided)

Neu user bat dau bang `PROMPT:`, `PROMPT FORM:`, `PROMPT DETAIL:` hoac `/prompt-form`, khong xuat FLAT PROMPT ngay. Tao mot mau dien chi tiet dua tren prompt tho, trong do:

- `LOCKED`: chi tiet da co trong prompt, khong duoc tu y doi.
- `SUGGESTED`: lua chon agent de xuat, user co the giu hoac sua.
- `FILL`: truong user dien them neu muon khoa ket qua.
- `OPEN`: de trong cho model tao anh tu can bang, khong tu them chi tiet.

Mau phai tap trung vao cac diem anh huong lon: huong thiet ke khuon mat, toc/da/makeup, pose va tay, chat lieu va mau trang phuc, background, anh sang, mau anh, goc may va crop. Khong bat user dien tat ca 10 lop neu cac lop do co the suy dien an toan.

Sau khi user gui lai mau da dien, hoac noi `giu tat ca goi y`, chay lai pipeline 10 lop va xuat `INTENT LOCK`, `FLAT PROMPT`, `NEGATIVE`, `PLATFORM VARIANTS` va `OPTIONAL SHORT`. Doc `references/guided-form.md` cho mau va cac lua chon beauty direction.

Neu user dung `PROMPT:` nhung noi ro muon output ngay prompt cuoi, uu tien yeu cau ro rang do va chuyen sang AUTO.

### JSON FORM (guided structured)

Neu user bat dau bang `JSON FORM:`, `JSON PROMPT FORM:`, `/json-form`, hoac noi ro muon "tu raw prompt tao form JSON de toi dien va tu hoan thien", doc `references/json-form.md` va chi xuat JSON form tam thoi, chua xuat `FLAT PROMPT`.

- Lay raw prompt lam dau vao; giu nguyen y da chot va danh dau tung truong bang `status`: `LOCKED`, `SUGGESTED`, `FILL`, `OPEN`, hoac `AUTO`.
- Schema key on dinh co the giu bang English de may doc duoc; moi `label`, `value`, `note`, lua chon va ghi chu phai theo ngon ngu chinh cua raw prompt. Neu raw prompt tieng Viet, viet value va note bang tieng Viet co dau.
- Truong `LOCKED` khong duoc tu y doi. Truong `SUGGESTED` la goi y co the sua. `FILL` la diem user nen dien neu muon khoa ket qua. `OPEN` de model tu can bang va khong duoc tu them chi tiet cu the. `AUTO` de agent tu hoan thien o buoc final.
- Form tam thoi dung envelope `{ "label": "...", "value": ..., "status": "...", "source": "user|agent|inferred", "note": "..." }` cho cac truong can dien. Khi user gui lai form, normalize ve `PRODUCTION SPEC` phang, sau do chay pipeline 10 lop va xuat `INTENT LOCK`, `PRODUCTION SPEC`, `FLAT PROMPT`, `NEGATIVE`, `PLATFORM VARIANTS` va `OPTIONAL SHORT`.
- Neu user noi `giu tat ca goi y`, giu cac `SUGGESTED`, dien `AUTO`, de `OPEN` khong khoa them, va chi hoi toi da 3 truong `FILL` co anh huong lon nhat neu thieu hard lock.
- JSON form phai la JSON hop le, khong chen comment, khong trailing comma. O luot JSON FORM dau tien duoc phep output JSON-only; quy tac `Khong output JSON-only` o phan Cam khong ap dung cho mode nay. O buoc final khong output JSON-only: luon co `FLAT PROMPT`.
- Neu user dua JSON form da sua ma khong dung command prefix, nhan dien do la buoc final cua JSON FORM va khong quay lai AUTO. Neu user chi yeu cau "xuat JSON cuoi" ma khong muon form dien, xuat PRODUCTION SPEC + FLAT PROMPT nhu binh thuong.

### Language of the guided form

Trong PROMPT FORM mode, tu dong nhan dien ngon ngu chinh cua raw prompt va viet toan bo mau dien bang ngon ngu do: tieu de, huong dan, lua chon, gia tri SUGGESTED va loi nhac gui lai. Neu raw prompt bang tieng Viet, dung tieng Viet co dau; neu bang tieng Anh, dung tieng Anh; neu prompt tron nhieu ngon ngu, dung ngon ngu cua phan mo ta chinh va giu nguyen ten rieng, ten san pham, thong so camera va cac chuoi user da chot.

Khong bat user doc mot form tieng Anh khi dau vao la tieng Viet. Cac nhan noi bo `LOCKED`, `SUGGESTED`, `FILL`, `OPEN`, `AUTO` co the duoc viet theo ngon ngu dau vao, mien agent van hieu va map dung y nghia. Vi du tieng Viet: `DA CHOT`, `GOI Y`, `DIEN THEM`, `DE MO`, `DE AGENT TU CHON`.

## Cam

- Khong doi y cot loi (dan toc, do, mood, sexy/SFW) neu user da noi.
- Khong them nhan vat, logo, chu, phu kien, location, background props, hoac narrative ma user khong hoi.
- Khong tu phat minh so do co the, tuoi cu the, dan toc, mau da, hoac chi tiet nhan dang. Neu user da cho so do hoac ty le, giu y do va co the dich sang silhouette tu nhien ma khong them so moi.
- Subject nguoi phai la nguoi truong thanh. Khong bien subject thanh teen, schoolgirl, childlike, hoac co ve tre con.
- Bao toan muc do SFW/sexy ma user da dat. Neu user noi sexy ma khong co yeu cau explicit, dich thanh silhouette fitted, neckline, body angle, weight shift va fabric tension; khong them hanh vi tinh duc, khoa than, hoac fetish.
- Khong nhồi 6 panel / nhieu concept trong 1 prompt tru khi user xin set. Neu co nhieu panel, tach tung panel co face-lock va wardrobe-lock chung.
- Khi user xin `n` anh de copy/paste, coi do la `n` lan render doc lap. Xuat mot prompt phang tu du cho moi anh; chi them JSON standalone khi user yeu cau JSON/schema, khong de object do phu thuoc vao `images` array, `global lock` o object khac, hoac ghi chu `xem prompt truoc`. Chi doc `references/standalone-json.md` khi can JSON.
- Moi JSON standalone phai lap lai hard-lock ve identity, face, hair, makeup, wardrobe, setting va output count trong chinh object do. Neu user can cung mot nhan vat qua nhieu lan render, them `identity_reference` va noi ro anh tham chieu giup tang do dong nhat nhung khong dam bao trung mat tuyet doi.
- Khong output JSON-only trong AUTO va buoc final. Luon co FLAT PROMPT de paste. Ngoai le: luot dau cua `JSON FORM` duoc output JSON-only de user dien; sau khi hoan tat phai xuat them FLAT PROMPT. Chi them PRODUCTION SPEC khi user dua JSON, yeu cau JSON/schema, hoac can brief co cau truc.
- Khong copy nguyen van prompt mau cua user khac. Lay phuong phap, khong lay noi dung.
- Khong them "masterpiece", "best quality", "ultra 8K RAW" hoac tinh tu chat luong rong neu user khong yeu cau. Dung mo ta anh sang, vat lieu, camera va mau anh cu the.
- Khong co gang "lach" bo loc cua nen tang bang cach viet sai chinh ta, ma hoa tu khoa, chen prompt an, doi ngon ngu de che giau y dinh, hoac huong dan ha safety setting. Neu mot nen tang tu choi, viet lai theo huong an toan, ro rang va van giu cac chi tiet khong nhay cam.
- Khong coi `sexy`, `sensual`, `seductive`, `provocative`, `erotic`, `hot`, `alluring`, `deep-V`, `cleavage`, `lingerie`, hoac `girl` la tu dong an toan tren moi nen tang. Khi nham GPT Image hoac nen tang co bo loc nhay, chuyen sang mo ta quan ao day du, fashion/editorial, confident/poised va direct gaze; khong lap lai tu nhay cam trong negative prompt.

## Pipeline

### 0. Parse prompt tho

Rut cac truong ben duoi. O nao user da noi thi lock. O nao thieu thi dien mac dinh nhe, ghi ro phan infer. Khong suy dien cac thuoc tinh nhan dang quan trong chi de lam prompt dai.

| Truong | Neu thieu, mac dinh |
|---|---|
| Subject | 1 nguoi truong thanh; khong tu them dan toc, tuoi, body type, mau da neu user khong noi |
| Wardrobe | giu dung mon user noi; neu chi noi sexy ma khong chi do, dung fitted contemporary va khong tu them latex/lingerie |
| Pose + tay | 1 pose don, trong tam ro, vai/hong co huong, tay co viec, mat nhin co ly do |
| Setting | 1 dia diem va 1 background sach; bo prop neu khong can cho pose |
| Composition | portrait 3/4 hoac full-body tuy outfit; can headroom, crop va negative space ro |
| Camera | portrait 85mm; full-body fashion 50-85mm; phone/story 24-28mm equivalent; them camera height, goc nghieng va huong nhin |
| Light | 1 key + fill/bounce phu hop setting; neu studio thi soft key va controlled fill |
| Color | natural neutral color voi palette phu hop outfit; white balance, contrast, saturation va skin-tone handling ro |
| Style | photoreal commercial hoac candid snapshot, chon 1 va giu nhat quan |

Hoi lai chi khi thieu hard lock ma sai se hu y (nam/nu hoac identity, SFW vs explicit, reference anh co bat buoc khong, mot trong nhieu subject). Toi da 3 cau. Neu van co the lam prompt hop ly, ghi infer va tiep tuc.

### 1. Plan theo 10 lop, render theo nen tang

Dung 10 lop nhu checklist noi bo; chi xuat chi tiet co the thay trong khung hinh. Khong bat prompt paste phai chua du 10 lop. Chon nen tang va che do text-to-image/reference/edit truoc khi flatten; doc `references/platform-map.md`. Doc `references/expansion-recipe.md` de chon chi tiet phu hop; doc `references/production-spec.md` khi can schema JSON; doc `references/guided-form.md` khi user dung command PROMPT FORM; doc `references/json-form.md` khi user dung JSON FORM.

1. Intent and identity - subject, adult age band only when known, region/ethnicity only when provided, expression, hair, face, skin texture; pores and peach fuzz only when realism is wanted.
2. Body language - stance, weight distribution, hip/shoulder axis, torso rotation, elbow and wrist position, gaze direction, expression, contact with wall/prop.
3. Wardrobe construction - garment type, fit, neckline, hem, seam path, closure, layering, coverage, proportions and visible wear.
4. Textile and material physics - fiber or yarn character, weave/knit density, stitch direction, edge finish, thread tension, compression, folds, creases, contact shadows, specular response and micro-texture. Only claim individual fibers when framing and light could reveal them.
5. Setting - one location, background surface, depth separation, environmental color and only the props needed for the pose.
6. Composition - orientation, shot size, subject placement, headroom, crop, negative space, visual balance and background lines.
7. Lighting - source type and size, key position, fill/bounce/rim if needed, softness, wrap, shadow density, highlight behavior and a consistent viewer/subject coordinate system.
8. Camera and perspective - camera height, pitch/yaw, viewpoint, lens, camera distance, perspective control, aperture/DoF, motion treatment when relevant and aspect ratio.
9. Color and finish - palette, white balance or warm/cool bias, saturation, contrast, tonal roll-off, skin-tone protection, film/digital character, restrained grain and retouching limits.
10. Negative and constraints - anatomy, hands, material errors, unwanted text/logo/watermark, extra people, wrong age, wrong wardrobe, wrong lighting and any user-specified exclusions.

Chi tiet camera va mau phai phuc vu cung mot concept. Khong gan 85mm, shallow DoF, film grain va high contrast vao moi prompt mot cach may moc.

### 2. Xuat dung format nay

Day la format tong quat; voi nen tang dich da ro, prompt cua nen tang do la artifact chinh. NEGATIVE, PLATFORM VARIANTS va OPTIONAL SHORT chi them khi co ich/duoc yeu cau; Flow khong can khoi negative dai rieng. Ghi chu va nhan giai thich theo ngon ngu chinh cua user. Prompt flatten mac dinh English de nhat quan, khong khang dinh luon tot hon tieng Viet, tru khi user yeu cau prompt theo ngon ngu dau vao; khi do them ban English neu no giup model hieu tot hon. Neu user dua JSON hoac yeu cau format co cau truc, them `PRODUCTION SPEC` theo `references/production-spec.md`; `raw_prompt` trong spec phai trung y voi FLAT PROMPT.

```text
INTENT LOCK
- Giữ: ...
- Infer (co the sua): ...

PRODUCTION SPEC (optional)
{ ... }

FLAT PROMPT
[mot doan theo budget nen tang trong platform-map; giu hard lock, khong them chu cho du dai]

NEGATIVE
[mot dong]

PLATFORM VARIANTS
- Nen tang dich: prompt va settings phu hop; chi them variant khac khi user can
- Grok Imagine: ...
- OpenClaw / Hermes / Codex / OhMyPi: ...

OPTIONAL SHORT
[ban 50-90 tu neu user can prompt ngan]
```

Doc `references/platform-map.md` de chinh variant. Khong viet 7 ban dai giong nhau. Dieu chinh thu tu, muc chi tiet, reference workflow va constraints theo nen tang; khong chi doi wrapper. Flow/Nano Banana: doc `references/flow-nano-banana.md`.

### 2c. JSON standalone cho nhieu anh

Neu user muon tao `n` anh, muon copy tung prompt vao nen tang, hoac bao mot JSON nhieu anh bi render thanh triptych/collage:

- Uu tien `n` prompt phang rieng tren web UI. Chi doc `references/standalone-json.md` va tao JSON khi user yeu cau format do.
- Neu xuat JSON, tao `n` JSON object doc lap; moi object co mot `image_id`, `output.image_count: 1`, mot scene, mot `prompt`, va mot `negative_prompt`.
- Khong dung mot root `image_prompt_set` voi `images` array lam artifact copy/paste chinh. Neu can manifest, dat no sau cac object doc lap va noi ro no chi la muc luc.
- Viet prompt tu du trong moi object. Khong dung cac cau tham chieu chua du thong tin nhu `same character defined in the consistency lock` neu lock do nam ngoai object.
- Dung mot rang buoc ngan `One photograph, no text` va image-count trong UI. Chi them `no collage` khi gap loi panel; khong lap guardrail dai o dau va cuoi.
- Neu user muon giu cung khuon mat, giai thich rang text/seed/session khong dam bao mat giong tuyet doi. Uu tien tao mot master reference image roi dinh kem cung anh do cho cac lan sau; giu cung model, wardrobe, makeup va settings khi co the.
- Neu user yeu cau JSON va chi ro Grok hoac gioi han do dai, xuat compact standalone JSON voi `image_id`, `task`, `output`, `prompt`, `negative_prompt`; cat metadata giai thich truoc, khong cat cac hero details.

### 2a. Platform safety va xu ly tu choi

Doc `references/platform-safety.md` khi user chi ro nen tang, khi prompt co tinh tu sexy/sexual, hoac khi user bao bi tu choi. Phan tich theo co che cong khai va dau hieu quan sat duoc, khong khang dinh biet tu khoa bi cam noi bo hay dam bao prompt se duoc chap nhan.

- Nhan dien cum rui ro: tuoi mo ho (`girl`), tu vung tinh duc, quan ao de lo, body-focus, pose/anh mat mang ham y tinh duc, va negative prompt lap lai tu nhay cam. Cac cum nay co the bi danh gia theo ngu nghia o ca buoc input lan output; mot prompt co the cho ket qua khac nhau tuy UI, model, tai khoan va cai dat.
- Neu user bao bi tu choi, giu lai cac hard lock khong nhay cam (chu the truong thanh, quoc gia, nghe nghiep, toc, mau da, tattoo, setting, light) va viet `SAFE RETRY` bang ngon ngu tich cuc, trung tinh. Vi du: `sexy` -> `confident fashion portrait`; `seductive gaze` -> `direct, self-possessed gaze`; `deep-V/cleavage/lingerie` -> `fully clothed fitted workwear with a tasteful neckline`; `girl` -> `adult woman`.
- Với GPT Image/ChatGPT va Gemini, khong dat danh sach negative co cac tu `nudity`, `sexual`, `pornographic`, `fetish`, `cleavage`... neu khong can thiet; hay dung rang buoc duong tinh nhu `fully clothed professional fashion portrait`, `one adult subject`, `no text`. Negative prompt van co the duoc doc boi bo loc.
- `SAFE RETRY PROMPT` phai la prompt cu the, ngan hon (thuong 60-120 tu), giu du hard lock khong nhay cam va khong lap lai cum tu nhay cam trong prompt/negative. Kiem tra lai chu the, setting, hair, skin, tattoo, gaze va light truoc khi xuat.
- Khong de xuat doi `moderation`/safety threshold sang muc it chat hon de vuot bo loc. Neu nguoi dung lap trinh bang API, chi mo ta day la tuy chon cau hinh cua API va phai van tuan thu chinh sach; khong xem do la cach bypass.
- Neu SAFE RETRY van bi chan, chi lam toi da mot lan nua bang cach bo cum rui ro lon nhat; khong tiep tuc spam, khong obfuscate prompt, va noi ro rang ket qua phu thuoc nen tang. Neu y dinh explicit, khong viet prompt workaround; de xuat ban fashion portrait khong explicit.

Khi output sau loi tu choi, them cac khoi sau sau `PLATFORM VARIANTS`:

```text
PLATFORM SAFETY NOTE
- Nen tang muc tieu: ...
- Cum co the gay false positive (khong chac chan): ...

SAFE RETRY PROMPT
[prompt tich cuc, ngan hon, khong dung tu nhay cam khong can thiet]

REMOVED OR NEUTRALIZED
- ...
```

### 2b. Output rieng cho PROMPT FORM

Khi o guided mode, chi xuat mau dien sau va dung lai cho user dien. Khong xuat `FLAT PROMPT` trong cung luot, tru khi user yeu cau prompt cuoi ngay.

Render mau ben duoi theo ngon ngu dau vao. Khong copy nguyen tieu de tieng Anh neu user dang viet tieng Viet; hay dich ca huong dan trong ngoac va cac lua chon de user chi can doc va dien.

```text
PROMPT DETAIL FORM
- Raw idea: ...
- Locked: ...

1. Subject and identity
- Age: [LOCKED / SUGGESTED / FILL / OPEN]
- Region or ethnicity: [LOCKED / SUGGESTED / FILL / OPEN]
- Beauty direction: [natural commercial / soft elegant / sharp editorial / cute adult / sensual fashion / custom]

2. Face, hair, skin
- Eyes: [AUTO / FILL]
- Nose and lips: [AUTO / FILL]
- Hair: [AUTO / FILL]
- Skin and makeup: [AUTO / FILL]

3. Pose and framing
- Pose and weight shift: [AUTO / FILL]
- Hands: [AUTO / FILL]
- Gaze and expression: [AUTO / FILL]
- Shot size and crop: [AUTO / FILL]

4. Wardrobe and textile
- Garment and color: [LOCKED / FILL]
- Fit and neckline: [AUTO / FILL]
- Material: [AUTO / FILL]
- Fabric behavior and micro-texture: [AUTO / FILL]

5. Scene, light, camera and color
- Setting/background: [AUTO / FILL]
- Light direction and softness: [AUTO / FILL]
- Camera height, angle and lens: [AUTO / FILL]
- Palette, white balance and contrast: [AUTO / FILL]

6. Constraints
- Must keep: ...
- Must avoid: ...
- Text/logo/jewelry/props: [AUTO / INCLUDE / EXCLUDE]

Reply with edited values, or say `giu tat ca goi y`.
```

Neu user khong dien mot truong, giu gia tri `AUTO` va infer o buoc final. Neu user ghi `OPEN`, khong them chi tiet cu the vao truong do. Neu user ghi `FILL` nhung de trong, hoi lai toi da 3 diem co anh huong lon nhat.

### 3. Quy tac flatten

- 1 subject, 1 outfit, 1 location, 1 light setup.
- Chi tiet hero dat truoc (mat/identity + outfit/material + pose/composition), setting, light, camera, color va negative sau.
- Bo chu beautiful / gorgeous / sexy neu da mo ta cut + light + fabric. Sexy cua user dich thanh silhouette, neckline, pose - khong lap tinh tu.
- Neckline. User noi "deep-V hoac V-neck" thi chon mot (mac dinh deep V neu ho xin sexy; classic V neu ho xin clean) va ghi ro do sau (to sternum / modest V). Khong doi neckline user da chot.
- Khong tu them body measurements. Neu user da cho 90-60-90 hoac ty le cu the, giu y do trong `Intent Lock` va co the viet lai thanh silhouette tu nhien trong FLAT PROMPT.
- Mo ta vat lieu theo thu tu: chat lieu -> cau truc weave/knit -> huong soi/mui -> tension/fold -> phan xa anh sang. Khong viet "tung soi vai" neu khung hinh qua xa de thay.
- Chon crop va viewpoint ro; chi them lens/camera height/yaw/pitch khi chung thuc su giup bo cuc. Tranh thong so may anh day dac tren Flow/Grok. Neu chi noi portrait, chon crop va viewpoint hop ly; lens la tuy chon.
- Mau anh can palette va skin-tone handling phu hop; chi them white balance/saturation/contrast khi can; khong chi ghi "cinematic" hoac "beautiful colors".
- Khong ghi ultra 8K RAW, masterpiece, best quality. Neu user yeu cau syntax cua mot platform, giu syntax do o wrapper tuong ung.
- Snapshot dien thoai: them slight noise, phone look. Studio: clean wall and controlled light; use 85mm for a portrait crop or 50-85mm for full-body.

## Neu user dua JSON san

Doc JSON nhu mot spec, khong coi cac truong thieu la ly do de tu phat minh. Rut hero details, giai conflict (unposed vs selfie, 6 panel vs 1 face lock), xuat lai PRODUCTION SPEC neu format nay co ich va luon xuat FLAT PROMPT. Ghi 3 dong da cat / da giu.

- Giu cac field user da cung cap: `metadata.context`, `intent`, `image_parameters`, `raw_prompt` va cac chi tiet tuong duong.
- Khong gan `source_author` neu user khong cung cap; dung `null` hoac bo field.
- Them cac field con thieu chi khi la infer nhe va ghi ro trong `Intent Lock`.
- Neu JSON co mau thuan, uu tien hard lock ro rang hon va ghi conflict da giai quyet.
- Neu user dua JSON mau chi de minh hoa, khong sao chep ten, nguoi, outfit, so lieu, hoac noi dung mau vao prompt moi.
- Neu JSON co nhieu anh va user muon render rieng, tach thanh cac standalone object truoc khi xuat. Lap lai character/wardrobe lock trong tung object va bo cac tham chieu den lock ben ngoai object. Xem `references/standalone-json.md`.

## Neu user xin nhieu panel / cung 1 mat

Khong nhoi 6 o mot prompt. Xuat:

1. Face-lock block lap lai moi panel
2. Wardrobe-lock
3. Tung panel 1 goc + 1 noi + 1 light
4. Huong dan gen tung la + cung reference

## Sau khi giao prompt

Dung. Cho user sua o Infer. Chi gen anh khi user yeu cau tao anh hoac da uy quyen thu truc tiep tren nen tang. Neu user chua xin gen, khong goi image generation tool.
