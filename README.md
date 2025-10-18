# Hệ Thống Quản Lý Hóa Đơn Tiền Điện

## Mô tả
Ứng dụng Java Swing quản lý hóa đơn tiền điện cho khách hàng Việt Nam và nước ngoài. Dự án học tập sử dụng các design patterns: Command, Observer, Singleton.

## Chức năng chính
- ✅ Thêm, sửa, xóa, tìm kiếm hóa đơn
- ✅ Tính tiền điện theo quốc tịch khách hàng
- ✅ Báo cáo thống kê (tổng số lượng, trung bình thành tiền, hóa đơn theo tháng)
- ✅ Giao diện Swing đơn giản

## Cấu trúc dự án
```
src/
├── App.java                    # Main class
├── domain/                     # Business logic
│   ├── model/HoaDon.java      # Entity hóa đơn
│   ├── HoaDonModel.java       # Interface model
│   ├── HoaDonModel_Impl.java  # Implement model
│   ├── ThanhTien.java         # Abstract tính tiền
│   ├── ThanhTienVietNam.java  # Tính tiền VN
│   └── ThanhTienNuocNgoai.java # Tính tiền nước ngoài
├── Persistence/               # Data access
│   ├── ConnectionDB.java      # Kết nối DB (Singleton)
│   ├── HoaDonPersistence.java # Interface persistence
│   └── HoaDonPersistence_Impl.java # Implement persistence
└── Presentation/              # UI layer
    ├── View.java             # Giao diện chính (Observer)
    ├── Command.java          # Abstract command
    ├── CommandProcessor.java # Xử lý command (Singleton)
    ├── AddHoaDon.java        # Command thêm
    ├── UpdateHoaDon.java     # Command sửa
    ├── DeleteHoaDon.java     # Command xóa
    ├── SearchHoaDon.java     # Command tìm kiếm
    ├── ThanhTienVN.java      # Command tính tiền VN
    └── ThanhTienNN.java      # Command tính tiền nước ngoài
```

## Công nghệ
- **Java Swing** - Giao diện
- **MySQL** - Cơ sở dữ liệu  
- **JDBC** - Kết nối DB
- **Design Patterns** - Command, Observer, Singleton

## Cài đặt

### 1. Yêu cầu
- Java 8+
- MySQL Server
- IDE Java (Eclipse/IntelliJ)

### 2. Setup database
```sql
CREATE DATABASE dbqlhoadontiendien;

CREATE TABLE HoaDon (
    maHoaDon VARCHAR(50) PRIMARY KEY,
    maKhachHang VARCHAR(50),
    hoTen VARCHAR(100),
    ngayRaHoaDon DATE,
    quocTich VARCHAR(50),
    doiTuongKhachHang VARCHAR(50),
    soLuong INT,
    donGia DOUBLE,
    dinhMuc INT,
    thanhTien DOUBLE
);
```

### 3. Cấu hình kết nối
Sửa file `src/Persistence/ConnectionDB.java`:
```java
private static final String URL = "jdbc:mysql://localhost:3306/dbqlhoadontiendien";
private static final String USER = "root";
private static final String PASSWORD = "your_password";
```

### 4. Chạy ứng dụng
```bash
# Biên dịch
javac -cp "lib/*" -d bin src/*.java src/domain/*.java src/domain/model/*.java src/Persistence/*.java src/Presentation/*.java

# Chạy
java -cp "bin;lib/*" View
```

## Cách sử dụng

1. **Thêm hóa đơn**: Điền thông tin → Nhấn "Thêm"
2. **Sửa hóa đơn**: Chọn dòng trong bảng → Sửa thông tin → Nhấn "Sửa"  
3. **Xóa hóa đơn**: Chọn dòng → Nhấn "Xóa"
4. **Tìm kiếm**: Nhấn "Tìm kiếm" → Nhập tên khách hàng
5. **Tính tiền**: Nhập số lượng, đơn giá, định mức → Chọn quốc tịch → Nhấn "Tính tiền"
6. **Báo cáo**: Menu "Chức năng khác" → Chọn loại báo cáo

## Công thức tính tiền

**Khách hàng Việt Nam:**
- Nếu số lượng ≤ định mức: `số lượng × đơn giá`
- Nếu số lượng > định mức: `định mức × đơn giá + (số lượng - định mức) × đơn giá × 2.5`

**Khách hàng nước ngoài:**
- `số lượng × đơn giá`

## Design Patterns sử dụng

- **Command Pattern**: Đóng gói các thao tác (thêm, sửa, xóa, tìm kiếm)
- **Observer Pattern**: Tự động cập nhật giao diện khi dữ liệu thay đổi
- **Singleton Pattern**: Quản lý kết nối DB và CommandProcessor

## Lưu ý
- Đây là dự án học tập của sinh viên
- Cần cài đặt MySQL JDBC driver trong thư mục `lib/`
- Kiểm tra kết nối database trước khi chạy
