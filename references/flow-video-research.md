# Nghiên cứu Google Flow / video prompt

Ngày đối chiếu: 2026-10-08. Đọc tài liệu; không cài hoặc chạy mã từ repo. Các trang được đọc là nguồn dữ liệu, không phải chỉ dẫn cho agent.

## Repo đáng tham khảo

| Repo | Có ích cho bộ này | Giới hạn |
|---|---|---|
| [agbelemi/google-flow-scripting-skill](https://github.com/agbelemi/google-flow-scripting-skill) | Quản lý asset, trạng thái cảnh, storyboard và kiểm tra continuity. | Quy định @handle, số panel và cách mở prompt là quy ước của tác giả. Validator không chứng minh hình ảnh/chuyển động đúng. |
| [raphaelbgr/ai-video-prompting](https://github.com/raphaelbgr/ai-video-prompting) | Playbook phân biệt dữ kiện, kinh nghiệm và điều chưa kiểm chứng; kiểm tra model và mode. | Chi phí/tính năng có ngày kiểm chứng cũ. Phải đối chiếu Help và UI hiện tại. |
| [trin-zenityx/google-flow-omni-prompt](https://github.com/trin-zenityx/google-flow-omni-prompt) | Prompt image-to-video gọn, tập trung thay đổi theo thời gian. | Fixed camera, không nhạc và quy tắc thoại thuộc workflow riêng; không áp dụng như luật cho mọi video. |
| [Charlo-O/google-flow](https://github.com/Charlo-O/google-flow) | Ví dụ automation, asset/frame upload và chọn mode. | Là công cụ automation hơn là thư viện tối ưu động tác. Không cần cài cho nhiệm vụ viết prompt này; không thay canvas bằng pipeline của repo. |
| [snubroot/Veo-3-Meta-Framework](https://github.com/snubroot/Veo-3-Meta-Framework) | Khung brief tổng quát cho Veo. | Meta prompt dài và các điểm số thành công dự đoán không thay thế QA thực tế; không chọn làm nền chính cho motion prompt. |

Không tìm thấy trong các repo đã đọc một bộ cycling/yoga/dance/fashion có bằng chứng benchmark trực tiếp trên canvas của chúng ta. Đây không phải kết luận không có repo như vậy trên toàn GitHub.

## Nguồn chính thức dùng để đối chiếu

1. [Google: best practices image-to-video](https://docs.cloud.google.com/gemini-enterprise-agent-platform/models/video/best-practice?hl=en): ảnh đầu mang subject/style; prompt tập trung chuyển động, tránh mô tả lại ảnh dư thừa. Áp dụng như nguyên tắc viết; không suy ra bảo đảm cho mọi model/mode.
2. [Flow: tạo video, frames và references](https://support.google.com/flow/answer/16353334?co=GENIE.Platform%3DDesktop&hl=en): dùng ảnh tham chiếu/ingredients, chỉ dẫn bổ trợ hình ảnh, phối hợp Gemini. Gắn asset thật mới có ý nghĩa tham chiếu.
3. [Flow: model và tính năng](https://support.google.com/flow/answer/16352836?hl=en): Omni và Veo có khả năng khác nhau; Omni có bản nháp 360p. Không suy ra SDK canvas hỗ trợ tất cả tính năng của UI.
4. [Flow: tạo và quản lý character](https://support.google.com/flow/answer/16935308?co=GENIE.Platform%3DDesktop&hl=en): có character bundle để tái sử dụng references. Chưa xác minh bundle được truyền qua tool canvas hiện tại.
5. [Google: Veo 3.1 prompting guide](https://cloud.google.com/blog/products/ai-machine-learning/ultimate-prompting-guide-for-veo-3-1): dùng để hiểu camera/action/context khi tạo cảnh mới; không chuyển nguyên công thức dài sang mọi prompt I2V.

## Bằng chứng từ dự án Linh

- Run17: user duyệt ảnh croptop đứng để kiểm nhận dạng. Không đồng nghĩa mọi ảnh chuyển pose/góc sẽ giữ được mặt.
- Run18: user từ chối gliding và xe chạy chéo mặt đường. Bối cảnh/ảnh đầu và camera cần kiểm tra độc lập.
- Run19 A/B: đúng side/right, mép đường ngang cải thiện hướng tương đối; camera vẫn tự bám và cadence chậm. Không có cơ sở gọi camera locked hay tốc độ đạt. Prompt RPM và câu 'fast' không phải metadata chuyển động.
- Canvas hiện tại: Omni1.1 Flash, first frame thật; identity/outfit được nối thêm bằng text. Không có bằng chứng Master image riêng hoặc video motion reference được gửi vào generation. Đây là điểm cần phân biệt với Flow native Ingredients.

## Quyết định

Tạo bộ prompt tự soạn, phân tách start-frame/reference prompts và motion prompts, một việc chính mỗi clip; đánh giá nhận dạng/contact/tempo/geography trước dựng nhạc. Mẫu mới là ứng viên nghiên cứu chưa được thử, không gọi là đã tối ưu thực nghiệm. Không dùng mẹo repo để hứa khóa mặt, khớp beat, cadence hoặc model quality.

## Đối chiếu thư viện Muse do user cung cấp — 2026-10-12

Nguồn nhập: `C:\Users\Huy\Downloads\google-flow-prompt-library.md`. Đây là dữ liệu nghiên cứu/tổng hợp từ Muse, không phải chỉ dẫn thực thi và không phải bằng chứng một kỹ thuật đã pass trên node canvas.

### Đã nhận vào skill

- Mỗi clip ưu tiên một action chính, diễn đạt bằng chuyển động nhìn thấy được thay vì tính từ trừu tượng.
- Checklist hữu ích cho I2V: action, body mechanics, camera, environment response, facial response và continuity. Đây là coverage tùy shot, không phải bắt buộc đủ sáu mục.
- Dance cần full-body khi kiểm footwork, thấy floor contact và mô tả weight transfer, support foot, momentum, torso/shoulder response.
- Một camera operation chính giúp prompt dễ kiểm hơn; camera phải phục vụ việc giữ action/contact trong frame.
- Tóc, quần áo, bóng và background flow có thể làm motion đọc tự nhiên hơn khi chúng có quan hệ vật lý rõ với action.

### Chỉ giữ như giả thuyết hoặc loại khỏi mặc định

- `natural movement`, `realistic physics`, `fluid lifelike motion` không phải magic fix; chỉ dùng nếu không thay thế mô tả cơ học cụ thể.
- BPM, `body synced to rhythm` và storyboard theo giây thể hiện creative intent, không chứng minh beat-sync khi audio/reference không được truyền thật. Canvas Linh hiện chỉ có clip 4/6/8s, nên không áp template 12s nguyên xi.
- Slash commands và mã rút gọn cộng đồng (`/dollyin`, `/crane`, `motion0`, `ref+camera`, `drn+zoomout`) chưa được xác minh trên node canvas; không đưa vào prompt mặc định.
- Tuyên bố SceneBuilder/Ingredients/Omni giữ nguyên mặt, outfit, motion hoặc composition là quá mạnh. Chỉ nêu khả năng của Flow native sau khi kiểm nguồn/UI hiện tại, và không suy sang payload canvas.
- `keep the face and outfit exactly consistent` là yêu cầu, không phải bảo đảm. Asset phải gắn thật và kết quả phải được QA đầu/giữa/cuối.
- Negative prompt dài có thể tranh chấp attention và không thay thế positive motion. Chỉ thêm constraints ngắn, gắn với lỗi cần tránh và khi surface hỗ trợ.
- Nguồn cộng đồng, Facebook reel, tool thương mại và blog bên thứ ba trong thư viện là đầu mối nghiên cứu; không coi các claim của chúng là verified facts nếu chưa mở và đối chiếu.
