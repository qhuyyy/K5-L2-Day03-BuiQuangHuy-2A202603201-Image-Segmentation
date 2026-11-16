# Báo cáo thực hành Day 3

- Mã học viên theo lớp: 2A202603201
- Ngày / CVAT local: buổi lab 11/11/2026, hoàn thành và nộp 12/11/2026; CVAT local (Docker)
- Công cụ đã dùng: CVAT local — SAM 2.1 (AI Tools) và vẽ tay để sửa mask

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | `easy_semantic.zip` | 3 / 3 | 20 |
| medium_instance | `medium_instance.zip` | 3 / 3 | 32 |
| hard_panoptic | `hard_panoptic.zip` | 2 / 2 | 30 |
| cp1_holes | chưa có | 0 / 1 | 3 |
| cp2_slice | chưa có | 0 / 1 | 3 |
| cp5_occlusion | chưa có | 0 / 1 | 3 |
| cp3_thin | chưa có | 0 / 1 | 3 |
| cp4_curb | chưa có | 0 / 1 | 3 |
| cp6_coverage | chưa có | 0 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Số mask đã nộp (đếm từ file export, không phải số object đúng):

- `medium_instance`: 000000181542 — 23 person, 5 motorcycle, 4 car, 1 bus; 000000373353 — 13 car, 9 person, 1 bus; 000000458325 — 10 person, 9 car.
- `hard_panoptic`: 000000350023 — stuff: road, sidewalk, sky, building ×4, vegetation ×2; things: 20 car, 6 person. 000000460147 — stuff: road ×2, sidewalk, sky, building ×6, vegetation ×10; things: 20 car, 3 truck, 3 motorcycle, 3 person.
- `easy_semantic`: phần pixel chưa gán nhãn lần lượt 17.7% / 9.7% / 7.5% ở ảnh 7ee6d192, 81ae7cbb, 817bca71.

`scripts/inspect_submissions.py` báo `OK` cho cả ba ZIP (chỉ xác nhận cấu trúc/format, không phải độ đúng).

**Chưa làm sáu checkpoint** do hết thời gian. **Nộp muộn**: hạn 12/11/2026 12:00 (UTC+7), các ZIP được export lúc khoảng 13:33 cùng ngày.

**Lỗi kỹ thuật đã gặp (ngày 12/11/2026):** SAM3 (nuclio, chạy CPU) trên CVAT local không hoạt động — lỗi `Cannot access accelerator device when none is available`, sau đó lỗi định dạng kết quả, lệch cổng (`host.docker.internal:61275` không kết nối được) và cuối cùng `504 Gateway Time-out` vì mỗi ảnh cần khoảng 7 phút trên CPU. Đã chuyển sang SAM 2.1 và vẽ tay để hoàn thành.

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium: ảnh đường phố trong `medium_instance`; các object người, xe máy, ô tô.
- Class và quy tắc chọn biên: mỗi vật đếm được (`person`, `motorcycle`, `car`) là một mask riêng, biên theo phần nhìn thấy.
- Gợi ý đã dùng: SAM 2.1. Với object xe máy, mask SAM 2.1 tạo ra không phủ kín xe nên tôi vẽ tay lại phần thiếu (chi tiết ở mục 3).

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: object xe máy được segment bằng SAM 2.1.
- Lỗi thuộc loại: biên / phủ vùng.
- Bằng chứng tôi nhìn thấy: mask SAM 2.1 không phủ kín xe máy, còn phần của xe nằm ngoài mask.
- Quy tắc và hành động sửa: mask phải phủ toàn bộ phần nhìn thấy của vật; tôi vẽ tay bổ sung phần xe máy còn thiếu.
- Sau sửa đã Save và export lại chưa? Đã Save; bản sửa nằm trong ZIP đã nộp.

Summary tự đánh giá / scorer: chưa có điểm.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| Xe máy rất nhỏ trong ảnh | Gán nhãn thành một instance `motorcycle` / bỏ qua vì quá nhỏ để vẽ biên chính xác | Instance: mỗi vật nhìn thấy là một mask; thiếu vật làm giảm recall | Hỏi coach: vật nhỏ đến mức nào thì vẫn cần gán nhãn? |
| Người đứng xa và bị vật khác che khuất | Gán một instance `person` cho phần nhìn thấy / bỏ qua vì quá xa và bị che | Chỉ vẽ phần nhìn thấy; vật bị che vẫn là một instance | Hỏi coach: người ở xa, bị che phần lớn có cần gán nhãn không? |
| Ô tô ở xa, bị mờ | Gán `car` / bỏ qua vì không nhìn rõ biên và hình dạng | Không bịa class cho pixel không chắc | Hỏi coach: vật mờ ở xa có cần gán nhãn không? |

