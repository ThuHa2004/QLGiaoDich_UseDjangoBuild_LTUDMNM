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

## 1. Bảng Khách hàng

Bảng này dùng để lưu thông tin khách hàng cầm 

### Các trường dữ liệu

| Tên trường | Kiểu dữ liệu | Mô tả |
|---|---|---|
| id | bigint(20) | Khóa chính của khách hàng |
| hoten | varchar(100) | Họ và tên khách hàng |
| sdt | varchar(15) | Số điện thoại |
| cccd | varchar(20) | Số CCCD/CMND |
| diachi | varchar(255) | Địa chỉ khách hàng |

---

## 2. Bảng Loại tài sản 

Bảng này dùng để lưu các loại tài sản được cầm .

| Tên trường | Kiểu dữ liệu | Mô tả |
|---|---|---|
| id | bigint(20) | Khóa chính của loại tài sản |
| ten_loai | varchar(100) | Tên loại tài sản |

---

## 3. Bảng Gói lãi suất 

Bảng này dùng để lưu thông tin lãi suất áp dụng cho hợp đồng cầm .

### Các trường dữ liệu

| Tên trường | Kiểu dữ liệu | Mô tả |
|---|---|---|
| id | bigint(20) | Khóa chính của gói lãi suất |
| ten_goi | varchar(100) | Tên gói lãi suất |
| phan_tram | double | Phần trăm lãi suất |
| chu_ky | varchar(10) | Chu kỳ tính lãi |

---

## 4. Bảng Hợp đồng

Đây là bảng quan trọng nhất của hệ thống. Bảng này dùng để lưu thông tin hợp đồng cầm đồ giữa khách hàng và tiệm cầm đồ.

Bảng này liên kết:

- Khách hàng
- Loại tài sản
- Gói lãi suất

thông qua khóa ngoại.

| Tên trường | Kiểu dữ liệu | Mô tả |
|---|---|---|
| id | bigint(20) | Khóa chính của hợp đồng |
| ten_tai_san_chi_tiet | varchar(255) | Tên chi tiết tài sản cầm cố |
| so_tien_cam | decimal(12,0) | Số tiền cầm |
| ngay_cam | date | Ngày cầm tài sản |
| han_tra | date | Hạn trả tiền |
| trang_thai | varchar(20) | Trạng thái hợp đồng |
| goi_lai_suat_id | bigint(20) | Khóa ngoại tới bảng Goi_lai_suat |
| nhan_vien_id | int(11) | ID nhân viên xử lý hợp đồng |
| khach_hang_id | bigint(20) | Khóa ngoại tới bảng Khach_hang |
| loai_tai_san_id | bigint(20) | Khóa ngoại tới bảng Loai_tai_san |

---

## 5.Bảng Lịch sử thanh toán

Bảng này dùng để lưu lịch sử thanh toán của từng hợp đồng.

### Các trường dữ liệu

| Tên trường | Kiểu dữ liệu | Mô tả |
|---|---|---|
| id | bigint(20) | Khóa chính của lịch sử thanh toán |
| ngay_dong | datetime(6) | Ngày giờ thanh toán |
| so_tien | decimal(12,0) | Số tiền thanh toán |
| loai_phi | varchar(20) | Loại phí thanh toán |
| ghi_chu | longtext | Ghi chú |
| hop_dong_id | bigint(20) | Khóa ngoại tới bảng Hop_dong |

---

## Quan hệ giữa các bảng

- Một khách hàng có thể có nhiều hợp đồng
- Một loại tài sản có thể thuộc nhiều hợp đồng
- Một gói lãi suất có thể áp dụng cho nhiều hợp đồng
- Một hợp đồng có thể có nhiều lần thanh toán
  
---

# SƠ ĐỒ QUAN HỆ

```text
Khach_hang
      |
      | 1 - n
      |
Hop_dong
  |      \
  |       \
  |        \
  |         \
  n          n
Loai_tai_san  Goi_lai_suat

Hop_dong
    |
    | 1 - n
    |
Lich_su_thanh_toan
```

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

---

# 🚀 Hướng dẫn cài đặt và triển khai
Môi trường sử dụng để xây dựng hệ thống này là Ubuntu Server. Ở bài tập trước môi trường này đã được cài đặt và cấu hình docker compose và docker. Kết nối được SSH vào Server. Trong bài này chỉ cần khởi tạo thư mục dự án và cấu hình xây dựng hệ thống web site tiệm cầm đồ.

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
Django
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
hoặc
sudo docker compose run --rm web django-admin startproject core .
```
Sau khi chạy lệnh này, hệ thống sẽ tự động sinh ra các file trong project core:
- settings.py
- urls.py
- asgi.py
- wsgi.py

<img width="1752" height="892" alt="image" src="https://github.com/user-attachments/assets/eafef200-870b-4d1d-a139-b1a85e91de9f" />


### 🗄 Cấu hình Database

Mở file:

```bash
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

### Tạo App `camdo`
```
sudo docker compose exec web python manage.py startapp camdo
```
Sau khi chạy lệnh trên, mở file core/settings.py thêm `'camdo'` vào cuối danh sách INSTALLED_APPS
`Sudo nano core/settings.py`

<img width="682" height="211" alt="image" src="https://github.com/user-attachments/assets/a8e46462-9a69-492e-989b-46c2152350e3" />

---

# 🗄 Cấu hình Database
Django mặc định dùng SQLite vì vậy trong bài này cần đổi sang MariaDB. Mở file `core/settings.py` tìm biến `DATABASES` và thay thế bằng:

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
# 🔄 Migration Database
Migration Database giúp tạo bảng tự động và đồng bộ model với database

## 8️⃣ Tạo migration
Chạy hai lệnh sau để tạo file migration từ models.py và Apply migration vào MariaDB (Django tự tạo bảng)
```bash
sudo docker compose exec web python manage.py makemigrations camdo

sudo docker compose exec web python manage.py migrate
```
<img width="1060" height="214" alt="image" src="https://github.com/user-attachments/assets/6c1fd405-2dd7-4a75-bb6d-85c822c84ea1" />

### Vào phpMyAdmin kiểm tra
PhpMyadmin có vai trò xem bảng dữ liệu, kiểm tra khóa ngoại, kiểm tra dữ liệu thật trong database. Mở PhpAdmin tại cổng `http://192.168.100.2:8089` đăng nhập bằng tài khoản đã tạo vào database thấy các bảng đã được tạo:
<img width="1919" height="905" alt="image" src="https://github.com/user-attachments/assets/eaf01282-01cd-48cd-8695-ee09828638b1" />

---
# 👤 Tạo tài khoản quản trị

## 🔟 Tạo tài khoản Superuser có quyền thêm, sửa, xóa

```bash
sudo docker compose exec web python manage.py createsuperuser
```
<img width="1224" height="195" alt="image" src="https://github.com/user-attachments/assets/0e05cd96-441e-4ee2-b8c8-56488b9f8959" />


Sau khi chạy lệnh này nhập: Username, Email (có thể để trống), Password để sau đó đăng nhập vào hệ thống.

----
# Truy cập vào trang quản trị Django 
`http://192.168.100.2:8005/admin` đăng nhập bằng tài khoản Superuser vừa tạo.
<img width="1919" height="991" alt="image" src="https://github.com/user-attachments/assets/eb9b27be-735a-4bc7-a628-b92d8e6cbf9e" />

<img width="1910" height="694" alt="image" src="https://github.com/user-attachments/assets/cbfbfd3b-763d-4407-a155-04345f729e83" />

## Nhập dữ liệu vào csdl trên Django
### Cách tạo dữ liệu trên trang Django Admin
Trong Django Admin, việc nhập liệu cần tuân thủ thứ tự ưu tiên do các bảng có mối quan hệ ràng buộc khóa ngoại (Foreign Key - FK).
Thứ tự nhập liệu chuẩn:
Nhân viên, Khách hàng & Tài sản: Nhập trước vì đây là các bảng độc lập (không chứa FK).
Hợp đồng: Nhập sau (vì cần tham chiếu đến Khách hàng, Nhân viên và Tài sản).
Thanh toán: Nhập cuối cùng (vì cần tham chiếu đến ID của Hợp đồng).

#### Thêm dữ liệu vào bảng khách hàng
- Truy cập vào mục Khách hàng -> Thêm vào
- Nhập thông tin cá nhân -> Save, sau đó Django sẽ tự động cấp ID duy nhất cho khách hàng 
<img width="1913" height="783" alt="image" src="https://github.com/user-attachments/assets/319e4ede-fe09-4e59-8ec2-fc2a08611f4d" />

<img width="1919" height="805" alt="image" src="https://github.com/user-attachments/assets/f42f6b1f-2414-4aa0-a2d7-286e603f0c8e" />

#### Thêm dữ liệu vào bảng tài sản:
- Truy cập mục Tài sản -> Thêm vào
- Nhập tên tài sản -> Lưu
<img width="1616" height="627" alt="image" src="https://github.com/user-attachments/assets/2fe322f3-99e4-4f1a-beab-f51fce6d2913" />

#### Thêm dữ liệu vào bảng Gói lãi suất
- Truy cập mục Gói lãi suất -> Thêm vào
- Nhập thông tin gói lãi suất -> Lưu
<img width="1497" height="668" alt="image" src="https://github.com/user-attachments/assets/0abbcac6-3424-4302-a400-5b4e1700b4e4" />

#### Thêm dữ liệu vào bảng Hợp đồng
- Truy cập mục Hợp đồng -> Thêm vào
- Nhập thông tin hợp  -> Lưu
<img width="1919" height="912" alt="image" src="https://github.com/user-attachments/assets/908d2235-1a92-4d09-90d6-eb452972e80a" />


## Kiểm tra dữ liệu đã nhập
Sau khi nhập dữ liệu trên giao diện Admin của Django, truy cập vào `192.168.100.2:8089` để kiểm tra dữ liệu vừa nhập
<img width="1577" height="811" alt="image" src="https://github.com/user-attachments/assets/a80dbd52-e063-4270-81ce-3e3b5a23940b" />



---

# 🌐 Truy cập hệ thống

## Truy cập vào trang chủ liệt kê các con nợ
`http://192.168.100.2:8005/` Trang này xem được mọi thông tin của các con nợ, trạng thái,...
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/4256efa7-66aa-4072-8795-1d7a7df77634" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9ed24ada-6170-4bb7-8526-3d4a8021d09a" />

---

# THÊM SERVICE CLOUDEFLARE ĐỂ PUBLIC WEBSITE
Mở file: 
```
docker-compose.yml
```

Thêm đoạn sau vào cuối file: 
```
  cloudflared:
    image: cloudflare/cloudflared:latest
    container_name: camdo_cloudflared
    command: tunnel --no-autoupdate run --token ${CLOUDFLARE_TOKEN} # điền token cloudflare của mình
    restart: unless-stopped
    depends_on:
      - django
    networks:
      - camdo_net
```

## Lấy Tunnel Token trên Cloudflare
### Đăng nhập vào Claudflare
Chọn `Zero Truse -> Networks -> Tunnels`

### Tạo tunnels
Chọn 
```
Create a Tunnel
```

Đặt tên 
```
camdo-django
```

### Chọn Docker
Cloudeflare sẽ hiển thị lệnh:
```
docker run cloudflare/cloudflared:latest tunnel --no-autoupdate run --token xxxxx
```
Coppy phần token xxxxx, đây chính là Token của claudflare
<img width="1910" height="945" alt="image" src="https://github.com/user-attachments/assets/94511154-7a86-4083-ae70-79952ffd9f9a" />

### Build và chạy lại toàn bộ hệ thống
```
docker compose up -d --build
```

---

## Tạo sub-domain public
Trên Cloudflare sau khi coppy token chọn continue -> Đặt tên Subdomain -> Service URL `http://camdo:`http://web:8000` -> Save

# KẾT QUẢ DEMO
<img width="1914" height="1028" alt="image" src="https://github.com/user-attachments/assets/01c40dd5-4c6f-4339-a523-0672bc29d2e1" />
<img width="1910" height="970" alt="image" src="https://github.com/user-attachments/assets/9cbd31ed-a861-4043-9d0a-6fee21208927" />



# ✅ Tính năng hiện có

- Quản lý khách hàng
- Quản lý tài sản cầm cố
- Quản lý hợp đồng cầm đồ
- Quản lý gói lãi suất
- Theo dõi lịch sử thanh toán
- Tìm kiếm dữ liệu nhanh
- Giao diện quản trị bằng Django Admin

---

# 📌 Mở rộng trong tương lai

Dự án có thể mở rộng thêm:

- REST API với Django REST Framework
- JWT Authentication
- Frontend React/Vue
- Dashboard thống kê doanh thu
- In hợp đồng PDF
- Quản lý nhân viên
- Hệ thống phân quyền
