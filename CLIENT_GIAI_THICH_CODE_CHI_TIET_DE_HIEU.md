# GIẢI THÍCH CODE CLIENT - BẢN SỬA LẠI CHI TIẾT

Nhóm: Lâm - Minh  
File chính: `Client/Form1.cs`

Tài liệu này chỉ giải thích phần code chính của Client: bàn cờ, đặt tàu, bắn tàu, hiệu ứng, âm thanh, Drone, Tàu dự phòng, timer lượt và giao tiếp Server. Không giải thích dài phần `using`, `Resources`, `Settings`, `AssemblyInfo`, vì mấy phần đó chỉ là phụ trợ hoặc Visual Studio tự sinh.

---

## 1. Hằng số chính của game

```csharp
const int BOARD_SIZE = 7;
const int BASE_SHIP_COUNT = 3;
const int TOTAL_SHIP_COUNT = 4;
const int SPARE_SHIP_ID = 3;
const int DRONE_SCAN_LENGTH = 4;
const int DRONE_BLINK_TOTAL_TIME = 1900;
const int SHIP_SPARE_CONFIRM_TIME = 1000;
const int SHOT_IMPACT_DELAY = 450;
const int SHOT_ANIMATION_STEPS = 14;
const int SHOT_ANIMATION_INTERVAL = 24;
const int TURN_TRANSITION_TOTAL_TIME = 1500;
const int TURN_TRANSITION_STEPS = 30;
const int TURN_COUNTDOWN_SECONDS = 15;
```

| Dòng code | Giải thích |
|---|---|
| `BOARD_SIZE = 7` | Quy định bàn cờ 7x7. Mọi chỗ tạo ô, kiểm tra biên, quét Drone đều dựa vào số này. |
| `BASE_SHIP_COUNT = 3` | Người chơi phải đặt 3 tàu ban đầu trước khi bấm sẵn sàng. |
| `TOTAL_SHIP_COUNT = 4` | Tính cả tàu dự phòng nên tổng tối đa là 4 tàu. |
| `SPARE_SHIP_ID = 3` | Tàu dự phòng dùng id 3 vì tàu ban đầu là 0, 1, 2. |
| `DRONE_SCAN_LENGTH = 4` | Drone chỉ dò 4 ô hàng ngang tính từ ô được thả vào. |
| `DRONE_BLINK_TOTAL_TIME = 1900` | Kết quả Drone nhấp nháy khoảng 1.9 giây. |
| `SHIP_SPARE_CONFIRM_TIME = 1000` | Hiệu ứng xác nhận tàu dự phòng kéo dài 1 giây. |
| `SHOT_IMPACT_DELAY = 450` | Khi bị bắn, Client chờ 450ms rồi mới xử lý hit/miss để game có nhịp hơn. |
| `SHOT_ANIMATION_STEPS = 14` | Ảnh hit/miss xuất hiện qua 14 bước nhỏ. |
| `SHOT_ANIMATION_INTERVAL = 24` | Mỗi bước cách nhau 24ms, tạo hiệu ứng rơi xuống/mờ rõ dần. |
| `TURN_TRANSITION_TOTAL_TIME = 1500` | Chữ chuyển lượt chạy trong 1.5 giây. |
| `TURN_TRANSITION_STEPS = 30` | 1.5 giây được chia 30 bước để animation mượt. |
| `TURN_COUNTDOWN_SECONDS = 15` | Mỗi lượt bắn có 15 giây, hết giờ thì gửi `SKIP_TURN`. |

---

## 2. Dữ liệu bàn cờ và tàu

```csharp
Label[,] ownCells = new Label[BOARD_SIZE, BOARD_SIZE];
Label[,] enemyCells = new Label[BOARD_SIZE, BOARD_SIZE];

bool[,] ownShips = new bool[BOARD_SIZE, BOARD_SIZE];
bool[,] ownHits = new bool[BOARD_SIZE, BOARD_SIZE];
bool[,] enemyShots = new bool[BOARD_SIZE, BOARD_SIZE];
bool[,] ownShotsReceived = new bool[BOARD_SIZE, BOARD_SIZE];

int[,] ownShipId = new int[BOARD_SIZE, BOARD_SIZE];

int[] shipSizes = new int[] { 2, 3, 4, 2 };
int[] shipHP = new int[] { 2, 3, 4, 2 };
bool[] shipPlaced = new bool[TOTAL_SHIP_COUNT];
```

| Dòng code | Giải thích |
|---|---|
| `ownCells` | Lưu 49 ô giao diện trên bàn của mình. Mỗi ô là một `Label`. |
| `enemyCells` | Lưu 49 ô giao diện trên bàn đối phương. Người chơi bấm vào đây để bắn. |
| `ownShips` | Ô nào có tàu thì giá trị là `true`. Đây là dữ liệu thật để xác định hit/miss. |
| `ownHits` | Ô tàu nào đã bị bắn trúng thì đánh dấu `true`, tránh trừ máu lại lần nữa. |
| `enemyShots` | Đánh dấu các ô trên bàn địch đã bắn rồi, tránh cho người chơi bắn trùng. |
| `ownShotsReceived` | Đánh dấu ô trên bàn mình đã bị đối thủ bắn. Tàu dự phòng không được đặt vào ô đã bị bắn. |
| `ownShipId` | Lưu id tàu tại từng ô. Nếu ô không có tàu thì là `-1`. |
| `shipSizes` | Độ dài từng tàu: 2, 3, 4 và tàu dự phòng 2 ô. |
| `shipHP` | Máu từng tàu. Khi bắn trúng một ô của tàu thì máu tàu giảm 1. |
| `shipPlaced` | Đánh dấu tàu đã đặt hay chưa. Dùng để kiểm tra đủ tàu trước khi READY. |

---

## 3. Hàm khởi tạo Client

```csharp
public Form1()
{
    InitializeComponent();

    InitializeBoardData();
    ConfigureDesignerUI();
    CreateBoards();
    CreateShipDock();
    SetupSkillMenu();
    HookClickSounds(this);
}
```

| Dòng code | Giải thích |
|---|---|
| `InitializeComponent();` | Tạo giao diện từ Designer: panel, label, nút, picturebox. Nếu thiếu dòng này thì Form mở ra trắng. |
| `InitializeBoardData();` | Khởi tạo dữ liệu bàn cờ, đặc biệt đưa `ownShipId` về `-1`. |
| `ConfigureDesignerUI();` | Cài đặt thêm giao diện: nền biển, nút, logo, avatar, khung trắng. |
| `CreateBoards();` | Tạo hai bàn cờ 7x7 bằng code. Không kéo thả từng ô vì sẽ quá nhiều control. |
| `CreateShipDock();` | Tạo khu vực tàu ban đầu để kéo thả. |
| `SetupSkillMenu();` | Tạo menu hai kỹ năng: Drone và Tàu dự phòng. |
| `HookClickSounds(this);` | Gắn âm click cho các control để có phản hồi âm thanh khi bấm. |

---

## 4. Điều kiện dùng kỹ năng

```csharp
private bool CanUseSkillBase()
{
    return connected && ready && myTurn && !gameEnded && !actionLocked && !turnTransitionRunning;
}
```

| Thành phần | Giải thích |
|---|---|
| `connected` | Phải kết nối Server rồi mới được dùng kỹ năng. |
| `ready` | Phải đặt tàu và bấm sẵn sàng. |
| `myTurn` | Chỉ dùng kỹ năng khi tới lượt mình. |
| `!gameEnded` | Trận chưa kết thúc. |
| `!actionLocked` | Không đang bị khóa do animation, đang bắn, hoặc đang chờ kết quả skill. |
| `!turnTransitionRunning` | Không được dùng skill trong lúc hiệu ứng chuyển lượt đang chạy. |

```csharp
private bool CanUseDroneSkill()
{
    return CanUseSkillBase() && !droneUsed;
}
```

Dòng này bắt Drone phải thỏa điều kiện chung và chưa dùng Drone trong trận.

```csharp
private bool CanUseShipSpareSkill()
{
    return CanUseSkillBase() && !shipSpareUsed;
}
```

Dòng này bắt Tàu dự phòng phải thỏa điều kiện chung và chưa dùng trong trận.

---

## 5. Kéo thả Drone

```csharp
private void picDroneSkill_MouseDown(object sender, MouseEventArgs e)
{
    if (e.Button != MouseButtons.Left)
    {
        return;
    }

    if (!CanUseDroneSkill())
    {
        lblStatus.Text = droneUsed ? "TRẠNG THÁI: DRONE ĐÃ DÙNG" : "TRẠNG THÁI: CHƯA THỂ DÙNG DRONE";
        return;
    }

    picDroneSkill.DoDragDrop("SKILL|DRONE", DragDropEffects.Copy);
}
```

| Dòng code | Giải thích |
|---|---|
| `if (e.Button != MouseButtons.Left)` | Chỉ cho kéo bằng chuột trái. |
| `return;` | Nếu không phải chuột trái thì thoát, không làm gì. |
| `if (!CanUseDroneSkill())` | Kiểm tra Drone có dùng được không. Nếu chưa tới lượt hoặc đã dùng rồi thì chặn. |
| `lblStatus.Text = ...` | Báo lý do không dùng được Drone. |
| `DoDragDrop("SKILL|DRONE", ...)` | Bắt đầu kéo thả. Chuỗi `SKILL|DRONE` là dữ liệu đi kèm để lúc thả xuống biết đây là Drone. |

---

## 6. Đọc kỹ năng đang được kéo

```csharp
private bool TryGetDraggedSkill(DragEventArgs e, out string skillName)
{
    skillName = "";
    object data = e.Data.GetData(DataFormats.Text);

    if (data == null)
    {
        data = e.Data.GetData(DataFormats.StringFormat);
    }

    if (data == null)
    {
        return false;
    }

    string text = data.ToString();

    if (!text.StartsWith("SKILL|"))
    {
        return false;
    }

    skillName = text.Replace("SKILL|", "").Trim().ToUpper();
    return skillName.Length > 0;
}
```

| Dòng code | Giải thích |
|---|---|
| `skillName = "";` | Gán mặc định rỗng để tránh biến output bị rác. |
| `GetData(DataFormats.Text)` | Lấy dữ liệu kéo thả dạng text. |
| `if (data == null)` | Nếu chưa lấy được dữ liệu thì thử kiểu khác. |
| `GetData(DataFormats.StringFormat)` | Lấy dữ liệu dạng chuỗi theo kiểu StringFormat. |
| `if (data == null) return false;` | Nếu không có dữ liệu thì không phải kéo skill. |
| `text.StartsWith("SKILL|")` | Chỉ nhận dữ liệu skill, bỏ qua dữ liệu kéo tàu hoặc dữ liệu khác. |
| `Replace("SKILL|", "")` | Bỏ phần tiền tố, lấy ra tên skill như `DRONE` hoặc `SPARE`. |
| `Trim().ToUpper()` | Xóa khoảng trắng và viết hoa để so sánh ổn định. |
| `return skillName.Length > 0;` | Có tên skill thì trả true. |

---

## 7. Dùng Drone

```csharp
private void TryUseDrone(int row, int col)
{
    if (!CanUseDroneSkill())
    {
        lblStatus.Text = "TRẠNG THÁI: CHƯA THỂ DÙNG DRONE";
        return;
    }

    if (!IsValidDroneStart(row, col))
    {
        MessageBox.Show("Drone cần đủ 4 ô ngang, hãy đặt từ cột 1 đến cột 4 của hàng cần dò.");
        return;
    }

    droneUsed = true;
    actionLocked = true;
    UpdateSkillMenuState();

    lblStatus.Text = "TRẠNG THÁI: DRONE ĐANG DÒ 4 Ô NGANG";
    SendLine("DRONE_SCAN|" + row + "|" + col);
}
```

| Dòng code | Giải thích |
|---|---|
| `if (!CanUseDroneSkill())` | Chặn nếu không đủ điều kiện dùng Drone. |
| `MessageBox.Show(...)` | Báo cho người chơi nếu thả Drone ở vị trí không đủ 4 ô ngang. |
| `droneUsed = true;` | Đánh dấu đã dùng Drone, không cho dùng lại. |
| `actionLocked = true;` | Khóa thao tác trong lúc chờ kết quả Drone từ Server. |
| `UpdateSkillMenuState();` | Làm icon Drone mờ đi sau khi dùng. |
| `lblStatus.Text = ...` | Báo Drone đang dò. |
| `SendLine("DRONE_SCAN|" + row + "|" + col);` | Gửi yêu cầu Drone lên Server. Server chuyển yêu cầu này sang Client đối thủ để kiểm tra tàu thật. |

---

## 8. Client bị Drone quét

```csharp
private void HandleDroneRequest(int row, int col)
{
    bool found = DoesDroneFindShip(row, col);
    SendLine("DRONE_RESULT|" + row + "|" + col + "|" + (found ? "1" : "0"));
}
```

| Dòng code | Giải thích |
|---|---|
| `DoesDroneFindShip(row, col)` | Kiểm tra thật trên bàn mình xem 4 ô ngang có tàu không. |
| `found ? "1" : "0"` | Nếu có tàu thì gửi 1, không có tàu thì gửi 0. |
| `SendLine("DRONE_RESULT|...")` | Gửi kết quả Drone về Server để Server trả cho người dùng Drone. |

```csharp
private bool DoesDroneFindShip(int row, int col)
{
    if (!IsValidDroneStart(row, col))
    {
        return false;
    }

    for (int i = 0; i < DRONE_SCAN_LENGTH; i++)
    {
        if (ownShips[row, col + i])
        {
            return true;
        }
    }

    return false;
}
```

| Dòng code | Giải thích |
|---|---|
| `IsValidDroneStart(row, col)` | Kiểm tra vùng 4 ô có nằm trong bàn cờ không. |
| `for (int i = 0; i < DRONE_SCAN_LENGTH; i++)` | Duyệt 4 ô ngang. |
| `ownShips[row, col + i]` | Kiểm tra ô thứ i trong vùng Drone có tàu không. |
| `return true;` | Gặp ít nhất một ô có tàu thì báo Drone tìm thấy. |
| `return false;` | Duyệt hết 4 ô không thấy tàu thì báo không tìm thấy. |

---

## 9. Hiển thị kết quả Drone

```csharp
private void HandleDroneResult(int row, int col, bool found)
{
    Color blinkColor = found ? Color.LimeGreen : Color.Red;
    lblStatus.Text = found ? "TRẠNG THÁI: DRONE PHÁT HIỆN TÍN HIỆU TÀU" : "TRẠNG THÁI: DRONE KHÔNG PHÁT HIỆN TÀU";

    BlinkHorizontalCells(enemyCells, row, col, DRONE_SCAN_LENGTH, blinkColor, DRONE_BLINK_TOTAL_TIME, "drone.wav", () =>
    {
        actionLocked = false;
        UpdateSkillMenuState();
    });
}
```

| Dòng code | Giải thích |
|---|---|
| `found ? Color.LimeGreen : Color.Red` | Có tàu thì xanh lá, không có tàu thì đỏ. |
| `lblStatus.Text = ...` | Hiện kết quả bằng chữ. |
| `BlinkHorizontalCells(...)` | Nhấp nháy 4 ô hàng ngang trên bàn đối phương. |
| `"drone.wav"` | Phát âm thanh Drone khi nhấp nháy. |
| `actionLocked = false;` | Mở khóa thao tác sau khi hiệu ứng Drone xong. |
| `UpdateSkillMenuState();` | Cập nhật lại menu skill sau khi mở khóa. |

---

## 10. Dùng Tàu dự phòng

```csharp
private void TryUseShipSpare(int row, int col)
{
    if (!CanUseShipSpareSkill())
    {
        lblStatus.Text = "TRẠNG THÁI: CHƯA THỂ DÙNG TÀU DỰ PHÒNG";
        return;
    }

    if (!CanPlaceSpareShip(row, col, 2, isHorizontal))
    {
        MessageBox.Show("Không đặt được tàu dự phòng ở vị trí này. Tàu cần 2 ô trống và chưa từng bị đối thủ bắn.");
        return;
    }

    shipSpareUsed = true;
    actionLocked = true;
    shipPlaced[SPARE_SHIP_ID] = true;
    shipHP[SPARE_SHIP_ID] = 2;

    ApplyShipPlacement(row, col, 2, isHorizontal, SPARE_SHIP_ID);
    SendLine("SPARE_PLACED|2");
    UpdateSkillMenuState();

    lblStatus.Text = "TRẠNG THÁI: ĐÃ TRIỆU HỒI TÀU DỰ PHÒNG";
}
```

| Dòng code | Giải thích |
|---|---|
| `CanUseShipSpareSkill()` | Kiểm tra có đủ điều kiện dùng skill không. |
| `CanPlaceSpareShip(row, col, 2, isHorizontal)` | Kiểm tra tàu 2 ô có đặt được tại vị trí đang thả không. |
| `shipSpareUsed = true;` | Đánh dấu đã dùng tàu dự phòng. |
| `actionLocked = true;` | Khóa thao tác trong lúc đặt và chạy hiệu ứng. |
| `shipPlaced[SPARE_SHIP_ID] = true;` | Đánh dấu tàu dự phòng đã được đặt. |
| `shipHP[SPARE_SHIP_ID] = 2;` | Tàu dự phòng dài 2 ô nên có 2 máu. |
| `ApplyShipPlacement(...)` | Ghi tàu vào mảng `ownShips`, `ownShipId`, đồng thời vẽ ảnh tàu lên bàn. |
| `SendLine("SPARE_PLACED|2");` | Báo Server rằng người chơi đã đặt thêm tàu 2 ô. Server sẽ tăng tổng ô tàu cần bắn chìm. |
| `UpdateSkillMenuState();` | Làm icon tàu dự phòng mờ đi. |
| `lblStatus.Text = ...` | Báo đặt tàu dự phòng thành công. |

---

## 11. Kiểm tra vị trí đặt Tàu dự phòng

```csharp
private bool CanPlaceSpareShip(int row, int col, int length, bool horizontal)
{
    for (int i = 0; i < length; i++)
    {
        int r = row;
        int c = col;

        if (horizontal)
        {
            c = col + i;
        }
        else
        {
            r = row + i;
        }

        if (r < 0 || r >= BOARD_SIZE || c < 0 || c >= BOARD_SIZE)
        {
            return false;
        }

        if (ownShips[r, c] || ownShotsReceived[r, c])
        {
            return false;
        }
    }

    return true;
}
```

| Dòng code | Giải thích |
|---|---|
| `for (int i = 0; i < length; i++)` | Duyệt từng ô mà tàu dự phòng sẽ chiếm. |
| `int r = row; int c = col;` | Ban đầu ô kiểm tra là ô được thả vào. |
| `if (horizontal) c = col + i;` | Nếu đặt ngang thì cột tăng dần. |
| `else r = row + i;` | Nếu đặt dọc thì dòng tăng dần. |
| `r < 0 || r >= BOARD_SIZE ...` | Nếu ô vượt khỏi bàn cờ thì không cho đặt. |
| `ownShips[r, c]` | Nếu ô đã có tàu cũ thì không cho đặt chồng. |
| `ownShotsReceived[r, c]` | Nếu ô đã bị đối phương bắn rồi thì không cho đặt tàu vào đó. |
| `return true;` | Tất cả ô hợp lệ thì cho đặt. |

---

## 12. Bắn vào bàn đối phương

```csharp
private void EnemyCell_Click(object sender, EventArgs e)
{
    if (!connected)
    {
        MessageBox.Show("Chưa kết nối máy chủ.");
        return;
    }

    if (!ready)
    {
        MessageBox.Show("Bạn phải đặt đủ tàu và nhấn sẵn sàng trước.");
        return;
    }

    if (gameEnded)
    {
        MessageBox.Show("Trận đấu đã kết thúc.");
        return;
    }

    if (actionLocked || turnTransitionRunning)
    {
        lblStatus.Text = "TRẠNG THÁI: ĐANG CHUYỂN LƯỢT";
        return;
    }

    if (!myTurn)
    {
        MessageBox.Show("Chưa đến lượt bạn.");
        return;
    }

    Label cell = sender as Label;
    Point p = (Point)cell.Tag;

    int row = p.X;
    int col = p.Y;

    if (enemyShots[row, col])
    {
        MessageBox.Show("Bạn đã bắn ô này rồi.");
        return;
    }

    enemyShots[row, col] = true;
    StopTurnCountdown();
    myTurn = false;
    UpdateSkillMenuState();

    SendLine("FIRE|" + row + "|" + col);

    lblTurn.Text = "LƯỢT: CHỜ";
    lblStatus.Text = "TRẠNG THÁI: ĐÃ BẮN";
}
```

| Dòng code | Giải thích |
|---|---|
| `if (!connected)` | Chưa kết nối Server thì không thể bắn. |
| `if (!ready)` | Chưa đặt đủ tàu/sẵn sàng thì không cho bắn. |
| `if (gameEnded)` | Trận đã kết thúc thì chặn bắn. |
| `if (actionLocked || turnTransitionRunning)` | Đang animation hoặc đang chuyển lượt thì không cho bắn. |
| `if (!myTurn)` | Chưa tới lượt thì không cho bắn. |
| `Label cell = sender as Label;` | Lấy ô vừa được click. |
| `Point p = (Point)cell.Tag;` | Lấy tọa độ row/col đã lưu trong Tag của ô. |
| `enemyShots[row, col]` | Kiểm tra ô này đã bắn chưa. |
| `enemyShots[row, col] = true;` | Đánh dấu đã bắn để không bắn lại. |
| `StopTurnCountdown();` | Bắn rồi thì dừng timer 15 giây. |
| `myTurn = false;` | Sau khi gửi lệnh bắn thì tạm thời không còn lượt. Server sẽ quyết định lượt tiếp theo. |
| `SendLine("FIRE|" + row + "|" + col);` | Gửi lệnh bắn lên Server. Server sẽ chuyển sang Client đối thủ. |
| `lblTurn.Text = "LƯỢT: CHỜ";` | Cập nhật khung lượt trong lúc chờ kết quả. |
| `lblStatus.Text = "TRẠNG THÁI: ĐÃ BẮN";` | Báo cho người chơi biết lệnh bắn đã gửi. |

---

## 13. Nhận lệnh từ Server

```csharp
private void ReceiveLoop()
{
    try
    {
        while (connected)
        {
            string msg = reader.ReadLine();

            if (msg == null)
            {
                break;
            }

            SafeInvoke(() => ProcessMessage(msg));
        }
    }
    catch
    {

    }

    SafeInvoke(() => MarkDisconnected());
}
```

| Dòng code | Giải thích |
|---|---|
| `while (connected)` | Còn kết nối thì tiếp tục đọc dữ liệu từ Server. |
| `reader.ReadLine()` | Đọc một dòng lệnh Server gửi về, ví dụ `TURN|1`, `FIRE_RESULT|HIT|2|3`. |
| `if (msg == null)` | Nếu đọc được null thì kết nối đã đóng. |
| `SafeInvoke(() => ProcessMessage(msg))` | Đưa việc xử lý tin nhắn về UI thread. WinForms không cho thread nền sửa giao diện trực tiếp. |
| `catch { }` | Nếu đọc mạng bị lỗi thì không làm chương trình crash. |
| `MarkDisconnected()` | Khi vòng đọc kết thúc thì đưa Client về trạng thái mất kết nối. |

---

## 14. Xử lý lệnh Server gửi về

```csharp
else if (parts[0] == "TURN")
{
    if (parts.Length >= 2)
    {
        int turnPlayer = int.Parse(parts[1]);
        StartTurnTransition(turnPlayer == playerNumber);
    }
}
```

| Dòng code | Giải thích |
|---|---|
| `parts[0] == "TURN"` | Server báo lượt hiện tại thuộc người chơi nào. |
| `int turnPlayer = int.Parse(parts[1]);` | Lấy số người chơi đang có lượt. |
| `StartTurnTransition(turnPlayer == playerNumber);` | Nếu lượt đó trùng với mình thì chạy hiệu ứng “LƯỢT CỦA BẠN”, nếu không thì “LƯỢT ĐỐI THỦ”. |

```csharp
else if (parts[0] == "FIRE_RESULT")
{
    string result = parts[1];
    int row = int.Parse(parts[2]);
    int col = int.Parse(parts[3]);

    if (result == "HIT")
    {
        DrawPin(panelEnemyBoard, row, col, true);
        lblStatus.Text = "TRẠNG THÁI: BẮN TRÚNG";
    }
    else
    {
        DrawPin(panelEnemyBoard, row, col, false);
        lblStatus.Text = "TRẠNG THÁI: BẮN HỤT";
    }
}
```

| Dòng code | Giải thích |
|---|---|
| `parts[0] == "FIRE_RESULT"` | Server trả kết quả phát bắn của mình. |
| `result = parts[1]` | Kết quả là `HIT` hoặc `MISS`. |
| `row`, `col` | Vị trí mình đã bắn. |
| `DrawPin(..., true)` | Vẽ ảnh hit lên bàn đối phương. |
| `DrawPin(..., false)` | Vẽ ảnh miss lên bàn đối phương. |
| `lblStatus.Text` | Cập nhật trạng thái bắn trúng hoặc bắn hụt. |

```csharp
else if (parts[0] == "DRONE_REQUEST")
{
    int row = int.Parse(parts[1]);
    int col = int.Parse(parts[2]);
    HandleDroneRequest(row, col);
}
```

Server chuyển yêu cầu Drone của đối thủ sang Client này. Client này sẽ kiểm tra bàn thật của mình rồi gửi lại `DRONE_RESULT`.

```csharp
else if (parts[0] == "DRONE_RESULT")
{
    int row = int.Parse(parts[1]);
    int col = int.Parse(parts[2]);
    bool found = parts[3] == "1";
    HandleDroneResult(row, col, found);
}
```

Đây là lúc Client dùng Drone nhận kết quả. Nếu `parts[3]` là `1` thì vùng Drone có tàu, nếu là `0` thì không có tàu.

```csharp
else if (parts[0] == "WIN")
{
    PlaySound("win.wav");
    ShowEndGameOptions("Bạn thắng!");
}
else if (parts[0] == "LOSE")
{
    PlaySound("lose.wav");
    ShowEndGameOptions("Bạn thua!");
}
```

Server báo thắng/thua. Client phát âm thanh tương ứng rồi hiện màn hình kết thúc.

---

## 15. Xử lý khi đối thủ bắn vào mình

```csharp
private void HandleEnemyFire(int row, int col)
{
    actionLocked = true;
    lblStatus.Text = "TRẠNG THÁI: ĐỐI THỦ ĐANG BẮN";

    System.Windows.Forms.Timer impactTimer = new System.Windows.Forms.Timer();
    impactTimer.Interval = SHOT_IMPACT_DELAY;
    impactTimer.Tick += (sender, e) =>
    {
        impactTimer.Stop();
        impactTimer.Dispose();
        ResolveEnemyFire(row, col);
    };
    impactTimer.Start();
}
```

| Dòng code | Giải thích |
|---|---|
| `actionLocked = true;` | Khóa thao tác trong lúc đang xử lý phát bắn của đối thủ. |
| `lblStatus.Text = ...` | Báo đối thủ đang bắn. |
| `new Timer()` | Tạo timer để delay trước khi hiện kết quả. |
| `Interval = SHOT_IMPACT_DELAY` | Chờ 450ms. |
| `impactTimer.Tick += ...` | Khi timer hết thời gian, chạy đoạn bên trong. |
| `Stop()` và `Dispose()` | Dừng và hủy timer vì chỉ cần chạy một lần. |
| `ResolveEnemyFire(row, col)` | Sau delay mới xử lý thật sự ô đó trúng hay trượt. |
| `impactTimer.Start();` | Bắt đầu chạy timer. |

```csharp
private void ResolveEnemyFire(int row, int col)
{
    bool hit = false;
    int hitShipId = -1;

    ownShotsReceived[row, col] = true;

    if (ownShips[row, col] && !ownHits[row, col])
    {
        hit = true;
        ownHits[row, col] = true;
        hitShipId = ownShipId[row, col];

        if (hitShipId >= 0)
        {
            shipHP[hitShipId]--;
        }

        DrawPin(panelOwnBoard, row, col, true);
    }
    else
    {
        DrawPin(panelOwnBoard, row, col, false);
    }

    if (hit)
    {
        SendLine("FIRE_RESULT|HIT|" + row + "|" + col);

        if (hitShipId >= 0 && shipHP[hitShipId] == 0)
        {
            int horizontalValue = shipIsHorizontal[hitShipId] ? 1 : 0;
            SendLine("SUNK|" + hitShipId + "|" + shipStartRow[hitShipId] + "|" + shipStartCol[hitShipId] + "|" + horizontalValue);
            PlaySound("ship_sunk.wav");
        }
    }
    else
    {
        SendLine("FIRE_RESULT|MISS|" + row + "|" + col);
    }

    actionLocked = false;
    UpdateSkillMenuState();
}
```

| Dòng code | Giải thích |
|---|---|
| `bool hit = false;` | Ban đầu giả sử bắn trượt. Nếu kiểm tra thấy có tàu thì đổi thành true. |
| `hitShipId = -1;` | Mặc định chưa biết trúng tàu nào. |
| `ownShotsReceived[row, col] = true;` | Đánh dấu ô này đã bị bắn, để tàu dự phòng không đặt lên đây. |
| `ownShips[row, col] && !ownHits[row, col]` | Chỉ tính là hit nếu ô có tàu và chưa từng bị bắn trúng trước đó. |
| `ownHits[row, col] = true;` | Ghi nhận ô tàu đã bị phá. |
| `hitShipId = ownShipId[row, col];` | Lấy id tàu bị trúng để trừ máu đúng tàu. |
| `shipHP[hitShipId]--;` | Trừ 1 máu của tàu. |
| `DrawPin(panelOwnBoard, ..., true)` | Vẽ ảnh hit trên bàn mình. |
| `DrawPin(..., false)` | Nếu trượt thì vẽ ảnh miss. |
| `SendLine("FIRE_RESULT|HIT|...")` | Báo Server phát bắn là trúng. |
| `shipHP[hitShipId] == 0` | Nếu máu tàu về 0 thì tàu chìm. |
| `horizontalValue` | Gửi hướng tàu cho đối thủ để vẽ cả con tàu bị chìm lên bàn đối phương. |
| `SendLine("SUNK|...")` | Báo tàu đã chìm. |
| `SendLine("FIRE_RESULT|MISS|...")` | Nếu không trúng thì báo miss. |
| `actionLocked = false;` | Xử lý xong thì mở lại thao tác. |

---

## 16. Hiệu ứng chuyển lượt và đếm ngược

```csharp
private void StartTurnTransition(bool isMyTurnNow)
{
    if (gameEnded)
    {
        return;
    }

    StopTurnCountdown();

    actionLocked = true;
    turnTransitionRunning = true;
    myTurn = false;

    lblTurn.Text = "ĐANG CHUYỂN LƯỢT";
    lblStatus.Text = isMyTurnNow ? "TRẠNG THÁI: SẮP ĐẾN LƯỢT BẠN" : "TRẠNG THÁI: SẮP ĐẾN LƯỢT ĐỐI THỦ";
}
```

| Dòng code | Giải thích |
|---|---|
| `if (gameEnded)` | Nếu game kết thúc thì không chạy chuyển lượt nữa. |
| `StopTurnCountdown();` | Dừng timer cũ trước khi chuyển lượt. |
| `actionLocked = true;` | Khóa bắn/kỹ năng trong lúc hiệu ứng chạy. |
| `turnTransitionRunning = true;` | Đánh dấu đang có animation chuyển lượt. |
| `myTurn = false;` | Tạm thời chưa ai được bắn cho đến khi animation xong. |
| `lblTurn.Text = ...` | Hiện trạng thái đang chuyển lượt. |
| `lblStatus.Text = isMyTurnNow ? ...` | Nếu sắp tới lượt mình thì báo “sắp đến lượt bạn”, ngược lại báo “sắp đến lượt đối thủ”. |

```csharp
private void StartTurnCountdown()
{
    StopTurnCountdown();

    if (!CanUseSkillBase())
    {
        return;
    }

    turnSecondsLeft = TURN_COUNTDOWN_SECONDS;
    lblTurn.Text = "LƯỢT: BẠN - " + turnSecondsLeft + "s";

    turnCountdownTimer = new System.Windows.Forms.Timer();
    turnCountdownTimer.Interval = 1000;
    turnCountdownTimer.Tick += (sender, e) =>
    {
        turnSecondsLeft--;

        if (turnSecondsLeft > 0)
        {
            lblTurn.Text = "LƯỢT: BẠN - " + turnSecondsLeft + "s";
            return;
        }

        StopTurnCountdown();
        HandleTurnTimeout();
    };
    turnCountdownTimer.Start();
}
```

| Dòng code | Giải thích |
|---|---|
| `StopTurnCountdown();` | Đảm bảo không có timer cũ chạy song song. |
| `if (!CanUseSkillBase()) return;` | Nếu chưa đủ điều kiện thao tác thì không đếm ngược. |
| `turnSecondsLeft = 15;` | Reset thời gian lượt về 15 giây. |
| `lblTurn.Text = ...` | Hiển thị số giây còn lại. |
| `new Timer()` | Tạo timer WinForms. |
| `Interval = 1000` | Mỗi 1 giây timer chạy một lần. |
| `turnSecondsLeft--;` | Mỗi giây trừ 1. |
| `if (turnSecondsLeft > 0)` | Nếu vẫn còn thời gian thì chỉ cập nhật chữ rồi chờ tiếp. |
| `HandleTurnTimeout();` | Nếu hết giờ thì xử lý mất lượt. |

```csharp
private void HandleTurnTimeout()
{
    if (!connected || !ready || gameEnded || !myTurn)
    {
        return;
    }

    myTurn = false;
    actionLocked = true;
    UpdateSkillMenuState();
    lblTurn.Text = "LƯỢT: HẾT GIỜ";
    lblStatus.Text = "TRẠNG THÁI: HẾT 15 GIÂY, MẤT LƯỢT";
    SendLine("SKIP_TURN");
}
```

| Dòng code | Giải thích |
|---|---|
| `if (!connected || !ready || gameEnded || !myTurn)` | Chỉ xử lý hết giờ nếu Client đang trong trận và đang tới lượt mình. |
| `myTurn = false;` | Hết giờ thì không còn lượt bắn. |
| `actionLocked = true;` | Khóa thao tác để tránh bắn sau khi hết giờ. |
| `UpdateSkillMenuState();` | Tắt kỹ năng vì không còn lượt. |
| `lblTurn.Text = "LƯỢT: HẾT GIỜ";` | Báo rõ người chơi bị hết giờ. |
| `SendLine("SKIP_TURN");` | Gửi lên Server để Server chuyển lượt cho đối thủ. |

---

## 17. Âm thanh và hiệu ứng hit/miss

```csharp
private void PlaySound(string fileName)
{
    try
    {
        string soundPath = FindSoundFile(fileName);

        if (soundPath == null)
        {
            return;
        }

        SoundPlayer player = new SoundPlayer(soundPath);
        player.Play();
    }
    catch
    {

    }
}
```

| Dòng code | Giải thích |
|---|---|
| `FindSoundFile(fileName)` | Tìm file âm thanh trong thư mục chạy hoặc thư mục `Sounds`. |
| `if (soundPath == null) return;` | Nếu không tìm thấy file thì bỏ qua, tránh crash game. |
| `new SoundPlayer(soundPath)` | Tạo đối tượng phát file `.wav`. |
| `player.Play();` | Phát âm thanh không chặn chương trình. Game vẫn chạy tiếp. |
| `catch { }` | Nếu file lỗi hoặc máy không phát được thì bỏ qua để game không bị tắt. |

```csharp
private void AnimateShotPictureBox(PictureBox pictureBox, Image originalImage, int finalLeft, int finalTop)
{
    int currentStep = 1;
    int startTop = pictureBox.Top;
    int startLeft = pictureBox.Left;

    System.Windows.Forms.Timer animationTimer = new System.Windows.Forms.Timer();
    animationTimer.Interval = SHOT_ANIMATION_INTERVAL;
}
```

| Dòng code | Giải thích |
|---|---|
| `currentStep = 1` | Bắt đầu animation từ bước đầu tiên. |
| `startTop`, `startLeft` | Lưu vị trí ban đầu của ảnh hit/miss để tính đường di chuyển. |
| `animationTimer` | Timer điều khiển từng bước animation. |
| `Interval = SHOT_ANIMATION_INTERVAL` | Mỗi 24ms ảnh cập nhật vị trí và độ mờ một lần. |

Trong phần Tick của timer:

| Dòng xử lý | Giải thích |
|---|---|
| `currentStep++` | Tăng bước animation. |
| `t = currentStep / SHOT_ANIMATION_STEPS` | Tính phần trăm hoàn thành animation. |
| `eased = 1.0 - Math.Pow(1.0 - t, 3.0)` | Làm chuyển động nhanh dần rồi chậm lại, nhìn tự nhiên hơn di chuyển đều. |
| `opacity = (float)t` | Càng về cuối ảnh càng rõ. |
| `CreateOpacityImage(originalImage, opacity)` | Tạo ảnh mới theo độ mờ hiện tại. |
| `pictureBox.Left/Top = ...` | Di chuyển ảnh từ vị trí ban đầu tới vị trí cuối. |
| `animationTimer.Stop()` | Khi đủ bước thì dừng animation. |

---

## 18. Màn hình thắng thua

```csharp
private void ShowEndGameOptions(string result)
{
    gameEnded = true;
    myTurn = false;
    actionLocked = true;
    turnTransitionRunning = false;
    StopTurnCountdown();

    lblTurn.Text = "KẾT THÚC";
    lblStatus.Text = "TRẠNG THÁI: " + result.ToUpper();

    DisableAllBoardCells();

    bool isWin = result.ToLower().Contains("thắng");
    ShowEndGameOverlay(isWin);
}
```

| Dòng code | Giải thích |
|---|---|
| `gameEnded = true;` | Đánh dấu trận đã kết thúc. |
| `myTurn = false;` | Không còn lượt bắn nữa. |
| `actionLocked = true;` | Khóa thao tác. |
| `StopTurnCountdown();` | Dừng timer lượt nếu đang chạy. |
| `lblTurn.Text = "KẾT THÚC";` | Đổi khung lượt sang kết thúc. |
| `DisableAllBoardCells();` | Khóa toàn bộ ô bàn cờ. |
| `isWin = result.ToLower().Contains("thắng")` | Kiểm tra kết quả là thắng hay thua. |
| `ShowEndGameOverlay(isWin)` | Hiện màn hình kết thúc màu vàng nếu thắng, xám nếu thua. |

---

## 19. Gửi dữ liệu lên Server

```csharp
private void SendLine(string msg)
{
    try
    {
        if (writer != null)
        {
            writer.WriteLine(msg);
        }
    }
    catch
    {
        MarkDisconnected();
    }
}
```

| Dòng code | Giải thích |
|---|---|
| `if (writer != null)` | Chỉ gửi khi đã có luồng ghi dữ liệu tới Server. |
| `writer.WriteLine(msg);` | Gửi một dòng lệnh qua TCP. Ví dụ `FIRE|2|3`, `READY`, `SKIP_TURN`. |
| `catch` | Nếu gửi lỗi do mất kết nối thì chuyển Client sang trạng thái disconnected. |

---

## 20. Tóm tắt luồng Client

1. Mở Form.
2. Tạo bàn cờ, tàu, skill, giao diện.
3. Người chơi kết nối Server.
4. Đặt tàu và bấm sẵn sàng.
5. Nhận lệnh `TURN` từ Server.
6. Nếu tới lượt mình thì chạy chuyển lượt, bật timer 15 giây.
7. Người chơi bắn hoặc dùng skill.
8. Client gửi `FIRE`, `DRONE_SCAN`, `SPARE_PLACED`, `SKIP_TURN` lên Server.
9. Client nhận kết quả từ Server rồi cập nhật giao diện.
10. Khi nhận `WIN` hoặc `LOSE` thì hiện màn hình kết thúc.
