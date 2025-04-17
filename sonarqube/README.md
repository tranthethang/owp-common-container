Cài sonarqube trên local bằng file docker-compose đính kèm

1. giải nén vào folder, và chạy docker compose up -d
2. truy cập địa chỉ http://localhost:9001 (tài khoản admin, mật khẩu admin)
3. tạo project mới -> tạo key -> sẽ nhận được 1 đoạn code cho sonarqube generate ra (xem các ảnh đính kèm từ 1 ~ 4)
4. cài sonar-scanner bằng npm

```
npm i -g sonar-scanner
```
5. cd vào thư mục chứa code, chạy lệnh có được ở bước 3





NOTE FOR UBUNTU
Ae nào cài sonarqube mà dùng Ubuntu nếu gặp lỗi run image mà bị exited luôn thì làm theo các bước sau nhé:
1: sudo nano /etc/sysctl.conf
2: Add line end file:
```
vm.max_map_count=262144
```

3: Run
```
sudo sysctl -p
```

4: RUN:
```
docker compose down
```

5: RUN
```
docker compose up -d
```
