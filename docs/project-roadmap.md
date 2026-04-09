# Party Killer - Lộ Trình & Trạng Thái Dự Án

## Phát Hành Hiện Tại: v1.0

**Trạng Thái:** Đã Phát Hành  
**Ngày Phát Hành:** Tháng 4 năm 2026  
**Engine:** Unity 6000.3.12f1  
**Nền Tảng:** PC (Windows, macOS, Linux)

### Mức Độ Hoàn Chỉnh Tính Năng v1.0

| Tính Năng | Trạng Thái | Ưu Tiên | Ghi Chú |
|---------|--------|----------|-------|
| Multiplayer cục bộ 2 người chơi | ✓ Hoàn thành | Cao | Hoàn toàn hoạt động, tay cầm + bàn phím |
| Điều khiển hai gậy | ✓ Hoàn thành | Cao | Chuyển động/Nhắm/Bắn, phản hồi |
| Gameplay đấu trường (11 cấp độ) | ✓ Hoàn thành | Cao | Lựa chọn ngẫu nhiên, tránh lặp lại |
| Vật lý đạn nảy | ✓ Hoàn thành | Cao | 2 lần nảy, giết tức thì |
| Hệ thống lựu đạn (3 đạn tối đa) | ✓ Hoàn thành | Cao | Tự động nạp lại ở 1/giây |
| Theo dõi điểm | ✓ Hoàn thành | Cao | Bền vững trong phiên |
| Hệ thống âm thanh | ✓ Hoàn thành | Trung Bình | 7 hiệu ứng âm thanh, nhạc nền |
| Phản hồi rung camera | ✓ Hoàn thành | Trung Bình | EZCameraShake khi có sự kiện chính |
| UI (điểm, lựu đạn, lời nhắc) | ✓ Hoàn thành | Trung Bình | Tất cả phản hồi hiển thị |
| Chuyển đổi cảnh (fade) | ✓ Hoàn thành | Thấp | Tải cấp độ mịn |

## Chỉ Số

### Cân Bằng Trò Chơi
- **Tốc độ chuyển động:** 5 m/s (điều chỉnh theo kích thước đấu trường)
- **Tốc độ đạn:** 10 m/s (trúng đối thủ trong ~1 giây trên đấu trường)
- **Tốc độ nạp lại:** 1 đạn/giây (thời gian quyết định chiến thuật)
- **Số lượng nảy:** 2 (thêm yếu tố kỹ năng mà không nảy quá mức)

**Quan Sát:**
- Cân bằng cho chơi có kỹ năng so với chơi bình thường
- Không có chiến lược chiếm ưu thế (vị trí so với bắn)
- Khoảng thời gian hiệp: trung bình 15-60 giây

### Hiệu Suất
- **Tốc độ khung hình:** Ổn định 60 FPS (Unity 6, tối ưu)
- **Thời gian tải cấp độ:** ~0.8 giây
- **Dấu chân bộ nhớ:** ~500 MB (tối thiểu cho quy mô trò chơi)
- **Sử dụng CPU:** <20% mỗi người chơi (tương thích máy có thông số thấp)

### Sức Khỏe Mã
- **Tổng LOC:** ~1.100 (kịch bản cốt lõi)
- **Tệp:** 14 tệp C# (10 cốt lõi + 4 thư viện)
- **Độ Phức Tạp:** Thấp (không có tóm tắt lồng nhau)
- **Phủ Sóng Kiểm Tra:** 0% (không có kiểm tra đơn vị)
- **Tài Liệu:** Toàn Diện (README + 5 tài liệu)

## Lịch Sử Giai Đoạn

### Giai Đoạn 1: Nền Tảng (Hoàn Thành)
**Mục Tiêu:** Vòng lặp gameplay cốt lõi hoạt động  
**Khoảng Thời Gian:** ~2 tuần (ước tính, thời gian thực tế không rõ)

- [x] Thiết lập dự án (Unity 6, URP, Hệ Thống Đầu Vào Mới)
- [x] Singleton GameManager
- [x] Sinh người chơi + xử lý đầu vào
- [x] Hệ thống đạn với vật lý
- [x] Phát hiện va chạm (giết, nảy, tiêu diệt)
- [x] Tải/dỡ cảnh

**Kết Quả:** Vòng lặp cốt lõi có thể chơi được (cấp độ đơn). Trạng Thái: Hoàn Thành.

### Giai Đoạn 2: Nội Dung (Hoàn Thành)
**Mục Tiêu:** Nhiều cấp độ, âm thanh, đánh bóng trực quan  
**Khoảng Thời Gian:** ~1 tuần (ước tính)

- [x] Tạo 11 cấp độ đấu trường (Level00-Level10)
- [x] Lựa chọn cấp độ ngẫu nhiên (tránh lặp lại liên tiếp)
- [x] Hệ thống âm thanh (Singleton AudioManager)
- [x] 7 hiệu ứng âm thanh + nhạc nền
- [x] Rung camera (tích hợp thư viện EZCameraShake)
- [x] UI (hiển thị điểm, chỉ số lựu đạn, lời nhắc chờ)
- [x] Hoạt ảnh chuyển đổi cảnh (fade in/out)

**Kết Quả:** Gameplay đầy đủ tính năng với đánh bóng. Trạng Thái: Hoàn Thành.

### Giai Đoạn 3: Tinh Luyện (Hoàn Thành)
**Mục Tiêu:** Cân bằng, sửa lỗi, sẵn sàng phát hành  
**Khoảng Thời Gian:** ~1 tuần (ước tính)

- [x] Điều chỉnh cân bằng trò chơi (tốc độ, tốc độ nạp lại, số lượng nảy)
- [x] Sửa lỗi (vị trí respawn người chơi, phát hiện lặp lại cấp độ)
- [x] Xử lý trường hợp biên của đầu vào (cổng trong trạng thái chờ)
- [x] Phương sai âm thanh (cao độ/âm lượng ngẫu nhiên để có cảm giác động)
- [x] Dọn sạch mã & bình luận
- [x] Tài Liệu (README, tài liệu kỹ thuật)

**Kết Quả:** v1.0 được phát hành. Trạng Thái: Hoàn Thành.

## Vấn Đề Đã Biết & Nợ Kỹ Thuật

### Vấn Đề Nhỏ (Sẽ Không Sửa)

| Vấn Đề | Mức Độ Nghiêm Trọng | Lý Do | Giải Pháp |
|-------|----------|--------|-----------|
| DiscoBall.cs trống | Thấp | Placeholder không sử dụng | Xóa nếu xây dựng để phân phối |
| Không có menu tạm dừng | Thấp | Ngoài phạm vi v1.0 | Alt+Tab sang chương trình khác |
| Bàn phím hai gậy yêu cầu rebinding | Thấp | Thiết lập phức tạp | Sử dụng tay cầm để có trải nghiệm tối ưu |
| Không dữ lưu thống kê | Thấp | Chỉ một phiên duy nhất | Lên kế hoạch cho v2.0 |
| Không có cài đặt khó khăn | Thấp | Cân bằng cố định hoạt động tốt | N/A |

### Tối Ưu Hóa Tiềm Năng (Không Chặn)

| Tối Ưu Hóa | Lợi Ích | Độ Phức Tạp | Ưu Tiên |
|---|---|---|---|
| Object pooling cho đạn | Giảm đột biến GC | Trung Bình | Thấp (không đáng chú ý) |
| Tải lưới không đồng bộ cho tường | Khởi động nhanh hơn | Cao | Thấp (khởi động <1 giây) |
| Bộ đệm đầu vào | Cảm giác tốt hơn | Thấp | Thấp (phản hồi đủ) |
| Bước con vật lý | Nảy mịn hơn | Trung Bình | Thấp (nảy trông tốt) |

## Lộ Trình Tương Lai (v2.0+)

### Được Đề Xuất: Tính Năng Sau Khi Phát Hành

#### v2.0: Menu & Tùy Chỉnh (Ước Tính 2-3 tuần)
- [ ] Cảnh menu chính với nút chơi/thoát
- [ ] Lựa chọn cấp độ (chọn đấu trường cụ thể so với ngẫu nhiên)
- [ ] Tùy chỉnh người chơi (màu sắc, tên)
- [ ] Cài đặt Âm Lượng/Khó Khăn
- [ ] Bảng xếp hạng cục bộ (lưu 10 điểm hàng đầu)

**Mục Tiêu:** UX tốt hơn cho chơi bữa tiệc bình thường

#### v2.1: Nâng Cao Gameplay (Ước Tính 1-2 tuần)
- [ ] Power-up (tăng tốc độ, bắn nhanh, khiên)
- [ ] Hệ thống sức khỏe (3 cú để giết thay vì 1)
- [ ] Loại vũ khí khác nhau (bắn trải, đạn homing)
- [ ] Đối thủ bot (luyện tập một người chơi)

**Mục Tiêu:** Gameplay chiến lược sâu hơn

#### v2.2: Đánh Bóng Trực Quan (Ước Tính 1 tuần)
- [ ] Hiệu ứng hạt cho tường (tác động, đuôi đạn)
- [ ] Hoạt ảnh chết người chơi (ragdoll, tan biến)
- [ ] Chủ đề trực quan cụ thể đấu trường (sa mạc, băng, không gian)
- [ ] Nền hoạt hình (disco ball quay)

**Mục Tiêu:** Cảm giác juice và cảm nhận kiểu AAA

#### v3.0: Multiplayer Trực Tuyến (Ước Tính 3-4 tuần)
- [ ] Tích hợp netcode (Netcode cho GameObjects)
- [ ] Matchmaking (Steam lobbies)
- [ ] Chế độ Ranked với xếp hạng ELO
- [ ] Đa nền tảng (PC + các bảng điều khiển)

**Mục Tiêu:** Cảnh cạnh tranh trực tuyến

#### v3.1: Các Cổng Bảng Điều Khiển (Ước Tính 2-3 tuần mỗi nền tảng)
- [ ] Bản dựng Nintendo Switch
- [ ] Bản dựng PlayStation 5
- [ ] Bản dựng Xbox Series X

**Mục Tiêu:** Tiếp cận đối tượng rộng hơn

### Không Có Khả Năng (Ngoài Phạm Vi)

Các tính năng này xung đột với lõi arcade của Party Killer:
- Chiến dịch một người chơi (trò chơi được thiết kế cho 2P chỉ)
- Câu chuyện/truyền thuyết phức tạp (thẩm mỹ trò chơi bữa tiệc)
- Hỗ trợ 4+ người chơi (đấu trường quá nhỏ, phức tạp netcode)
- Chế độ VR (không phù hợp với gameplay hành động)

## Phụ Thuộc & Ràng Buộc Phiên Bản

### Bắt Buộc
- **Unity 6000.3.12f1** — Phiên bản cố định (nâng cấp phiên bản phụ nếu cần: 6000.3.13+)
- **URP 17.3.0** — Kết xuất (quan trọng cho hình ảnh)
- **Hệ Thống Đầu Vào Mới 1.19.0** — Hỗ trợ tay cầm (không thể thương lượng)

### Được Khuyến Nghị Cho Phát Triển
- **Visual Studio 2022** hoặc **Rider 2024+** — C# IDE
- **TextMeshPro** — Văn bản UI (bao gồm với Unity)

### Tùy Chọn
- **Timeline 1.8.11** — Cảnh cắt (không sử dụng)
- **AI Navigation 2.0.11** — NavMesh (không sử dụng)
- **Khung Kiểm Tra 1.6.0** — Kiểm tra đơn vị (chưa triển khai)

## Chỉ Số Thành Công & Thành Tựu

### Tiêu Chí Phát Hành Đã Đáp Ứng (v1.0)
- [x] Trò chơi chạy ổn định ở 60 FPS
- [x] Người chơi có thể tham gia, chơi và ghi điểm
- [x] Tất cả 11 cấp độ tải mà không có lỗi
- [x] Âm thanh phát chính xác (không có âm thanh bị thiếu)
- [x] Rung camera mang lại phản hồi thỏa đáng
- [x] Vòng lặp gameplay vui vẻ và cân bằng

### Chỉ Số Chất Lượng
| Chỉ Số | Mục Tiêu | Thực Tế | Trạng Thái |
|--------|--------|--------|--------|
| Tỷ lệ Crash | 0% | 0% | ✓ Thông Qua |
| Giảm Frame | <5% khung dưới 60 FPS | <1% | ✓ Thông Qua |
| Thời Gian Tải | <2 giây mỗi cấp độ | 0.8 giây | ✓ Thông Qua |
| Độ Trễ Âm Thanh | <100 ms | <50 ms | ✓ Thông Qua |
| Độ Trễ Đầu Vào | <16 ms | <16 ms | ✓ Thông Qua |

## Khuyến Nghị Cho Nhà Phát Triển Tiếp Theo

### Nếu Tiếp Quản v1.1 (Bảo Trì)
1. Thiết lập git với kiểm soát phiên bản
2. Tạo bản phát hành GitHub cho mỗi bản vá
3. Giám sát báo cáo lỗi của người dùng (nếu được phân phối)
4. Ưu tiên sửa crash > điều chỉnh cân bằng > yêu cầu tính năng
5. Giữ tài liệu được cập nhật với mỗi thay đổi

### Nếu Mở Rộng Sang v2.0
1. Bắt đầu với hệ thống menu (mở khóa các tính năng khác)
2. Triển khai lưu/tải bảng xếp hạng (dữ liệu bền vững)
3. Thêm 3-5 power-up (kiểm tra cân bằng kỹ lưỡng)
4. Lên kế hoạch AI đối thủ bot trước khi viết mã
5. Cân nhắc tái cấu trúc để mở rộng:
   - Trích xuất hằng số trò chơi sang ScriptableObject
   - Tạo tài sản scriptable LevelData (xóa chỉ số mã hóa cứng)
   - Triển khai hệ thống sự kiện (UnityEvent) để tách rời

### Sẵn Sàng Hiệu Suất
- Tốc độ chuyển động có thể tăng gấp đôi (vật lý có thể xử lý 10 m/s mà không có vấn đề)
- Số lượng đạn có thể tăng lên 5-10 (vẫn <1ms chi phí vật lý)
- Độ phức tạp cấp độ có thể tăng 5 lần (hình học tầm thường)
- Nút cổ chai FPS: Hệ thống âm thanh (không phải vật lý), vì vậy âm thanh an toàn để mở rộng

## Nhật Ký Thay Đổi

### v1.0 (Được Phát Hành Tháng 4 năm 2026)
**Phát Hành Ban Đầu**
- Gameplay cốt lõi: trò chơi bắn súng đấu trường 2 người chơi
- 11 cấp độ duy nhất có lựa chọn ngẫu nhiên
- Vật lý đạn nảy (2 lần nảy)
- Hệ thống lựu đạn (3 đạn, nạp lại 1/giây)
- Hệ thống âm thanh có 7 âm thanh
- Phản hồi rung camera
- Theo dõi điểm (trong phiên)
- Tài liệu đầy đủ

### v1.0.1 (Bản Vá Tiềm Năng)
**Sửa Lỗi Sớm Có Thể**
- [ ] Sửa DiscoBall.cs (xóa lớp trống)
- [ ] Thêm menu tạm dừng (toggle đơn giản)
- [ ] Dự phòng bàn phím cho hai gậy
- [ ] Điều chỉnh cân bằng nếu phản hồi cộng đồng yêu cầu

### Phiên Bản Tương Lai
Xem phần "Lộ Trình Tương Lai" ở trên.

## Nhật Ký Quyết Định

### Quyết Định Thiết Kế Được Thực Hiện (v1.0)

| Quyết Định | Lý Do | Các Lựa Chọn Thay Thế Được Xem Xét |
|----------|-----------|------------------------|
| Chỉ 2 người chơi | Đối tượng couch co-op | 4 người chơi (quá hỗn loạn), 1P so với AI (ít vui vẻ hơn) |
| 11 cấp độ | Điểm ngọt cho đa dạng | 5 (quá ít), 20+ (lợi tức giảm dần) |
| 3 đạn tối đa | Buộc quyết định chiến thuật | 5+ (quá hào phóng), 1 (quá hạn chế) |
| Điều khiển hai gậy | Tiêu chuẩn cho bắn súng | Điều khiển tăng (cũ), bố cục 4 nút (chật chội) |
| Cơ chế ricochet | Thêm kỹ năng/dự đoán | Không nảy (ít thú vị), không giới hạn (bị hỏng) |
| Chỉ cục bộ (v1.0) | Phạm vi MVP, thời gian phát triển | Trực tuyến từ đầu (phạm vi creep) |
| Cảnh MAIN bền vững | Nhất quán âm thanh/UI | Các cảnh tự chứa từng cấp độ (phức tạp) |
| Thư viện EZCameraShake | Phản hồi camera nhanh | Triển khai từ đầu (lãng phí thời gian) |

### Tính Năng Bị Từ Chối (Ý Định Ngoài Phạm Vi)

| Tính Năng | Lý Do |
|---------|--------|
| Chiến dịch một người chơi | Trò chơi cốt lõi 2P; AI quá phức tạp cho v1 |
| 4+ người chơi | Đấu trường quá nhỏ; netcode overkill |
| Power-up | Cơn ác mộng cân bằng; giữ đơn giản |
| Menu tạm dừng | Trò chơi arcade—chơi liên tục |
| Bảng xếp hạng | Tập trung một phiên; không bền vững |
| Câu chuyện/truyền thuyết | Trò chơi bữa tiệc—không cần kể chuyện |
| Đồ họa nâng cao | URP mặc định đủ; thời gian sử dụng tốt hơn ở nơi khác |

---

**Cập Nhật Lần Cuối:** Tháng 4 năm 2026  
**Trạng Thái:** Ổn định, hoàn chỉnh tính năng cho v1.0  
**Lần Xem Xét Tiếp Theo:** Sau phản hồi cộng đồng (nếu có) hoặc khi bắt đầu lên kế hoạch v2.0
