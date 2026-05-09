# BÀI TẬP MÔN: PHÁT TRIỂN ỨNG DỤNG VỚI MÃ NGUỒN MỞ 
--------
# Họ tên: Trần Thị Thu Hà
# MSSV: K225480106009
# Lớp: K58KTP.01
# Deadline : 23h59 ngày 09 tháng 5 năm 2026
----------
# 🏦 Hệ Thống Quản Lý Tiệm Cầm Đồ (Django + Docker)

## 📌 Giới thiệu

Dự án **Hệ Thống Quản Lý Tiệm Cầm Đồ** được xây dựng bằng **Python/Django** nhằm hỗ trợ quản lý hoạt động tiệm cầm đồ một cách hiệu quả, trực quan và dễ mở rộng.

Hệ thống sử dụng:

- **Django** cho Backend và trang quản trị
- **MariaDB** làm cơ sở dữ liệu
- **Docker + Docker Compose** để đóng gói và triển khai môi trường

Mục tiêu của dự án là giúp việc cài đặt và vận hành trở nên nhanh chóng, đồng nhất trên nhiều môi trường khác nhau, đặc biệt là Ubuntu/Linux Server.

---

# 🛠 Công nghệ sử dụng

| Thành phần | Công nghệ |
|---|---|
| Ngôn ngữ lập trình | Python 3.10 |
| Framework Backend | Django 4.2 |
| Cơ sở dữ liệu | MariaDB 10.11 |
| Quản trị Database | phpMyAdmin |
| Containerization | Docker |
| Điều phối Container | Docker Compose |

---

# PHÂN TÍCH HỆ THỐNG
## 1. Các chức năng chính

Hệ thống gồm các chức năng:

- Quản lý khách hàng
- Quản lý loại tài sản
- Quản lý gói lãi suất
- Quản lý hợp đồng cầm đồ
- Quản lý lịch sử thanh toán
- Hiển thị danh sách khách hàng quá hạn

---

## 2. Kiến trúc hệ thống

Hệ thống hoạt động theo mô hình:

```text
Người dùng
    |
    v
Django Web Application
    |
    v
MariaDB Database
```

Trong đó:

- Django xử lý logic và giao diện
- MariaDB lưu dữ liệu
- phpMyAdmin dùng để kiểm tra dữ liệu
- Docker dùng để quản lý container

---

# THIẾT KẾ CƠ SỞ DỮ LIỆU

## Phân tích cơ sở dữ liệu

Sau khi phân tích nghiệp vụ, hệ thống gồm 5 bảng:

1. KhachHang
2. LoaiTaiSan
3. GoiLaiSuat
4. HopDong
5. LichSuThanhToan

<img width="960" height="1280" alt="image" src="https://github.com/user-attachments/assets/820851f9-f9df-44b5-9737-6066ac0c9d1e" />

---

## 1. Bảng KhachHang

Bảng này dùng để lưu thông tin khách hàng.

### Các trường dữ liệu

- Mã khách hàng
- Họ tên
- Số điện thoại
- CCCD
- Địa chỉ

---

## 2. Bảng LoaiTaiSan

Bảng này dùng để lưu các loại tài sản.

Ví dụ:

- Điện thoại
- Laptop
- Xe máy
- Đồng hồ

### Các trường dữ liệu

- Mã loại tài sản
- Tên loại tài sản

---

## 3. Bảng GoiLaiSuat

Bảng này dùng để lưu thông tin lãi suất.

### Các trường dữ liệu

- Mã gói lãi suất
- Tên gói
- Phần trăm lãi
- Chu kỳ

---

## 4. Bảng HopDong

Đây là bảng quan trọng nhất của hệ thống.

Bảng này liên kết:

- Khách hàng
- Loại tài sản
- Gói lãi suất

thông qua khóa ngoại.

### Các trường dữ liệu

- Mã hợp đồng
- Khách hàng
- Loại tài sản
- Gói lãi suất
- Chi tiết tài sản
- Số tiền cầm
- Ngày cầm
- Hạn trả
- Trạng thái
- Nhân viên

---

## 5.Bảng LichSuThanhToan

Bảng này dùng để lưu lịch sử thanh toán của từng hợp đồng.

### Các trường dữ liệu

- Mã thanh toán
- Hợp đồng
- Ngày đóng
- Số tiền
- Loại phí
- Ghi chú

---

# 📂 Cấu trúc thư mục dự án

```
ql_camdo_django/
├── camdo/                      # App xử lý nghiệp vụ cầm đồ
│   ├── migrations/             # Lưu vết thay đổi database
│   ├── templates/              # Giao diện HTML (Jinja2)
│   ├── __init__.py
│   ├── admin.py                # Quản lý giao diện Admin Django
│   ├── apps.py
│   ├── models.py               # Định nghĩa bảng CSDL (Khách hàng, Hợp đồng)
│   ├── urls.py                 # Điều hướng nội bộ của app camdo
│   └── views.py                # Xử lý logic hiển thị dữ liệu
├── core/                       # Thư mục cấu hình dự án (Project Root)
│   ├── __init__.py
│   ├── asgi.py                 # Cấu hình cho máy chủ web bất đồng bộ (Asynchronous)
│   ├── settings.py             # Cấu hình quan trọng nhất (Database, App, Static...)
│   ├── urls.py                 # Điều hướng chính của toàn bộ website
│   └── wsgi.py                 # Cấu hình cho máy chủ web đồng bộ (Synchronous)
├── docker-compose.yml          # Quản lý 3 container: MariaDB, PhpMyAdmin, Django
├── Dockerfile                  # Chỉ dẫn xây dựng môi trường cho Django container
├── manage.py                   # File thực thi các lệnh Django (migrate, runserver...)
├── README.md                   # Hướng dẫn dự án
└── requirements.txt            # Danh sách thư viện Python cần thiết
```
----

# 🚀 Hướng dẫn cài đặt và triển khai

## 1️⃣ Khởi tạo thư mục dự án
Sử dụng hai lệnh dưới đây để tạo và truy cập vào thư mục dự án: 
```
mkdir ql_camdo_django
cd ql_camdo_django
```

<img width="926" height="332" alt="image" src="https://github.com/user-attachments/assets/70a06c8d-036d-4eb7-ae9c-db43d52e6e02" />

---

# ⚙️ Cấu hình Docker

## 2️⃣ Tạo file `requirements.txt`
Lệnh tạo `sudo nano requirements.txt` sau đó gán nội dung sau vào file:
```txt
Django>=4.2,<5.0
mysqlclient
```

<img width="704" height="168" alt="image" src="https://github.com/user-attachments/assets/21194117-5470-4907-b822-8c0542ff72ae" />

---

## 3️⃣ Tạo file `Dockerfile` (dùng để tạo image cho Django)
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

---

## 4️⃣ Tạo file `docker-compose.yml`
File docker-compose.yml dùng để khai báo tàon bộ hệ thống, gồm có các service:
1. Container MariaDB dùng để:
- Lưu dữ liệu
- Tạo bảng
- Quản lý database
Ngoài ra dữ liệu được mount ra ngoài để tránh mất dữ liệu khi container bị xóa.

2. Service phpMyAdmin dùng để:
- Xem database
- Kiểm tra dữ liệu
- Kiểm tra khóa ngoại
- Kiểm tra dữ liệu thật được lưu trong database

3. Service Django (trong file này em viết là web) dùng để:
- Chạy web application
- Xử lý logic hệ thống
- Kết nối database
- Render giao diện HTML


```
services:
  # 1. Container chứa CSDL MariaDB
  db:
    image: mariadb:10.11
    container_name: camdo_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: camdo_db
      MYSQL_USER: admin_camdo
      MYSQL_PASSWORD: password123
    volumes:
      - db_data:/var/lib/mysql
    ports:
      - "3308:3306"

  # 2. Container chứa phpMyAdmin để soi CSDL
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: camdo_pma
    restart: always
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: rootpassword
    ports:
      - "8089:80"
    depends_on:
      - db

  # 3. Container chứa mã nguồn Django
  web:
    build: .
    container_name: camdo_web
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/app  # Cực kỳ quan trọng: Mount thư mục hiện tại vào /app để dễ dàng dùng sudo nano edit code
    ports:
      - "8005:8000"
    environment:
      - DB_HOST=db
      - DB_NAME=camdo_db
      - DB_USER=admin_camdo
      - DB_PASS=password123
    depends_on:
      - db

  # 4 Claudflảre Tunnel để tạo đường hầm bảo mật từ Internet vào container web
  cloudflare_tunnel:
    image: cloudflare/cloudflared:latest
    container_name: camdo_tunnel
    restart: always
    command: tunnel run
    environment:
      - TUNNEL_TOKEN=eyJhIjoiNzZhODhlM2E5MWJkYWZmYzEzOTdmZWZkNDI1Y2IwN2IiLCJ0IjoiYTE0MDFjYTQtYWVhZS00NDdhLWJkN2YtNzliOTQ1YzQ1YzM3IiwicyI6Ik1EUm1NakV4TkRZdE5qUTBPQzAwWlRrMExXSTVNRFl0TkdGbU1tTTJNMkkwWldRMSJ9
    depends_on:
      - web

volumes:
  db_data: # Tạo volume lưu trữ dữ liệu DB để khi tắt container không bị mất dữ liệu
```

<img width="665" height="92" alt="image" src="https://github.com/user-attachments/assets/805e7df0-94fd-4a6a-a6ac-d70dd966189e" />

----
# ▶️ Khởi động hệ thống
## 5️⃣ Chạy Docker Compose
Sau khi cấu hình xong cần build hệ thống.
Chạy lệnh: `sudo nano docker compose up -d`
Quá trình build sẽ:
- Download image
- Cài thư viện
- Tạo môi trường chạy Django
<img width="637" height="167" alt="image" src="https://github.com/user-attachments/assets/c16cc27b-ebde-4878-a72c-be040c5e3397" />


## Kiểm tra container
Sau khi chạy, cần kiểm tra lại các container có hoạt động hay bị lỗi không
```
sudo docker ps
```
<img width="1857" height="263" alt="image" src="https://github.com/user-attachments/assets/7ce8d4b8-6737-4380-917b-6f399484d37a" />

-----

# 🧱 Khởi tạo Django Project
## 6️⃣ Tạo Django Project
Sau khi vào container Django cần tạo Django Project. Lệnh
```
sudo docker compose exec web django-admin startproject core .
```
Project sẽ chứa:
- settings.py
- urls.py
- cấu hình hệ thống

### 🗄 Cấu hình Database

Mở file:

```
core/settings.py
```

Tìm biến `DATABASES` và thay thế bằng:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'camdo_db',
        'USER': 'admin_camdo',
        'PASSWORD': 'password123',
        'HOST': 'db',
        'PORT': '3306',
    }
}
```

### Tạo App nghiệp vụ
#### Tạo App `camdo`
```
sudo docker compose exec web python manage.py startapp camdo
```
Sau khi chạy lệnh trên, mở file core/settings.py thêm `'camdo'` vào cuối danh sách INSTALLED_APPS
`Sudo nano core/settings.py`

<img width="682" height="211" alt="image" src="https://github.com/user-attachments/assets/a8e46462-9a69-492e-989b-46c2152350e3" />


---

# 🗄 Cấu hình Database

Mở file:

```text
core/settings.py
```

Tìm biến `DATABASES` và thay thế bằng:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'camdo_db',
        'USER': 'user_camdo',
        'PASSWORD': 'password123',
        'HOST': 'db',
        'PORT': '3306',
    }
}
```

---



-----------
# BÀI LÀM 
## Cấu trúc thư mục: 

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

Khởi động hệ thống Chạy `sudo docker compose up -d`
<img width="622" height="124" alt="image" src="https://github.com/user-attachments/assets/8d97ecfb-400c-4046-b880-f34328c79a76" />

Triển khai csdl bằng django
mở file camdo/models.py 
Tạo db trong mariadb bằng lệnh migrate:
`sudo docker compose exec web python manage.py migrate`

<img width="1060" height="214" alt="image" src="https://github.com/user-attachments/assets/385fc415-a0fe-423b-aeab-1aab7ffa1986" />

<img width="1102" height="330" alt="image" src="https://github.com/user-attachments/assets/a3dd7a85-907a-4570-8362-8ee11a64a51f" />
Tạo tài khoản admin django:
`sudo docker compose exec web python manage.py createsuperuser`
<img width="1042" height="207" alt="image" src="https://github.com/user-attachments/assets/528d5088-5afd-44ca-ae2b-a3ef708d70c9" />


<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/719cfc1c-accf-4c76-967e-3ffb7e976b8c" />

<img width="1915" height="1078" alt="image" src="https://github.com/user-attachments/assets/2217bc9d-2688-4f2c-8b77-28be2d7d8d17" />

<img width="1917" height="1061" alt="image" src="https://github.com/user-attachments/assets/a171d6c3-df91-4222-8ae9-10abefb9b914" />

Thêm dữ liệu vào bảng trên Django: 
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/ea4f587b-3a22-4981-9a4e-ff7c43132817" />


Sau khi thêm kiểm tra trên giao diện web phpMyadmin: 
<img width="1029" height="209" alt="image" src="https://github.com/user-attachments/assets/291d1707-6bba-43a7-9add-6c7c94c57a21" />

Giao diện web: 
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/db5f4514-6843-4d21-b1e8-9ba2087c9f28" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3634d1f7-b70b-4ab7-b060-a13d6d78903a" />

Truy cập bằng tên miền:
<img width="1919" height="1077" alt="image" src="https://github.com/user-attachments/assets/80d31787-7d73-45ab-afdb-cc8a7516c5ae" />

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/abb15737-0d0b-4780-8c1a-4715e012d3ae" />


