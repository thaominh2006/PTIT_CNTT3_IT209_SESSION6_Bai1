## 1. Mục tiêu

Tạo cấu trúc thư mục dùng chung cho ứng dụng web tại `/var/www/my-app`, phân quyền bằng chmod dạng số (octal) và gán chủ sở hữu/nhóm sở hữu bằng chown.

## 2. Nhật ký các lệnh đã thực hiện

### Bước 1: Tạo cấu trúc thư mục

```bash
sudo mkdir -p /var/www/my-app/public
sudo mkdir -p /var/www/my-app/logs
```

Giải thích: `-p` tạo cả thư mục cha nếu chưa tồn tại. Cần `sudo` vì `/var/www` thuộc sở hữu của root.

### Bước 2: Phân quyền thư mục public (750)

```bash
sudo chmod 750 /var/www/my-app/public
```

| Đối tượng | Quyền | Giá trị |
|-----------|-------|---------|
| Owner     | rwx   | 7       |
| Group     | r-x   | 5       |
| Others    | ---   | 0       |

Giải thích: Owner đọc/ghi và điều hướng; Group chỉ đọc và truy cập; Others không có quyền. Với thư mục, quyền `x` là bắt buộc để điều hướng (`cd`) và truy cập file bên trong, nên quyền "đọc" của Group được hiểu là r-x (5) chứ không phải r-- (4). Cách hiểu này khớp với kết quả mong đợi `drwxr-x---` của đề bài.

### Bước 3: Phân quyền thư mục logs (770)

```bash
sudo chmod 770 /var/www/my-app/logs
```

| Đối tượng | Quyền | Giá trị |
|-----------|-------|---------|
| Owner     | rwx   | 7       |
| Group     | rwx   | 7       |
| Others    | ---   | 0       |

### Bước 4: Gán chủ sở hữu và nhóm sở hữu

```bash
sudo chown -R $USER:www-data /var/www/my-app
```

Giải thích: Chủ sở hữu là tài khoản non-root hiện tại (`admin1`), nhóm sở hữu là `www-data` (nhóm của máy chủ web trên Ubuntu). Tùy chọn `-R` áp dụng đệ quy cho toàn bộ cây thư mục.

## 3. Kiểm tra

```bash
ls -la /var/www/my-app
```

Kết quả thực tế:

```text
total 16
drwxr-xr-x 4 admin1 www-data 4096 Oct  6 20:23 .
drwxr-xr-x 3 root   root     4096 Oct  6 20:23 ..
drwxrwx--- 2 admin1 www-data 4096 Oct  6 20:23 logs
drwxr-x--- 2 admin1 www-data 4096 Oct  6 20:23 public
```

## 4. Đối chiếu kết quả

| Thư mục | Quyền mong đợi   | Quyền thực tế | Owner:Group     | Kết quả |
|---------|------------------|---------------|-----------------|---------|
| public  | drwxr-x--- (750) | drwxr-x---    | admin1:www-data | [PASS]  |
| logs    | drwxrwx--- (770) | drwxrwx---    | admin1:www-data | [PASS]  |

## 5. Kết luận

Đã tạo đúng cấu trúc `/var/www/my-app/{public,logs}`, phân quyền theo hệ bát phân và gán chủ sở hữu/nhóm sở hữu đúng yêu cầu.
'@

[System.IO.File]::WriteAllText((Join-Path $dir "README.md"), $content, (New-Object System.Text.UTF8Encoding($false)))
Get-Content (Join-Path $dir "README.md") -Encoding UTF8 -TotalCount 5