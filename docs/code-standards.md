# Party Killer - Tiêu Chuẩn & Quy Ước Mã

## Hướng Dẫn Kiểu C#

### Quy Ước Đặt Tên

**Lớp & Phương Thức:** PascalCase
```csharp
public class PlayerController { }
public void OnFire() { }
public void Shoot() { }
```

**Thuộc Tính & Trường:** camelCase (riêng tư), PascalCase (công khai)
```csharp
private float nextBulletTime = 0f;
public float bulletSpeed = 10f;
public GameObject bulletPrefab;
```

**Biến Cục Bộ:** camelCase
```csharp
Vector2 move;
Vector3 moveDir = new Vector3(-move.y, 0f, move.x);
int bounce = 2;
```

**Hằng Số:** UPPER_SNAKE_CASE (hiếm khi dùng, thích các trường được tuần tự)
```csharp
// Không thường dùng trong codebase này
```

**Callback:** Mẫu đặt tên OnXxx
```csharp
void OnPlayerJoined(PlayerInput input) { }
void OnCollisionEnter(Collision col) { }
void OnMove(InputValue value) { }
void OnAim(InputValue value) { }
void OnFire() { }
```

### Cách Sử Dụng Namespace

Không sử dụng namespace rõ ràng (không gian tên toàn cầu mặc định). Nếu mở rộng, tuân theo mẫu:
```csharp
namespace PartyKiller.Gameplay { }
namespace PartyKiller.Audio { }
namespace PartyKiller.UI { }
```

### Bộ Chỉnh Sửa Truy Cập

- **Công Khai:** Các tham số gameplay được hiển thị inspector, sự kiện, phương thức trình quản lý
- **Riêng Tư:** Chi tiết triển khai, trạng thái nội bộ
- **SerializeField:** Điều chỉnh inspector mà không hiển thị API công khai

```csharp
public float moveSpeed = 5f;  // Điều chỉnh được trong inspector
private float nextBulletTime = 0f;  // Nội bộ
```

## Mẫu Cụ Thể Của Unity

### Mẫu Singleton

**AudioManager** (tồn tại trên các cảnh):
```csharp
public class AudioManager : MonoBehaviour {
    public static AudioManager instance;

    private void Awake() {
        if (instance != null) {
            Destroy(gameObject);
        } else {
            instance = this;
            DontDestroyOnLoad(gameObject);  // Tồn tại qua các lần tải cảnh
        }
    }
}
```

**GameManager** (đặt lại mỗi phiên):
```csharp
public class GameManager : MonoBehaviour {
    public static GameManager Instance;

    private void Awake() {
        Instance = this;  // Không DontDestroyOnLoad—đặt lại với cảnh MAIN
    }
}
```

### Mẫu Coroutine

Tải cảnh với async yield:
```csharp
IEnumerator LoadNextLevel() {
    yield return new WaitForSeconds(.75f);
    fader.SetTrigger("FadeOut");
    yield return new WaitForSeconds(.75f);
    
    AsyncOperation unload = SceneManager.UnloadSceneAsync(currentLevel);
    while (!unload.isDone) {
        yield return 0;  // Chờ một frame
    }
}
```

### Mẫu Callback

Callback Input System:
```csharp
void OnMove(InputValue value) {
    move = value.Get<Vector2>();
}

void OnFire() {
    weapon.Shoot();
}
```

Callback va chạm:
```csharp
private void OnCollisionEnter(Collision col) {
    if (col.collider.CompareTag("Player")) {
        GameManager.Instance.KillPlayer(col.collider.gameObject);
    }
}
```

### Cách Sử Dụng Vật Lý & Rigidbody

Chuyển động qua Rigidbody:
```csharp
rb.MovePosition(rb.position + moveDir * moveSpeed * Time.fixedDeltaTime);
```

Không phải điển hình `transform.Translate()` (không tương thích với vật lý).

Áp dụng lực:
```csharp
ballRB.AddForce(firePoint.forward * bulletSpeed, ForceMode.VelocityChange);
```

Sử dụng `VelocityChange` mode cho tốc độ tức thì (bỏ qua khối lượng).

### Mẫu Sự Kiện

Theo dõi điểm tĩnh:
```csharp
public static int playerAScore = 0;
public static int playerBScore = 0;
```

Cập nhật trực tiếp khi có sự kiện:
```csharp
playerBScore++;  // Tăng thêm trong KillPlayer()
```

## Tổ Chức Mã

### Cấu Trúc Tệp
- Một lớp trên mỗi tệp (quy ước C# tiêu chuẩn)
- Tiện ích liên quan (Sound.cs) được ghép với các trình quản lý
- Các tệp kịch bản phẳng trong thư mục Assets/Scripts/

### Tổ Chức Phương Thức

Mẫu tiêu chuẩn:
1. Trường/Thuộc Tính
2. Singleton/Awake
3. Vòng Đời (Start, Update, FixedUpdate)
4. Xử lý sự kiện (On*)
5. Phương thức công khai
6. Phương thức riêng tư

Ví Dụ (PlayerWeapon.cs):
```csharp
public class PlayerWeapon : MonoBehaviour {
    // Trường
    public GameObject bulletPrefab;
    public float bulletSpeed = 10f;
    private int bulletsReady = 0;
    
    // Vòng Đời
    public void Reset() { }
    private void Update() { }
    
    // Phương thức công khai
    public void Shoot() { }
}
```

### Thụt Lề & Định Dạng

- 4 khoảng trắng trên mỗi cấp thụt lề (C# tiêu chuẩn)
- Dấu ngoặc mở ở cùng dòng với khai báo
- Khoảng cách: dòng trống giữa các phương thức

```csharp
public void DoSomething() {
    if (condition) {
        action();
    }
}
```

### Bình Luận

Bình luận tối thiểu (mã tự ghi chép). Chỉ thêm bình luận cho logic không rõ ràng:

```csharp
// Tránh cùng một cấp độ hai lần liên tiếp
int random = currentLevel;
while (random == currentLevel) {
    random = Random.Range(1, SceneManager.sceneCountInBuildSettings);
}
```

Không có bình luận khối hoặc tài liệu XML (không được thực hiện trong dự án này).

## Quy Ước Vật Lý

### Thiết Lập Rigidbody

Thiết lập người chơi/đạn tiêu chuẩn:
- Loại Cơ Thể: Động
- Trọng Lực: Vô Hiệu Hóa
- Ràng Buộc: Đóng Y (chơi từ trên xuống giống 2D)
- Phát Hiện Va Chạm: Rời Rạc (mặc định)

### Loại Collider

| Loại | Trường Hợp Sử Dụng |
|------|----------|
| Capsule | Nhân vật người chơi |
| Sphere | Đạn nhân vật |
| Box | Tường, đấu trường |
| Mesh | Hình học phức tạp |

### Cách Sử Dụng Lớp Vật Lý

Không có lớp tùy chỉnh được cấu hình; sử dụng lớp mặc định (mọi thứ trong "Default").

## Quy Ước Hệ Thống Đầu Vào

### Tên Hành Động

Được định nghĩa trong Controls.inputactions:
- **Move** → InputValue.Get<Vector2>()
- **Aim** → InputValue.Get<Vector2>()
- **Fire** → Callback OnFire() (không cần giá trị)

### Ràng Buộc Đầu Vào

Bố cục tay cầm hai gậy:
- Gậy Trái → Chuyển Động
- Gậy Phải → Nhắm
- Kích Hoạt Phải → Bắn

Dự phòng bàn phím: WASD + Phím Mũi Tên + Thanh Cách.

### Kiểm Tra Trạng Thái Trò Chơi trong Đầu Vào

Callback đầu vào cổng trên trạng thái trò chơi:
```csharp
void OnMove(InputValue value) {
    if (GameManager.Instance.waitingForPlayers)
        move = Vector2.zero;
    else
        move = value.Get<Vector2>();
}
```

Ngăn chặn đầu vào trong thời gian chờ.

## Quy Ước Hệ Thống Âm Thanh

### Cấu Trúc Âm Thanh

Lớp dữ liệu Sound.cs:
```csharp
public string name;
public AudioClip clip;
public bool loop;
public float volume = 1f;
public float pitch = 1f;
public float volumeVariance = 0f;
public float pitchVariance = 0f;
public AudioSource source;
```

### Phát Âm Thanh

Âm thanh có tên, phân biệt chữ hoa và thường:
```csharp
AudioManager.instance.Play("Shoot");
AudioManager.instance.Play("Bounce");
AudioManager.instance.Play("BigExplode");
```

Tự động áp dụng phương sai ngẫu nhiên:
```csharp
s.source.volume = s.volume * (1f + Random.Range(-s.volumeVariance / 2f, s.volumeVariance / 2f));
```

## Quy Ước Hoạt Ảnh

### Tham Số Animator

Được sử dụng làm kích hoạt (không phải trạng thái):
```csharp
fader.SetTrigger("FadeOut");
fader.SetTrigger("FadeIn");
waitingForPlayersUI.SetTrigger("Stop");
```

Không có tham số boolean hoặc số nguyên trong triển khai hiện tại.

### Tệp Hoạt Ảnh

Được lưu trong Assets/Animation/:
- Ball.controller → Clip nảy/lơ lửng
- BulletIndicator.controller → Phản hồi UI lựu đạn
- Fader.controller → Chuyển đổi Fade
- WaitingUI.controller → Hoạt ảnh lời nhắc tham gia

## Tùy Chọn Loại

### Loại Số

- **Chuyển Động/Vật Lý:** float (Vector2, Vector3)
- **Bộ Đếm:** int (đạn, nảy, điểm)
- **Thời Gian:** float (Time.time, Time.deltaTime)

### Bộ Sưu Tập

Tránh Danh Sách chung nếu không cần. Sử dụng mảng hoặc đếm đơn giản:
```csharp
Sound[] sounds;  // Mảng kích thước cố định trong AudioManager
int bulletsReady = 0;  // Bộ đếm đơn giản
```

### Kiểm Tra Null

Kiểm tra null rõ ràng (không null-coalescing trong các đường dẫn quan trọng):
```csharp
if (playerA == null) { }
if (s == null) { Debug.LogWarning(...); }
```

## Xử Lý Lỗi

Xử lý lỗi tối thiểu (trò chơi giả định thiết lập chính xác):
```csharp
Sound s = Array.Find(sounds, item => item.name == sound);
if (s == null) {
    Debug.LogWarning("Sound: " + name + " not found!");
    return;
}
```

Ghi nhật ký chỉ cho các vấn đề về tài sản/cấu hình, không phải gameplay.

## Cân Nhắc Hiệu Suất

### Update vs FixedUpdate

- **Update():** Khảo sát đầu vào, logic không phải vật lý
- **FixedUpdate():** Chuyển động vật lý qua Rigidbody.MovePosition()

Ví Dụ (PlayerController):
```csharp
private void FixedUpdate() {
    rb.MovePosition(rb.position + moveDir * moveSpeed * Time.fixedDeltaTime);
}

void OnMove(InputValue value) {
    move = value.Get<Vector2>();
}
```

### Tham Chiếu Lưu Trữ

Tham chiếu được lưu trong Start/Awake:
```csharp
private void Start() {
    rb = GetComponent<Rigidbody>();
    weapon = GetComponent<PlayerWeapon>();
}
```

Không được lưu trong Update (mối quan tâm về hiệu suất).

### Khởi Tạo

Prefabs được khởi tạo chỉ cho đạn/hiệu ứng:
```csharp
GameObject ball = Instantiate(bulletPrefab, firePoint.position, firePoint.rotation);
GameObject explode = Instantiate(explosionPrefab, transform.position, Quaternion.identity);
```

Không được sử dụng cho các đối tượng được tái chế (đạn bị tiêu diệt sau khi nổ tung).

## Quy Ước Cảnh & Prefab

### Đặt Tên Prefab

- Prefab người chơi: `playerA`, `playerB`
- Đạn: `Bullet`
- Hiệu Ứng: `PlayerDeathFX`, hạt nổ tung
- Tường: Theo kích thước/loại (ví dụ: `BasicWall`, `SquareWall`)

### Cách Sử Dụng Thẻ

Thẻ bắt buộc (phân biệt chữ hoa và thường):
- "Player" — nhân vật người chơi
- "Bullet" — đạn nhân vật
- "Wall" — tường đấu trường
- "SpawnPointA", "SpawnPointB" — vị trí xuất hiện
- "FX" — hiệu ứng trực quan (tiêu diệt khi dỡ cấp độ)

## Quy Ước Kiểm Tra

Không có kiểm tra đơn vị trong dự án hiện tại. Nếu thêm, tuân theo:
- Định Nghĩa Lắp Ráp: `Tests/`
- Đặt Tên Tệp: `*Tests.cs`
- Phương Thức: `Test_FeatureName_ExpectedBehavior()`

## Tiêu Chuẩn Tài Liệu

### Bình Luận Nội Tuyến

Chỉ cho logic không rõ ràng. Ví Dụ:
```csharp
// Tránh lặp lại cấp độ giống nhau
int random = currentLevel;
while (random == currentLevel) {
    random = Random.Range(1, SceneManager.sceneCountInBuildSettings);
}
```

Không phải cho mỗi dòng mã.

### Bình Luận Cấp Lớp

Không cần cho lớp đơn giản. Các hệ thống phức tạp được ghi chép ở đầu:
```csharp
/// <summary>
/// Quản lý trạng thái trò chơi bền vững, sinh người chơi và chuyển đổi cấp độ.
/// Mẫu Singleton—truy cập qua GameManager.Instance.
/// </summary>
public class GameManager : MonoBehaviour { }
```

## Quy Ước Kiểm Soát Phiên Bản

### Thư từ Commit

Kiểu commit thông thường (nếu sử dụng git):
```
feat: add bullet ricochet system
fix: correct player spawn position
docs: update README with controls
```

### Tổ Chức Tệp

Không có thư mục con trong Assets/Scripts/. Tất cả kịch bản phẳng:
```
Assets/Scripts/
├── GameManager.cs
├── PlayerController.cs
├── PlayerWeapon.cs
├── Bullet.cs
├── AudioManager.cs
└── ...
```

---

**Cập Nhật Lần Cuối:** Tháng 4 năm 2026  
**Áp Dụng Cho:** Unity 6000.3.12f1, C# 12  
**Thực Hiện Qua:** Xem xét mã, kiểm tra thủ công
