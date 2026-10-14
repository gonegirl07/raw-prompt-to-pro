# Bổ sung motion và social content — v1.1, 2026-10-09
Phối hợp MCP Gemini3.8FlashHigh, Codex biên tập. **CANDIDATE UNTESTED**: chưa chứng minh cải thiện độ ổn định. Đọc phần này trước các mẫu cycling cũ của v1.0 khi làm Linh đô thị.

## Quy tắc chọn và chuẩn bị
- Chốt mục tiêu clip: action nào phải nhìn thấy, ảnh nào mang identity, phần nào dựng sau. Mỗi clip một hành động chính + một hành vi camera; động tác chân/thân/tay phối hợp trong cùng hành động không cần tách thành nhiều shot.
- Mẫu I2V bên dưới chỉ dùng sau khi ảnh đầu đúng pose/góc/props đã QA. Chuẩn bị ảnh trực tiếp từ reference đã duyệt, giữ body/outfit user chốt. Dance/fashion/social dùng outfit/location riêng đã chốt, không tự áp croptop Linh.
- Nêu camera tương đối với chủ thể; chuyển động đường/tóc không thay bằng chứng chân quay đúng. Một static shot có thể phù hợp dance/demo; không mặc định toàn video phải tĩnh.
- Ảnh đầu sai identity phải sửa trước video. Tuy nhiên, ảnh đầu đẹp cũng không bảo đảm mặt giữ đúng giữa clip. Không biến Run17 đứng thành firstFrame đang đạp: Run17 là reference identity, Run23front là ảnh đầu lịch sử.
- Linh: front3/4 đô thị là góc ưu tiên theo user, chưa motion pass. Side đô thị chật bị loại cho task này; đây không phải cấm sideview trên mọi cycling task.
- Camera/cadence/identity là yêu cầu cần đo, không là kết quả đã được control. Không suy trầnmodel/attention/trainingdata từ vài run. First/lastframe không xác định số vòng của một chuyển động tuần hoàn.
- Trước Generate đọc resolvedprompt, firstFrameID, settings thật. Current canvas chỉ có bằng chứng firstFrame và model cố định; không hứa có motionreference/seed/ingredients.
- Planned settings cho test ngắn nếu user yêu cầu: canvas4s9:16360p. Fashion/dance cần động tác dài hơn thì chọn duration thật phù hợp, không ép4s. Settings ở UI ngoài prompt. Không tạo media trong yêu cầu chỉ research/prompt.

## Cycling / đi xe
Ba mẫu cho xe đạp road theo task Linh; không dùng mô tả pedal cho xe máy/ô tô. Với phương tiện khác, khóa người lái, vị trí tay/chân, đường chạy và camera theo phương tiện thực; không suy claim an toàn/lái xe từ video AI.

### C-R1: phối hợp ngoài yên, ưu tiên giữ bodyresponse
Ảnh đầu: front3/4 đúng làn; hông đã nhấc nhẹ, bàn chân gắn hai pedal đối pha, hai tayhoods. Nếu dùng ảnh ngồi Run23 thì giữ baseline Run26 có chuyển từ ngồi lên; không gọi đó là test nhanh ngay frame0.
```text
She rides out of the saddle along the straight lane, transferring her weight over each descending pedal as both feet continue complete alternating circles. Her hips shift with each stroke, the bicycle leans slightly beneath her, and her bent elbows respond while both hands stay on the brake hoods. Her gaze remains on the road and her shoulders and hair follow the repeated effort. The camera travels ahead at the existing front three-quarter angle, keeping the full pedaling action visible throughout the uninterrupted take.
```
QA: bodyresponse Run26 phải còn; không tự claim clip này nhanh hơn.

### C-R2: cadence clause — chỉ ghép vào baseline để thử
Ảnh đầu/camera/settings đúng baseline. Đây là bản hoàn chỉnh cho ảnh đã ngoài yên; muốn cadence-only A/B trên Run26 phải giữ exactRun26 và chỉ đổi clause, xem [video-stability-test-plan.md](video-stability-test-plan.md).
```text
Already riding out of the saddle, she repeats compact, brisk pedal rotations at a continuous pace from the opening through the final frame. Both feet stay on opposite pedals through complete circles. Her weight follows each descending pedal, her hips shift over the supporting foot, and her bent elbows respond to the bicycle's slight alternating lean. Both hands remain on the brake hoods and her gaze stays on the road. The camera continues ahead at the established front three-quarter angle with the same lead distance.
```
QA: khoảng chu kỳ đầu/giữa/cuối, footcontact/bodyresponse. Từ brisk không bảo đảm0.6–0.7s/vòng; không thêm target số vòng đãfail như cam kết.

### C-R3: camera tiến chậm hơn xe — thử riêng sau motionpass
Ảnh đầu front3/4 có khoảng tiến và đủ xe. Không đổi gesture/cadence cùng lúc để rồi quy mọi cải thiện cho camera.
```text
She continues the established pedaling action straight along the lane, with both feet circling on the pedals and both hands on the brake hoods. Her hips, elbows and shoulders respond to the alternating effort. The camera retreats along the lane slightly more slowly than the bicycle, allowing her to approach gradually and become larger in the frame while preserving the front three-quarter view. The shot ends before any part of the rider or bicycle is cropped.
```
QA: subjectscale tăng thật, roadgeometry không đổi/xe không đi chéo. Không bắt rời khung để tạo chuyển cảnh.

## Dance
Ảnh chuẩn: người trưởng thành, outfit/location đã chốt, toàn chân/tay, contactshadow đọc được, khoảng trống phù hợp bước. Không có nhạc/reference truyền thật thì không claim beat-sync.

### D-R1: step-touch có điểm tựa
```text
She repeats a lively step-touch to the right and left. Each sideways step takes her weight before the trailing foot taps beside it; the tapping foot stays light. Her knees soften with each transfer, her torso follows the supporting side, and relaxed arms counterbalance the steps. Her head and hair respond to the groove without a held pose. The camera holds a fixed full-body frontal view with both shoes and their floor contact visible.
```
QA: weighttransfer/supportfoot, không footslide, cadence không tự chậm đuôi.

### D-R2: groove vai, một camera tĩnh
Ảnh medium thấy hai tay; chỉ kiểm torso, không lấy làm bằng chứng footwork.
```text
With her feet remaining planted, she grooves through alternating shoulder accents and small ribcage shifts. Soft knee pulses support the upper-body rhythm, and her forearms move loosely with the alternating sides. Her head follows the motion lightly and her expression stays responsive rather than frozen. The camera holds the existing medium framing at a constant height, keeping both hands visible for the entire continuous shot.
```
QA: vai/ribcage có phối hợp, tay không xuyên body, face không morph.

### D-R3: bước chéo rồi trở lại
Ảnh toànthân có chỗ bước phía trước.
```text
She steps her right foot diagonally forward, transfers her weight into a soft bend of that knee, then pushes from the floor to return to her starting stance. She repeats this compact diagonal step with relaxed arms counterbalancing the change of direction. Her torso and hair follow the momentum as her supporting foot remains grounded. The camera stays fixed in the full-body view so the outward and return paths of both feet remain clear.
```
QA: path về đúng vị trí, không glide; không ép spin/đổi mặt.

## Fashion
Chốt garment/material từ reference thật trước ảnh đầu. Chuyển động phải lộ chi tiết muốn giới thiệu; không mặc định runwaycrossstep, áo croptop hay sexy.

### F-R1: đi tự nhiên, camera lùi
Ảnh toànthân bắt đầu bước, giày/mặt rõ, đường đi đủ dài.
```text
She walks forward with a natural, even stride, rolling her weight through each supporting foot before the next step lands. Her arms swing comfortably in opposition to her legs, while the garment and hair respond to the steps according to their visible weight. Her gaze follows the walkway. The camera retreats directly ahead at a matching pace, maintaining a full-length view of the outfit and shoes.
```
QA: heel/toecontact không slide, phom/seam/accessory không biến đổi.

### F-R2: shift trọng tâm để lộ phom
Ảnh đứng thấy toàn phần đồ cần review; không cần bước/spin mới.
```text
She settles her weight onto her left leg while her right knee relaxes, letting the garment's folds adjust and come to rest. Her shoulders respond with a small natural shift and her arms remain comfortably beside her body. Her gaze stays in the direction established by the starting frame. The camera holds its position and framing so the change in silhouette and fabric drape remains visible.
```
QA: thay đổi do body thật, không thaygarment/bodyratio.

### F-R3: góc bên nhẹ
Ảnh đứng cân bằng; muốn fullback tạo ảnh/góc riêng.
```text
With both feet settled on the floor, she makes a small balanced turn toward the side to reveal the garment's side silhouette. Her head follows the shoulders comfortably instead of twisting back to hold eye contact. Her arms stay relaxed as the hem and hair settle from the adjustment. The camera remains fixed, keeping the complete outfit in frame throughout the turn.
```
QA: mặt/hairline/body so cùng góc; không claim góc nhỏ là viewlưng đầyđủ.

## Facebook/TikTok footage
Đây là clipnguồn độc lập; caption/brand/CTA đặt trong editor. Nếu cần thoại, chỉ viết khi user có script và model/mode thực hỗ trợ, kiểm lipsync; không tự thêm lời nói.

### S-R1: chi tiết outfit, một thao tác vải
Ảnh medium thấy [GARMENT_DETAIL] và hai tay; điền bằng chi tiết có thật user muốn giới thiệu.
```text
She gently smooths [GARMENT_DETAIL] once with one hand, allowing the material to settle naturally before releasing it. Her other hand stays relaxed and her posture remains balanced. The gesture makes the existing fabric and construction visible without changing the garment. The camera holds the established detail framing, keeping the active hand and the garment feature fully visible.
```
QA: hand/fabric không merge; chi tiết và texture còn đúng reference.

### S-R2: phản ứng với một tác nhân
Ảnh medium người đang chú ý lệchlens; tác nhân [APPROVED_OFF_CAMERA_CUE] phải chốt ngoài prompt, không ép cười/gật hai lần mặc định.
```text
She notices the off-camera cue and briefly meets the filmer's gaze in acknowledgment. Her eyebrows and expression respond with a small, relaxed change, while her head makes only a slight adjustment and her shoulders remain comfortable. The camera holds the existing eye-level medium view so the reaction reads clearly as one brief response.
```
QA: timing stimulus→response do edit/cue thực; không chỉ smilepose và không fabricate testimonial.

### S-R3: trình bày sản phẩm user cung cấp
Chỉ dùng khi [USER_PROVIDED_PROP] có ảnh/asset thật; ảnh đầu item được giữ vững hai tay, label cần chính xác phải compositing/editor. Không tự thêm chức năng/claim.
```text
Holding [USER_PROVIDED_PROP] securely in both hands, she makes one small controlled turn of the item to reveal its side profile. Her fingers preserve their grip and the object's shape stays consistent while her arms support it comfortably. The camera remains fixed on the established presentation framing, keeping the item and both hands visible throughout the turn.
```
QA: itemshape/handcontact/branddetail không morph, không nhận nhãn chữ AI sai là sảnphẩmđúng.

## Lớp sản xuất ngoài generation
Theo [TikTok](https://ads.tiktok.com/resources/help/article/creative-best-practices?redirected=1) và [Meta Reels](https://www.facebook.com/business/ads/facebook-instagram-reels-ads), ưu tiên khung dọc9:16, audio và vùng giao diện không che thông tin. Đây là tài liệu quảng cáo; không bảo đảm organic engagement. TikTok khuyến nghị ít nhất720p cho creative ads; draft360p của ta chỉ là bản kiểm motion.

| Lớp | Cách chuẩn bị | Cách kiểm |
|---|---|---|
| Hook | Mở bằng động tác/chi tiết đã đạt, nêu giá trị sớm | Khán giả hiểu đề tài; không dùng speedramp che cadencefail |
| Action | Một hành động nguồn đọc rõ | Motion/contact/identity trước nhạc |
| Payoff | Chi tiết/biểu cảm/kếtquả có thật footage | Không tự bịa tính năng/chứng thực |
| CTA | Chọn một hành động phù hợp mục tiêu user | Thêm chữ trong editor, giữ gọn |
| Caption/graphics | Copy riêng, chữ render sau | Preview từng placement, caption length và UIoverlay |
| Audio | File dùng được, mix không lấn thoại nếu có | Beatcuts dựa track thật; không claim generationbeat-sync |

Timeline minh họa15s: hook0–2/action2–9/payoff9–12/CTA12–15. Đây là heuristic dựng, không thông số platform và không là prompt15s trong mộtgeneration. Có thể đổi theo thể loại; không ép yoga/dance/riding cùng nhịp cuts.
Đặt subject/tay/props và text quan trọng ở vùng đọc được khi preview Facebook/TikTok placement thực; không ghi phầntrăm/pixel chung như bảo đảm safezone. Không crop mất chân để qua QA.
Test sáng tạo social sau motionpass: cùng footage/bodycopy/CTA, đổi hook A vs B; chỉ đo retention/conversion khi user chạy và cung cấp số liệu. Không tự đăng bài/chạyads.

