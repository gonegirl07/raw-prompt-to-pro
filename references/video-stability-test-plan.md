# Kế hoạch kiểm chứng độ ổn định (chưa chạy)
Ngày 2026-10-09. Không có authorization gen mới trong yêu cầu research này.

## Hai loại độ ổn định
1. Trong clip: khoảng chu kỳ đầu/giữa/cuối, supporting contact, body response, face/outfit/set.
2. Giữa các lần gen: cùng input/settings vẫn có biến thiên. Một clip đẹp hoặc một cặp A/B không đủ kết luận ổn định.

## Thử nghiệm tối thiểu nếu user yêu cầu
- Linh: cùng firstFrame Run23 front đã dùng Run26–28; đối chiếu master Run17 trước gửi. Omni1.1Flash4s9:16360p, locks/notes blank theo UI thật. Exact resolved prompt + actual assetID phải lưu trước/sau. Seed chỉ khóa nếu tool thực sự hỗ trợ.
- A: exact Run26 prompt làm baseline, không regenerate ảnh. B: giữ nguyên A chỉ sửa một clause cadence đã chọn. Bản Run26 cũ chỉ là historical comparator nếu không có A mới; đừng gọi controlled A/B.
- Lưu toàn nguồn, playback1x không nhạc/speedup. Sample100ms để tìm chu kỳ rồi đọc frame liền quanh same-phase event; minimum2 complete intervals; phase mờ/che thì report không đo được. So reference0.6–0.7s/vòng chỉ là ước lượng manual theo góc khác, không exactRPM.
- Đo median/range chu kỳ, so đầu/giữa/cuối; body response phải giữ mức Run26 theo user, không đổi cadence lấy thân cứng. Không chọn đoạn giữa nhanh để che đầu/cuối chậm.
- Dance: step-touch chu kỳ trái→phải→trái, supporting-foot contact và không slide; fashion: bước/contact/garment continuity; social: một thao tác và item/hands không morph. Check identity ở ít nhất đầu/giữa/cuối và mọi frame quay mặt đáng kể.
- Stop: khi cùng lỗi tái diễn với hai giả thuyết wording khác nhau, không đổi thêm synonym. Kiểm control/model/mode được app hỗ trợ thật; nếu thiếu thì ghi blocker/candidate app enhancement, không bịa payload hoặc thay nativevideo.
- Muốn kết luận ổn định qua nhiều lần: user chốt budget cho số lần lặp cùng cấu hình; log toàn bộ output không cherry-pick, tách pass cadence/contact/identity/body. Không paid auto-retry.

## Cổng trước dựng
Chỉ ghép nhạc/caption/đa góc sau motion pass của clip nguồn. Dựng tốt có thể hỗ trợ kể chuyện nhưng không chứng minh model sửa cơ học. Draft360p dùng motion review; bản đăng cần generation/export chất lượng phù hợp, upscale360p không tái tạo chi tiết thật.

