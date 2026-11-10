# Mini Guideline: Gán nhãn xe 4 lớp

## 1. Định nghĩa 4 lớp

- **0 - car:** xe ô tô con chở người như sedan, hatchback, SUV, taxi và xe bán tải dùng cá nhân/gia đình.
- **1 - truck:** xe tải chở hàng, xe đầu kéo, xe có thùng hàng tách riêng khỏi cabin.
- **2 - bus:** xe buýt và xe khách chở nhiều người, thân dài, có hàng cửa sổ dọc hai bên; xe buýt khớp nối tính là một xe.
- **3 - van:** xe thân hộp chở hàng hoặc chở người cỡ vừa (van, minivan), cao hơn car và ngắn hơn bus.

## 2. Quy tắc vẽ box

- **Độ sát:** box ôm sát phần thân xe nhìn thấy, không lấy thêm mặt đường, bầu trời hay xe bên cạnh; mỗi xe một box riêng.
- **Bị che khuất:** vẽ box quanh phần nhìn thấy nếu từ đó vẫn nhận ra lớp; hai xe chồng nhau thì vẽ hai box riêng. Xe chỉ còn mảnh nhỏ không xác định được lớp thì không gán.
- **Bị cắt bởi rìa ảnh:** box chạm đúng rìa ảnh, không đoán phần nằm ngoài khung hình.
- **Không gán:** mô tô/xe máy, xe ba bánh, người đi bộ, xe đạp, biển báo, đèn giao thông, phản chiếu trong kính, hình xe trên bảng quảng cáo.

## 3. Ba trường hợp mơ hồ và quyết định

1. **Xe buýt khớp nối** : gồm hai thân xe nối nhau. Quyết định: vẽ một box bao cả xe, lớp `bus`, vì đây là một phương tiện chở khách; vẽ hai box sẽ đếm thành hai xe.
2. **Xe ba bánh chở hàng và xe máy cạnh xe con** : hình dáng khác bốn lớp của schema. Quyết định: không gán, vì không thuộc car/truck/bus/van.
3. **Xe bị cột đèn che một phần** : thân xe bị chắn một dải. Quyết định: vẽ box quanh phần nhìn thấy, lớp `car`, vì vẫn nhận ra được loại xe từ phần còn lại.
