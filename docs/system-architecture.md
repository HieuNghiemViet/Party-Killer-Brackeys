# Party Killer - Kiến Trúc Hệ Thống

## Sơ Đồ Kiến Trúc Cấp Cao

```
┌─────────────────────────────────────────────────────────────────┐
│                   Cảnh MAIN (Bền Vững)                    │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │ GameManager (Singleton)                                    │  │
│  │ • Trạng thái trò chơi (điểm, cờ)                              │  │
│  │ • Quản lý sinh/chết người chơi                           │  │
│  │ • Tải/dỡ cấp độ                                 │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                   │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │ AudioManager (Singleton, DontDestroyOnLoad)               │  │
│  │ • Phát âm thanh bền vững                               │  │
│  │ • Tìm kiếm âm thanh có tên + phương sai                           │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                   │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │ Lớp UI (Canvas)                                          │  │
│  │ • ScoreUI (hiển thị playerAScore, playerBScore)           │  │
│  │ • Chỉ số đạn (3 ảnh UI)                         │  │
│  │ • Hoạt ảnh lời nhắc chờ                                │  │
│  │ • Animator Fader (chuyển đổi fade in/out)                │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                   Cảnh Cấp Độ (Tải Phụ Gia)                    │
│  [Được chọn từ Level00-Level10, ngẫu nhiên]                         │
│                                                                   │
│  ┌──────────────────┐         ┌──────────────────┐              │
│  │ Người Chơi A         │         │ Người Chơi B         │              │
│  │ • Rigidbody      │         │ • Rigidbody      │              │
│  │ • Collider       │         │ • Collider       │              │
│  │ • Controller     │         │ • Controller     │              │
│  │ • Weapon         │         │ • Weapon         │              │
│  │ • PlayerInput    │         │ • PlayerInput    │              │
│  └──────────────────┘         └──────────────────┘              │
│           │                            │                         │
│           └────────────┬───────────────┘                         │
│                        ↓                                         │
│            Hệ Thống Đầu Vào (Tay Cầm/Bàn Phím)                      │
│                                                                   │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │ Đấu Trường (Tường, Hình Học Tĩnh)                             │  │
│  │ • Instance BasicWall, SquareWall                         │  │
│  │ • Hoạt ảnh Riser (tường nhô lên)                          │  │
│  │ • Collider tĩnh                                        │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                   │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │ Instance Đạn (Sinh ở Thời Gian Chạy)                      │  │
│  │ • Rigidbody + Sphere collider                             │  │
│  │ • Bộ đếm nảy                                          │  │
│  │ • Chuyển động được điều khiển bởi vật lý                                 │  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                   │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │ Hiệu Ứng (Sinh khi Giết/Nảy)                           │  │
│  │ • Hạt nổ tung                                     │  │
│  │ • Tiêu diệt sau timeout hoặc tải cấp độ tiếp theo              │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

## Quản Lý Cảnh

### Cấu Trúc Cảnh

**Cảnh MAIN (Chỉ Số 0):**
- Tải khi bắt đầu trò chơi
- Không bao giờ dỡ trong gameplay
- Chứa:
  - GameObject GameManager
  - GameObject AudioManager
  - Canvas (UI)
  - PlayerInputManager
  - Animator Fader

**Cảnh Cấp Độ (Chỉ Số 1-11):**
- Level00, Level01, ..., Level10
- Tải phụ gia ở trên MAIN
- Dỡ khi hiệp kết thúc
- Chứa:
  - Hình học đấu trường (tường)
  - Đánh dấu điểm xuất hiện (gắn thẻ "SpawnPointA", "SpawnPointB")
  - Không logic trò chơi (thuần tĩnh)

### Trình Tự Tải

```
Bắt Đầu Trò Chơi
  ↓
Tải MAIN (chỉ số 0)
  ↓
GameManager.Awake() → StartCoroutine(LoadFirstLevel())
  ↓
Chọn cấp độ ngẫu nhiên (1-10)
  ↓
SceneManager.LoadScene(level, Additive)
  ↓
Tìm điểm xuất hiện qua thẻ
  ↓
Chờ tham gia người chơi (PlayerInputManager)
```

### Trình Tự Chuyển Đổi Cấp Độ

```
Hiệp Kết Thúc (Đạn trúng Người Chơi)
  ↓
GameManager.KillPlayer()
  ↓
Đặt gameIsEnded = true
  ↓
StartCoroutine(LoadNextLevel())
  ↓
Chờ 0.75 giây (độ trễ mỹ phẩm)
  ↓
Kích hoạt Fader "FadeOut"
  ↓
Chờ 0.75 giây
  ↓
Tiêu diệt tất cả các đối tượng được gắn thẻ "FX" và "Bullet"
  ↓
SceneManager.UnloadSceneAsync(currentLevel)
  ↓
Chờ dỡ hoàn tất
  ↓
Chọn cấp độ ngẫu nhiên mới (tránh lặp lại)
  ↓
SceneManager.LoadScene(newLevel, Additive)
  ↓
Đặt lại vị trí, vận tốc người chơi
  ↓
Kích hoạt Fader "FadeIn"
  ↓
AudioManager phát "Ready"
  ↓
gameIsEnded = false, waitingForPlayers = false
```

## Hệ Thống Người Chơi

### Thứ Bậc Người Chơi

```
GameObject Người Chơi (Prefab: playerA hoặc playerB)
├── Rigidbody
│   ├── Trọng lực: vô hiệu
│   ├── Ràng buộc: Đóng xoay Y
│   └── Khối lượng: 1 (mặc định)
├── Collider (Capsule, trigger=false)
├── PlayerController
│   ├── moveSpeed: 5
│   └── Trình nghe đầu vào: OnMove, OnAim, OnFire
├── PlayerWeapon
│   ├── bulletPrefab: tham chiếu
│   ├── bulletSpeed: 10
│   ├── bulletRefillRate: 1/giây
│   ├── bulletsReady: 0-3
│   └── Tham chiếu UI: bulletUI01, bulletUI02, bulletUI03
└── PlayerInput (từ PlayerInputManager)
    └── Điều khiển các sự kiện đầu vào
```

### Chuyển Động Người Chơi

**Đầu Vào:**
- Gậy Trái (Chuyển Động) → Callback OnMove → vectơ chuyển động (X, Z)
- Gậy Phải (Nhắm) → Callback OnAim → vectơ nhắm

**Vật Lý:**
```
FixedUpdate() {
    moveDir = (-move.y, 0, move.x)  // Xoay gậy sang tọa độ thế giới
    rb.MovePosition(rb.position + moveDir * moveSpeed * dt)
}
```

**Xoay:**
```
if (aim.magnitude >= 0.5) {
    transform.rotation = LookRotation((-aim.y, 0, aim.x)) * Euler(0, 90, 0)
}
```

Nhắm kiểm soát hướng phía trước. Deadzone tối thiểu 0.5 để ngăn chặn rung.

### Hệ Thống Vũ Khí

**Tạp Chí:** 3 đạn tối đa
**Tốc Độ Nạp Lại:** 1 đạn/giây (có thể cấu hình)
**Ban Đầu:** 1 đạn sẵn sàng khi hiệp bắt đầu

**Logic Nạp Lại:**
```
Update() {
    if (waitingForPlayers || bulletsReady >= 3) return;
    
    if (Time.time >= nextBulletTime) {
        bulletsReady++
        nextBulletTime = Time.time + 1.0f / bulletRefillRate
        UpdateUI()  // Hiển thị chỉ số đạn mới
    }
}
```

**Logic Bắn:**
```
Shoot() {
    if (bulletsReady > 0) {
        Instantiate(bulletPrefab, firePoint.position, firePoint.rotation)
        ballRB.AddForce(firePoint.forward * bulletSpeed, VelocityChange)
        bulletsReady--
        nextBulletTime = Time.time + 1.0f / bulletRefillRate
        AudioManager.Play("Shoot")
        CameraShaker.ShakeOnce(...)
    }
}
```

Không phản hồi hoạt ảnh (bắn tức thì).

## Hệ Thống Đạn

### Vòng Đời Đạn

```
Khởi Tạo tại firePoint
  ↓
AddForce(forward * bulletSpeed) → vận tốc ban đầu
  ↓
Động cơ vật lý di chuyển đạn mỗi frame
  ↓
OnCollisionEnter() kích hoạt
  ├─ Trúng người chơi → Giết người chơi, nổ tung
  ├─ Trúng đạn → Tiêu diệt cả hai đạn
  └─ Trúng tường → Bộ đếm nảy−−1
       ├─ bounce > 0 → tiếp tục (phát "Bounce")
       └─ bounce == 0 → nổ tung (phát "Explode")
  ↓
Tiêu diệt gameObject
```

### Vật Lý Đạn

**Rigidbody:**
- Trọng lực: vô hiệu (hành vi 2.5D)
- Ràng buộc: Không (xoay tự do)
- Phát Hiện Va Chạm: Rời Rạc (mặc định)
- Khối lượng: 1 (mặc định, không đáng kể do VelocityChange)

**Chuyển Động:**
```
ballRB.AddForce(firePoint.forward * bulletSpeed, ForceMode.VelocityChange)
```

Chế độ VelocityChange đặt vận tốc tuyệt đối (bỏ qua khối lượng).

### Cơ Chế Nảy

**Bộ Đếm:** Cho phép 2 lần nảy
**Phát Hiện Nảy:**
```
OnCollisionEnter(Collision col) {
    if (col.collider.CompareTag("Wall")) {
        if (Time.time >= lastBounceTime + 0.05f) {  // Debounce
            if (bounce == 0) {
                Explode()
            } else {
                bounce--
                lastBounceTime = Time.time
                AudioManager.Play("Bounce")
            }
        }
    }
}
```

**Debounce:** Ngăn chặn va chạm nhiều lần trong cùng một frame (cửa sổ 0.05 giây).

**Vật Lý:** Rigidbody nảy tự nhiên từ tường (không có phản xạ thủ công). Bộ đếm nảy chỉ theo dõi các lần nảy được phép.

## Hệ Thống Va Chạm

### Loại Va Chạm

| Collider A | Collider B | Trigger? | Phản Hồi |
|-----------|-----------|----------|----------|
| Người Chơi | Đạn | sai | Giết + Nổ Tung |
| Đạn | Đạn | sai | Tiêu Diệt Cả Hai |
| Đạn | Tường | sai | Nảy/Nổ Tung |
| Đạn | Khác | sai | Nảy/Nổ Tung |
| Người Chơi | Người Chơi | sai | Va Chạm Đàn Hồi (vật lý) |

### Thiết Lập Lớp

Không có lớp tùy chỉnh. Mọi thứ trong lớp "Default".

### Vật Liệu Vật Lý

Không có vật liệu tùy chỉnh. Áp dụng ma sát/đàn hồi mặc định.

### Phát Hiện Giết

```
Bullet.OnCollisionEnter(Collision col) {
    if (col.collider.CompareTag("Player")) {
        GameManager.Instance.KillPlayer(col.collider.gameObject)
        Explode()
    }
}

GameManager.KillPlayer(GameObject p) {
    gameIsEnded = true
    if (p == playerA)
        playerBScore++
    else
        playerAScore++
    
    AudioManager.Play("BigExplode")
    CameraShaker.ShakeOnce(...)
    p.SetActive(false)
    Instantiate(playerDeathPrefab, ...)
    StartCoroutine(LoadNextLevel())
}
```

## Hệ Thống Đầu Vào

### Ánh Xạ Đầu Vào (Controls.inputactions)

| Hành Động | Loại | Ràng Buộc | Đầu Vào |
|--------|------|---------|-------|
| Move | Giá Trị, 2D | Gậy Trái | Tương Tự |
| Aim | Giá Trị, 2D | Gậy Phải | Tương Tự |
| Fire | Hành Động | Kích Hoạt Phải | Kỹ Thuật Số |

### Xử Lý Đầu Vào

**Callback trong PlayerController:**
```
OnMove(InputValue value) → move = value.Get<Vector2>()
OnAim(InputValue value) → aim = value.Get<Vector2>()
OnFire() → weapon.Shoot()
```

Các phương thức được đặt tên On{Action} tự động đăng ký qua Hệ Thống Đầu Vào Mới.

**Cổng Đầu Vào:**
```
void OnMove(InputValue value) {
    if (GameManager.Instance.waitingForPlayers)
        move = Vector2.zero
    else
        move = value.Get<Vector2>()
}
```

Ngăn chặn chuyển động trong khi tham gia/respawn.

### Tham Gia Người Chơi

**PlayerInputManager:**
- Sinh Người Chơi A từ prefab khi tham gia lần đầu
- Thay đổi prefab thành Người Chơi B cho lần tham gia thứ 2
- Gọi callback GameManager.OnPlayerJoined()
- Vô hiệu hóa tham gia sau khi Người Chơi B tham gia

```
OnPlayerJoined(PlayerInput input) {
    if (!inputManager.joiningEnabled) return
    
    if (playerA == null) {
        playerA = input.gameObject
        inputManager.playerPrefab = playerPrefabB
    } else {
        playerB = input.gameObject
        inputManager.DisableJoining()
        waitingForPlayers = false
        AudioManager.Play("Ready")
    }
}
```

## Hệ Thống Âm Thanh

### Kiến Trúc Trình Quản Lý Âm Thanh

**Singleton (DontDestroyOnLoad):**
```
AudioManager
├── AudioMixerGroup (URP master)
├── Sound[] sounds (được điền inspector)
└── AudioSource[] (được tạo mỗi Âm Thanh trong Awake)
```

**Cấu Trúc Âm Thanh:**
```
Sound {
    string name
    AudioClip clip
    bool loop
    float volume, pitch
    float volumeVariance, pitchVariance
    AudioSource source  // Được tạo khi chạy
}
```

### Phát Âm Thanh

**Tìm kiếm theo tên:**
```
Play(string sound) {
    Sound s = Array.Find(sounds, item => item.name == sound)
    if (s == null) {
        Debug.LogWarning("Not found")
        return
    }
    s.source.volume = s.volume * (1 + Random.Range(-variance/2, variance/2))
    s.source.pitch = s.pitch * (1 + Random.Range(-variance/2, variance/2))
    s.source.Play()
}
```

**Âm Thanh Được Sử Dụng:**
- "PartyMusic" — vòng lặp, nền
- "Ready" — bắt đầu hiệp
- "Shoot" — bắn vũ khí (kích hoạt ngay lập tức)
- "Bounce" — va chạm tường đạn
- "Explode" — hết hạn đạn
- "BigExplode" — giết người chơi
- "PowerDown" — cảnh sát thắng (hương vị tùy chọn)

### Khởi Tạo Âm Thanh

```
Awake() {
    // Mẫu Singleton
    if (instance != null) Destroy(gameObject)
    else instance = this; DontDestroyOnLoad(gameObject)
    
    // Tạo AudioSources
    foreach (Sound s in sounds) {
        s.source = gameObject.AddComponent<AudioSource>()
        s.source.clip = s.clip
        s.source.loop = s.loop
        s.source.outputAudioMixerGroup = mixerGroup
    }
    
    Play("PartyMusic")  // Tự động khởi động nền
}
```

## Hiệu Ứng Camera & Màn Hình

### Rung Camera (EZCameraShake)

**Thư Viện:** Bên thứ 3, bao gồm trong Assets/Scripts/EZCameraShake/

**Cách Sử Dụng:**
```
CameraShaker.Instance.ShakeOnce(magnitude, roughness, fadeInTime, fadeOutTime)
```

**Điểm Kích Hoạt:**
- Bắn: ShakeOnce(1.3, 1.3, 0.05, 0.25)
- Nảy: ShakeOnce(2, 2, 0.05, 0.35)
- Giết: ShakeOnce(6, 6, 0.1, 1.5)

**Triển Khai:** Thành phần CameraShaker trên camera chính. Không cần mã camera thủ công.

### Fade Màn Hình

**Animator Fader:**
- Thành Phần: Animator có 2 trạng thái
- Chuyển Đổi: Kích hoạt FadeOut → đen, Kích hoạt FadeIn → hiển thị
- Thời Lượng: 0.75 giây mỗi fade

**Cách Sử Dụng:**
```
fader.SetTrigger("FadeOut")  // Trước khi dỡ
// ... chờ ...
fader.SetTrigger("FadeIn")   // Sau khi tải
```

Phối hợp với dỡ/tải cảnh (không phải độ trễ gameplay).

## Hệ Thống UI

### Cấu Trúc Canvas

**Canvas Cảnh MAIN:**
```
Canvas (RenderMode: ScreenSpace)
├── ScoreDisplay (TextMeshPro)
│   ├── Văn Bản: "Player A: {playerAScore}"
│   └── Văn Bản: "Player B: {playerBScore}"
├── BulletIndicators (Nhóm Ảnh)
│   ├── bulletUI01 (Ảnh, hoạt động = đạn >= 1)
│   ├── bulletUI02 (Ảnh, hoạt động = đạn >= 2)
│   └── bulletUI03 (Ảnh, hoạt động = đạn >= 3)
├── WaitingPrompt (Animator)
│   └── Văn Bản: "Chờ người chơi..."
└── Fader (Animator)
    └── Ảnh (lớp phủ đen)
```

### Cập Nhật ScoreUI

```
ScoreUI.cs {
    void Update() {
        scoreTextA.text = "Player A: " + GameManager.playerAScore
        scoreTextB.text = "Player B: " + GameManager.playerBScore
    }
}
```

Khảo sát điểm tĩnh GameManager mỗi frame.

### Cập Nhật Chỉ Số Đạn

```
PlayerWeapon.Update() {
    // Cập nhật ảnh UI nào đang hoạt động
    bulletUI01.SetActive(bulletsReady >= 1)
    bulletUI02.SetActive(bulletsReady >= 2)
    bulletUI03.SetActive(bulletsReady >= 3)
}
```

Chuyển đổi trực tiếp trạng thái hoạt động của đối tượng trò chơi.

## Luồng Dữ Liệu

### Luồng Hiệp

```
Bắt Đầu Hiệp
  ↓
waitingForPlayers = true
gameIsEnded = false
PlayerInputManager nghe tham gia
  ↓
Người Chơi A Tham Gia → playerA = input.gameObject
Người Chơi B Tham Gia → playerB = input.gameObject, waitingForPlayers = false
  ↓
AudioManager.Play("Ready")
  ↓
Đầu vào được bật, vòng lặp trò chơi chạy
  ↓
Người chơi bắn đạn → va chạm với đối thủ
  ↓
Bullet.OnCollisionEnter() phát hiện cú đánh người chơi
  ↓
GameManager.KillPlayer() được gọi
  ↓
Điểm được cập nhật (playerAScore hoặc playerBScore)
  ↓
gameIsEnded = true (chặn các cái chết tiếp theo trong hiệp này)
  ↓
Hiệu ứng (nổ tung, rung camera, âm thanh)
  ↓
Coroutine LoadNextLevel() bắt đầu
  ↓
Fade, dỡ, nạp, fade lại
  ↓
Hiệp tiếp theo bắt đầu
```

### Biến Trạng Thái

**GameManager:**
- `playerAScore`, `playerBScore` — tĩnh, trong suốt phiên
- `waitingForPlayers` — cụ thể cho hiệp, cổng đầu vào
- `gameIsEnded` — cụ thể cho hiệp, cổng giết

**PlayerWeapon:**
- `bulletsReady` — cụ thể cho người chơi, giảm khi bắn
- `nextBulletTime` — cụ thể cho người chơi, theo dõi để nạp lại

**Đạn:**
- `bounce` — cụ thể cho instance, giảm khi trúng tường

## Đặc Điểm Hiệu Suất

### Tốc Độ Cập Nhật Vật Lý
- Bước thời gian cố định 60 FPS (Time.fixedDeltaTime = 0.0167 giây)
- Rigidbody.MovePosition() được gọi trong FixedUpdate
- Không bỏ qua frame thủ công hoặc các bước con vật lý

### Bộ Nhớ

**Mỗi Hiệp:**
- 2 instance Người Chơi (bền vững)
- Tối đa 3-6 instance Đạn (tiêu diệt tức thì)
- 1 cảnh Cấp Độ (hình học đấu trường)

**Mỗi Suốt Đời:**
- AudioManager (không bao giờ bị tiêu diệt)
- GameManager (tiêu diệt với MAIN, tạo lại lần chạy tiếp theo)

### Thu Gom Rác

Phân bổ tối thiểu:
- Vector/Quaternion được tạo mỗi frame (tạm thời)
- Không có bộ sưu tập bền vững
- Coroutine được dọn sạch mỗi chuyển đổi cấp độ

### Thời Gian Tải

**Tải cấp độ:** <1 giây (hình học đơn giản)
**Dỡ cảnh:** <0.5 giây (dọn sạch đạn/FX)
**Chuyển đổi tổng cộng:** ~2 giây (với độ trễ hoạt ảnh)

## Điểm Khả Mở Rộng

### Thêm Âm Thanh Mới

1. Tạo tệp .wav trong Assets/Audio/
2. Thêm đối tượng Sound vào AudioManager.sounds[] trong inspector
3. Gọi AudioManager.instance.Play("SoundName")

### Thêm Cấp Độ Mới

1. Tạo cảnh mới với tường + điểm xuất hiện
2. Thêm vào Cài Đặt Xây Dựng (chỉ số tiếp theo có sẵn)
3. Cập nhật phạm vi GetRandomLevelIndex() nếu cần
4. Gắn thẻ điểm xuất hiện "SpawnPointA", "SpawnPointB"

### Thêm Hiệu Ứng Camera

1. Sử dụng CameraShaker.Instance.ShakeOnce() với tham số tùy chỉnh
2. Hoặc thêm hiệu ứng hạt vào prefab PlayerDeathFX

### Sửa Đổi Cân Bằng Trò Chơi

Tất cả các giá trị có thể điều chỉnh được hiển thị dưới dạng trường công khai:
- PlayerController.moveSpeed
- PlayerWeapon.bulletSpeed, bulletRefillRate
- Số lượng nảy đạn (mã hóa cứng như `int bounce = 2`)
- Cường độ CameraShaker (điều chỉnh cho mỗi sự kiện)

---

**Cập Nhật Lần Cuối:** Tháng 4 năm 2026  
**Kiến Trúc Style:** Dựa trên thành phần với trình quản lý Singleton  
**Nền Tảng Đích:** PC (Windows/Mac/Linux)
