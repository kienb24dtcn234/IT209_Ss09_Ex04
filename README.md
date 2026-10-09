# Session 09 - Bài 4: Lưu trữ dữ liệu đơn giản với Docker Volume

**Họ tên:** Nguyễn Thế Kiên | **Lớp:** K24 - DevOps PTIT

## Mục tiêu
Tạo một Named Volume để lưu dữ liệu bền vững giữa các lần chạy container, rồi kiểm tra dữ liệu có còn sau khi container bị xóa hay không.

---

## Các lệnh đã gõ

### 1. Tạo volume `easy-volume`
```bash
docker volume create easy-volume
```
```
easy-volume
```

### 2. Container Alpine thứ nhất: ghi dữ liệu vào volume
```bash
docker run --rm -v easy-volume:/data alpine sh -c "echo 'Luu tru du lieu Docker' > /data/test.txt"
```
```
nable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
ec67817084c6: Download complete 
36c38f094eae: Download complete 
Digest: sha256:294b683cb724975bec92580e1e685676bd4b50bda910ddb8c51d4cabeaec77e6
Status: Downloaded newer image for alpine:latest
```

### 3. Container Alpine thứ hai: đọc lại tệp
```bash
docker run --rm -v easy-volume:/data alpine cat /data/test.txt
```
```
Luu tru du lieu Docker
```

### 4. Kiểm tra danh sách volume
```bash
docker volume ls
```
```
DRIVER    VOLUME NAME
local     4d254804d4f5bc264516c709665091b5cf2efa90d1eea9f43cfcd72e7e29afa0
local     easy-volume
local     ptit_final_ai_integrated_007_langfuse_db_data
```

---

## Giải thích
- `docker volume create easy-volume`: tạo một Named Volume do Docker quản lý, nằm ngoài vòng đời của container.
- `-v easy-volume:/data`: gắn volume vào thư mục `/data` bên trong container.
- `--rm`: tự xóa container ngay sau khi chạy xong.

Container thứ nhất ghi file rồi bị xóa (`--rm`), nhưng file `test.txt` nằm trong volume chứ không nằm trên Writable Layer của container. Vì vậy container thứ hai (hoàn toàn mới) gắn cùng volume vẫn đọc được file.

## Kết quả
Terminal in ra đúng dòng:
> Luu tru du lieu Docker

→ Dữ liệu lưu trong volume **không bị mất** khi container dừng hoặc bị xóa.
