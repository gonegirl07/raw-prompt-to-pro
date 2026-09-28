# Platform safety va refusal recovery

Tai lieu nay dung khi user nham mot nen tang cu the, muon giam false positive, hoac bao prompt tao anh bi tu choi. Muc tieu la viet lai ro rang, an toan va giu y dinh hop le; khong tim cach vuot guardrail.

## Co che nen tang co the quan sat duoc

- Cac dich vu tao anh co the loc ca prompt dau vao va anh dau ra. Loi tu choi khong phai luc nao cung chi do mot tu; cum ngu nghia moi la yeu to quan trong: tuoi mo ho, tinh duc, quan ao de lo, body-focus, pose/anh mat ham y va negative prompt lap lai tu nhay cam.
- Ket qua khong hoan toan xac dinh: UI, model, tai khoan, khu vuc, anh tham chieu va cai dat co the lam cung prompt cho ket qua khac nhau. Khong tuyen bo biet danh sach tu khoa noi bo va khong dam bao prompt se duoc chap nhan.
- OpenAI image models loc prompt va anh theo content policy; tai API, tai lieu co `moderation` voi `auto` la mac dinh va `low` la tuy chon it chat hon. Loi block co the cho biet stage `input`/`output`/`unknown` va nhan danh muc thô nhu `sexual`; khong xem `low` la cach bypass va UI ChatGPT co the khong cho chon tham so nay. Tham khao: https://developers.openai.com/api/docs/guides/image-generation
- Gemini co bo loc theo nhom nguy co, co the tra ve `promptFeedback`, `finishReason: SAFETY` va `safetyRatings`; mot so safety threshold co the cau hinh trong API/AI Studio nhung van phai dung trong muc dich hop le. Gemini khuyen mo ta duong tinh va dung semantic negative prompt: mo ta canh mong muon thay vi lap lai danh sach dieu cam. Tham khao: https://ai.google.dev/gemini-api/docs/safety-settings va https://ai.google.dev/gemini-api/docs/image-generation
- Grok Imagine co API va content policy review, nhung tai lieu cong khai khong phai la bang tu khoa day du. Khong mac dinh Grok permissive hon hoac dung no de ne bo loc. Tham khao: https://docs.x.ai/developers/model-capabilities/imagine

## Ho so viet prompt theo nen tang

| Nen tang | Cach viet mac dinh | Can tranh |
|---|---|---|
| ChatGPT / GPT Image UI | Prompt ngan, tich cuc, cu the: `one adult woman`, `fully clothed`, `tasteful fashion portrait`, `confident`, `editorial`, `natural skin`. Giua lai toc, nghe nghiep, setting, light va camera. | Lap lai `sexy`, `sensual`, `seductive`, `erotic`, `provocative`, `hot`, `deep-V`, `cleavage`, `lingerie` trong prompt va negative; body measurements, body-focus, tu `girl` khi tuoi khong ro. |
| OpenAI Image API | Dung cung safe wording nhu UI. Neu can debug, phan biet loi input va output theo thong tin moderation cua API; khong coi moderation parameter la co che bypass. | Tu dong khuyen ha `moderation`, hoac dua tu nhay cam vao negative prompt chi de tranh no. |
| Gemini web / API | Mo ta duong tinh, mot subject truong thanh, quan ao day du, pose thoi trang; neu bi chan thi kiem tra `promptFeedback`, `finishReason` va `safetyRatings` khi API tra ve. | Dung negative list dai co `sexual`, `nudity`, `pornographic`; yeu cau model ignore safety; tu y doi threshold de vuot policy. |
| Grok Imagine | Co the dung FLAT PROMPT chi tiet hon, nhung van giu adult + fully clothed + fashion/editorial va khong viet ham y tinh duc. | Gia dinh Grok se luon cho qua hoac dung tu khoa obfuscation. |
| Midjourney / Flux / local | Theo syntax cua tung cong cu; `--no` chi nen dung cho loi hinh anh, khong chen danh sach tu nguoi lon de "danh lua" bo loc. | Keyword spam, prompt injection, doi ngon ngu/ky tu de che giau y dinh. |
| OpenClaw / Hermes / Codex / OhMyPi | Wrapper noi ro `one adult subject`, `fully clothed`, `professional fashion portrait`; neu bi chan thi viet lai mot lan theo safe profile. | Lap lai request vo han, tu dong goi generator sau khi bi chan, hoac huong dan bypass. |

## Quy trinh SAFE RETRY

1. Giu hard lock khong nhay cam: phu nu truong thanh, quoc gia, nghe nghiep, toc, da, tattoo, setting, anh sang va crop neu hop le.
2. Trung tinh hoa cac cum co rui ro:
   - `sexy` -> `confident fashion portrait` hoac `poised editorial presence`
   - `sensual/seductive gaze` -> `direct, self-possessed gaze`
   - `provocative` -> `stylish, confident, editorial`
   - `erotic` -> `non-sexual fashion portrait`
   - `deep-V/cleavage/lingerie` -> `fully clothed fitted workwear with a tasteful neckline`
   - `girl` -> `adult woman`
3. Dung goc chup chan dung ngang tam mat hoac 3/4; uu tien chest-up/waist-up trung tinh, tranh crop vao nguc, mong, eo va mo ta co the theo so do.
4. Viet rang buoc duong tinh thay vi negative co tu nhay cam: `fully clothed professional workwear`, `natural adult proportions`, `professional editorial portrait`, `one subject`, `no text`. Trong SAFE RETRY va NEGATIVE, khong lap lai cac tu `sexy`, `sensual`, `seductive`, `erotic`, `provocative`, `hot`, `deep-V`, `cleavage`, `lingerie`.
5. SAFE RETRY thuong 60-120 tu va phai giu lai moi hard lock khong nhay cam; dung checklist: one adult subject, nationality/identity, hair, skin, tattoos, gaze, setting, light, positive wording, no explicit acts, no sensitive-word negative list, no bypass instruction.
6. Neu van bi chan, bo cum rui ro lon nhat mot lan nua. Khong obfuscate, khong chen prompt an, khong spam retry; tong so lan retry toi da 2. Neu y dinh da explicit, dung lai va chi de xuat ban non-explicit neu nguoi dung muon.

## Mau output khi bi tu choi

```text
PLATFORM SAFETY NOTE
- Nen tang muc tieu: GPT Image / ChatGPT UI
- Cum co the gay false positive (khong chac chan): tu sexy/ seductive, mo ta co the va negative prompt co tu nudity/sexual.

SAFE RETRY PROMPT
[mot doan prompt tich cuc, ngan hon, adult, fully clothed, fashion/editorial]

REMOVED OR NEUTRALIZED
- ...
```
