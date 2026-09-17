# Báo cáo Day 5 — Nguyễn Hải Nam

- Mã học viên theo lớp: 2A202602257
- Ngày / CVAT local: 17/09/2026 / http://localhost:8080
- Công cụ đã dùng: CVAT (Brush, Polygon)

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | chưa có | 0 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | chưa có | 0 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Ghi chú: cp5_occlusion và cp6_coverage chưa kịp hoàn thành trong giờ lab do hết thời gian. Dữ liệu trong CVAT đã được Save nhưng chưa export. Không tạo ZIP rỗng theo hướng dẫn.

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh đầu tiên của medium_instance (000000017627.jpg), xe ô tô (car) ở vị trí trung tâm khung hình. Tự vẽ bằng công cụ Brush trong CVAT trước khi xem bất kỳ gợi ý tự động nào cho object đó.
- Class và quy tắc tôi dùng để chọn biên: Class `car`. Biên được xác định theo phần thân xe nhìn thấy được — dừng tại điểm bánh xe chạm mặt đường (không tô phần gầm khuất), dừng tại điểm xe bị vật khác che khuất (không đoán phần ẩn sau vật cản).
- Nếu không dùng gợi ý: Không dùng gợi ý tự động; toàn bộ mask vẽ thủ công bằng Brush. Quyết định gán nhãn: Tô sát viền ngoài thân xe, bỏ qua vùng đổ bóng dưới gầm xe vì đó là nền đường chứ không phải phần xe nhìn thấy.

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: cp2_slice, ảnh 000000017627.jpg, vùng hai xe sát nhau ở giữa ảnh.
- Lỗi thuộc loại: gộp-tách — Ban đầu vẽ một mask bao phủ cả hai xe thành một instance duy nhất.
- Bằng chứng tôi nhìn thấy: Danh sách Objects bên phải chỉ hiện 1 mục trong khi quan sát rõ có 2 phương tiện tách biệt về vật lý; mask phủ liền cả vùng tiếp giáp giữa hai xe.
- Quy tắc và hành động sửa: Theo quy tắc instance segmentation, mỗi vật thể vật lý riêng biệt phải là một mask riêng dù cùng class và đứng sát nhau (đây chính là nội dung cp2_slice kiểm tra). Tôi xóa mask cũ, vẽ lại mask riêng cho từng xe, ranh giới tại đường tiếp xúc giữa hai thân xe.
- Sau sửa đã Save và export lại chưa? Đã Save trong CVAT và export COCO 1.0, lưu thành cp2_slice.zip.

Ghi chú tự đánh giá: Chưa có kết quả từ GitHub Actions do ground truth chưa được phát tại thời điểm nộp.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| easy_semantic — vùng bóng đổ trên mặt đường, góc phải ảnh 2 | Bóng đổ là một phần của `road` (vì bề mặt vẫn là đường), hoặc là `building`/`vegetation` vì bóng do chúng tạo ra | Bóng đổ không thay đổi bản chất bề mặt phía dưới; pixel đó vẫn là mặt đường nhìn thấy | Giữ là `road` vì đang nhìn thấy bề mặt đường, dù có bóng phủ lên |
| cp3_thin — chân cột điện chạm mặt đường, khó xác định ranh giới | Ranh giới cột (`pole`) và đường (`road`) tại điểm tiếp xúc: tính pixel tiếp xúc vào `pole` hay `road`? | `pole` là vật thể đứng; đường tiếp xúc chính là chân cột; chỉ tô phần nhìn thấy của cột | Tô `pole` đến sát mặt đường, để 1–2 pixel đường làm nền; không khoét lỗ đường dưới chân cột |
| hard_panoptic — xe ô tô bị che khuất một phần bởi người đứng trước | Xe là một instance thing, phần bị người che không nhìn thấy: có tô tiếp vào vùng bị che không? | Quy tắc: chỉ tô phần nhìn thấy; không đoán hoặc vẽ xuyên qua vật che | Tô `car` chỉ phần thân xe lộ ra hai bên và phía trên người, bỏ qua vùng bị che — giữ là một instance |
