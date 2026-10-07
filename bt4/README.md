# Bài 4: Quản lý tiến trình nền với nohup và tín hiệu Kill

## 1. Tạo và cấp quyền kịch bản (script)
Mã nguồn shell script đã được lưu tại tệp `loop-monitor.sh`.
Cấp quyền thực thi cho script:
```bash
chmod +x loop-monitor.sh
```

## 2. Khởi chạy tiến trình dưới nền độc lập
Khởi chạy script bằng `nohup` kết hợp với `&` để nó vẫn tiếp tục hoạt động kể cả khi ngắt kết nối SSH, bỏ qua log output mặc định (chuyển vào `/dev/null`):
```bash
nohup ./loop-monitor.sh > /dev/null 2>&1 &
```

## 3. Kiểm tra tiến trình và file log

### 3.1. Tìm định danh tiến trình (PID)
Lệnh tìm PID của kịch bản đang chạy:
```bash
pgrep -f loop-monitor.sh
# Hoặc: ps aux | grep loop-monitor.sh
```
*(Ví dụ kết quả PID trả về là: `14560`)*

### 3.2. Kiểm tra việc ghi log
Xem 10 dòng cuối cùng của file log để xác nhận script vẫn đang ghi dữ liệu liên tục:
```bash
tail -n 10 /tmp/monitor.log
```

**Kết quả (Đầu ra dự kiến):**
```text
System time: Mon Oct  7 22:49:01 +07 2026
System time: Mon Oct  7 22:49:06 +07 2026
System time: Mon Oct  7 22:49:11 +07 2026
...
```

## 4. Tắt tiến trình an toàn
Sử dụng lệnh `kill` với tín hiệu SIGTERM (15) để yêu cầu tắt tiến trình một cách an toàn:
```bash
kill -15 14560
```
*(Thay thế `14560` bằng PID thực tế lấy được ở bước 3.1)*

Sau khi gửi tín hiệu, kịch bản sẽ biến mất khỏi danh sách tiến trình.
