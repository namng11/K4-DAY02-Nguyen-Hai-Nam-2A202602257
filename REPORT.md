**Họ và tên:** Nguyễn Hải Nam<br>
**MSSV:** 2A202602257<br>
**Hình thức:** cá nhân<br>
**Mã cặp:** SOLO

## 1. Bài độc lập và nguồn dữ liệu

- Mã SHA-256 của ZIP ảnh được cấp: F7D99888F21440FB0374D84962B93213BD8C14E665D093CC8D37F4C61B71ED33
- Bốn mã ảnh: drive_008.jpg, drive_022.jpg, drive_033.jpg, drive_038.jpg
- Số vật thể thực tế: 74
- Mã SHA-256 của gói YOLO của bạn: 2D36922E09A0164C10097ED42498B6CA752CD5B829585DB4AF571082AC3A12DF
- Mã SHA-256 của gói CVAT gốc của bạn: 6C8C0CBD055DA71E0913502E710B8ED5BA45181DF5A3AC11E6224ED86943A5F1
- Nguồn đối chiếu: bạn cùng cặp hoặc bộ tham chiếu do người hướng dẫn thực hành cấp: Bộ tham chiếu do người hướng dẫn thực hành cấp (Thầy giáo)
- Mã SHA-256 của gói đối chiếu: C8BBC767D8BB9A29F4CA5ABF0C3516E5C2AF94C58143A980B0148CFE0B500D2B
- Nếu làm cá nhân, ghi mã lần phát và thời điểm nhận bộ tham chiếu: [Điền mã/thời điểm]

Giải thích vì sao bài của bạn vẫn độc lập trước khi đối chiếu:

Tôi tự thực hiện gán nhãn trên CVAT của riêng mình, xuất dữ liệu và chốt mã SHA-256 trước khi tải và đối chiếu với bộ tham chiếu do Lab Coach cung cấp.

## 2. Quyết định phân lớp

| Ảnh/vật thể | Lớp | Dấu hiệu nhìn thấy | Quy tắc áp dụng |
| --- | --- | --- | --- |
| drive_022.jpg / ID 12 | truck | Xe có thùng hàng hở rõ ràng ở phía sau | Xe tải (truck) có thùng, ben hoặc sàn hàng rõ ràng |

Nêu một ví dụ cho thấy lớp và thuộc tính là hai loại thông tin khác nhau:

Lớp (như `car` hoặc `truck`) dùng để phân loại bản chất của phương tiện đó là loại xe gì. Trong khi đó, thuộc tính (như `visibility = occluded`) mô tả trạng thái vật lý của phương tiện đó trong bức ảnh (ví dụ: đang bị cái cây che khuất), trạng thái này có thể xảy ra với bất kỳ lớp phương tiện nào.

## 3. Tự kiểm tra và sửa nhãn

| Trước khi sửa | Loại lỗi | Cách phát hiện | Sau khi sửa và quy tắc |
| --- | --- | --- | --- |
| Gán nhầm xe bán tải thành xe tải | lớp | Soát lại thủ công | Đổi về `car` do quy tắc xe bán tải dùng như xe con thuộc lớp `car` |

- Số hộp `needs_review` trước và sau khi kiểm: Trước khi kiểm: 3 hộp, Sau khi kiểm: 0 hộp (đã tự xử lý hết).
- Một quyết định chưa đủ bằng chứng và cách bạn xin hỗ trợ: Có một chiếc xe bị khuất ở xa đuôi ảnh `drive_033`, tôi đã gán `needs_review` để hỏi Lab Coach và được tư vấn là xóa hộp đó vì vật thể quá mờ không thể đoán lớp.

## 4. Một dòng nhãn YOLO

- Dòng `class x_center y_center width height`: 1 0.940438 0.459469 0.083906 0.110594
- Tên lớp và tọa độ điểm ảnh `xyxy`: Lớp `truck` (1). Tâm=(0.9404, 0.4595), Kích thước=(0.0839, 0.1106). Tọa độ pixel xyxy: [575.0, 258.7, 628.7, 329.5].
- Vì sao dòng đúng định dạng vẫn có thể sai lớp, phạm vi hoặc hình học?
Định dạng YOLO (chỉ chứa các số vô hướng) không lưu thông tin trực quan. Dòng dữ liệu đúng chuẩn cú pháp vẫn có thể sai về nhãn (ví dụ nhầm xe tải thành xe con) hoặc hộp bị vẽ quá rộng/quá hẹp so với viền thực tế của xe trên ảnh.

## 5. Huấn luyện và dự đoán thử

- Ba mã ảnh huấn luyện: drive_022, drive_033, drive_038
- Mã ảnh thẩm định: drive_008
- Mô tả một dự đoán trong `detect_result.jpg`: Mô hình có thể nhận diện đúng vị trí một số xe ô tô nhưng độ tự tin (confidence) chưa cao.
- Dự đoán đó gợi ý cần kiểm lại quy tắc hoặc dữ liệu nào? Cần kiểm tra lại các hộp bounding box đã vẽ xem có bám sát viền xe chưa, hoặc có bị nhầm lẫn giữa car và van/truck không.
- Minh chứng nào có thể bác bỏ nhận định của bạn? Nếu mAP trên tập validation lớn hoặc IoU rất cao trên các ảnh khác.
- Vì sao kết quả trên bốn ảnh không phải phép đánh giá mô hình dùng thực tế?
Số lượng 4 ảnh là quá ít để huấn luyện một mô hình Deep Learning. Bộ dữ liệu không bao phủ đủ các điều kiện ánh sáng, góc chụp và các loại phương tiện đa dạng ngoài đời thực.

## 6. Đối chiếu nhãn

- Số hộp ghép được: 48
- IoU trung bình và trung vị: Trung bình: 0.86008, Trung vị: 0.88418
- Mức đồng thuận lớp: 72.92% (0.729167)
- Số hộp phía bạn không ghép được: 26
- Số hộp phía đối chiếu không ghép được: 2
- Một điểm khác biệt cụ thể: Số lượng vật thể tôi gán nhiều hơn rõ rệt (tôi gán 74 hộp, đối chiếu chỉ có 50 hộp), dẫn đến 26 hộp của tôi không ghép được.
- Quy tắc hoặc hành động sửa phát sinh: Cần giới hạn lại phạm vi gán nhãn, bỏ bớt các vật thể quá xa, quá nhỏ, hoặc không có bằng chứng phân lớp rõ ràng.
- Vì sao mức đồng thuận cao không chứng minh mọi nhãn đều đúng?
Hai người có thể đồng thuận và có IoU cao nhưng vẫn cùng sai (ví dụ cùng gán nhầm xe bán tải thành xe tải, hoặc cùng dùng chung một công cụ tự động gán nhãn sai). Mức đồng thuận chỉ cho thấy độ nhất quán, chứ không đại diện cho chân lý tuyệt đối.

## 7. Kiểm tra kho GitHub cá nhân

- [ ] Có phiếu quy tắc với ba tình huống mơ hồ.
- [ ] Có kết quả kiểm hai gói xuất.
- [ ] Có thông tin lần huấn luyện và ảnh dự đoán.
- [ ] Có tóm tắt, bảng và ảnh phủ của bước đối chiếu.
- [ ] Không có gói xuất thô, bộ nhãn tham chiếu hoặc trọng số mô hình.
- [ ] Không có dữ liệu VinFast/khách hàng/ảnh cá nhân/mật khẩu/mã truy cập.

Minh chứng mạnh nhất trong bài và câu hỏi còn lại cho Lab Coach:

Minh chứng mạnh nhất là tôi đã phân biệt đúng toàn bộ xe tải/van dựa vào đặc điểm thùng hàng và ghi chú cẩn thận các trường hợp bị che khuất. Câu hỏi cho Lab Coach: Đối với các xe ở tít xa cuối đường, nếu không thể xác định loại xe nhưng biết chắc đó là ô tô, thì có nên quy định thêm nhãn "unknown_vehicle" để tránh bỏ sót không?
