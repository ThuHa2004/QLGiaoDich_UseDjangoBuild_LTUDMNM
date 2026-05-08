# BÀI TẬP MÔN: PHÁT TRIỂN ỨNG DỤNG VỚI MÃ NGUỒN MỞ 
--------
# Họ tên: Trần Thị Thu Hà
# MSSV: K225480106009
# Lớp: K58KTP.01
# 
------------
# YÊU CẦU: 
## deadline : 23h59 ngày 09 tháng 5 năm 2026
### 
-----------
# BÀI LÀM 
## 1. Tổ chức csdl cho hệ thống quản lý tiệm cầm đồ
## 2. 
Tạo thư mục ql_camdo_django: 
<img width="926" height="332" alt="image" src="https://github.com/user-attachments/assets/70a06c8d-036d-4eb7-ae9c-db43d52e6e02" />

Tạo file requirements.txt, lệnh: `sudo nano requirements.txt`
Gán nội dung sau vào file và lưu lại:
```
Django
mysqlclient
```
<img width="704" height="168" alt="image" src="https://github.com/user-attachments/assets/21194117-5470-4907-b822-8c0542ff72ae" />

Tạo file Dockerfile và paste đoạn mã sau vào file: 
```
# Sử dụng image Python phiên bản nhẹ
FROM python:3.10-slim

# Đặt thư mục làm việc bên trong container là /app
WORKDIR /app

# Cài đặt các gói thư viện hệ thống cần thiết của Ubuntu để có thể build thư viện mysqlclient
RUN apt-get update && apt-get install -y \
    gcc \
    pkg-config \
    default-libmysqlclient-dev \
    && rm -rf /var/lib/apt/lists/*

# Copy file requirements.txt từ máy thật vào container
COPY requirements.txt /app/

# Cài đặt các thư viện Python
RUN pip install --no-cache-dir -r requirements.txt

# Mặc định, project Django sẽ chạy ở cổng 8000
EXPOSE 8000
```
<img width="1031" height="576" alt="image" src="https://github.com/user-attachments/assets/ce06fb6c-93b4-4402-b0d7-9fa25568d3b4" />

Tạo file docker-compose.yml

<img width="665" height="92" alt="image" src="https://github.com/user-attachments/assets/805e7df0-94fd-4a6a-a6ac-d70dd966189e" />

Khởi tạo project django
Lệnh: `sudo docker compose run --rm web django-admin startproject core .` Sau khi chạy lệnh này sẽ tạo ra file manage.py và thư mục core
<img width="1752" height="892" alt="image" src="https://github.com/user-attachments/assets/091e3a73-d1fd-4efa-9a20-ad9b0b69b531" />

<img width="692" height="69" alt="image" src="https://github.com/user-attachments/assets/8f26711d-c2ed-4bb4-8218-4f428fc3119d" />

Cấu hình Django và kết nối với Mariadb `sudo nano core/settings.py`
Cấu hình file setting đoạn database như sau: 
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'camdo_db',
        'USER': 'user_camdo',
        'PASSWORD': 'password123',
        'HOST': 'db',   # Tên service db trong docker-compose.yml
        'PORT': '3306',
    }
}
```

Khởi động hệ thống 

