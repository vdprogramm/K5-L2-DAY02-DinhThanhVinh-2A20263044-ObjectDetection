# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên:Đinh Thành Vinh
- MSSV:2A20263044
- Email:dinhthanhvinhnb205@gmail.com
- Lớp:K5 Level 2 (ADAS)
- Link repo bài làm: 
- Link Colab đã chạy: https://colab.research.google.com/drive/12NS-89YZsBAGTSIf4Kc30BsVH0PXiglP

## 2. Schema và mini guideline

Dẫn tới GUIDELINE_MINI.md. Tóm tắt ba ca mơ hồ đã quyết định:

Xe bị che khuất một phần (Occlusion > 50%): Chỉ vẽ bounding box bao quanh phần thân xe thực tế nhìn thấy được; nếu xe bị che khuất quá 80% chỉ còn lại mảnh nhỏ không xác định được hình khối thì bỏ qua không gán.
Xe cắt mép viền ảnh (Truncation): Bounding box chạm đúng mép viền của ảnh, tuyệt đối không suy đoán phần thân xe nằm ngoài khung hình.
Phân định xe bán tải (Pickup truck) và Van: Xe bán tải cỡ nhỏ phục vụ gia đình xếp vào lớp 0: car; các dòng xe tải chở hàng thùng kín/lửng xếp vào lớp 1: truck; các dòng chở khách 9–16 chỗ hoặc xe tải nhỏ thùng kín xếp vào 3: van.

## 3. Annotation và self-QC

Số file nhãn train: 50 file YOLO (dataset/labels/train/drive_001.txt đến drive_050.txt).
Kết quả checker và lỗi đã xử lý: Chạy python3 tools/check_yolo_labels.py đạt 50/50 file hợp lệ; không có box vượt biên ảnh, tọa độ chuẩn hóa 
[
0
,
1
]
.
Danh sách ít nhất 10 ảnh đã rà bằng mắt: drive_001, drive_002, drive_005, drive_012, drive_015, drive_020, drive_026, drive_033, drive_041, drive_050.
Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa: Phát hiện model gán sót xe đỗ phía xa và xe bị ngược sáng; tiến hành bổ sung box và chuẩn hóa lại ID lớp tránh nhầm lẫn giữa car và van.
Commit khóa nhãn ban đầu: a5b0f94

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 351 | không rework |
| FP | 210 | không rework |
| FN | 302 | không rework |
| Precision | 0.6257 | không rework |
| Recall | 0.5375 | không rework |
| F1 | 0.5783 | không rework |
| mean_iou_of_matches | 0.8722 | không rework |
| passed | false | không rework |

Kết quả nằm ở [`outputs/annotation_evaluation.json`](../../outputs/annotation_evaluation.json). 

## 5. Kết quả YOLO

- Runtime: Google Colab, GPU Tesla T4, Python 3.13.16, Ultralytics 8.4.208, Torch 2.14.1+cu130
- Tham số thay đổi so với notebook: không thay đổi (50 epoch, imgsz 640, batch tự động). [ĐIỀN: xác nhận bạn không sửa cell nào]
- Best checkpoint trong phiên chạy: `/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt` (không nộp file `.pt`)
- Link Colab đã chạy: [ĐIỀN: link Share, Anyone with the link]

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.4101 |
| Recall | 0.2298 |
| mAP50 | 0.2719 |
| mAP50–95 | 0.2016 |

(Lấy từ `model_eval_metrics.json`.)

Theo từng lớp (`yolo26_runs/vehicle_eval`):

| Lớp | Số xe eval | Precision | Recall | mAP50 | mAP50–95 |
|---|---:|---:|---:|---:|---:|
| car | 94 | 0.833 | 0.636 | 0.738 | 0.523 |
| truck | 5 | 0.673 | 0.200 | 0.239 | 0.205 |
| bus | 2 | 0 | 0 | 0.008 | 0.004 |
| van | 12 | 0.134 | 0.083 | 0.103 | 0.074 |

Ghi chú về độ ổn định: tôi chạy notebook nhiều lần trên bộ nhãn gốc của mình và mAP50 dao động từ 0.27 đến 0.64, vì eval chỉ có 10 ảnh, bus có 2 xe và truck có 5 xe. Kết quả nộp là lần chạy cuối ở trên, không chọn lần cao nhất. [ĐIỀN: xác nhận mọi lần chạy đều dùng nhãn gốc]

Tệp kết quả: [ảnh dự đoán 10 ảnh eval](assets/eval_predictions/visuals/), [log và đường cong train](assets/yolo26_runs/vehicle_annotation/), [chỉ số đánh giá](assets/yolo26_runs/vehicle_eval/).

Ảnh và vật thể cụ thể: trong `drive_051.jpg` model dự đoán chiếc xe màu đỏ ở dưới cùng là `van` với độ tin cậy 0.99, và trong `drive_054.jpg` chiếc xe màu đỏ sẫm bên phải là `van` 0.97. Hai xe này nhìn giống xe con. [ĐIỀN: mở `dataset/labels/eval/drive_051.txt` và `drive_054.txt`, xác nhận nhãn đúng của hai xe này là car (class 0); nếu đúng thì giữ nhận xét này là lỗi car bị dự đoán thành van, nếu không thì bỏ câu này]

Bốn câu hỏi cuối notebook:

1. Lớp có recall thấp nhất là bus (0 trên 2 xe), tiếp theo là van (0.083 trên 12 xe) và truck (0.2 trên 5 xe). Ví dụ `drive_053.jpg`: xe buýt lớn ở phía trước được dự đoán là bus 0.85 nhưng có hai khung chồng nhau, một khung rất lớn hơn thân xe, nên có thể IoU với nhãn thấp. [ĐIỀN: so với `dataset/labels/eval/drive_053.txt` để xác nhận]
2. Chỉ số mAP50 (0.272) và mAP50–95 (0.202) không chênh quá xa, nên vấn đề chính ở đây là phát hiện sót và phân lớp, chưa phải độ khít của box. Nếu mAP50 cao mà mAP50–95 thấp thì cần kiểm tra các cạnh của box dự đoán và box nhãn ở ngưỡng IoU cao (box lỏng hoặc lệch). Với nhãn tôi gán, mean IoU so với gold là 0.87 nên box nhìn chung khá sát.
3. Lỗi lớp có thể liên quan guideline: ranh giới car và van. Trong ảnh dự đoán, tổng số `van` model đưa ra là 8 (nhãn eval có 12 xe van) nhưng recall van chỉ 0.083, nghĩa là phần lớn dự đoán van không khớp xe van thật. Một số xe trông giống xe con (ví dụ hai xe đỏ ở `drive_051` và `drive_054`) bị gọi là van. Model cũng đưa ra 9 dự đoán bus trong khi nhãn eval chỉ có 2 xe bus, nên precision của bus bằng 0. Tôi nghi ngờ ranh giới car/van/bus trong dữ liệu train (số mẫu van, bus, truck rất ít: 53, 46 và 20 box trong nhãn train) và việc nhãn của tôi chưa chuẩn cho các lớp này đều góp phần, nhưng 10 ảnh eval quá nhỏ để kết luận chắc nguyên nhân. [ĐIỀN: mở `assets/yolo26_runs/vehicle_eval/confusion_matrix.png`, ghi một con số cụ thể bạn thấy, ví dụ số van bị nhận thành car]
4. Không dùng 10 ảnh eval để train để việc đánh giá không bị rò rỉ dữ liệu. Tuy nhiên trong notebook này eval cũng làm validation để chọn `best.pt`, nên nó không phải test set độc lập và số liệu có phần lạc quan.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Chấm gold | Lệnh báo không có gold labels | Giải nén `.rar` tạo thêm một tầng thư mục | Dùng `gold_train_labels/gold_train_labels`, chạy được |
| Chấm gold | Một lần chấm trả F1 = 1.0, mean IoU = 1.0, 653 box (không thể là nhãn tự gán) | Nghi thư mục nhãn trên máy bị lẫn dữ liệu gold [ĐIỀN: nguyên nhân thật nếu bạn biết] | Loại kết quả đó, khôi phục nhãn gốc từ file xuất CVAT, chấm lại ra F1 = 0.5783 |
| Colab lần 1 | Chạy CPU, mất khoảng 34 phút | Bật GPU T4 | Các lần sau mất khoảng 1,5 phút |
| Colab lần sau | `git pull --ff-only` báo lỗi 128 | Xóa runtime để clone lại | Chạy được |

## 7. Tự đánh giá theo rubric

- Hoàn thành và định dạng YOLO: có đủ 50 file, checker đạt 50/50.
- Đầy đủ vật thể và độ bám sát box: chưa đạt tốt. FN cao (302) và mean IoU 0.87 cho thấy box sát nhưng còn sót/khác lớp nhiều.
- Phân lớp nhất quán: [ĐIỀN: tự đánh giá, chưa kiểm tra từng ảnh]
- Mini guideline: có định nghĩa lớp, quy tắc và ba ca mơ hồ.
- Gold-set QC: chạy được, nhưng F1 0.5783 chưa đạt ngưỡng 0.80; mean IoU đạt. [ĐIỀN: ghi "chưa rework" hoặc mô tả rework]
- Pipeline YOLO: Colab chạy được, không dùng eval để train, có metrics và ảnh eval; phân tích còn hạn chế do eval nhỏ.

## 8. Công cụ AI và tài liệu tham khảo

Tôi dùng Claude (Anthropic) để: hướng dẫn quy trình làm bài, hướng dẫn thao tác CVAT/GitHub/Colab, đọc giải thích log và kết quả chấm, và góp ý/soạn bản nháp văn bản cho GUIDELINE_MINI.md và báo cáo. Phần tôi tự làm: gán nhãn 50 ảnh bằng CVAT, chạy checker, chạy evaluator và Colab, chép số liệu từ kết quả thật. [ĐIỀN: xác nhận và nêu rõ phần bạn đã tự đọc lại, sửa lại]
Tài liệu ngoài repo: tài liệu CVAT và Ultralytics YOLO. Không đính kèm gold train, token hay mật khẩu.
