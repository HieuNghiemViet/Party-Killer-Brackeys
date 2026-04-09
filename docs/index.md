# Chỉ Mục Tài Liệu Party Killer

Tài liệu hoàn chỉnh cho Party Killer, một trò chơi bắn súng cục bộ 2 người chơi từ trên xuống được xây dựng trong Unity 6.

## Liên Kết Nhanh

**Mới dùng dự án này?** Bắt đầu ở đây:
1. [README.md](../README.md) — Tổng quan trò chơi, thiết lập, điều khiển, khắc phục sự cố
2. [project-overview-pdr.md](./project-overview-pdr.md) — Định nghĩa sản phẩm, tính năng, cân bằng
3. [system-architecture.md](./system-architecture.md) — Cách mọi thứ phù hợp với nhau

**Đang phát triển?** Đọc những điều này:
1. [codebase-summary.md](./codebase-summary.md) — Cấu trúc tệp, danh sách kịch bản, phụ thuộc
2. [code-standards.md](./code-standards.md) — Quy ước C#, mẫu, thực tiễn tốt nhất
3. [system-architecture.md](./system-architecture.md) — Đi sâu kỹ thuật

**Lên kế hoạch cho công việc trong tương lai?** Xem:
1. [project-roadmap.md](./project-roadmap.md) — Trạng thái phát hành, chỉ số, kế hoạch tương lai

---

## Tệp Tài Liệu

### Gốc Dự Án
- **[README.md](../README.md)** (147 dòng)
  - Khởi động nhanh, hướng dẫn thiết lập, điều khiển, cấu trúc dự án, khắc phục sự cố
  - Đối tượng: Người chơi, nhà phát triển mới

### Thư Mục Tài Liệu

#### Tổng Quan & Lên Kế Hoạch
- **[project-overview-pdr.md](./project-overview-pdr.md)** (126 dòng)
  - Định nghĩa sản phẩm, khái niệm trò chơi, tính năng, cân bằng, yêu cầu
  - Đối tượng: Nhà quản lý sản phẩm, nhà thiết kế, các bên liên quan

#### Mã & Cấu Trúc
- **[codebase-summary.md](./codebase-summary.md)** (278 dòng)
  - Cấu trúc tệp, danh sách kịch bản, phụ thuộc, mẫu, thống kê mã
  - Đối tượng: Nhà phát triển, kiến trúc sư

- **[code-standards.md](./code-standards.md)** (484 dòng)
  - Quy ước đặt tên C#, mẫu, tổ chức, hướng dẫn hiệu suất
  - Đối tượng: Nhà phát triển duy trì/mở rộng codebase

#### Kiến Trúc
- **[system-architecture.md](./system-architecture.md)** (653 dòng)
  - Quản lý cảnh, hệ thống trò chơi (đầu vào, vật lý, âm thanh, UI), luồng dữ liệu
  - Đối tượng: Nhà phát triển triển khai tính năng, gỡ lỗi

#### Lộ Trình & Trạng Thái
- **[project-roadmap.md](./project-roadmap.md)** (280 dòng)
  - Trạng thái phát hành, chỉ số, kế hoạch tương lai, nhật ký quyết định, khuyến nghị
  - Đối tượng: Dẫn dắt dự án, người bảo trì tiếp theo

---

## Theo Vai Trò

### Nhà Thiết Kế Trò Chơi
→ Đọc: [project-overview-pdr.md](./project-overview-pdr.md), [project-roadmap.md](./project-roadmap.md)

### Lập Trình Viên Gameplay
→ Đọc: [system-architecture.md](./system-architecture.md), [codebase-summary.md](./codebase-summary.md), [code-standards.md](./code-standards.md)

### Lập Trình Viên Công Cụ/Động Cơ
→ Đọc: [system-architecture.md](./system-architecture.md) (Scene Loading, Physics sections), [codebase-summary.md](./codebase-summary.md) (Dependencies)

### Tester QA
→ Đọc: [README.md](../README.md), [project-overview-pdr.md](./project-overview-pdr.md) (Cân Bằng Trò Chơi)

### Thành Viên Đội Mới
→ Đọc theo thứ tự: [README.md](../README.md) → [codebase-summary.md](./codebase-summary.md) → [system-architecture.md](./system-architecture.md) → [code-standards.md](./code-standards.md)

### Người Bảo Trì Tiếp Theo
→ Đọc: [project-roadmap.md](./project-roadmap.md) (Khuyến Nghị), [code-standards.md](./code-standards.md), [codebase-summary.md](./codebase-summary.md)

---

## Những Sự Kiện Chính Một Cách Nhanh Chóng

| Khía Cạnh | Giá Trị |
|--------|-------|
| **Engine** | Unity 6000.3.12f1 |
| **Thể Loại** | Multiplayer Cục Bộ, Bắn Súng Từ Trên Xuống |
| **Người Chơi** | 2 (chỉ cục bộ co-op) |
| **Cấp Độ** | 11 đấu trường (Level00-Level10, lựa chọn ngẫu nhiên) |
| **Mã** | ~1.100 LOC (14 kịch bản C#) |
| **Đường Ống Kết Xuất** | URP 17.3.0 |
| **Hệ Thống Đầu Vào** | Hệ Thống Đầu Vào Mới 1.19.0 (tay cầm + bàn phím) |
| **Âm Thanh** | 7 âm thanh + nhạc nền, singleton AudioManager |
| **Trạng Thái** | v1.0 Đã Phát Hành, Hoàn Chỉnh Tính Năng |

---

## Tiêu Chuẩn Tài Liệu

Tất cả tài liệu:
- **Chính Xác:** Xác minh mã (đọc kịch bản, xác nhận hành vi)
- **Ngắn Gọn:** Viết kỹ thuật, tối thiểu văn xuôi
- **Được Tổ Chức:** Cấu trúc phân cấp, điều hướng rõ ràng
- **Ví Dụ:** Đoạn mã thực, không được tạo
- **Tham Chiếu Chéo:** Liên kết giữa các tài liệu liên quan
- **Dễ Duy Trì:** Cập nhật khi mã thay đổi

---

## Cách Cập Nhật Tài Liệu

Khi thay đổi mã xảy ra:

1. **Xác định tài liệu bị ảnh hưởng:** (Ví dụ: thay đổi PlayerWeapon → ảnh hưởng đến codebase-summary, code-standards, system-architecture)
2. **Cập nhật các phần liên quan:** (Ví dụ: cập nhật số LOC, tên tham số, chữ ký phương thức)
3. **Xác minh tính chính xác:** (Ví dụ: đọc lại mã thực, xác nhận thay đổi)
4. **Kiểm tra tham chiếu chéo:** (Ví dụ: đảm bảo tài liệu khác không mâu thuẫn)
5. **Cập nhật chỉ mục này nếu cấu trúc thay đổi**

### Cập Nhật Thường Gặp
| Thay Đổi | Tài Liệu Bị Ảnh Hưởng |
|--------|--------------|
| Kịch bản mới | codebase-summary.md (thêm vào bảng kịch bản) |
| Lớp/phương thức được đổi tên | code-standards.md, system-architecture.md, codebase-summary.md |
| Tinh chỉnh cân bằng trò chơi | project-overview-pdr.md, project-roadmap.md (chỉ số) |
| Âm thanh/cấp độ mới | codebase-summary.md (cập nhật số lượng tài sản) |
| Tái cấu trúc kiến trúc | system-architecture.md (xây dựng lại sơ đồ/phần) |

---

## Liên Hệ & Quyền Sở Hữu

**Dự Án:** Party Killer  
**Chủ Sở Hữu:** Brackeys / Nhà Phát Triển Độc Lập  
**Phiên Bản:** 1.0 (Đã Phát Hành Tháng 4 năm 2026)  
**Tài Liệu:** Được Tạo Ngày 7 Tháng 4 năm 2026

---

## Thống Kê Tài Liệu

| Tài Liệu | Dòng | Kích Thước | Thể Loại |
|----------|-------|------|----------|
| README.md | 147 | 5.0 KB | Bắt Đầu |
| project-overview-pdr.md | 126 | 4.9 KB | Định Nghĩa Sản Phẩm |
| codebase-summary.md | 278 | 7.9 KB | Cấu Trúc |
| code-standards.md | 484 | 11 KB | Tiêu Chuẩn |
| system-architecture.md | 653 | 21 KB | Kiến Trúc |
| project-roadmap.md | 280 | 11 KB | Lộ Trình |
| **TỔNG** | **1.968** | **64 KB** | Tất Cả Tài Liệu |

Tất cả tệp tuân thủ giới hạn 800 LOC. Không cần chia tách tệp.

---

**Cập Nhật Lần Cuối:** Ngày 7 Tháng 4 năm 2026  
**Đối Tượng:** Tất cả những người liên quan dự án (nhà thiết kế, nhà phát triển, dẫn dắt, tester)
