# Party Killer - Tóm Tắt Codebase

## Cấu Trúc Tệp

### Tệp Dự Án Gốc
- **Party Killer.sln** - Tệp giải pháp Visual Studio
- **Assembly-CSharp.csproj** - Tệp dự án C# (được tự động tạo bởi Unity)
- **Controls.inputactions** - Tài sản hành động đầu vào cho Hệ Thống Đầu Vào Mới

### Thư Mục Assets

#### Các Cảnh (12 tổng cộng)
| Cảnh | Loại | Mục Đích |
|-------|------|---------|
| MAIN | Bền Vững | Trình quản lý trò chơi, hệ thống âm thanh, lớp UI |
| Level00-Level10 | Phụ Gia | 11 cấp độ đấu trường, tải/dỡ khi cần |

#### Kịch Bản (14 tệp C#, ~1.100 LOC)
| Tệp | LOC | Vai Trò |
|------|-----|------|
| GameManager.cs | 195 | Trạng thái trò chơi, sinh người chơi, ghi điểm, chuyển đổi cấp độ |
| PlayerController.cs | 54 | Chuyển động, nhắm, xử lý đầu vào |
| PlayerWeapon.cs | 79 | Hệ thống lựu đạn, bắn đạn, UI nạp lại |
| Bullet.cs | 55 | Vật lý đạn, nảy, phát hiện va chạm |
| AudioManager.cs | 51 | Hệ thống âm thanh Singleton, phát âm thanh |
| Sound.cs | 28 | Cấu trúc dữ liệu âm thanh (tên, clip, âm lượng, cao độ) |
| ScoreUI.cs | 28 | Hiển thị điểm và cập nhật |
| Rotator.cs | 22 | Tiện ích xoay ngẫu nhiên để có sự đa dạng trực quan |
| Riser.cs | 20 | Hoạt ảnh Perlin noise cho tường nhô lên |
| DiscoBall.cs | 18 | Placeholder (lớp trống) |
| CameraShaker.cs (EZCameraShake) | ~80 | Thư viện hiệu ứng rung camera (bên thứ 3) |
| CameraShakeInstance.cs | ~30 | Dữ liệu instance rung riêng lẻ |
| CameraShakePresets.cs | ~50 | Cấu hình rung được đặt trước |
| CameraUtilities.cs | ~15 | Hàm tiện ích cho các hoạt động camera |

#### Tài Sản Âm Thanh
- BigExplode.wav - Nổ tung tiêu diệt người chơi
- Bounce.wav - Va chạm tường đạn
- Explode.wav - Hết hạn đạn
- PartyMusic.wav - Vòng nhạc nền
- PowerDown.wav - Âm thanh chiến thắng cảnh sát
- Ready.wav - Âm thanh bắt đầu hiệp
- Shoot.wav - Bắn vũ khí

#### Prefabs
- playerA, playerB - Nhân vật người chơi (Rigidbody, Collider, Controllers, Weapon)
- Bullet - Đạn nhân vật (Sphere collider, Rigidbody, Bullet script)
- PlayerDeathFX - Hiệu ứng hạt nổ tung
- Walls/ - Biến thể tường (hộp, hộp) với hoạt ảnh Riser
- BulletIndicators (UI) - Trực quan hóa lựu đạn

#### Mô Hình
- BasicWall.fbx - Hình học tường đơn giản
- SquareWall.fbx - Tường thay thế
- WallMaterials/ - Định nghĩa vật liệu URP

#### Vật Liệu
- Vật liệu tương thích URP cho tường, người chơi và hiệu ứng
- Vật liệu hệ thống hạt

#### Hoạt Ảnh
- Ball - Hoạt ảnh nảy/lơ lửng cho phản hồi trực quan
- BulletIndicator - Hoạt ảnh chỉ số UI lựu đạn
- Fader - Chuyển đổi cảnh Fade in/fade out
- WaitingUI - Hoạt ảnh UI "Chờ người chơi"

#### Phông Chữ & UI
- Họ phông chữ Oxanium (Tài sản TextMeshPro SDF)
- Bố cục UI cho điểm, chỉ số đạn, lời nhắc chờ

#### Cài Đặt & Cấu Hình
- Cài Đặt URP (17.3.0)
- Cài Đặt Dự Án (Chất Lượng, Vật Lý, Kết Xuất)

## Kiến Trúc Cốt Lõi

### Mẫu: Singleton

**GameManager**
```csharp
public static GameManager Instance { get; private set; }
```
- Giữ trạng thái trò chơi (điểm, cờ trò chơi, tham chiếu người chơi)
- Được truy cập toàn cầu cho vụ giết người chơi, sự kiện cấp độ

**AudioManager**
```csharp
public static AudioManager instance { get; }
DontDestroyOnLoad(gameObject)
```
- Tồn tại trên các lần tải cảnh
- Tập trung hóa phát âm thanh

### Mẫu: Dựa Trên Thành Phần (Unity ECS-lite)

**Người Chơi:**
- GameObject với Rigidbody (vật lý chuyển động)
- PlayerController (đầu vào → chuyển động)
- PlayerWeapon (đầu vào → đạn)
- Collider (phát hiện người chơi)

**Đạn:**
- GameObject với Rigidbody (vật lý đạn)
- Kịch bản Bullet (logic va chạm)
- Collider (kích hoạt cho giết/nảy)

### Mẫu: Coroutines

**Chuyển đổi cấp độ:**
```csharp
IEnumerator LoadNextLevel()
IEnumerator LoadFirstLevel()
```
- Tải cảnh không đồng bộ
- Chờ dỡ hoàn tất
- Hoạt ảnh Fade phối hợp với tải

### Chiến Lược Tải Cảnh

**Tải Phụ Gia:**
1. Cảnh MAIN được tải trước tiên (bền vững)
2. Các cảnh cấp độ được tải phụ gia ở trên
3. Tham chiếu người chơi được duy trì trên các lần hoán đổi cấp độ
4. Dọn dẹp: Tiêu diệt các đối tượng đạn/FX trước khi dỡ

**Ánh Xạ Chỉ Số:**
- Thứ tự xây dựng cảnh: 0=MAIN, 1=Level00, 2=Level01, ..., 11=Level10
- Lựa chọn ngẫu nhiên: `Random.Range(1, 11)` bỏ qua MAIN (chỉ số 0)

## Các Hệ Thống Chính Được Giải Thích

### Hệ Thống Đầu Vào (Hệ Thống Đầu Vào Mới)

**Controls.inputactions định nghĩa:**
- Move (Giá Trị, gậy 2D)
- Aim (Giá Trị, gậy 2D)
- Fire (Hành Động, nút)

**PlayerInputManager** sinh người chơi khi tham gia:
- Tham gia lần đầu → Người Chơi A (qua callback OnPlayerJoined)
- Tham gia lần thứ hai → Người Chơi B
- Vô hiệu hóa tham gia sau khi Người Chơi B tham gia

### Vật Lý & Va Chạm

**Va Chạm Người Chơi:**
- Rigidbody với trọng lực vô hiệu
- Collider Capsule
- Không kinematic (bị ảnh hưởng bởi lực)

**Va Chạm Đạn:**
- Rigidbody với trọng lực vô hiệu
- Sphere collider (trigger = sai, nên va chạm kích hoạt)
- AddForce() khi spawn qua hướng forward

**Va Chạm Tường:**
- Collider tĩnh (không Rigidbody)
- Collider Box/Mesh

**Callback Va Chạm:**
- OnCollisionEnter() được sử dụng (không kích hoạt)
- Đạn phát hiện: Người Chơi, Đạn Khác, Tường
- Người Chơi phát hiện: Đạn (qua GameManager.KillPlayer())

### Hệ Thống Ghi Điểm

**Điểm Bền Vững:**
```csharp
public static int playerAScore = 0;
public static int playerBScore = 0;
```
- Biến tĩnh (sống trong suốt thời gian phiên)
- Cập nhật trong KillPlayer()
- Hiển thị bởi ScoreUI

### Phát Âm Thanh

**Mảng âm thanh** trong AudioManager:
```csharp
public Sound[] sounds;
```
- Mảng các đối tượng Âm Thanh được điền inspector
- Âm Thanh = tên + clip + vòng lặp + âm lượng + cao độ + phương sai
- Play(string name) tìm và phát theo tên

**Khởi Tạo:**
- Awake() tạo AudioSource cho mỗi âm thanh
- Gán AudioMixerGroup
- Tự động phát "PartyMusic" khi khởi động

### Rung Camera

**Cách sử dụng thư viện EZCameraShake:**
- `CameraShaker.Instance.ShakeOnce(magnitude, roughness, fadeInTime, fadeOutTime)`
- Được gọi khi: Bắn, Nảy, Sự kiện Giết
- Không có mã chuyển động camera thủ công (thư viện xử lý tất cả)

### Hoạt Ảnh Fade

**Animator Fader:**
- Kích hoạt "FadeOut" → fade màn hình đen
- Kích hoạt "FadeIn" → fade từ đen
- Phối hợp với dỡ/tải cấp độ

## Tóm Tắt Phụ Thuộc

| Gói | Phiên Bản | Mục Đích |
|---------|---------|---------|
| com.unity.render-pipelines.universal | 17.3.0 | Đường ống kết xuất URP |
| com.unity.inputsystem | 1.19.0 | Hệ Thống Đầu Vào Mới (tay cầm, bàn phím) |
| com.unity.ai.navigation | 2.0.11 | Hỗ trợ NavMesh (tùy chọn) |
| com.unity.timeline | 1.8.11 | Hoạt ảnh Timeline (tùy chọn) |
| com.unity.ugui | 2.0.0 | UI dựa trên Canvas |
| com.unity.test-framework | 1.6.0 | Kiểm tra đơn vị (tùy chọn) |

**Bên Thứ Ba:**
- EZCameraShake (bao gồm trong Scripts/)

## Thống Kê Mã

| Chỉ Số | Giá Trị |
|--------|-------|
| Tổng Kịch Bản Cốt Lõi | 10 |
| Tổng LOC (Cốt Lõi) | ~1.100 |
| Tổng Tài Sản | 80+ (mô hình, sprite, âm thanh, hoạt ảnh) |
| Số Lượng Cảnh | 12 (1 bền vững + 11 cấp độ) |
| Prefabs | 8+ |
| Vật Liệu | 15+ |
| Clip Âm Thanh | 7 |
| Hành Động Đầu Vào | 3 |

## Thiết Lập Phát Triển

### Phần Mềm Bắt Buộc
- Unity 6000.3.12f1
- Visual Studio 2022 / Rider 2024+
- .NET 6+ (được tự động cài đặt với Visual Studio)

### Tích Hợp IDE
- Plugin Visual Studio Code (tùy chọn)
- Tạo dự án Visual Studio (tự động)

### Đường Ống Xây Dựng
1. Mở Assets/Scenes/MAIN.unity
2. File → Cài Đặt Xây Dựng
3. Thêm Cảnh: MAIN, Level00-Level10 (theo thứ tự)
4. Chọn nền tảng đích
5. Xây Dựng

## Mẫu & Quy Ước Chung

### Đặt Tên GameObject
- Người Chơi: "Player A", "Player B"
- Đạn: "Bullet (Clone)"
- Hiệu Ứng: Được đặt tên bằng thẻ "FX"
- Tường: Được đặt tên theo cấp độ (ví dụ: "Wall_Large")

### Gắn Thẻ
- Người Chơi → "Player"
- Đạn → "Bullet"
- Tường → "Wall"
- Điểm Xuất Hiện → "SpawnPointA", "SpawnPointB"
- Hiệu Ứng → "FX"

### Mẫu Coroutine
- Tải cảnh: WaitForSeconds, async yield
- Chờ hoạt ảnh: Kích hoạt Animator + yield

### Cài Đặt Vật Lý
- Trọng Lực: Y=-9.81 (tiêu chuẩn)
- Vật liệu mặc định: Không có ma sát (bề mặt trơn)
- Ràng Buộc: Rigidbodies bị khóa vào chuyển động XZ chỉ (Y bị đóng)

---

**Tổng Kích Thước Codebase:** ~1.100 LOC (kịch bản cốt lõi) + 80+ tài sản  
**Kiến Trúc Style:** Dựa trên thành phần với trình quản lý Singleton  
**Tổ Chức Mã:** Cấu trúc phẳng (tất cả kịch bản trong Assets/Scripts/)
