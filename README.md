<div align="center">

# 🖊️ VanPhongPham Online

**Ứng dụng web bán buôn lẻ văn phòng phẩm đầy đủ chức năng**

Xây dựng bằng ASP.NET Core MVC · Entity Framework Core · SQL Server

![.NET](https://img.shields.io/badge/.NET-8.0-512BD4?logo=dotnet&logoColor=white)
![C#](https://img.shields.io/badge/C%23-12-239120?logo=csharp&logoColor=white)
![ASP.NET Core MVC](https://img.shields.io/badge/ASP.NET%20Core-MVC-5C2D91?logo=dotnet&logoColor=white)
![EF Core](https://img.shields.io/badge/EF%20Core-8.0-6DB33F)
![SQL Server](https://img.shields.io/badge/SQL%20Server-LocalDB-CC2927?logo=microsoftsqlserver&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap-4-7952B3?logo=bootstrap&logoColor=white)
![PayPal](https://img.shields.io/badge/PayPal-Sandbox-00457C?logo=paypal&logoColor=white)


</div>

---

## 📑 Mục lục

- [Giới thiệu](#-giới-thiệu)
- [Tính năng chính](#-tính-năng-chính)
- [Công nghệ sử dụng](#-công-nghệ-sử-dụng)
- [Hướng dẫn cài đặt và chạy dự án](#-hướng-dẫn-cài-đặt-và-chạy-dự-án)
- [Tài khoản thử nghiệm](#-tài-khoản-thử-nghiệm)
- [Cấu trúc dự án](#-cấu-trúc-dự-án)
- [Lưu ý bảo mật](#-lưu-ý-bảo-mật)
- [Crédits](#-crédits)

---

## 🔎 Giới thiệu

**VanPhongPham Online** (tên mã *MultiShop*) là cửa hàng trực tuyến bán văn phòng phẩm và dụng cụ học tập. Ứng dụng gồm 2 phần:

| Phần | Người dùng | Họ có thể làm gì |
|---|---|---|
| 🛒 **Giao diện khách hàng** | Khách hàng | Duyệt và tìm sản phẩm, quản lý giỏ hàng, thanh toán (COD hoặc PayPal), quản lý tài khoản và lịch sử đơn hàng |
| 🛠️ **Phần quản trị** | Nhân viên & Admin | Quản lý sản phẩm, khách hàng, nhân viên, đơn hàng, phòng ban và phân công nhân viên |

---

## ✨ Tính năng chính

### 🛒 Phía khách hàng
- **Xác thực**: đăng ký (có tải ảnh đại diện), đăng nhập và đăng xuất bằng xác thực cookie.
- **Khôi phục mật khẩu**: mã OTP một lần được gửi qua SMTP, hết hạn sau 5 phút. Mật khẩu chỉ có thể được đặt lại sau khi OTP đã được xác thực.
- **Danh mục sản phẩm**: lọc theo phân loại, phân trang, tìm kiếm theo tên, xem chi tiết sản phẩm.
- **Giỏ hàng**: lưu trong phiên làm việc; thêm, cập nhật số lượng và xóa sản phẩm.
- **Thanh toán**:
  - Tiền khi nhận hàng (COD)
  - **PayPal Sandbox**, tự động quy đổi VND → USD và xác thực lấy tiền ở phía máy chủ
- **Tài khoản**: sửa hồ sơ, xem lịch sử đơn hàng có phân trang, xóa tài khoản (xóa mềm).
- **Bản liên hệ**: gửi phản hồi được lưu vào cơ sở dữ liệu.

### 🛠️ Phía quản trị
- **Trang đăng nhập riêng cho nhân viên**, sử dụng scheme cookie riêng nên phiên làm việc nhân viên độc lập với khách hàng.
- **Kiểm soát quyền truy cập dựa trên vai trò (RBAC)**:

  | Chức năng | Admin | Nhân viên |
  |---|:---:|:---:|
  | Quản lý sản phẩm (upload ảnh, phân loại, nhà cung cấp) | ✅ | ❌ |
  | Quản lý khách hàng | ✅ | ❌ |
  | Quản lý nhân viên | ✅ | ❌ |
  | Quản lý phòng ban & phân công | ✅ | ❌ |
  | Xem và xác nhận đơn hàng | ✅ | ✅ |
  | Sửa hồ sơ cá nhân | ✅ | ✅ |

- Tìm kiếm sản phẩm, khách hàng, nhân viên và đơn hàng.
- Thông báo toast cho các hành động CRUD.

---

## 🧰 Công nghệ sử dụng

| Tầng | Công nghệ |
|---|---|
| **Backend** | ASP.NET Core MVC 8, C# |
| **Truy cập dữ liệu** | Entity Framework Core 8 (database-first) |
| **Cơ sở dữ liệu** | Microsoft SQL Server / LocalDB |
| **Xác thực** | Xác thực cookie (hai scheme) với chính sách phân quyền dựa trên vai trò |
| **Thanh toán** | PayPal REST API v2 (Sandbox) |
| **Email** | SMTP qua `System.Net.Mail` (Gmail) |

---

## 🚀 Hướng dẫn cài đặt và chạy dự án

### 1. Điều kiện trước

- [.NET SDK 8.0](https://dotnet.microsoft.com/download/dotnet/8.0)
- [Microsoft SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) hoặc SQL Server LocalDB (Windows)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (nếu bạn dùng macOS và không có LocalDB)

### 2. Chuẩn bị cơ sở dữ liệu

Mở **SQL Server Management Studio (SSMS)** hoặc `sqlcmd`, kết nối đến SQL Server, rồi chạy 2 file sau theo thứ tự:

```sql
-- File đầu tiên: tạo database và các bảng
database/01_create_schema.sql

-- File thứ hai: nhập dữ liệu mẫu + tài khoản admin mặc định
database/02_seed_data.sql
```

Tài khoản admin mặc định sau khi nhập dữ liệu mẫu:
- Email: `admin@gmail.com`
- Mật khẩu: `123456`

> **Lưu ý quan trọng cho macOS:** LocalDB chỉ chạy trên Windows. Nếu bạn dùng macOS, hãy chạy SQL Server qua Docker theo hướng dẫn mục 4 dưới đây.

### 3. Cấu hình file cục bộ (tùy chọn)

Sao chép file ví dụ và điền thông tin của bạn:

```bash
cd VanPhongPhamOnline
cp appsettings.Local.example.json appsettings.Local.json
```

Nội dung `appsettings.Local.json`:

```json
{
  "ConnectionStrings": {
    "MultiShop": "Data Source=localhost,1433;Initial Catalog=MultiShop;User ID=sa;Password=YourStrong!Passw0rd;Trust Server Certificate=True"
  },
  "PaypalOptions": {
    "AppId": "<PAYPAL_SANDBOX_CLIENT_ID>",
    "AppSecret": "<PAYPAL_SANDBOX_SECRET>",
    "Mode": "sandbox",
    "VndPerUsd": 25000
  },
  "EmailSettings": {
    "Mail": "<your-email@gmail.com>",
    "Password": "<GMAIL_APP_PASSWORD>",
    "Host": "smtp.gmail.com",
    "Port": 587,
    "EnableSsl": true
  }
}
```

File `appsettings.Local.json` đã được liệt kê trong `.gitignore`, **không bao giờ được commit lên GitHub**.

### 4. Chạy ứng dụng

#### Trên macOS (khuyên dùng Docker cho SQL Server)

**Bước 1: Khởi động container SQL Server**

```bash
docker run -e "ACCEPT_EULA=Y" \
  -e "SA_PASSWORD=YourStrong!Passw0rd" \
  -p 1433:1433 \
  --name sqlserver \
  -d mcr.microsoft.com/mssql/server:2022-latest
```

**Bước 2: Copy 2 file SQL vào container và import**

```bash
docker cp database/01_create_schema.sql sqlserver:/tmp/01_create_schema.sql
docker cp database/02_seed_data.sql sqlserver:/tmp/02_seed_data.sql

docker exec -it sqlserver /opt/mssql-tools18/bin/sqlcmd \
  -S localhost -U sa -P 'YourStrong!Passw0rd' \
  -i /tmp/01_create_schema.sql -C

docker exec -it sqlserver /opt/mssql-tools18/bin/sqlcmd \
  -S localhost -U sa -P 'YourStrong!Passw0rd' \
  -i /tmp/02_seed_data.sql -C
```

**Bước 3: Cấu hình `appsettings.Local.json`** (xem mục 3 ở trên, sửa chuỗi kết nối thành `localhost,1433` với user `sa`).

**Bước 4: Chạy dự án**

```bash
cd /Users/trannguyengiahuy/Downloads/VanPhongPhamOnline-main
dotnet run --project VanPhongPhamOnline
```

Mở trình duyệt và truy cập: `http://localhost:5000` (hoặc `https://localhost:5001`)

#### Trên Windows

Mở `VanPhongPhamOnline.sln` bằng Visual Studio, nhấn **F5** hoặc chạy qua terminal:

```bash
dotnet run --project VanPhongPhamOnline
```

---

## 👤 Tài khoản thử nghiệm

| Vai trò | Đường dẫn đăng nhập | Email | Mật khẩu |
|---|---|---|---|
| Admin / Nhân viên | `/DangNhap` | `admin@gmail.com` | `123456` |
| Khách hàng | `/KhachHang/DangKy` | Đăng ký mới | Tự đặt |

---

## 📁 Cấu trúc dự án

```
VanPhongPhamOnline-main/
├── database/
│   ├── 01_create_schema.sql   # Tạo database và các bảng
│   └── 02_seed_data.sql       # Dữ liệu mẫu + tài khoản admin
└── VanPhongPhamOnline/
    ├── Controllers/             # Các controller (KhachHang, Cart, Admin/...)
    ├── Data/                    # Entity + DbContext (MultiShopContext)
    ├── ViewModels/              # Model dành cho view
    ├── Views/                   # Razor views
    ├── wwwroot/                 # Tệp tĩnh: hình ảnh, CSS, JS
    ├── appsettings.json         # Cấu hình chung (không chứa secret)
    ├── appsettings.Local.example.json
    ├── Program.cs               # Đăng ký DI, Auth, DB
    └── VanPhongPhamOnline.csproj
```

---

## 🔐 Lưu ý bảo mật

- Mật khẩu sử dụng **salted MD5** để tương thích với dữ liệu cũ (đây là hạn chế đã biết, sẽ cải tiến sau).
- Không commit `appsettings.Local.json` (chứa mật khẩu, khóa API).
- Tất cả form POST có bảo vệ CSRF (`ValidateAntiForgeryToken`).
- Chỉ cho phép upload ảnh các phần mở rộng: `.jpg`, `.jpeg`, `.png`, `.gif`, `.webp`.
- Thông tin nhạy cảm (mật khẩu, khóa API) không được lưu trong kho mã nguồn.

---

## 🙏 Crédits

- Giao diện UI: [MultiShop](https://htmlcodex.com/online-shop-website-template) bởi **HTML Codex** (CC BY 4.0)
- Được phát triển làm dự án nhóm cho môn học Công nghệ Phần mềm

Nếu để trống `PaypalOptions` và `EmailSettings`, ứng dụng vẫn chạy bình thường, chỉ có tính năng thanh toán PayPal và gửi email OTP bị tắt.
