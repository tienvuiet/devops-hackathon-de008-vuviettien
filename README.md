DevOps Hackathon – Đề : Quản lý sản phẩm (Shop)

Thông tin sinh viên: Vũ Việt Tiến, mã sinh viên B24DTCN276, lớp HN-K24-CNTT4. Tài khoản Linux: tienvv-hnk24cntt4. GitHub: tienvuiet. Website: http://221.121.3.208:8080. Repository: https://github.com/tienvuiet/devops-hackathon-de002-nguyenvana, nhánh main.

Môi trường triển khai: VPS chạy Ubuntu 24.04, Nginx 1.24.0 (Ubuntu), Git 2.43.0; sử dụng UFW và curl. Mã nguồn được triển khai tại /var/www/devops-hackathon-de002-nguyenvana/.

Cấu trúc dự án: src/index.html chứa trang web; nginx/tienvv-hnk24cntt4.conf chứa cấu hình Nginx; screenshots/ lưu sáu ảnh minh chứng từ 01-user.png đến 06-update.png. File .gitignore khai báo các tệp bỏ qua, README.md mô tả cấu hình dự án.

Cấu hình Nginx: listen dùng cổng 8080 cho IPv4 và IPv6; server_name là 221.121.3.208; root trỏ đến /var/www/devops-hackathon-de002-nguyenvana/src; index là index.html. Log truy cập và lỗi lần lượt lưu tại /var/log/nginx/tienvv-hnk24cntt4.access.log và /var/log/nginx/tienvv-hnk24cntt4.error.log. Trong location /, allow all cho phép mọi nguồn truy cập, try_files $uri $uri/ =404 tìm tệp hoặc thư mục và trả lỗi 404 khi không tồn tại. Cấu hình được kích hoạt qua sites-available và sites-enabled, kiểm tra bằng sudo nginx -t rồi tải lại bằng sudo systemctl reload nginx. UFW cho phép cổng 22/tcp và 8080/tcp cho IPv4 và IPv6.


Phần 1 — Chuẩn bị VPS
Từ PowerShell Windows:
ssh root@221.121.3.208
tạo tài khoản:
adduser --shell /bin/bash tienvv-hnk24cntt4
usermod -aG sudo tienvv-hnk24cntt4
su - tienvv-hnk24cntt4
id
whoami

01-user.png: chụp kết quả id, whoami, thấy đúng tài khoản và nhóm sudo.
Cài phần mềm và bật Nginx:
sudo apt update
sudo apt install -y nginx git ufw curl
sudo systemctl enable --now nginx

Cấu hình Git:
git config --global user.name "VuVietTien"
git config --global user.email "tienxinhzai241@gmail.com"

Phần 2 — Mã nguồn và GitHub trên Windows
Mở PowerShell mới:
cd D:\abcd\vvt
code .

Dự án cần có src/index.html, nginx/tienvv-hnk24cntt4.conf, screenshots/, README.md, .gitignore.
Hoàn thiện src/index.html:
- HTML5, lang="vi", UTF-8, viewport; tiêu đề có đề 002 và họ tên.
- H1 đúng đề, điền họ tên, mã sinh viên, lớp.
- Thông tin sinh viên: họ tên, mã, lớp, email, tài khoản Linux, GitHub kèm liên kết.
- Thông tin thi: đề 002, chủ đề Shop, IP, cổng, ngày thi, liên kết repository.
- Giới thiệu Shop 2–3 câu.
.gitignore:
*.log
.DS_Store
Thumbs.db
.vscode/
.idea/

File nginx/tienvv-hnk24cntt4.conf:
server {
    listen 8080;
    listen [::]:8080;
    server_name 221.121.3.208;

    root /var/www/devops-hackathon-de002-nguyenvana/src;
    index index.html;

    access_log /var/log/nginx/tienvv-hnk24cntt4.access.log;
    error_log /var/log/nginx/tienvv-hnk24cntt4.error.log;

    location / {
        allow all;
        try_files $uri $uri/ =404;
    }
}

Khởi tạo Git nếu chưa có, rồi cấu hình:
git init -b main
git config user.name "VuVietTien"
git config user.email "tienxinhzai241@gmail.com"

Tạo ít nhất 4 commit có thay đổi thật, sau khi lưu từng nội dung:
git add .gitignore
git commit -m "Them gitignore cho log va IDE"

git add src/index.html
git commit -m "Tao trang thong tin sinh vien va bai thi"

git add README.md
git commit -m "Them tai lieu thong tin va cau truc du an"

git add nginx/
git commit -m "Them cau hinh Nginx cho website"

Tạo repository Public trên GitHub tên devops-hackathon-de002-nguyenvana. Nếu chưa có remote:
git remote add origin https://github.com/tienvuiet/devops-hackathon-de002-nguyenvana.git
git push -u origin main
git log --oneline

📸 05-git-log.png: chụp lịch sử thấy ít nhất 4 commit.
Phần 3 — Triển khai trên VPS
Đăng nhập bằng tài khoản thường:
ssh tienvv-hnk24cntt4@221.121.3.208

Tạo thư mục và clone chỉ lần đầu:
sudo mkdir -p /var/www/devops-hackathon-de002-nguyenvana
sudo chown tienvv-hnk24cntt4:tienvv-hnk24cntt4 /var/www/devops-hackathon-de002-nguyenvana
git clone https://github.com/tienvuiet/devops-hackathon-de002-nguyenvana.git /var/www/devops-hackathon-de002-nguyenvana
cd /var/www/devops-hackathon-de002-nguyenvana
find . -type d -exec chmod 755 {} \;
find . -type f -exec chmod 644 {} \;

Nếu đã clone, chỉ cần:
cd /var/www/devops-hackathon-de002-nguyenvana
git pull origin main

Cài cấu hình Nginx:
sudo cp nginx/tienvv-hnk24cntt4.conf /etc/nginx/sites-available/tienvv-hnk24cntt4.conf

Tạo liên kết nếu chưa có:
sudo ln -s /etc/nginx/sites-available/tienvv-hnk24cntt4.conf /etc/nginx/sites-enabled/tienvv-hnk24cntt4.conf

Kiểm tra rồi reload khi kiểm tra thành công:
sudo rm -f /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl reload nginx
systemctl is-enabled nginx
systemctl status nginx --no-pager

📸 02-nginx.png: thấy test is successful, enabled, active (running).
Mở UFW đúng thứ tự:
sudo ufw allow 22/tcp
sudo ufw allow 8080/tcp
sudo ufw enable
sudo ufw status verbose

📸 03-ufw.png: thấy active, cổng 22 và 8080 được phép cho IPv4/IPv6.
Trên trang quản lý VPS, thêm quy tắc IN → ACCEPT → TCP → cổng đích 8080, nguồn All, giữ quy tắc SSH 22.
Kiểm tra:
curl -I http://221.121.3.208:8080

Cần trả 200 OK. Mở trình duyệt http://221.121.3.208:8080.
📸 04-website.png: thấy thanh địa chỉ và nội dung đã điền đầy đủ.
Hoàn tất bằng chứng cập nhật và bài nộp
Trên Windows, thêm vào <body> của src/index.html:
<p>Cập nhật lần 2 - [điền ngày giờ thực tế]</p>

Commit và push:
git add src/index.html
git commit -m "Them dong cap nhat lan 2"
git push origin main

Trên VPS:
git pull origin main

Tải lại website bằng Ctrl + F5.
📸 06-update.png: thấy dòng cập nhật và thanh địa chỉ.
Lưu cả 6 ảnh vào D:\abcd\vvt\screenshots. Hoàn thiện README với thông tin sinh viên, URL website/repository, cấu trúc dự án, cách triển khai, giải thích cấu hình Nginx và quy trình cập nhật. Sau đó trên Windows:
git add README.md screenshots/
git commit -m "Hoan thien README va anh minh chung"
git push origin main
git status




ssh tienvv-hnk24cntt4@221.121.3.208

