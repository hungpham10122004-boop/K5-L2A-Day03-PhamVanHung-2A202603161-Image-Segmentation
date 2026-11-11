# Báo cáo thực hành Day 3 — Image Segmentation

- Mã học viên theo lớp: 2A202603161 (Phạm Văn Hùng)
- Ngày / CVAT local: 11/11/2026 / CVAT local (localhost:8080)
- Công cụ đã dùng: CVAT, chủ yếu công cụ Polygon vẽ thủ công; không dùng SAM

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | chưa có | 0 / 1 | 3 |
| cp2_slice | chưa có | 0 / 1 | 3 |
| cp5_occlusion | chưa có | 0 / 1 | 3 |
| cp3_thin | chưa có | 0 / 1 | 3 |
| cp4_curb | chưa có | 0 / 1 | 3 |
| cp6_coverage | chưa có | 0 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object tự vẽ đầu tiên: ảnh 1 (đường cao tốc lúc chiều tối, có hai dải đồi hai bên), vùng sky phía trên đường chân đồi. Tôi vẽ thủ công bằng Polygon, không dùng gợi ý tự động.
- Class và quy tắc tôi dùng để chọn biên: class sky; tôi cho mask dừng đúng ở đường chân đồi và để phần trời nhiều sương sát đồi vẫn là sky. Mặt đường (road) chỉ vẽ phần nhìn thấy, chừa chỗ hai chiếc xe đang chạy trên đường.
- Nếu dùng gợi ý sau đó: không dùng.
- Nếu không dùng gợi ý: không dùng. Quyết định gán nhãn của tôi là xem đồi khô màu nâu là vùng riêng, chỉ gán vegetation cho mảng cây rậm sẫm màu trên đồi bên phải (xem ca 3 ở mục 4).

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: easy_semantic, ảnh dãy nhà ở khu dân cư, vùng mặt đường và vùng vegetation bên phải
- Lỗi thuộc loại: sai lớp / biên
- Bằng chứng tôi nhìn thấy: có một vệt hồng của nét sidewalk nằm trên mặt đường; mask vegetation lấn sang hộp tủ điện màu xanh xám; mask sky cắt vào tán cây ở góc trên phải
- Quy tắc và hành động sửa: mặt đường phải là road nên tôi xóa vệt hồng bằng Eraser; tủ điện không phải cây nên tôi cắt khỏi mask vegetation; tôi chỉnh biên sky bám mép tán cây
- Sau sửa đã Save và export lại chưa? Đã Save và export lại ZIP.

Kết quả tự đánh giá: chưa có điểm.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. easy_semantic, dãy nhà bị cây và cột điện che ngang | Vẽ mask building liền xuyên qua cây, hoặc chia thành nhiều mảng chỉ ở phần nhìn thấy | Chỉ vẽ phần nhìn thấy; phần cây thuộc vegetation | Chia nhiều mask building, không vẽ xuyên qua cây |
| 2. Ảnh có vạch qua đường (crosswalk) | Vẽ crosswalk thành lớp riêng, hoặc tính là road | classes.json không có lớp vạch kẻ đường, vạch sơn nằm trên mặt đường | Gán vạch qua đường vào road, không tạo label riêng |
| 3. easy_semantic, ảnh đường cao tốc, đồi khô màu nâu hai bên đường | Tính cả đồi khô là vegetation, hoặc chỉ tính mảng cây rậm sẫm màu | Đồi khô gần như không có cây xanh, còn mảng sẫm màu bên phải là bụi cây rõ ràng | Chỉ gán vegetation cho mảng cây rậm; hỏi coach đồi cỏ khô nâu có nên tính là vegetation không |
