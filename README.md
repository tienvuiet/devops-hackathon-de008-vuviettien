DevOps Hackathon – Đề : Quản lý thư viện (library)

Thông tin sinh viên: Vũ Việt Tiến, mã sinh viên B24DTCN276, lớp HN-K24-CNTT4. Tài khoản Linux: tienvv-hnk24cntt4. GitHub: tienvuiet. Website: http://221.121.3.208:8080/. Repository: https://github.com/tienvuiet/devops-hackathon-de008-vuviettien, nhánh main.

Môi trường triển khai: VPS chạy Ubuntu 24.04, Nginx 1.24.0 (Ubuntu), Git 2.43.0; sử dụng UFW và curl. Mã nguồn được triển khai tại /var/www/devops-hackathon-de002-nguyenvana/.

Cấu trúc dự án: src/index.html chứa trang web; nginx/tienvv-hnk24cntt4.conf chứa cấu hình Nginx; screenshots/ lưu sáu ảnh minh chứng từ 01-user.png đến 06-update.png. File .gitignore khai báo các tệp bỏ qua, README.md mô tả cấu hình dự án.

Cấu hình Nginx: listen dùng cổng 8080 cho IPv4 và IPv6; server_name là 221.121.3.208; root trỏ đến /var/www/devops-hackathon-de008-vuviettien/src; index là index.html. Log truy cập và lỗi lần lượt lưu tại /var/log/nginx/tienvv-hnk24cntt4.access.log và /var/log/nginx/tienvv-hnk24cntt4.error.log. Trong location /, allow all cho phép mọi nguồn truy cập, try_files $uri $uri/ =404 tìm tệp hoặc thư mục và trả lỗi 404 khi không tồn tại. Cấu hình được kích hoạt qua sites-available và sites-enabled, kiểm tra bằng sudo nginx -t rồi tải lại bằng sudo systemctl reload nginx. UFW cho phép cổng 22/tcp và 8080/tcp cho IPv4 và IPv6.


