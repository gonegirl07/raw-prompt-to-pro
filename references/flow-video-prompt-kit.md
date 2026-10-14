# Bộ prompt video Google Flow
Phiên bản 1.0 • 2026-10-08 • Phối hợp Gemini 3.8 Flash (High), biên tập và đối chiếu bởi Codex.

**Ứng viên chưa thử:** bộ này cải thiện cách viết và chuẩn bị, chưa chứng minh sửa được cadence, rolling hay drift Run19. Prompt trong code block để paste riêng từng cảnh. Không paste toàn bộ tài liệu vào generation.

Bổ sung 2026-10-12 từ thư viện Muse do user cung cấp: giữ các nguyên tắc mô tả chuyển động quan sát được, full-body/weight transfer cho dance và một camera operation; loại các magic phrase, slash command, BPM và lời hứa consistency chưa được xác minh. Xem phần đối chiếu nguồn trong `flow-video-research.md`.

Bổ sung 2026-10-09: đọc [MOTION-SOCIAL-KIT.md](MOTION-SOCIAL-KIT.md) cho 12 candidate cycling/dance/fashion/social và Facebook/TikTok production; đọc [STABILITY-TEST-PLAN.md](STABILITY-TEST-PLAN.md) khi kiểm độ ổn định. Với Linh đô thị, ưu tiên front3/4 theo feedback mới; C1 sideview bên dưới là lịch sử, không còn mặc định cho task đó. Run26 chỉ cải thiện tự nhiên một phần, Run28 vẫn chậm; chưa có motion quality pass. Các run20–28 đã xác minh có thể để text locks/notes blank, nhưng motion-only vẫn fail nên không coi adapter này là giải pháp.

## Cách dùng và giữ nhân vật

- Giữ ảnh chuẩn nhận dạng riêng với outfit và ảnh đầu từng cảnh. Linh: đối chiếu ảnh gốc, Run17 croptop đứng là ảnh user duyệt. Không suy số đo hay tự sửa thân hình. Yoga/dance/fashion chưa chốt outfit/set thì dùng [OUTFIT]/[LOCATION], không tự mặc đồ cycling.
- Chuẩn bị pose/góc từ ảnh nhận dạng đã duyệt. Không dùng ảnh đứng cạnh xe để yêu cầu ngay đang đạp, hoặc ảnh ngồi để nhảy đứng. Không regenerate thành chuỗi nhiều thế hệ nếu có thể quay lại master.
- Duyệt mặt/mái/hairline, body, đồ, tay/chân và đạo cụ trước video. So tỷ lệ ở góc và phối cảnh tương đương; không biến khác biệt lens thành số đo thật.
- I2V: ảnh đầu mang ngoại hình/bối cảnh; prompt tập trung motion. [Google best practices](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/video/best-practice?hl=en). Nếu cần câu ngắn: “Retain the person and outfit shown in the attached starting frame.” Không bảo đảm mặt tuyệt đối.
- Ingredients: gắn assets thật rồi mô tả vai trò từng ảnh. Tên Linh hoặc @Linh gõ trong text không chứng minh asset được gắn. First+last chỉ dùng khi mode hỗ trợ và hai pose có đường chuyển khả thi.
- Kiểm đầu/giữa/cuối clip. Frame cuối chỉ làm nguồn nối nếu tốt; không nối mù từ clip đã drift. Đổi góc lớn thì chuẩn bị ảnh đầu đúng góc.
- Settings đặt ngoài prompt: aspect, resolution, duration, model, output count. Canvas hiện có 4/6/8s Omni1.1 firstFrame; draft360p. 360p kiểm chuyển động, chưa đủ duyệt chi tiết mặt cho bản cuối.

### Canvas hiện tại và đề xuất sửa
Master → Scene → Start Frame duyệt → Video → Review. Video test Linh vẫn qua canvas. Canvas tự nối identity/outfit dài vào motion, và chỉ có bằng chứng gửi firstFrame; không có ảnh Master riêng/video motion reference trong payload đã quan sát. Đọc tên first frame và prompt tổng hợp trước Generate.

Đề xuất adapter theo mode: I2V motion gọn, optional preservation một câu; locks đầy đủ dùng trong chuẩn bị ảnh/reference brief. Đây là giả thuyết cần A/B, không khẳng định text locks gây mọi drift. Lượt research này chưa sửa app canvas. Chỉ bổ sung references/end frame/video editing sau khi xác minh SDK hỗ trợ; không bịa endpoint hoặc ghép firstFrame+references tùy ý.

### Coverage chuyển động: dùng như checklist, không phải template bắt buộc

Chỉ giữ các mục phục vụ shot và mô tả bằng dấu hiệu có thể nhìn thấy:

1. **Action:** một hành động chính và hướng tiến triển của nó.
2. **Body mechanics:** support foot/contact point, weight transfer, khớp nào dẫn động, torso/shoulder/hip phản ứng ra sao.
3. **Camera:** một operation chính, nêu quan hệ với subject và điều phải còn trong frame.
4. **Environment response:** tóc, vải, bóng, mặt đường hoặc background flow phản ứng đúng nguyên nhân; không thêm hiệu ứng trang trí vô cớ.
5. **Facial response:** chỉ một thay đổi nhỏ có lý do nếu shot cần; không ép cười/nháy mắt vào mọi clip.
6. **Continuity:** giữ đúng các yếu tố ảnh đầu đã cung cấp; asset phải được gắn thật, câu chữ không tự tạo face-lock.

Với dance, ưu tiên full-body khi cần nghiệm thu chân: thấy trọn tay/chân, floor contact và đủ khoảng trống cho path. Tả `weight transfers onto the supporting foot`, knee/torso counterbalance, momentum của tóc/vải và contact shadow cụ thể hơn việc lặp `natural movement`, `realistic physics`, `fluid lifelike motion`. Nếu shot chỉ kiểm torso ở medium view, ghi rõ không dùng nó để nghiệm thu footwork.

Tempo/BPM và mốc thời gian có thể giúp diễn đạt ý đồ cho clip dài hoặc workflow có audio/reference thật, nhưng không bảo đảm sync hay cadence. Trên canvas Linh 4/6/8s, mặc định tách mỗi clip thành một action thay vì nhồi entry/phrase/resolve dài; chỉ dùng storyboard theo giây khi duration thực và hành động đủ chỗ hoàn tất.

Không mặc định thêm negative dài hoặc lệnh cộng đồng như `/dollyin`, `motion0`, `ref+camera`, `drn+zoomout`. Chỉ dùng khi đúng surface/model hiện tại đã cho thấy control đó tồn tại và có tác dụng; nếu không, viết camera/motion bằng câu rõ ràng trong prompt và kiểm media thật.

## Bốn mẫu chuẩn bị ảnh đầu
Đính kèm ảnh nhận dạng thật. Điền placeholders bằng brief đã chốt. Mỗi mẫu là một ảnh riêng.

### S-C: đạp xe đúng làn
```text
Create one photograph of the adult woman in the attached approved identity reference, preserving her recognizable face, hair and original body proportions. Keep her approved cycling outfit and black road bicycle with drop handlebars and bottle. She is seated on the saddle, both hands on the hoods and feet on opposite pedals. Exact side view, bicycle facing right along a straight night road. The bicycle wheel-to-wheel axis, curb and lane markings run parallel across the image. Both tires contact the asphalt. Show the entire rider, both wheels and crank inside the vertical frame, with space around the bicycle and a clearly illuminated face.
```
Duyệt: hướng xe DỌC làn, tay/feet đúng, đủ bánh; nếu pass-by camera đứng thì ảnh rộng có đường trống phía trước, cho phép rời khung.

### S-Y: yoga Warrior II
```text
Create one full-body photograph of the adult woman in the attached approved identity reference, preserving her recognizable face, hair and original body proportions. She wears [OUTFIT] at [LOCATION]. She holds Warrior II on a yoga mat, front knee bent in line with the foot, rear leg extended, both feet grounded, arms at shoulder height and gaze over the front hand. A side view clearly shows the stance. Include the whole mat, both hands and feet with readable limb separation and consistent lighting.
```
Duyệt: pose/tiếp điểm nhìn rõ, không ép biên độ; đây là mô tả cảnh biểu diễn, không hướng dẫn tập cá nhân.

### S-D: dance sẵn sàng
```text
Create one full-body photograph of the adult woman in the attached approved identity reference, preserving her recognizable face, hair and original body proportions. She wears [OUTFIT] at [LOCATION]. She faces the camera in a relaxed dance-ready stance, feet comfortably separated, knees soft and arms free beside her body. Keep both shoes visible with floor contact shadows. Leave room on either side for small lateral steps. Use a level frontal view and a relaxed attentive expression.
```
Duyệt: đủ chi, chân tách rõ, chỗ bước; không tự thêm props.

### S-F: fashion bắt đầu bước
```text
Create one full-body fashion photograph of the adult woman in the attached approved identity reference, preserving her recognizable face, hair and original body proportions. She wears [OUTFIT] at [LOCATION]. Place her at the start of a natural forward walk, one foot ahead in a believable weight-bearing stance, relaxed arms and gaze along the walkway. Show her complete outfit and shoes with room for movement. Keep the garment construction, accessories and material faithful to the outfit reference, with consistent lighting and contact shadows.
```

## Đạp xe: ba motion prompts

### C1: camera song song, nhịp đạp chính
Ảnh: S-C hướng phải; test cơ học trước tương tác.
```text
The cyclist rides briskly to the right along the lane, keeping the bicycle parallel to the curb. Her feet alternate through quick continuous pedal circles, knees rising and falling with each stroke. Both tires roll on the asphalt as the crank turns. Camera tracks beside her on a parallel path, keeping the side view. Nearby lane markers and lamp posts pass toward the left, hair trails behind, and her eyes stay on the road.
```

### C2: chào V một lần
Ảnh ngang đang đạp, hai tay trên hoods; chỉ thử khi chuyển động chính đạt.
```text
While continuing to pedal along the lane, she notices the companion filming beside her. She briefly meets the camera's gaze, lifts her near hand in a small two-finger greeting, then returns it to the handlebar hood and looks ahead. Her other hand remains on the hood throughout. The camera follows parallel to the bicycle; the wheels keep rolling and the street passes behind. Her response is brief and relaxed rather than a sustained pose.
```

### C3: gạt tóc
Ảnh ngang, tóc gần má, tay rõ. Nếu gesture làm ngừng đạp thì tách sang cảnh xe đã dừng, không nhồi thêm fast.
```text
She continues pedaling along the lane, eyes on the road. Her near hand briefly brushes a loose strand away from her cheek and immediately returns to the handlebar hood. The other hand stays on the hood. Her legs keep their alternating circular movement and the bicycle rolls forward parallel to the curb. The camera tracks alongside without circling her, while wind carries the loose hair backward.
```

## Yoga: ba motion prompts

### Y1: Warrior II sang Reverse Warrior
Ảnh S-Y; chọn6s nếu4s không đủ cho động tác nhẹ.
```text
From the held Warrior II stance, she gradually lifts the front arm overhead and lets the back hand rest lightly on the rear thigh. Her torso opens into a gentle reverse-warrior shape while the front knee remains bent and both feet keep their positions on the mat. The movement follows one calm breath and finishes with a balanced hold. The camera stays still in the full-body side view.
```

### Y2: giữ pose với hơi thở
Ảnh một pose đã duyệt; không đổi pose trong prompt này.
```text
She maintains the pose with small natural movements from breathing. The shoulders soften on the exhale and the torso rises slightly on the next inhale, while the hands and feet preserve their supporting contact. Her gaze stays calm and steady. A very gentle camera push brings her closer without cropping the supporting limbs. The posture stays balanced throughout rather than changing into another pose.
```

### Y3: xoay thân khi ngồi
Ảnh riêng ngồi xếp bằng, lưng thẳng tự nhiên, thấy tay và thảm.
```text
Seated cross-legged, she lengthens her spine with a quiet inhale, then gently rotates her torso toward her right. Her left hand settles on the right knee and her right hand supports her on the mat behind the hip. Her head follows the torso comfortably. She pauses in the mild twist with both sitting bones grounded. The camera remains fixed at seated eye level, keeping her hands visible.
```

## Dance: ba motion prompts

### D1: step-touch vui, kiểm chân
Ảnh S-D full body. Không orbit trước khi groove/contact đạt.
```text
She performs an upbeat repeating step-touch, stepping to one side and bringing the other foot beside it, then reversing direction. Her weight transfers onto each supporting foot, knees spring softly, and relaxed arms swing with the groove. Her shoulders and hair respond naturally to the movement. She stays engaged without holding a smile for the camera. A fixed full-body frontal shot keeps the shoes and floor contact visible.
```

### D2: vai/thân trên
Ảnh medium đứng vững; không dùng cảnh này để nghiệm thu footwork.
```text
With her feet planted, she grooves through small alternating shoulder accents and a relaxed side-to-side torso shift. One forearm opens outward and returns as the other side takes over. Her head follows the rhythm lightly, while her expression stays responsive and natural. The camera holds a level medium view with both hands in frame. The motion remains compact and continuous, without a spin or change of location.
```

### D3: một bước chéo rồi về
Ảnh S-D có không gian; không thêm xoay180° và đổi mặt liên tục.
```text
She steps her right foot diagonally forward, transfers her weight onto it, and lets the arms open lightly with the movement. She then pushes back into the starting stance and repeats the small diagonal step with a lively bounce. The supporting foot stays connected to the floor during each weight transfer. Camera remains fixed and full-body so the path of the feet is clear. Her gaze follows the movement naturally.
```

## Fashion: ba motion prompts

### F1: bước tự nhiên, nhìn phom đồ
Ảnh S-F. Điều chỉnh pace theo brief, không mặc định slow motion.
```text
She walks toward the camera with a confident, natural stride, transferring weight cleanly from one foot to the other. Her arms swing comfortably and the fabric responds to each step with movement appropriate to its weight. Her gaze follows the walkway. The camera retreats directly ahead of her at a matching pace, maintaining a full-length view of the outfit and shoes. Hair moves with the stride rather than a separate posed head turn.
```

### F2: chuyển trọng tâm, chất liệu
Ảnh đứng, khung thấy phần đồ cần giới thiệu; tay không cầm props chưa chốt.
```text
She settles her weight onto one leg and makes a small, relaxed turn of the shoulders so the garment catches the light from a slightly different angle. Her arms remain comfortable beside her body and the fabric folds settle naturally. Her gaze follows the direction of the turn without a prolonged stare at the lens. The camera gently moves closer, keeping the relevant garment details and her hands visible.
```

### F3: dừng bước, lộ dáng bên
Ảnh đang bước về camera, đủ chân. Nếu cần thấy lưng, tạo ảnh/góc riêng thay vì spin cả vòng.
```text
She finishes the current step and slows into a balanced standing position. With both feet settled, she turns her torso slightly to one side to reveal the garment's side silhouette, letting the arms relax into place. The hair and hem settle after the walk. The camera holds its position, keeping her entire outfit in frame. The change is a small natural adjustment rather than a dramatic runway spin.
```

## Dựng20s: theo từng thể loại
Đây là timeline biên tập, không paste timestamps/nămshot vào một generation. 5 đoạn đạt×4s=20s; có thể tạo6s chọn4s tốt. Nếu không đủ footage đạt, cần cảnh bổ sung, không che bằng kéo chậm/loop vô cớ. Crop detail là insert, không gọi là góc máy mới.

| Thể loại | 0–4s | 4–8s | 8–12s | 12–16s | 16–20s |
|---|---|---|---|---|---|
| Cycling | C1 toàn xe | chân/crank từ đoạn đạt hoặc cảnh riêng | C2 | C1 góc khác ảnh đầu đã duyệt | C3 |
| Yoga | Y2 pose đầu | Y1 | Y2 pose cuối | Y3 cảnh ngồi riêng có chủ ý | Y2 ngồi |
| Dance | D1 | D2 | D3 | D1 góc khác có ảnh đầu riêng | groove kết |
| Fashion | F1 | F2 chi tiết | F1 góc bên/lưng riêng | F3 | F2 silhouette |

Không trộn các thể loại nếu user không yêu cầu. Với yoga, tạo6/8s và dựng ít cuts hơn nếu cần giữ hơi thở tự nhiên; timeline trên là lựa chọn coverage, không luật pacing.
Ghép nhạc có file dùng được. Unity đã có file user cung cấp cho cycling; không mặc định áp vào yoga. Cắt theo beat thật sau khi có footage. Text RPM/BPM không bảo đảm tốc độ hay khớp vũ đạo chính xác; reference video cần được công cụ truyền thật và xác định vai trò, không chỉ gửi tên file.

## Nghiệm thu trước ghép nhạc

| Kiểm | Quan sát | Không coi là bằng chứng |
|---|---|---|
| Identity | mặt/mái/body/đồ ở đầu, giữa, cuối so master, góc tương đương | chữsameface, tênLinh, Masterthumbnail |
| Cycling | xe song song lane; pedal/feet có pha quay liên tục; tire contact; road flow đúng hướng | spokes blur, frame thưa, chữ100RPM |
| Yoga | điểm tựa hợp lý; pose không tự đổi; khớp/chi không méo | một frame cuối đẹp |
| Dance | weight transfer/foot contact; khôngtrượt; tay không xuyên; groove tự nhiên | nhạc che motion chậm |
| Fashion | bước không slide; phom/vải/phụ kiện nhất quán | chỉcận mặt che phầnchân |
| Continuity | outfit/light/props/set; cuts có ý, khônggiả nối hai đường khác nhau | blendcheteleport |

Vòng thử có giá trị: cùngảnh/model/settings, đổi đúng một yếu tố. Với canvas, so bảnmotiongọn và bảnautoappendlocks khi UI cho phép cấu hình thật; chưa thực hiện A/B này. Nếu lặp lỗi chậm, cân nhắc model/mode được hỗ trợ, không chỉ đổi từ đồng nghĩafast. Mọi video test Linh theo canvas.

Lưu assetIDs, prompt đã ghép, plannedsettings và verifiedmetadata riêng; contactreview +feedback. Bộ mẫu mới chưa sinh media/tiêu credits. Nguồn repo và giới hạn xem RESEARCH.md.

