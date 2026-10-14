## Chạy hệ thống

```bash
# 1. Kiểm thử backend
cd backend
./gradlew test

# 2. Build Docker Image
docker build -t campus-hub-backend .

# 3. Xem danh sách image
docker images

# 4. Chạy backend và MySQL bằng Docker Compose (từ thư mục gốc repo)
cd ..
docker compose config
docker compose up -d --build
docker compose ps
```

## Kiểm tra

- URL: http://localhost:8080/actuator/health
- Kết quả mong đợi: HTTP 200 và `{"status":"UP"}`

```bash
curl -i http://localhost:8080/actuator/health
```

Dừng hệ thống: `docker compose down` (thêm `-v` nếu muốn xóa dữ liệu MySQL).
