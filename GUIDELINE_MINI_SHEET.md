# Phiếu quy tắc gán nhãn — Ngày 2

**Họ và tên:** Nguyễn Hải Nam<br>
**MSSV:** 2A202602257<br>
**Hình thức:** cá nhân<br>
**Mã cặp:** SOLO

## 1. Phạm vi

- Chỉ gán phương tiện thuộc bốn lớp bên dưới.
- Mỗi phương tiện là một hộp; không gộp nhiều xe.
- Không gán người, xe máy, xe đạp, biển báo hoặc phần phản chiếu.
- Vật thể quá nhỏ hoặc mờ đến mức không thể phân lớp có căn cứ: không đoán; ghi lý do vào nhật ký quyết định.

## 2. Bốn lớp cố định

| Mã | Lớp | Gán khi nhìn thấy | Không gán vào lớp này |
| ---: | --- | --- | --- |
| 0 | `car` (ô tô con) | sedan, hatchback, SUV, taxi, xe bán tải dùng như xe con | xe có thùng/ben rõ; thân xe buýt; xe van thân hộp |
| 1 | `truck` (xe tải) | thùng, ben, sàn hàng hoặc thiết bị công vụ rõ ràng | ô tô con; thân xe buýt; xe van kín một khối |
| 2 | `bus` (xe buýt) | thân xe khách dài, nhiều cửa sổ hoặc hàng ghế | xe van nhỏ; xe tải; ô tô con |
| 3 | `van` (xe van) | thân hộp nhỏ, kín, dùng chở người hoặc hàng | thân xe buýt; khoang hàng tách biệt như xe tải |

Thứ tự lớp là cố định: `0 car, 1 truck, 2 bus, 3 van`.

## 3. Hộp giới hạn

- Vẽ sát phần vật thể nhìn thấy.
- Không ước lượng phần bị xe khác che.
- Vật thể chạm mép ảnh vẫn được gán nếu đủ bằng chứng phân lớp.
- Không để hộp chứa nhiều nền hoặc nhiều phương tiện.

## 4. Ba thuộc tính

| Thuộc tính | Giá trị | Ý nghĩa |
| --- | --- | --- |
| `visibility` (mức nhìn thấy) | `clear` (rõ), `occluded` (bị che), `unclear` (không rõ) | mức bằng chứng nhìn thấy |
| `boundary` (quan hệ mép ảnh) | `inside` (trong ảnh), `truncated` (bị cắt) | vật thể có bị mép ảnh cắt hay không |
| `review_state` (trạng thái xem lại) | `confident` (tự tin), `needs_review` (cần xem lại) | đánh dấu quyết định cần quay lại |

YOLO không lưu ba thuộc tính này. Vì vậy phải xuất thêm `CVAT for images 1.1` từ cùng công việc.

## 5. Ba tình huống mơ hồ

Hoàn thành trước khi xem bài của người khác hoặc bộ nhãn tham chiếu.

### Tình huống A — xe buýt hay xe van?

- Ảnh và mã vật thể: drive_038.jpg - ID 15
- Dấu hiệu nhìn thấy: Xe có hình hộp nhưng thân hơi dài, góc chụp từ phía sau nên tôi không đếm được có bao nhiêu cửa sổ hay hàng ghế bên trong.
- Quy tắc áp dụng: Dựa theo mục 2, `bus` phải có thân dài, nhiều cửa sổ hoặc hàng ghế rõ ràng. `van` thì có thân hộp nhỏ và kín hơn.
- Quyết định: van (Do kích thước tổng thể trông khá gọn gàng, không đủ dài như xe buýt thông thường).
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? Tôi sẽ phóng to lên 100% để rọi xem có dãy ghế nào không. Nếu vẫn không chắc chắn, tôi sẽ đánh dấu `review_state = needs_review` để hỏi thêm thay vì tự đoán.

### Tình huống B — xe tải hay xe van/ô tô con?

- Ảnh và mã vật thể: drive_008.jpg - ID 9
- Dấu hiệu nhìn thấy: Một chiếc xe bán tải đang chạy trên đường, có thùng phía sau nhưng không có vẻ gì là chở hàng nặng hay có thiết bị công vụ.
- Quy tắc áp dụng: Xe bán tải dùng như xe con thì xếp vào lớp `car`. `truck` chỉ dùng khi có thùng, ben hoặc thiết bị công vụ rõ ràng.
- Quyết định: car
- Nếu vẫn thiếu bằng chứng, bạn sẽ làm gì? Tôi sẽ đặt `review_state = needs_review` và chụp ảnh lại để trao đổi với Lab Coach.

### Tình huống C — bị che, bị mép ảnh cắt hay không đủ bằng chứng?

- Ảnh và mã vật thể: drive_033.jpg - ID 21
- Dấu hiệu nhìn thấy khi phóng 100%: Một chiếc ô tô con bị mép ảnh bên trái cắt mất phần đầu, chỉ nhìn thấy từ cửa trước đến đuôi, đồng thời bị cột điện che một phần thân.
- Giá trị `visibility`: occluded
- Giá trị `boundary`: truncated
- Trạng thái `review_state`: confident
- Lý do: Mặc dù xe vừa bị cắt ở mép ảnh (truncated) vừa bị cột điện che (occluded), nhưng hình dáng phần đuôi hatchback hiển thị đủ rõ để tôi tự tin chốt đây là ô tô con (confident).

## 6. Xác nhận tự kiểm tra

- [x] Đã rà đủ bốn ảnh.
- [x] Đã kiểm vật thể thiếu và trùng.
- [x] Đã kiểm lớp và hình học từng hộp.
- [x] Mỗi hộp có đủ ba thuộc tính.
- [x] Đã xử lý mọi hộp `needs_review`.
- [x] Đã hoàn thành ba tình huống trước khi xem nguồn đối chiếu.
- [ ] Nếu làm theo cặp, hai người đã xuất bài độc lập trước khi trao đổi.
- [x] Nếu làm cá nhân, bài riêng đã được kiểm trước khi nhận bộ tham chiếu.
- [x] Số vật thể thực tế: 74 — 40–60 là mục tiêu khối lượng, không phải điểm cắt.
