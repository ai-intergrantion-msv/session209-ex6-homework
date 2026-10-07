# Bài 2: Cấu hình phân quyền Nhóm và sudoers bằng visudo

## 1. Nhật ký các lệnh đã thực hiện

### 1.1. Tạo nhóm và người dùng mới
Thực thi các lệnh sau để tạo nhóm `devops-admin`, tạo user `deployer` và thêm user này vào nhóm:
```bash
sudo groupadd devops-admin
sudo adduser deployer
sudo usermod -aG devops-admin deployer
```

### 1.2. Cấu hình quyền sudo
Mở file cấu hình sudoers một cách an toàn bằng `visudo`:
```bash
sudo visudo
```

**Trích dẫn dòng cấu hình đã thêm vào cuối file `/etc/sudoers`:**
```text
%devops-admin ALL=(ALL) NOPASSWD: /usr/bin/systemctl start *, /usr/bin/systemctl stop *, /usr/bin/systemctl restart *, /usr/bin/systemctl status *
```
*(Lưu ý: Ký tự `%` ở đầu dùng để biểu thị cấu hình áp dụng cho một Group)*

## 2. Kết quả kiểm tra

### 2.1. Kiểm tra quyền của tài khoản
Chuyển sang tài khoản `deployer` vừa tạo:
```bash
su - deployer
```

Kiểm tra danh sách các lệnh sudo được phép chạy:
```bash
sudo -l
```

**Kết quả (Đầu ra của lệnh `sudo -l`):**
```text
Matching Defaults entries for deployer on hostname:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User deployer may run the following commands on hostname:
    (ALL) NOPASSWD: /usr/bin/systemctl start *, /usr/bin/systemctl stop *, /usr/bin/systemctl restart *, /usr/bin/systemctl status *
```

### 2.2. Kiểm thử thực tế
Khởi động lại dịch vụ hệ thống (ví dụ: `cron`) để kiểm tra quyền NOPASSWD:
```bash
sudo systemctl restart cron
sudo systemctl status cron
```
*(Lệnh chạy thành công ngay lập tức, hoàn toàn không yêu cầu nhập mật khẩu của user deployer)*
