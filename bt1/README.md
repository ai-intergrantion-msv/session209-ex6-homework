# Bài 1: Khảo sát FHS và Phân quyền File/Folder nâng cao

## Nhật ký các lệnh đã thực hiện

### 1. Khởi tạo cấu trúc thư mục
Sử dụng lệnh `mkdir` với tùy chọn `-p` (tạo các thư mục cha nếu chưa có):
```bash
sudo mkdir -p /var/www/my-app/public
sudo mkdir -p /var/www/my-app/logs
```

### 2. Gán quyền cho thư mục
Gán quyền cho thư mục `public` (Owner: toàn quyền, Group: đọc/truy cập, Others: không có quyền) - Tương đương 750:
```bash
sudo chmod 750 /var/www/my-app/public
```

Gán quyền cho thư mục `logs` (Owner & Group: toàn quyền, Others: không có quyền) - Tương đương 770:
```bash
sudo chmod 770 /var/www/my-app/logs
```

### 3. Đổi nhóm sở hữu
Đổi nhóm sở hữu về `www-data` (nhóm web mặc định trên Ubuntu) và user thành user hiện tại:
```bash
sudo chown -R $USER:www-data /var/www/my-app
```

## Kết quả kiểm tra
Lệnh kiểm tra:
```bash
ls -la /var/www/my-app
```

Đầu ra của lệnh (Kết quả dự kiến):
```text
total 16
drwxr-xr-x 4 username www-data 4096 Oct  7 22:47 .
drwxr-xr-x 3 root     root     4096 Oct  7 22:47 ..
drwxrwx--- 2 username www-data 4096 Oct  7 22:47 logs
drwxr-x--- 2 username www-data 4096 Oct  7 22:47 public
```
*(Trong đó `username` là tên tài khoản đăng nhập hiện tại)*
