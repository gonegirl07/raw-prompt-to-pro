# Motion transfer — dance, yoga, đạp xe

CANDIDATE UNTESTED • 2026-10-14. Nghiên cứu trên X và tài liệu Flow/DeepMind. Chưa có run trên canvas Linh, chưa tiêu credit, không phải motion pass.

Đọc file này trước [motion-social-kit.md](motion-social-kit.md) và các mẫu chữ C/Y/D trong [flow-video-prompt-kit.md](flow-video-prompt-kit.md) khi động tác là vũ đạo, đổi pose yoga, hoặc vòng đạp xe. Mẫu chữ vẫn là fallback.

## Quyết định đường đi

Chọn một đường. Không trộn prompt chữ dài vào đường có video mẫu: chữ mô tả lại vũ đạo tranh vai với video và làm motion kém hơn trong các test cộng đồng.

| Điều kiện thật | Đường | Không làm |
|---|---|---|
| Surface nhận video (Flow Ingredients hoặc video-to-video) và có clip động tác được phép dùng | A dance/yoga, hoặc B đạp xe | Không viết thêm bước chân, BPM, slash command |
| Yoga, không có clip, Flow có first frame và last frame | C, một chuyển pose | Không nhồi cả chuỗi vào một generation |
| Canvas chỉ gửi first frame, hoặc không upload được video | Fallback chữ: C-R/D-R hoặc Y1–Y3/D1–D3/C1–C3 | Không gọi đó là motion transfer |

Hỏi đúng một câu nếu không rõ surface có nhận video hay không. Nếu user nói “Flow” nhưng tác vụ đang chạy trên canvas Linh đã ghi chỉ có first frame, dùng fallback và nói rõ vì sao.

Mỗi clip một hành động chính và một camera — camera đó phải là camera của clip mẫu, không thêm orbit/dolly trong prompt transfer. Subject là người trưởng thành. Đây là cảnh biểu diễn, không phải hướng dẫn tập hay bằng chứng lái xe an toàn.

## Chuẩn bị asset

Clip mẫu:

- Toàn thân nếu nghiệm thu chân, tay, pedal hoặc điểm chạm thảm. Medium shot chỉ được dùng khi không nghiệm thu footwork.
- Camera gần như khóa, hoặc đúng đường camera muốn giữ. Ánh sáng đều, một người, không cắt cảnh.
- Mute. Không yêu cầu beat-sync trừ khi user xác nhận mode hiện tại đọc audio của clip và có file nhạc thật. Tag API `<VIDEO_REF>` được mô tả là bỏ audio.
- Độ dài gần với clip xuất. Tài liệu API (Philipp Schmid, 2026-08-27) ghi video reference khoảng 3 giây. UI Flow có thể nhận dài hơn; đọc giới hạn ô upload đang mở, không điền số trần vào prompt.
- User phải có quyền dùng clip: tự quay, stand-in vừa tạo, hoặc stock được phép. Không chỉ agent đi lấy dance của một người cụ thể trên mạng.

Ảnh nhận dạng:

- Cùng họ góc với frame đầu của clip mẫu khi có thể (front với front, 3/4 với 3/4).
- Đủ phần thân mà động tác dùng. Chân chạm sàn hoặc tay/chân ở vị trí bắt đầu hợp lý.
- Outfit đã chốt. Không lấy mặt và đồ từ người trong clip mẫu.

## Đường A — dance hoặc yoga, có clip mẫu

Gán vai trong UI: ảnh = nhân vật, video = motion. Prompt ngắn.

### MT-D / MT-Y

```text
Use the image as the only identity, outfit, and body reference.
Use the video only as the strict motion reference.
Keep the same timing, contacts, weight shifts, and full action.
Do not copy the reference person's face, clothes, or background.
The reference video is silent. No music. No on-screen text. No cuts.
```

Dance: thêm một câu nếu outfit khác hẳn biên độ clip mẫu, ví dụ `Keep the approved outfit, even where the reference clothing is tighter.` Không mô tả tên điệu.

Yoga: thêm `Keep both hands and both feet readable. Do not change the pose sequence.` Không thêm tên tư thế nếu clip đã chứa chuỗi đó.

QA: thứ tự động tác và điểm chạm giống clip mẫu hơn là giống tính từ trong prompt; mặt và đồ bám ảnh, không bám người mẫu; tay/chân không thừa ngón, không xuyên thân. Một frame đẹp không phải pass. Không lấy nhạc ghép sau làm bằng chứng sync.

## Đường B — đạp xe

Chữ “fast / RPM / brisk” không khóa số vòng. First frame và last frame cũng không khóa số vòng. Tách làm hai generation.

### B1. Stand-in, chưa phải Linh

Tạo clip người trưởng thành trung tính, không cần mặt Linh, trên làn thẳng hoặc nền tách được. Chọn đúng góc sẽ giữ (front 3/4 hoặc side). Hai tay trên hoods, hai bàn chân đối pha trên pedal, xe song song làn.

```text
An adult cyclist rides a road bicycle along a straight lane, green-screen background.
Both hands stay on the brake hoods. Both feet stay on opposite pedals and keep alternating full circles for the whole clip.
The bicycle remains parallel to the lane. The camera holds a steady front three-quarter view with the full pedaling action visible.
No cuts. No music. No text.
```

Đổi `front three-quarter` thành `exact side view, bicycle facing right` nếu task cần side. Duyệt stand-in trước: có vòng pedal thật, lốp tiếp xúc mặt đường, xe không đi chéo. Fail thì sửa stand-in, không nhảy sang bước thay người.

### B2. Thay người, giữ động tác — MT-C

Video-to-video Omni. Ảnh Linh đã duyệt là identity. Prompt bám cách nói của Flow:

```text
Use the source video as the strict video-to-video motion reference.
Keep the same shot order, timing, camera movement, framing, and full pedaling action.
Replace the rider with the woman from the image. Keep her face, body, and outfit.
Both hands stay on the hoods. Both feet stay on the pedals.
Do not copy the stand-in face. No music. No text.
```

QA: pha pedal và hướng làn còn như stand-in; mặt/đồ là Linh; không claim nhanh hơn stand-in nếu chu kỳ không đo được. Cadence chữ trong [motion-social-kit.md](motion-social-kit.md) chỉ dùng khi không có stand-in.

## Đường C — yoga không có clip

Không viết sun salutation đủ bộ trong một prompt. Làm một ảnh pose sheet hoặc hai ảnh pose đã duyệt, rồi Frames to Video: ảnh đầu = first frame, ảnh cuối = last frame. Một chuyển động giữa hai pose. Chuỗi dài là nhiều clip 4–6 giây nối lại, mỗi clip một chuyển.

Pose sheet, nếu cần tạo ảnh trước (2 hoặc 3 ô, không hơn trong một lần):

```text
One monochrome instruction sheet, 1x3 grid, equal panels, white background.
The same adult woman from the attached identity reference, same face and body proportions, in [OUTFIT].
Panel 1: [POSE_A], full body, both hands and feet visible.
Panel 2: the midpoint between the two poses, full body.
Panel 3: [POSE_B], full body, both hands and feet visible.
No extra people, no color scene, no text except the panel numbers 1, 2, and 3.
```

Chỉ điền pose user đã chốt. Không tự chọn bài tập.

### MT-Y-KEY

```text
Animate from the first frame to the last frame in one continuous move.
She changes from the start pose to the end pose once, with the hands and feet following a clear path.
Keep her identity, outfit, and the room from the frames.
The camera stays fixed and full-body. No cuts. No music. No text.
```

QA: pose cuối khớp last frame; không thêm pose thứ ba; điểm tựa không trượt xuyên thảm. Sheet này là giả thuyết chuyển từ workflow pose-sheet (đã thấy trên model khác, chưa thử trên Omni).

## Việc không đưa vào prompt

- Slash command và mã rút gọn (`/dollyin`, `motion0`, `ref+camera`).
- BPM, `body synced to the beat`, timestamp dài, khi audio không được truyền thật.
- `natural movement`, `realistic physics`, `fluid lifelike motion` thay cho clip mẫu.
- Negative dài. Câu cấm mặt/đồ của người mẫu đã nằm trong prompt dương.
- Tag XML `<VIDEO_REF_0>` trừ khi user đang gọi đúng surface API có các tag đó. Trên ô prompt Flow, câu tiếng Anh phân vai ở trên là đủ; không bịa rằng UI bắt buộc XML.
- Hứa face-lock, khóa cadence, hoặc khớp nhạc.

## Output của agent

Khi đi đường A, B, hoặc C, xuất đúng các khối sau. Không xuất pipeline 10 lớp ảnh. Không tự generate media nếu user chỉ xin prompt.

```text
SURFACE
- Flow UI hay canvas; model; mode nhận video hay chỉ first frame.

ASSETS
- Ảnh: vai trò, góc, phần thân phải thấy.
- Video hoặc cặp frame: độ dài, mute, một động tác. Nếu không có, ghi fallback nào và vì sao.

FLAT PROMPT
- Một khối MT-D, MT-Y, MT-C, hoặc MT-Y-KEY. Stand-in B1 là một prompt riêng, không dán chung với B2.

QA
- Ba điểm sẽ nhìn trên clip xuất. Ghi CANDIDATE UNTESTED nếu chưa có media của chính task.
```

## Nguồn

Đối chiếu 2026-10-14, chưa chạy lại trên canvas:

- [Flow: green-screen rồi video-to-video](https://x.com/FlowbyGoogle/status/2085812758726262812). Câu “strict video-to-video motion reference” lấy từ bài này.
- [Philipp Schmid: tag vai, VIDEO_REF, timecode](https://x.com/_philschmid/status/2093012878211072183). Trần 3 giây và việc bỏ audio là mô tả API, không chép thành luật UI.
- [Flow: prompting Omni, pose/motion từ video sang ảnh](https://x.com/FlowbyGoogle/article/2105398907702595802).
- [DeepMind: prompt guide Omni](https://deepmind.google/models/gemini-omni/prompt-guide/).
- [Flow Help: model và Ingredients / video-to-video](https://support.google.com/flow/answer/16352836?hl=en).
- Test cộng đồng, không phải bằng chứng của canvas này: chuyển motion yoga/dance trong Flow ([video 2026-06-04](https://www.youtube.com/watch?v=ghSkittZd2k)); prompt ngắn, clip dance mute, ảnh là look ([video 2026-10-03](https://www.youtube.com/watch?v=OVKvmntv1xI)).
- Pose sheet nhiều ô là ý từ workflow model khác ([bài 2026-04-28](https://x.com/egeberkina/status/2049143762929021290)), mới hạ xuống 2–3 ô cho first/last frame. Chưa thử trên Omni.
