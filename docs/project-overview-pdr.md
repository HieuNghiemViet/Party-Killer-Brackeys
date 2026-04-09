# Party Killer - Tổng Quan Dự Án & Định Nghĩa Sản Phẩm

## Định Nghĩa Sản Phẩm

**Party Killer** là một trò chơi bắn súng toàn thể cục bộ với 2 người chơi nhanh chóng. Một người chơi là "bữa tiệc", người chơi khác là "cảnh sát". Người đầu tiên tiêu diệt đối thủ được 1 điểm. Trò chơi tiếp tục vô hạn với theo dõi điểm bền vững trong nhiều hiệp và cấp độ.

### Đối Tượng Mục Tiêu
- Những người yêu thích chơi game multiplayer cục bộ (chơi cùng sofa)
- Những người chơi arcade/trò chơi bình thường
- Các sự kiện trò chơi bữa tiệc, game jam, môi trường giáo dục
- Những người chơi tìm kiếm cuộc cạnh tranh nhanh, dựa trên kỹ năng

### Nền Tảng
- PC (Windows, macOS, Linux)
- Được xây dựng cho chơi cục bộ chỉ (không có multiplayer trực tuyến)

## Khái Niệm Trò Chơi

### Vòng Lặp Cốt Lõi
1. Hai người chơi tham gia (tay cầm hoặc bàn phím)
2. Đấu trường ngẫu nhiên được tải
3. Người chơi xuất hiện ở các đầu đối diện
4. Hiệp chiến đấu bắt đầu (chuyển động + bắn)
5. Người chơi đầu tiên bị tiêu diệt → đối thủ ghi điểm
6. Đấu trường mới được tải, người chơi respawn
7. Lặp lại

### Tính Năng Độc Đáo
- **Đạn nảy:** Đạn nảy 2 lần trên tường trước khi nổ, thêm yếu tố kỹ năng/câu đố
- **Thiếu lựu đạn:** Tối đa 3 đạn, ~1 giây nạp lại—buộc phải bắn chiến thuật
- **Đa dạng đấu trường:** 11 cấp độ độc đáo với lựa chọn ngẫu nhiên để ngăn chặn lặp lại
- **Chủ đề Disco:** Nhạc bữa tiệc, hiệu ứng hình ảnh, thẩm mỹ arcade
- **Phản hồi đáp ứng:** Camera rung, hiệu ứng âm thanh, cập nhật UI liên kết gameplay với hành động người chơi

## Yêu Cầu Chức Năng

| Yêu Cầu | Trạng Thái | Triển Khai |
|---|---|---|
| Multiplayer cục bộ 2 người chơi | Hoàn thành | PlayerInputManager + sinh đôi người chơi |
| Lược đồ điều khiển hai gậy | Hoàn thành | Trái=chuyển động, Phải=nhắm, Kích hoạt=bắn |
| Vật lý đạn nảy | Hoàn thành | Bộ đếm nảy 2 + phát hiện va chạm tường |
| Nạp lại đạn tự động | Hoàn thành | Nạp lại 1 đạn/giây, tối đa 3 |
| Tiêu diệt người chơi = ghi điểm | Hoàn thành | Va chạm đạn → KillPlayer() → cập nhật điểm |
| Xoay vòng cấp độ đấu trường | Hoàn thành | Lựa chọn cấp độ ngẫu nhiên, tránh lặp lại liên tiếp |
| Hệ thống hiệu ứng âm thanh | Hoàn thành | Singleton AudioManager với phát âm thanh có tên |
| Phản hồi UI (đạn, điểm) | Hoàn thành | Chỉ số đạn + hiển thị ScoreUI |
| Rung camera khi có sự kiện | Hoàn thành | Tích hợp EZCameraShake |
| Hoạt ảnh Fade | Hoàn thành | Animator Fader cho chuyển đổi cảnh |

## Yêu Cầu Không-Chức Năng

| Yêu Cầu | Mục Tiêu | Trạng Thái |
|---|---|---|
| Tốc độ khung hình | Tối thiểu 60 FPS | Hoàn thành (Unity 6, vật lý tối ưu) |
| Thời gian tải mỗi hiệp | <2 giây | Hoàn thành (tải cảnh không đồng bộ) |
| Độ trễ đầu vào | <16 ms | Hoàn thành (FixedUpdate ở 60Hz) |
| Độ trễ âm thanh | Không nhận thấy (<100 ms) | Hoàn thành (phát AudioSource) |
| Khả năng mở rộng | 2 người chơi cố định | Hoàn thành (không cần scaling multiplayer) |

## Cân Bằng Trò Chơi

### Cân Bằng Vũ Khí
- **Tốc độ đạn:** 10 m/s (có thể điều chỉnh qua PlayerWeapon.bulletSpeed)
- **Tốc độ nạp lại:** 1 đạn/giây (có thể điều chỉnh qua PlayerWeapon.bulletRefillRate)
- **Tạp chí:** Tối đa 3 đạn (có thể điều chỉnh qua số lượng chỉ số UI)
- **Sát thương nảy:** Không giới hạn (đạn không mất sức mạnh khi nảy)

### Cân Bằng Chuyển Động
- **Tốc độ chuyển động:** 5 m/s (có thể điều chỉnh qua PlayerController.moveSpeed)
- **Không chạy nước rút/né:** Khuyến khích chơi vị trí và dự đoán

### Thiết Kế Đấu Trường
- Không có item hồi máu hoặc power-up
- Tường tĩnh (không có nguy hiểm như dung nham/gai)
- Điểm xuất hiện cách xa nhau để ngăn chặn các cái chết tức thì

## Chỉ Số Thành Công (v1)

- Trò chơi chạy ổn định ở 60 FPS
- Thời gian tải cấp độ < 2 giây
- Người chơi có thể tham gia và chơi hiệp đầy đủ mà không có lỗi
- Âm thanh phát chính xác (không có âm thanh bị thiếu)
- Rung camera mang lại phản hồi thỏa đáng
- Lựa chọn cấp độ ngẫu nhiên ngăn chặn cùng một cấp độ hai lần liên tiếp

## Những Hạn Chế Đã Biết

1. **Không có hệ thống menu:** Người chơi xuất hiện trực tiếp vào cấp độ đầu tiên
2. **Không có chức năng tạm dừng:** Gameplay liên tục
3. **Không có chia cấp độ khó:** Cân bằng giống nhau cho tất cả mức kỹ năng
4. **Hỗ trợ bàn phím:** Hai gậy yêu cầu tay cầm hoặc ánh xạ bàn phím tùy chỉnh
5. **Không có replay:** Chỉ chơi trong một phiên
6. **Không theo dõi thống kê:** Điểm dữ lưu cho đến khi ứng dụng đóng

## Ràng Buộc Kỹ Thuật

- **Động cơ vật lý:** Unity Rigidbody 3D (không có vật lý 2D)
- **Kết xuất:** URP (tiêu chuẩn cho Unity 6)
- **Tải cảnh:** Chỉ phụ gia (MAIN bền vững + cảnh cấp độ)
- **Âm thanh:** AudioMixer + AudioSources (không có FMOD/Wwise)
- **Đầu vào:** Hệ thống đầu vào mới 1.19.0 (không có Input Manager cũ)

## Lộ Trình Tương Lai (Ngoài Phạm Vi)

- [ ] Menu chính & lựa chọn cảnh
- [ ] Chức năng tạm dừng/tiếp tục
- [ ] Multiplayer trực tuyến (dự án riêng)
- [ ] Power-up (tăng tốc độ, bắn nhanh, v.v.)
- [ ] Cài đặt âm lượng/khó khăn
- [ ] Trình chỉnh sửa đấu trường tùy chỉnh
- [ ] Tùy chỉnh người chơi (màu sắc, tên)
- [ ] Theo dõi thống kê/bảng xếp hạng (lưu cục bộ)
- [ ] Đối thủ bot (AI)
- [ ] Phát hành bảng điều khiển (các cổng Switch/PS5)

## Lịch Sử Phiên Bản

| Phiên Bản | Ngày | Trạng Thái | Ghi Chú |
|---|---|---|---|
| 1.0 | Tháng 4 năm 2026 | Đã phát hành | Trò chơi bắn súng multiplayer cục bộ ban đầu |

---

**Chủ Sở Hữu:** Brackeys / Nhà Phát Triển Độc Lập  
**Cập Nhật Lần Cuối:** Tháng 4 năm 2026  
**Phiên Bản Engine:** Unity 6000.3.12f1
