# Phân tích lỗi & Bảng Test Cases

## 1. Phân tích lỗi (Debug Report)

**Dòng lệnh sai logic trong mã nguồn cũ:**
```c
if (user_list[0].days_overdue <= 3) {
    total_revenue += user_list[0].monthly_fee;
```

**Nguyên nhân kỹ thuật dẫn đến sự cố nghiệp vụ:**
Trong vòng lặp `for` duyệt qua danh sách các tài khoản khách hàng (`user_list`), thay vì dùng biến chạy `i` làm chỉ số mảng để truy xuất tuần tự từng phần tử trong mảng (tức là dùng `user_list[i]`), lập trình viên tiền nhiệm lại gán cứng (hardcode) chỉ số mảng là `0` (tức là `user_list[0]`). 

Điều này dẫn đến các hệ quả:
1. **Kiểm tra sai điều kiện:** Biểu thức điều kiện luôn kiểm tra số ngày quá hạn của phần tử đầu tiên (`user_list[0].days_overdue <= 3`). Vì phần tử 0 đúng hạn (0 ngày), điều kiện này luôn đánh giá là `true` cho mọi vòng lặp. Do đó, tất cả các tài khoản đều in ra trạng thái là "Hop le", bất kể thực tế chúng đã quá hạn hơn 3 ngày.
2. **Tính toán sai doanh thu:** Biến `total_revenue` ở mỗi lần lặp đều được cộng thêm phí hàng tháng của phần tử đầu tiên (`user_list[0].monthly_fee = 180000`). Vì vòng lặp chạy 4 lần, kết quả tổng doanh thu luôn bị tính sai là `180000 * 4 = 720000 VND` thay vì cộng đúng doanh thu của các tài khoản hợp lệ (1001 và 1003).

**Cách khắc phục:**
Sửa `user_list[0]` thành `user_list[i]` tại cả hai vị trí: trong biểu thức điều kiện `if` và trong lệnh tính tổng `total_revenue`.

---

## 2. Bảng Test Cases

| Trường hợp kiểm thử (Test Case) | Dữ liệu đầu vào (Input) | Kết quả sai thực tế (Actual / Bug) | Kết quả đúng mong đợi (Expected) |
| :--- | :--- | :--- | :--- |
| 1. Kiểm tra trạng thái của các tài khoản in ra màn hình | Mảng `user_list` gồm 4 tài khoản với số ngày quá hạn lần lượt là:<br>- TK 1001: 0 ngày<br>- TK 1002: 5 ngày<br>- TK 1003: 1 ngày<br>- TK 1004: 4 ngày | Tất cả 4 tài khoản đều in ra trạng thái "Hop le". | TK 1001: "Hop le"<br>TK 1002: "Qua han"<br>TK 1003: "Hop le"<br>TK 1004: "Qua han" |
| 2. Kiểm tra tổng doanh thu thực thu | Phí tháng của các tài khoản (monthly_fee):<br>- TK 1001: 180,000<br>- TK 1002: 90,000<br>- TK 1003: 260,000<br>- TK 1004: 90,000 | In ra: 720000 VND <br>*(Tính từ biểu thức 180,000 x 4 lần)* | In ra: 440000 VND <br>*(Chỉ cộng TK 1001 và TK 1003: 180,000 + 260,000)* |
