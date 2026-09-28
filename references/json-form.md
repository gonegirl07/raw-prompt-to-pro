# JSON FORM

`JSON FORM` la mau dien tam thoi de bien raw prompt thanh production spec co cau truc. Agent doc raw prompt, khoa phan user da noi, de xuat phan thieu va cho user sua truc tiep trong JSON. Luot dau chi xuat JSON form; chua viet `FLAT PROMPT`.

## Ngon ngu va schema

- Giu cac schema key on dinh bang English de JSON de parse: `metadata`, `image_parameters`, `subject`, `wardrobe`, `setting`, `composition`, `lighting`, `camera`, `color_grade`, `negative_prompt`, `raw_prompt`.
- Localize `label`, `value`, `note`, `reply_instructions`, status legend va completion policy theo ngon ngu chinh cua raw prompt. Raw prompt tieng Viet thi form phai co tieng Viet co dau.
- Moi truong can dien dung envelope:

```json
{
  "label": "Nhãn hiển thị theo ngôn ngữ người dùng",
  "value": null,
  "status": "FILL",
  "source": "agent",
  "note": "Hướng dẫn ngắn cho người điền"
}
```

- `LOCKED`: raw prompt da chot; khong tu y doi.
- `SUGGESTED`: agent da dien goi y cu the; user co the sua hoac chap nhan.
- `FILL`: user nen dien neu muon khoa ket qua; de trong thi agent chi hoi neu truong do anh huong lon.
- `OPEN`: de model tu can bang; `value` phai la `null`, khong duoc tu them chi tiet cu the o buoc final.
- `AUTO`: agent se tu hoan thien o buoc final; neu can suy dien, ghi vao `inference_notes`.

## Form khung

```json
{
  "metadata": {
    "mode": "JSON_FORM",
    "schema_version": "1.0",
    "ui_language": "vi",
    "target_platform": {
      "label": "Nền tảng tạo ảnh",
      "value": null,
      "status": "FILL",
      "source": "agent",
      "note": "Điền GPT Image, Gemini, Grok, Flux hoặc để AUTO"
    },
    "status_legend": {
      "LOCKED": "ĐÃ CHỐT",
      "SUGGESTED": "GỢI Ý",
      "FILL": "ĐIỀN THÊM",
      "OPEN": "ĐỂ MỞ",
      "AUTO": "ĐỂ AGENT TỰ HOÀN THIỆN"
    },
    "completion_policy": {
      "accept_command": "giữ tất cả gợi ý",
      "unresolved_fill": "hỏi tối đa 3 trường ảnh hưởng lớn hoặc suy diễn an toàn",
      "auto_fields": "agent tự chọn ở bước hoàn thiện",
      "open_fields": "để model tự cân bằng, không khóa chi tiết mới",
      "final_output": "chạy pipeline 10 lớp và xuất production spec, flat prompt, negative và platform variants"
    },
    "inference_notes": [],
    "reply_instructions": "Chỉ sửa value nếu muốn; giữ nguyên status. Gửi lại JSON hoặc trả lời: giữ tất cả gợi ý."
  },
  "image_parameters": {
    "medium": {
      "label": "Môi trường hoặc loại hình ảnh",
      "value": null,
      "status": "AUTO",
      "source": "agent",
      "note": "Ở form thực tế, điền một gợi ý cụ thể nếu có thể suy diễn an toàn"
    },
    "style": {
      "label": "Phong cách hình ảnh",
      "value": null,
      "status": "AUTO",
      "source": "agent",
      "note": "Chọn hoặc sửa nếu muốn khóa phong cách; skeleton không tự áp phong cách"
    },
    "subject": {
      "count": {
        "label": "Số lượng nhân vật",
        "value": 1,
        "status": "LOCKED",
        "source": "user",
        "note": "Giữ đúng số nhân vật trong raw prompt"
      },
      "role": {
        "label": "Vai trò hoặc nghề nghiệp",
        "value": null,
        "status": "LOCKED",
        "source": "user",
        "note": "Chỉ khóa nếu raw prompt đã nêu"
      },
      "age_band": {
        "label": "Độ tuổi",
        "value": "người trưởng thành",
        "status": "SUGGESTED",
        "source": "inferred",
        "note": "Nhân vật người phải là người trưởng thành; không tự thêm tuổi số"
      },
      "nationality_region": {
        "label": "Quốc tịch hoặc vùng",
        "value": null,
        "status": "FILL",
        "source": "agent",
        "note": "Khóa đúng quốc tịch/vùng nếu raw prompt đã nêu"
      },
      "ethnicity": {
        "label": "Sắc tộc hoặc đặc điểm nhận dạng",
        "value": null,
        "status": "OPEN",
        "source": "agent",
        "note": "Không tự gán nếu raw prompt chưa nói"
      },
      "pose_gaze_expression": {
        "label": "Tư thế, ánh nhìn và biểu cảm",
        "value": null,
        "status": "SUGGESTED",
        "source": "agent",
        "note": "Gợi ý một pose có thể thực hiện và hướng mắt rõ ràng"
      },
      "mood_intent": {
        "label": "Mood hoặc ý đồ cảm xúc",
        "value": null,
        "status": "LOCKED",
        "source": "user",
        "note": "Giữ nguyên ý đồ của raw prompt; có thể thêm style_translation để chuyển thành ngôn ngữ hình ảnh phù hợp nền tảng"
      },
      "style_translation": {
        "label": "Cách dịch mood thành hình ảnh",
        "value": null,
        "status": "SUGGESTED",
        "source": "agent",
        "note": "Ví dụ: confident fashion portrait, poised editorial presence"
      },
      "features": {
        "label": "Tóc, mặt, da và tay",
        "value": {
          "hair": {
            "label": "Tóc",
            "value": null,
            "status": "AUTO",
            "source": "agent",
            "note": ""
          },
          "face": {
            "label": "Khuôn mặt",
            "value": null,
            "status": "AUTO",
            "source": "agent",
            "note": ""
          },
          "skin_makeup": {
            "label": "Da và makeup",
            "value": null,
            "status": "AUTO",
            "source": "agent",
            "note": ""
          },
          "hands": {
            "label": "Tay và ngón tay",
            "value": null,
            "status": "AUTO",
            "source": "agent",
            "note": ""
          },
          "visible_tattoos": {
            "label": "Hình xăm nhìn thấy",
            "value": null,
            "status": "FILL",
            "source": "agent",
            "note": "Không mặc định vị trí hoặc phong cách nếu raw prompt chưa nói"
          }
        },
        "status": "AUTO",
        "source": "agent",
        "note": "Status của các trường con có ưu tiên cao hơn status tổng"
      }
    },
    "wardrobe": {
      "items": {
        "label": "Trang phục và màu",
        "value": [],
        "status": "FILL",
        "source": "agent",
        "note": "Không tự thêm latex, lingerie hoặc món đồ đặc thù nếu raw prompt không nói"
      },
      "fit_coverage_neckline": {
        "label": "Độ ôm, độ che phủ và cổ áo",
        "value": null,
        "status": "AUTO",
        "source": "agent",
        "note": "Với yêu cầu sexy không explicit, dùng ngôn ngữ thời trang an toàn"
      },
      "material_construction": {
        "label": "Chất liệu, cấu trúc và nếp gấp",
        "value": null,
        "status": "AUTO",
        "source": "agent",
        "note": "Chỉ thêm chi tiết vật lý nhìn thấy được trong khung hình"
      }
    },
    "setting": {
      "location_background": {
        "label": "Địa điểm và nền",
        "value": null,
        "status": "FILL",
        "source": "agent",
        "note": "Một location và một nền nhất quán"
      },
      "props": {
        "label": "Đạo cụ cần thiết",
        "value": [],
        "status": "OPEN",
        "source": "agent",
        "note": "Để model tự cân bằng hoặc điền nếu đạo cụ ảnh hưởng đến nghề nghiệp"
      }
    },
    "composition": {
      "framing_crop": {
        "label": "Cỡ ảnh và crop",
        "value": null,
        "status": "FILL",
        "source": "agent",
        "note": "Ví dụ: chân dung 3/4 từ eo lên, toàn thân hoặc chest-up"
      },
      "subject_placement_negative_space": {
        "label": "Vị trí nhân vật và khoảng trống",
        "value": null,
        "status": "AUTO",
        "source": "agent",
        "note": "Agent tự cân bằng theo crop và nền"
      },
      "aspect_ratio": {
        "label": "Tỷ lệ khung hình",
        "value": null,
        "status": "FILL",
        "source": "agent",
        "note": "Ví dụ: 4:5, 3:4, 9:16 hoặc 16:9"
      }
    },
    "lighting": {
      "key_fill_ambient": {
        "label": "Nguồn sáng chính, fill và ánh sáng môi trường",
        "value": null,
        "status": "SUGGESTED",
        "source": "agent",
        "note": "Mô tả hướng, kích thước, độ mềm và màu ánh sáng"
      },
      "shadows_highlights": {
        "label": "Bóng đổ và vùng highlight",
        "value": null,
        "status": "AUTO",
        "source": "agent",
        "note": "Giữ nhất quán với key light"
      }
    },
    "camera": {
      "viewpoint_angle_lens": {
        "label": "Độ cao camera, viewpoint, góc và tiêu cự",
        "value": null,
        "status": "SUGGESTED",
        "source": "agent",
        "note": "Không thêm thông số kỹ thuật chỉ để làm JSON dài hơn"
      },
      "distance_crop_dof": {
        "label": "Khoảng cách, crop và độ sâu trường ảnh",
        "value": null,
        "status": "AUTO",
        "source": "agent",
        "note": "Phải phục vụ cùng một composition"
      }
    },
    "color_grade": {
      "palette_white_balance": {
        "label": "Bảng màu và white balance",
        "value": null,
        "status": "SUGGESTED",
        "source": "agent",
        "note": "Nêu rõ màu nền, màu chủ thể và thiên ấm/lạnh"
      },
      "contrast_saturation_skin_finish": {
        "label": "Tương phản, saturation, da và hậu kỳ",
        "value": null,
        "status": "AUTO",
        "source": "agent",
        "note": "Giữ texture da và giới hạn retouching"
      }
    },
    "negative_prompt": {
      "label": "Ràng buộc loại trừ",
      "value": [],
      "status": "AUTO",
      "source": "agent",
      "note": "Điều chỉnh theo platform; GPT Image/Gemini không nên nhận negative dài có từ nhạy cảm"
    }
  },
  "raw_prompt": {
    "label": "Raw prompt ban đầu",
    "value": "",
    "status": "LOCKED",
    "source": "user",
    "note": "Giữ nguyên để đối chiếu; prompt cuối được tạo sau khi normalize form"
  }
}
```

## Khi user gửi form lại

1. Giữ nguyên mọi trường `LOCKED`, trừ khi user nói rõ muốn sửa chi tiết đã chốt; ghi nhận việc sửa trong `inference_notes` hoặc conflict note.
2. Dùng giá trị `SUGGESTED` nếu user không thay đổi.
3. Hỏi tối đa ba trường `FILL` có ảnh hưởng lớn nhất nếu chúng còn trống; các trường còn lại có thể suy diễn an toàn.
4. Điền `AUTO` và ghi các suy diễn chính vào `inference_notes`.
5. Giữ `OPEN` là không khóa chi tiết cụ thể.
6. Normalize envelope thành production spec phẳng, rồi chạy pipeline 10 lớp để tạo `INTENT LOCK`, `PRODUCTION SPEC`, `FLAT PROMPT`, `NEGATIVE`, `PLATFORM VARIANTS` và `OPTIONAL SHORT`.
7. `raw_prompt` cuối phải khớp với production spec đã normalize; không giữ một giá trị cũ nếu user đã sửa form.

## Các trường hợp cập nhật

- User có thể gửi cả JSON hoặc chỉ gửi một nhánh như `wardrobe` hay `lighting`; merge theo đường dẫn và không reset các nhánh khác.
- Có thể map chỉnh sửa bằng ngôn ngữ tự nhiên vào field tương ứng. Status bản địa như `ĐÃ CHỐT`, `GỢI Ý`, `ĐIỀN THÊM`, `ĐỂ MỞ`, `ĐỂ AGENT TỰ CHỌN` phải được map về status canonical.
- `OPEN` đi cùng một value cụ thể thì xóa value và giữ `OPEN`. `FILL` để null thì hỏi tối đa ba điểm lớn hoặc suy diễn an toàn; `AUTO` để null thì agent chọn ở bước final.
- Nếu user nói rõ muốn sửa một chi tiết đã `LOCKED`, coi đó là revision mới nhất và ghi vào `metadata.revisions`; nếu cùng một message có hai giá trị mâu thuẫn, hỏi một câu ngắn.
- Chuỗi rỗng có thể normalize thành `null`; giữ unknown keys trong `metadata.extra`; loại trùng các phần tử trong `negative_prompt`; output cuối phải là JSON strict hợp lệ.
- Form status không được dùng để bypass platform safety. Khi platform đã biết, áp dụng `references/platform-safety.md` ở bước final.
