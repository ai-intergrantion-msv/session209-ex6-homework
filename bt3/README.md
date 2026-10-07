# Bài 3: Cấu hình tường lửa UFW và chuẩn đoán cổng mạng

## 1. Nhật ký cấu hình UFW

### 1.1. Thiết lập chính sách mặc định
Cấu hình chặn tất cả các kết nối đi vào (incoming) và cho phép tất cả các kết nối đi ra (outgoing) mặc định:
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

### 1.2. Mở cổng mạng
Mở cổng kết nối SSH (port 22) để không bị mất kết nối tới máy chủ, và mở cổng 8080/tcp cho ứng dụng Web:
```bash
sudo ufw allow 22/tcp
sudo ufw allow 8080/tcp
```

### 1.3. Kích hoạt UFW
Bật tường lửa và áp dụng các quy tắc vừa tạo (nhập `y` khi có cảnh báo):
```bash
sudo ufw enable
```

## 2. Kết quả kiểm tra

### 2.1. Kiểm tra trạng thái tường lửa
Lệnh kiểm tra:
```bash
sudo ufw status verbose
```

**Kết quả hiển thị (Status: active):**
```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere                  
8080/tcp                   ALLOW IN    Anywhere                  
22/tcp (v6)                ALLOW IN    Anywhere (v6)             
8080/tcp (v6)              ALLOW IN    Anywhere (v6)             
```

### 2.2. Kiểm tra các cổng đang lắng nghe (Listen) trên máy chủ
Lệnh kiểm tra:
```bash
sudo ss -tlnp
```

**Kết quả minh họa:**
```text
State    Recv-Q   Send-Q     Local Address:Port      Peer Address:Port   Process                                       
LISTEN   0        128              0.0.0.0:22             0.0.0.0:*       users:(("sshd",pid=850,fd=3))
LISTEN   0        511                    *:8080                 *:*       users:(("node",pid=1520,fd=18))
LISTEN   0        128                 [::]:22                [::]:*       users:(("sshd",pid=850,fd=4))
```
*(Kết quả cột `Process` sẽ hiển thị ứng dụng thực tế đang chạy ở cổng 8080 như Node.js, Java, Nginx, v.v.)*
