# 🎓 Hệ Thống Quản Lý Sinh Viên  
**University Student Management System**

> Dự án xây dựng **cơ sở dữ liệu PostgreSQL** nhằm giải quyết các bài toán phức tạp trong môi trường đại học:  
> quản lý thông tin, tự động hóa đăng ký học phần, kiểm soát ràng buộc toàn vẹn và báo cáo thống kê.

---

## 📋 Mục lục
- [📖 Giới thiệu](#-giới-thiệu)
- [🗂 Cơ sở dữ liệu & Cấu trúc](#-cơ-sở-dữ-liệu--cấu-trúc)
- [⚙️ Tính năng & Quy tắc nghiệp vụ](#️-tính-năng--quy-tắc-nghiệp-vụ)
- [💻 Kỹ thuật áp dụng](#-kỹ-thuật-áp-dụng)
- [🚀 Hướng phát triển](#-hướng-phát-triển)

---

## 📖 Giới thiệu
Việc quản lý thủ công hoặc sử dụng Excel thường dẫn đến:
- ❌ Sai sót dữ liệu (trùng mã sinh viên)
- ❌ Khó kiểm tra ràng buộc (điều kiện tiên quyết)
- ❌ Tốn thời gian tổng hợp và thống kê

### 🎯 Mục tiêu hệ thống
- **Tự động hóa**: Đăng ký học phần, nhập điểm, kiểm tra điều kiện.
- **Toàn vẹn dữ liệu**: Sử dụng khóa ngoại, Trigger và Constraints.
- **Hỗ trợ đa vai trò**: Quản lý – Giảng viên – Sinh viên.

---

## 🗂 Cơ sở dữ liệu & Cấu trúc
Hệ thống gồm **11 bảng thực thể chính**, thiết kế theo chuẩn hóa dữ liệu.

### 1️⃣ Nhóm quản lý thông tin cơ bản
- **SinhVien** – Hồ sơ sinh viên  
- **GiangVien** – Hồ sơ giảng viên  
- **Khoa** – Danh sách khoa  
- **Lop** – Lớp hành chính (VD: K65CNTT)  
- **PhongHoc** – Thông tin cơ sở vật chất  

### 2️⃣ Nhóm đào tạo & học phần
- **HocPhan** – Danh mục môn học  
- **LopHocPhan** – Lớp tín chỉ theo học kỳ  
- **DieuKienTienQuyet** – Điều kiện môn học tiên quyết  
- **PhanCongGiangDay** – Phân công giảng viên  

### 3️⃣ Nhóm đăng ký & kết quả
- **DangKyNguyenVongHocPhan** – Nguyện vọng đăng ký  
- **KetQuaHocTap** – Điểm số & trạng thái học tập  

---

## ⚙️ Tính năng & Quy tắc nghiệp vụ

### 🔐 1. Đăng ký học phần
- ✅ **Điều kiện tiên quyết**:  
  Sinh viên chỉ được đăng ký nếu đã hoàn thành môn tiên quyết với  
  **điểm tổng kết ≥ 4.0**
- 📊 **Giới hạn tín chỉ**:  
  Tối đa **25 tín chỉ / học kỳ**
- 📅 **Kiểm tra lịch & sức chứa**:  
  Tự động chặn trùng lịch hoặc phòng học quá tải

### 👨‍🏫 2. Quản lý giảng dạy
- Ràng buộc khoa:  
  > Giảng viên và Học phần **phải thuộc cùng một Khoa**  
  → Tránh phân công sai chuyên môn

### 📝 3. Quản lý điểm
- Tự động tính điểm tổng kết
- Cập nhật trạng thái:
  - **Hoàn thành**
  - **Trượt**

---

## 💻 Kỹ thuật áp dụng
Dự án tận dụng các **tính năng nâng cao của PostgreSQL**.

### ⚡ Triggers (Tự động hóa)
- **trg_check_prerequisites**  
  - Kích hoạt: `BEFORE INSERT` vào `DangKyNguyenVongHocPhan`
  - Chức năng:  
    Kiểm tra `DieuKienTienQuyet`  
    → Nếu chưa đạt (`< 4.0`) → `RAISE EXCEPTION`

- **trg_check_instructor_department**  
  - Kiểm tra khoa của Giảng viên và Học phần  
  - Sai → báo lỗi

- **Các trigger khác**
  - Kiểm tra trùng lịch học
  - Kiểm tra sức chứa phòng
  - `trg_delete_ket_qua_khi_xoa_sinh_vien`:  
    Dọn dữ liệu rác khi xóa sinh viên

---

### 📊 Views & Materialized Views
- **vw_ket_qua_hoc_tap_sinh_vien**  
  → Tổng hợp bảng điểm và trạng thái học tập

- **mv_thong_ke_sv_dang_ky_hoc_phan**  
  → Thống kê số lượng sinh viên đăng ký theo học phần  
  *(Materialized View giúp tăng hiệu suất)*

---

### 🛠 Stored Procedures & Optimization
- **Stored Procedures**
  - `sp_dang_ky_hoc_phan`
  - `sp_nhap_diem`

- **Indexing**
  - `ma_sinh_vien`
  - `ma_hoc_phan`
  - `email`

---

## 🚀 Hướng phát triển
- [ ] Tích hợp giao diện **Web/Mobile**
- [ ] Thông báo tự động **Email / SMS**
- [ ] Mở rộng module **Học phí & Ký túc xá**

---

📌 *Dự án phục vụ mục đích học tập và nghiên cứu cơ sở dữ liệu nâng cao.*
