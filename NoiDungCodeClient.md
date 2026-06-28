# CLIENT - GIẢI THÍCH CODE CHÍNH THEO TỪNG DÒNG

Nhóm: **Lâm - Minh**

Tài liệu này dùng để đưa lên GitHub làm phần giải thích code. Nội dung tập trung vào **code chính** của đồ án Battleship: giao diện tự viết trong runtime, logic đặt tàu/bắn tàu, hiệu ứng, âm thanh, kỹ năng Drone/Tàu dự phòng và giao tiếp Client-Server.

Các phần phụ như `using`, comment, dấu ngoặc nhọn, `AssemblyInfo`, `Resources.Designer`, `Settings.Designer` chỉ được giải thích sơ vì đa số là code tự sinh hoặc metadata, không phải logic đồ án.


## File `Program.cs` — điểm khởi động Client

File này khá ngắn, chỉ dùng để chạy form chính. Giải thích sơ các dòng quan trọng:

### Dòng 9
```csharp
    internal static class Program
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại.

### Dòng 14
```csharp
        [STAThread]
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại.

### Dòng 15
```csharp
        static void Main()
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại.

### Dòng 17
```csharp
            Application.EnableVisualStyles();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket.

### Dòng 18
```csharp
            Application.SetCompatibleTextRenderingDefault(false);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket.

### Dòng 19
```csharp
            Application.Run(new Form1());
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket.


---

## Phần GUI tự sinh trong `Form1.Designer.cs`

Phần này do Visual Studio Designer sinh ra. Không cần giải thích từng dòng `Location`, `Size`, `Font` vì đó chỉ là vị trí/kích thước. Các control quan trọng cần biết:

| Dòng | Control | Loại | Công dụng |
|---:|---|---|---|
| 31 | `lblIPCaption` | `Label` | Control giao diện hỗ trợ form hoạt động. |
| 32 | `txtIP` | `TextBox` | TextBox nhập IP Server hoặc IP lắng nghe. |
| 33 | `lblNameCaption` | `Label` | Control giao diện hỗ trợ form hoạt động. |
| 34 | `txtName` | `TextBox` | TextBox nhập tên người chơi phía Client. |
| 35 | `lblPortCaption` | `Label` | Control giao diện hỗ trợ form hoạt động. |
| 36 | `txtPort` | `TextBox` | TextBox nhập cổng TCP. |
| 37 | `btnConnect` | `Button` | nút Client kết nối đến Server. |
| 38 | `btnReady` | `Button` | nút gửi READY sau khi đặt đủ tàu. |
| 39 | `btnRotate` | `Button` | nút xoay hướng đặt tàu ngang/dọc. |
| 40 | `btnPlayAgain` | `Button` | nút chơi lại sau khi hết trận. |
| 41 | `btnExitGame` | `Button` | nút thoát game. |
| 42 | `lblPlayer` | `Label` | Label hiển thị người chơi số mấy. |
| 43 | `lblOwnTitle` | `Label` | Control giao diện hỗ trợ form hoạt động. |
| 44 | `lblEnemyTitle` | `Label` | Control giao diện hỗ trợ form hoạt động. |
| 45 | `lblTurn` | `Label` | Label hiển thị lượt hiện tại. |
| 46 | `lblStatus` | `Label` | Label hiển thị trạng thái game. |
| 47 | `panelOwnBoard` | `Panel` | Panel chứa bàn cờ bản thân. |
| 48 | `panelEnemyBoard` | `Panel` | Panel chứa bàn cờ đối phương. |
| 49 | `panelShipDock` | `Panel` | Panel chứa ảnh tàu để kéo thả. |
| 50 | `picLogo` | `PictureBox` | PictureBox hiển thị logo. |
| 51 | `picOwnAvatar` | `PictureBox` | PictureBox avatar bản thân. |
| 52 | `picEnemyAvatar` | `PictureBox` | PictureBox avatar đối thủ. |
| 53 | `panelSkillMenu` | `Panel` | Panel menu kỹ năng góc trái. |
| 54 | `picDroneSkill` | `PictureBox` | PictureBox icon kỹ năng Drone. |
| 55 | `picShipSpareSkill` | `PictureBox` | PictureBox icon kỹ năng Tàu dự phòng. |
| 56 | `lblTurnBanner` | `Label` | Label chữ lớn giữa màn hình khi chuyển lượt. |

Các dòng event quan trọng trong Designer:

| Dòng | Code | Ý nghĩa |
|---:|---|---|
| 139 | `this.btnConnect.Click += new System.EventHandler(this.btnConnect_Click);` | Gắn control với hàm xử lý trong `Form1.cs`; khi người dùng thao tác thì logic chính được gọi. |
| 149 | `this.btnReady.Click += new System.EventHandler(this.btnReady_Click);` | Gắn control với hàm xử lý trong `Form1.cs`; khi người dùng thao tác thì logic chính được gọi. |
| 159 | `this.btnRotate.Click += new System.EventHandler(this.btnRotate_Click);` | Gắn control với hàm xử lý trong `Form1.cs`; khi người dùng thao tác thì logic chính được gọi. |
| 170 | `this.btnPlayAgain.Click += new System.EventHandler(this.btnPlayAgain_Click);` | Gắn control với hàm xử lý trong `Form1.cs`; khi người dùng thao tác thì logic chính được gọi. |
| 181 | `this.btnExitGame.Click += new System.EventHandler(this.btnExitGame_Click);` | Gắn control với hàm xử lý trong `Form1.cs`; khi người dùng thao tác thì logic chính được gọi. |
| 253 | `this.panelOwnBoard.DragDrop += new System.Windows.Forms.DragEventHandler(this.OwnBoard_DragDrop);` | Gắn control với hàm xử lý trong `Form1.cs`; khi người dùng thao tác thì logic chính được gọi. |
| 254 | `this.panelOwnBoard.DragEnter += new System.Windows.Forms.DragEventHandler(this.Board_DragEnter);` | Gắn control với hàm xử lý trong `Form1.cs`; khi người dùng thao tác thì logic chính được gọi. |
| 255 | `this.panelOwnBoard.Paint += new System.Windows.Forms.PaintEventHandler(this.DrawPanelBorder);` | Gắn control với hàm xử lý trong `Form1.cs`; khi người dùng thao tác thì logic chính được gọi. |
| 264 | `this.panelEnemyBoard.Paint += new System.Windows.Forms.PaintEventHandler(this.DrawPanelBorder);` | Gắn control với hàm xử lý trong `Form1.cs`; khi người dùng thao tác thì logic chính được gọi. |
| 273 | `this.panelShipDock.Paint += new System.Windows.Forms.PaintEventHandler(this.DrawPanelBorder);` | Gắn control với hàm xử lý trong `Form1.cs`; khi người dùng thao tác thì logic chính được gọi. |
| 315 | `this.panelSkillMenu.Paint += new System.Windows.Forms.PaintEventHandler(this.DrawPanelBorder);` | Gắn control với hàm xử lý trong `Form1.cs`; khi người dùng thao tác thì logic chính được gọi. |
| 328 | `this.picDroneSkill.MouseDown += new System.Windows.Forms.MouseEventHandler(this.picDroneSkill_MouseDown);` | Gắn control với hàm xử lý trong `Form1.cs`; khi người dùng thao tác thì logic chính được gọi. |
| 341 | `this.picShipSpareSkill.MouseDown += new System.Windows.Forms.MouseEventHandler(this.picShipSpareSkill_MouseDown);` | Gắn control với hàm xử lý trong `Form1.cs`; khi người dùng thao tác thì logic chính được gọi. |
| 391 | `this.FormClosing += new System.Windows.Forms.FormClosingEventHandler(this.Form1_FormClosing);` | Gắn control với hàm xử lý trong `Form1.cs`; khi người dùng thao tác thì logic chính được gọi. |
| 392 | `this.Load += new System.EventHandler(this.Form1_Load);` | Gắn control với hàm xử lý trong `Form1.cs`; khi người dùng thao tác thì logic chính được gọi. |

> Tóm lại: Designer giữ bố cục kéo thả; logic thật nằm trong `Form1.cs`.


---

## File `Form1.cs` — giải thích từng dòng code chính

Mình bỏ qua dòng trắng, `using`, `namespace`, comment và dấu ngoặc nhọn đơn lẻ. Các dòng còn lại là phần chính: biến trạng thái, GUI runtime, socket, game logic, hiệu ứng, âm thanh và skill.

#### Dòng 13
```csharp
    public partial class Form1 : Form
```
**Giải thích:** Khai báo cửa sổ chính `Form1`, kế thừa `Form` của WinForms. Toàn bộ giao diện và logic chính của chương trình nằm trong class này.

#### Dòng 15
```csharp
        const int BOARD_SIZE = 7;
```
**Giải thích:** Khai báo hằng số `BOARD_SIZE`. quy định bàn cờ có 7 dòng và 7 cột; mọi vòng lặp tạo ô, kiểm tra tọa độ, đặt tàu đều dựa vào hằng số này. Vì là `const`, giá trị không đổi khi chương trình chạy.

#### Dòng 16
```csharp
        const int BASE_SHIP_COUNT = 3;
```
**Giải thích:** Khai báo hằng số `BASE_SHIP_COUNT`. số tàu cơ bản phải đặt trước khi bấm Sẵn sàng; không tính tàu dự phòng. Vì là `const`, giá trị không đổi khi chương trình chạy.

#### Dòng 17
```csharp
        const int TOTAL_SHIP_COUNT = 4;
```
**Giải thích:** Khai báo hằng số `TOTAL_SHIP_COUNT`. tổng số vị trí trong mảng tàu, gồm 3 tàu ban đầu và 1 tàu dự phòng. Vì là `const`, giá trị không đổi khi chương trình chạy.

#### Dòng 18
```csharp
        const int SPARE_SHIP_ID = 3;
```
**Giải thích:** Khai báo hằng số `SPARE_SHIP_ID`. mã riêng của tàu dự phòng trong các mảng shipSizes, shipHP, shipPlaced, ownShipId. Vì là `const`, giá trị không đổi khi chương trình chạy.

#### Dòng 19
```csharp
        const int DRONE_SCAN_LENGTH = 4;
```
**Giải thích:** Khai báo hằng số `DRONE_SCAN_LENGTH`. số ô ngang Drone kiểm tra mỗi lần dùng kỹ năng. Vì là `const`, giá trị không đổi khi chương trình chạy.

#### Dòng 20
```csharp
        const int DRONE_BLINK_TOTAL_TIME = 1900;
```
**Giải thích:** Khai báo hằng số `DRONE_BLINK_TOTAL_TIME`. tổng thời gian hiệu ứng nhấp nháy Drone, đồng bộ với cảm giác âm thanh drone.wav. Vì là `const`, giá trị không đổi khi chương trình chạy.

#### Dòng 21
```csharp
        const int SHIP_SPARE_CONFIRM_TIME = 1000;
```
**Giải thích:** Khai báo hằng số `SHIP_SPARE_CONFIRM_TIME`. thời gian nhấp nháy xác nhận khi đặt tàu dự phòng. Vì là `const`, giá trị không đổi khi chương trình chạy.

#### Dòng 22
```csharp
        const int SHOT_IMPACT_DELAY = 450;
```
**Giải thích:** Khai báo hằng số `SHOT_IMPACT_DELAY`. delay trước khi xử lý kết quả bắn của đối thủ, làm nhịp trận chậm và có cảm giác va chạm. Vì là `const`, giá trị không đổi khi chương trình chạy.

#### Dòng 23
```csharp
        const int SHOT_ANIMATION_STEPS = 14;
```
**Giải thích:** Khai báo hằng số `SHOT_ANIMATION_STEPS`. số frame animation ảnh hit/miss rơi xuống và rõ dần. Vì là `const`, giá trị không đổi khi chương trình chạy.

#### Dòng 24
```csharp
        const int SHOT_ANIMATION_INTERVAL = 24;
```
**Giải thích:** Khai báo hằng số `SHOT_ANIMATION_INTERVAL`. khoảng thời gian giữa hai frame animation hit/miss. Vì là `const`, giá trị không đổi khi chương trình chạy.

#### Dòng 25
```csharp
        const int TURN_TRANSITION_TOTAL_TIME = 2000;
```
**Giải thích:** Khai báo hằng số `TURN_TRANSITION_TOTAL_TIME`. tổng thời gian khóa thao tác khi chuyển lượt; chữ lượt hiện rồi biến mất trong khoảng này. Vì là `const`, giá trị không đổi khi chương trình chạy.

#### Dòng 26
```csharp
        const int TURN_TRANSITION_STEPS = 40;
```
**Giải thích:** Khai báo hằng số `TURN_TRANSITION_STEPS`. số bước chia nhỏ hiệu ứng chuyển lượt để chữ di chuyển/mờ mượt hơn. Vì là `const`, giá trị không đổi khi chương trình chạy.

#### Dòng 31
```csharp
        Label[,] ownCells = new Label[BOARD_SIZE, BOARD_SIZE];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `ownCells`. mảng Label 2 chiều chứa các ô bàn cờ của chính người chơi. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 32
```csharp
        Label[,] enemyCells = new Label[BOARD_SIZE, BOARD_SIZE];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `enemyCells`. mảng Label 2 chiều chứa các ô bàn cờ đối phương, nơi người chơi click để bắn hoặc thả Drone. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 34
```csharp
        bool[,] ownShips = new bool[BOARD_SIZE, BOARD_SIZE];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `ownShips`. mảng bool đánh dấu ô nào trên bàn mình có tàu thật. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 35
```csharp
        bool[,] ownHits = new bool[BOARD_SIZE, BOARD_SIZE];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `ownHits`. mảng bool đánh dấu các ô tàu của mình đã bị đối thủ bắn trúng. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 36
```csharp
        bool[,] enemyShots = new bool[BOARD_SIZE, BOARD_SIZE];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `enemyShots`. mảng bool đánh dấu những ô trên bàn địch mà mình đã bắn, để không bắn trùng. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 37
```csharp
        bool[,] ownShotsReceived = new bool[BOARD_SIZE, BOARD_SIZE];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `ownShotsReceived`. mảng bool đánh dấu ô trên bàn mình đã từng bị bắn; tàu dự phòng không được đặt lên ô đã bị bắn. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 39
```csharp
        int[,] ownShipId = new int[BOARD_SIZE, BOARD_SIZE];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `ownShipId`. mảng int cho biết mỗi ô tàu thuộc tàu nào; nhờ vậy khi bị hit có thể trừ máu đúng tàu. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 41
```csharp
        int[] shipSizes = new int[] { 2, 3, 4, 2 };
```
**Giải thích:** Khai báo và/hoặc khởi tạo `shipSizes`. mảng độ dài các tàu: tàu 2 ô, 3 ô, 4 ô và tàu dự phòng 2 ô. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 42
```csharp
        int[] shipHP = new int[] { 2, 3, 4, 2 };
```
**Giải thích:** Khai báo và/hoặc khởi tạo `shipHP`. mảng máu còn lại của từng tàu; mỗi hit vào tàu sẽ trừ 1. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 43
```csharp
        bool[] shipPlaced = new bool[TOTAL_SHIP_COUNT];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `shipPlaced`. mảng đánh dấu tàu đã đặt hay chưa. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 44
```csharp
        PictureBox[] shipDockPictures = new PictureBox[BASE_SHIP_COUNT];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `shipDockPictures`. mảng ảnh tàu trong khu vực dock để người chơi kéo thả trước khi vào trận. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 46
```csharp
        int[] shipStartRow = new int[TOTAL_SHIP_COUNT];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `shipStartRow`. lưu dòng bắt đầu của mỗi tàu, dùng khi vẽ/xóa/kéo lại tàu. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 47
```csharp
        int[] shipStartCol = new int[TOTAL_SHIP_COUNT];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `shipStartCol`. lưu cột bắt đầu của mỗi tàu. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 48
```csharp
        bool[] shipIsHorizontal = new bool[TOTAL_SHIP_COUNT];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `shipIsHorizontal`. lưu hướng của mỗi tàu: true là ngang, false là dọc. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 50
```csharp
        bool droneUsed = false;
```
**Giải thích:** Khai báo và/hoặc khởi tạo `droneUsed`. đánh dấu kỹ năng Drone đã dùng trong trận, vì mỗi trận chỉ dùng một lần. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 51
```csharp
        bool shipSpareUsed = false;
```
**Giải thích:** Khai báo và/hoặc khởi tạo `shipSpareUsed`. đánh dấu kỹ năng tàu dự phòng đã dùng trong trận. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 53
```csharp
        TcpClient client;
```
**Giải thích:** Khai báo biến `client`. đối tượng TcpClient đại diện kết nối TCP từ Client đến Server.

#### Dòng 54
```csharp
        StreamReader reader;
```
**Giải thích:** Khai báo biến `reader`. luồng đọc text từ Server.

#### Dòng 55
```csharp
        StreamWriter writer;
```
**Giải thích:** Khai báo biến `writer`. luồng ghi text gửi lên Server.

#### Dòng 56
```csharp
        Thread receiveThread;
```
**Giải thích:** Khai báo biến `receiveThread`. thread nền liên tục nhận lệnh Server để giao diện không bị treo.

#### Dòng 58
```csharp
        Encoding encoding = new UTF8Encoding(false);
```
**Giải thích:** Khai báo và/hoặc khởi tạo `encoding`. mã hóa UTF-8 không BOM, giúp gửi tiếng Việt qua socket ổn hơn. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 60
```csharp
        int playerNumber = 0;
```
**Giải thích:** Khai báo và/hoặc khởi tạo `playerNumber`. số người chơi do Server cấp: 1 hoặc 2. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 61
```csharp
        int shipCount = 0;
```
**Giải thích:** Khai báo và/hoặc khởi tạo `shipCount`. số tàu cơ bản đã đặt xong. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 63
```csharp
        bool connected = false;
```
**Giải thích:** Khai báo và/hoặc khởi tạo `connected`. trạng thái đã kết nối Server hay chưa. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 64
```csharp
        bool ready = false;
```
**Giải thích:** Khai báo và/hoặc khởi tạo `ready`. mảng trạng thái sẵn sàng của hai Client. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 65
```csharp
        bool myTurn = false;
```
**Giải thích:** Khai báo và/hoặc khởi tạo `myTurn`. cho biết hiện tại có phải lượt bắn của mình. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 66
```csharp
        bool gameEnded = false;
```
**Giải thích:** Khai báo và/hoặc khởi tạo `gameEnded`. cho biết trận đã kết thúc; khi true thì khóa bắn và khóa skill. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 67
```csharp
        bool isHorizontal = true;
```
**Giải thích:** Khai báo và/hoặc khởi tạo `isHorizontal`. hướng đặt tàu hiện tại. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 68
```csharp
        bool actionLocked = false;
```
**Giải thích:** Khai báo và/hoặc khởi tạo `actionLocked`. khóa thao tác trong lúc animation/delay/skill đang chạy để tránh click sai thời điểm. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 69
```csharp
        bool turnTransitionRunning = false;
```
**Giải thích:** Khai báo và/hoặc khởi tạo `turnTransitionRunning`. cho biết animation chuyển lượt đang chạy. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 71
```csharp
        Panel panelEndOverlay;
```
**Giải thích:** Khai báo biến `panelEndOverlay`. panel phủ toàn màn hình khi thắng/thua.

#### Dòng 72
```csharp
        Label lblEndResult;
```
**Giải thích:** Khai báo biến `lblEndResult`. label chữ thắng/thua lớn.

#### Dòng 73
```csharp
        Label lblEndReplay;
```
**Giải thích:** Khai báo biến `lblEndReplay`. label dạng nút Chơi lại trên màn hình kết thúc.

#### Dòng 74
```csharp
        Label lblEndExit;
```
**Giải thích:** Khai báo biến `lblEndExit`. label dạng nút Thoát trên màn hình kết thúc.

#### Dòng 76
```csharp
        private class BoardCellLabel : Label
```
**Giải thích:** Khai báo một loại Label riêng cho ô bàn cờ. Nó kế thừa Label nhưng được tùy biến cách vẽ viền.


---

### Nhóm hàm `OnPaint`

**Vai trò:** vẽ viền đen cho từng ô bàn cờ.

#### Dòng 78
```csharp
            protected override void OnPaint(PaintEventArgs e)
```
**Giải thích:** Khai báo hàm `OnPaint(PaintEventArgs e)`. Công dụng chính: vẽ viền đen cho từng ô bàn cờ.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 80
```csharp
                base.OnPaint(e);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `OnPaint`, tức phần vẽ viền đen cho từng ô bàn cờ..

#### Dòng 84
```csharp
                    e.Graphics.DrawRectangle(pen, 0, 0, this.Width - 1, this.Height - 1);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `OnPaint`, tức phần vẽ viền đen cho từng ô bàn cờ..


---

### Nhóm hàm `Form1`

**Vai trò:** khởi tạo form và gọi các hàm chuẩn bị game.

#### Dòng 89
```csharp
        public Form1()
```
**Giải thích:** Hàm khởi tạo `Form1`. Khi cửa sổ mở, hàm này chạy đầu tiên để tạo control, khởi tạo dữ liệu, tạo bàn cờ, dock tàu, menu skill và âm thanh click.

#### Dòng 91
```csharp
            InitializeComponent();
```
**Giải thích:** Gọi hàm `InitializeComponent` để tạo các control từ Designer trước khi code truy cập chúng. Ngữ cảnh: dòng này nằm trong `Form1`, tức phần khởi tạo form và gọi các hàm chuẩn bị game..

#### Dòng 93
```csharp
            InitializeBoardData();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `Form1`, tức phần khởi tạo form và gọi các hàm chuẩn bị game..

#### Dòng 94
```csharp
            ConfigureDesignerUI();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `Form1`, tức phần khởi tạo form và gọi các hàm chuẩn bị game..

#### Dòng 95
```csharp
            CreateBoards();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `Form1`, tức phần khởi tạo form và gọi các hàm chuẩn bị game..

#### Dòng 96
```csharp
            CreateShipDock();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `Form1`, tức phần khởi tạo form và gọi các hàm chuẩn bị game..

#### Dòng 97
```csharp
            SetupSkillMenu();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `Form1`, tức phần khởi tạo form và gọi các hàm chuẩn bị game..

#### Dòng 98
```csharp
            HookClickSounds(this);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `Form1`, tức phần khởi tạo form và gọi các hàm chuẩn bị game..


---

### Nhóm hàm `InitializeBoardData`

**Vai trò:** đưa mảng mã tàu về trạng thái chưa có tàu.

#### Dòng 102
```csharp
        private void InitializeBoardData()
```
**Giải thích:** Khai báo hàm `InitializeBoardData()`. Công dụng chính: đưa mảng mã tàu về trạng thái chưa có tàu.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 104
```csharp
            for (int row = 0; row < BOARD_SIZE; row++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `InitializeBoardData`, tức phần đưa mảng mã tàu về trạng thái chưa có tàu..

#### Dòng 106
```csharp
                for (int col = 0; col < BOARD_SIZE; col++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `InitializeBoardData`, tức phần đưa mảng mã tàu về trạng thái chưa có tàu..

#### Dòng 108
```csharp
                    ownShipId[row, col] = -1;
```
**Giải thích:** Cập nhật phần tử mảng `ownShipId[row, col]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `InitializeBoardData`, tức phần đưa mảng mã tàu về trạng thái chưa có tàu..


---

### Nhóm hàm `ConfigureDesignerUI`

**Vai trò:** cấu hình giao diện runtime sau khi Designer tạo control.

#### Dòng 113
```csharp
        private void ConfigureDesignerUI()
```
**Giải thích:** Khai báo hàm `ConfigureDesignerUI()`. Công dụng chính: cấu hình giao diện runtime sau khi Designer tạo control.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 116
```csharp
            ApplySeaBackground();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ConfigureDesignerUI`, tức phần cấu hình giao diện runtime sau khi Designer tạo control..

#### Dòng 118
```csharp
            StyleButton(btnConnect);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ConfigureDesignerUI`, tức phần cấu hình giao diện runtime sau khi Designer tạo control..

#### Dòng 119
```csharp
            StyleButton(btnReady);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ConfigureDesignerUI`, tức phần cấu hình giao diện runtime sau khi Designer tạo control..

#### Dòng 120
```csharp
            StyleButton(btnRotate);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ConfigureDesignerUI`, tức phần cấu hình giao diện runtime sau khi Designer tạo control..

#### Dòng 121
```csharp
            StyleButton(btnPlayAgain);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ConfigureDesignerUI`, tức phần cấu hình giao diện runtime sau khi Designer tạo control..

#### Dòng 122
```csharp
            StyleButton(btnExitGame);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ConfigureDesignerUI`, tức phần cấu hình giao diện runtime sau khi Designer tạo control..

#### Dòng 124
```csharp
            AddLogo();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ConfigureDesignerUI`, tức phần cấu hình giao diện runtime sau khi Designer tạo control..

#### Dòng 125
```csharp
            SetupAvatars();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ConfigureDesignerUI`, tức phần cấu hình giao diện runtime sau khi Designer tạo control..

#### Dòng 128
```csharp
            this.DoubleBuffered = true;
```
**Giải thích:** Gán `true` cho `this.DoubleBuffered`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ConfigureDesignerUI`, tức phần cấu hình giao diện runtime sau khi Designer tạo control..


---

### Nhóm hàm `StyleButton`

**Vai trò:** định dạng các nút cho đồng bộ.

#### Dòng 131
```csharp
        private void StyleButton(Button button)
```
**Giải thích:** Khai báo hàm `StyleButton(Button button)`. Công dụng chính: định dạng các nút cho đồng bộ.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 133
```csharp
            button.FlatStyle = FlatStyle.Flat;
```
**Giải thích:** Gán `FlatStyle.Flat` cho `button.FlatStyle`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `StyleButton`, tức phần định dạng các nút cho đồng bộ..

#### Dòng 134
```csharp
            button.FlatAppearance.BorderColor = Color.Black;
```
**Giải thích:** Gán `Color.Black` cho `button.FlatAppearance.BorderColor`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `StyleButton`, tức phần định dạng các nút cho đồng bộ..

#### Dòng 135
```csharp
            button.FlatAppearance.BorderSize = 2;
```
**Giải thích:** Gán `2` cho `button.FlatAppearance.BorderSize`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `StyleButton`, tức phần định dạng các nút cho đồng bộ..

#### Dòng 136
```csharp
            button.BackColor = Color.White;
```
**Giải thích:** Đổi màu nền của `button`. Game dùng màu để thể hiện hiệu ứng, trạng thái hit/miss, skill hoặc thắng/thua. Ngữ cảnh: dòng này nằm trong `StyleButton`, tức phần định dạng các nút cho đồng bộ..

#### Dòng 137
```csharp
            button.UseVisualStyleBackColor = false;
```
**Giải thích:** Gán `false` cho `button.UseVisualStyleBackColor`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `StyleButton`, tức phần định dạng các nút cho đồng bộ..

#### Dòng 138
```csharp
            button.Font = new Font("Segoe UI", 10, FontStyle.Bold);
```
**Giải thích:** Tạo Font để chỉnh kiểu chữ, kích thước và độ đậm cho control. Ngữ cảnh: dòng này nằm trong `StyleButton`, tức phần định dạng các nút cho đồng bộ..


---

### Nhóm hàm `DrawPanelBorder`

**Vai trò:** vẽ viền panel.

#### Dòng 141
```csharp
        private void DrawPanelBorder(object sender, PaintEventArgs e)
```
**Giải thích:** Khai báo hàm `DrawPanelBorder(object sender, PaintEventArgs e)`. Công dụng chính: vẽ viền panel.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 143
```csharp
            Control control = sender as Control;
```
**Giải thích:** Gán `sender as Control` cho `Control control`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawPanelBorder`, tức phần vẽ viền panel..

#### Dòng 145
```csharp
            if (control == null)
```
**Giải thích:** Kiểm tra điều kiện `(control == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `DrawPanelBorder`, tức phần vẽ viền panel..

#### Dòng 147
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `DrawPanelBorder`, tức phần vẽ viền panel..

#### Dòng 152
```csharp
                e.Graphics.DrawRectangle(pen, 0, 0, control.Width - 1, control.Height - 1);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `DrawPanelBorder`, tức phần vẽ viền panel..


---

### Nhóm hàm `ApplySeaBackground`

**Vai trò:** nạp ảnh nền biển.

#### Dòng 156
```csharp
        private void ApplySeaBackground()
```
**Giải thích:** Khai báo hàm `ApplySeaBackground()`. Công dụng chính: nạp ảnh nền biển.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 158
```csharp
            Image sea = LoadImageSafe("sea.jpg");
```
**Giải thích:** Gán `LoadImageSafe("sea.jpg")` cho `Image sea`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ApplySeaBackground`, tức phần nạp ảnh nền biển..

#### Dòng 160
```csharp
            if (sea != null)
```
**Giải thích:** Kiểm tra điều kiện `(sea != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ApplySeaBackground`, tức phần nạp ảnh nền biển..

#### Dòng 162
```csharp
                this.BackgroundImage = sea;
```
**Giải thích:** Gán `sea` cho `this.BackgroundImage`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ApplySeaBackground`, tức phần nạp ảnh nền biển..

#### Dòng 163
```csharp
                this.BackgroundImageLayout = ImageLayout.Stretch;
```
**Giải thích:** Gán `ImageLayout.Stretch` cho `this.BackgroundImageLayout`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ApplySeaBackground`, tức phần nạp ảnh nền biển..


---

### Nhóm hàm `AddLogo`

**Vai trò:** nạp logo.

#### Dòng 167
```csharp
        private void AddLogo()
```
**Giải thích:** Khai báo hàm `AddLogo()`. Công dụng chính: nạp logo.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 169
```csharp
            Image logoImage = LoadImageSafe("logo.png");
```
**Giải thích:** Gán `LoadImageSafe("logo.png")` cho `Image logoImage`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AddLogo`, tức phần nạp logo..

#### Dòng 171
```csharp
            if (picLogo == null)
```
**Giải thích:** Kiểm tra điều kiện `(picLogo == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `AddLogo`, tức phần nạp logo..

#### Dòng 173
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `AddLogo`, tức phần nạp logo..

#### Dòng 176
```csharp
            picLogo.Image = logoImage;
```
**Giải thích:** Gán ảnh cho `picLogo` để control hiển thị hình tương ứng. Ngữ cảnh: dòng này nằm trong `AddLogo`, tức phần nạp logo..

#### Dòng 177
```csharp
            picLogo.Visible = logoImage != null;
```
**Giải thích:** Ẩn/hiện `picLogo`. Ví dụ nút Chơi lại chỉ hiện khi trận kết thúc. Ngữ cảnh: dòng này nằm trong `AddLogo`, tức phần nạp logo..

#### Dòng 178
```csharp
            picLogo.BringToFront();
```
**Giải thích:** Đưa control lên trên cùng để không bị panel/ảnh khác che. Ngữ cảnh: dòng này nằm trong `AddLogo`, tức phần nạp logo..


---

### Nhóm hàm `SetupAvatars`

**Vai trò:** nạp avatar bản thân và đối thủ.

#### Dòng 181
```csharp
        private void SetupAvatars()
```
**Giải thích:** Khai báo hàm `SetupAvatars()`. Công dụng chính: nạp avatar bản thân và đối thủ.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 183
```csharp
            Image ownAvatar = LoadImageSafe("avt1.png");
```
**Giải thích:** Gán `LoadImageSafe("avt1.png")` cho `Image ownAvatar`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `SetupAvatars`, tức phần nạp avatar bản thân và đối thủ..

#### Dòng 184
```csharp
            Image enemyAvatar = LoadImageSafe("avt2.png");
```
**Giải thích:** Gán `LoadImageSafe("avt2.png")` cho `Image enemyAvatar`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `SetupAvatars`, tức phần nạp avatar bản thân và đối thủ..

#### Dòng 185
```csharp
            if (ownAvatar != null)
```
**Giải thích:** Kiểm tra điều kiện `(ownAvatar != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `SetupAvatars`, tức phần nạp avatar bản thân và đối thủ..

#### Dòng 187
```csharp
                picOwnAvatar.Image = ownAvatar;
```
**Giải thích:** Gán ảnh cho `picOwnAvatar` để control hiển thị hình tương ứng. Ngữ cảnh: dòng này nằm trong `SetupAvatars`, tức phần nạp avatar bản thân và đối thủ..

#### Dòng 190
```csharp
            if (enemyAvatar != null)
```
**Giải thích:** Kiểm tra điều kiện `(enemyAvatar != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `SetupAvatars`, tức phần nạp avatar bản thân và đối thủ..

#### Dòng 192
```csharp
                picEnemyAvatar.Image = enemyAvatar;
```
**Giải thích:** Gán ảnh cho `picEnemyAvatar` để control hiển thị hình tương ứng. Ngữ cảnh: dòng này nằm trong `SetupAvatars`, tức phần nạp avatar bản thân và đối thủ..

#### Dòng 195
```csharp
            picOwnAvatar.SizeMode = PictureBoxSizeMode.Zoom;
```
**Giải thích:** Gán `PictureBoxSizeMode.Zoom` cho `picOwnAvatar.SizeMode`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `SetupAvatars`, tức phần nạp avatar bản thân và đối thủ..

#### Dòng 196
```csharp
            picEnemyAvatar.SizeMode = PictureBoxSizeMode.Zoom;
```
**Giải thích:** Gán `PictureBoxSizeMode.Zoom` cho `picEnemyAvatar.SizeMode`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `SetupAvatars`, tức phần nạp avatar bản thân và đối thủ..

#### Dòng 197
```csharp
            picOwnAvatar.BackColor = Color.Transparent;
```
**Giải thích:** Đổi màu nền của `picOwnAvatar`. Game dùng màu để thể hiện hiệu ứng, trạng thái hit/miss, skill hoặc thắng/thua. Ngữ cảnh: dòng này nằm trong `SetupAvatars`, tức phần nạp avatar bản thân và đối thủ..

#### Dòng 198
```csharp
            picEnemyAvatar.BackColor = Color.Transparent;
```
**Giải thích:** Đổi màu nền của `picEnemyAvatar`. Game dùng màu để thể hiện hiệu ứng, trạng thái hit/miss, skill hoặc thắng/thua. Ngữ cảnh: dòng này nằm trong `SetupAvatars`, tức phần nạp avatar bản thân và đối thủ..


---

### Nhóm hàm `SetupSkillMenu`

**Vai trò:** thiết lập icon kỹ năng Drone và Tàu dự phòng.

#### Dòng 201
```csharp
        private void SetupSkillMenu()
```
**Giải thích:** Khai báo hàm `SetupSkillMenu()`. Công dụng chính: thiết lập icon kỹ năng Drone và Tàu dự phòng.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 203
```csharp
            Image droneImage = LoadImageSafe("drone.png");
```
**Giải thích:** Gán `LoadImageSafe("drone.png")` cho `Image droneImage`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `SetupSkillMenu`, tức phần thiết lập icon kỹ năng Drone và Tàu dự phòng..

#### Dòng 204
```csharp
            Image spareImage = LoadImageSafe("shipspare.png");
```
**Giải thích:** Gán `LoadImageSafe("shipspare.png")` cho `Image spareImage`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `SetupSkillMenu`, tức phần thiết lập icon kỹ năng Drone và Tàu dự phòng..

#### Dòng 206
```csharp
            if (picDroneSkill != null)
```
**Giải thích:** Kiểm tra điều kiện `(picDroneSkill != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `SetupSkillMenu`, tức phần thiết lập icon kỹ năng Drone và Tàu dự phòng..

#### Dòng 208
```csharp
                picDroneSkill.Image = droneImage;
```
**Giải thích:** Gán ảnh cho `picDroneSkill` để control hiển thị hình tương ứng. Ngữ cảnh: dòng này nằm trong `SetupSkillMenu`, tức phần thiết lập icon kỹ năng Drone và Tàu dự phòng..

#### Dòng 209
```csharp
                picDroneSkill.SizeMode = PictureBoxSizeMode.Zoom;
```
**Giải thích:** Gán `PictureBoxSizeMode.Zoom` cho `picDroneSkill.SizeMode`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `SetupSkillMenu`, tức phần thiết lập icon kỹ năng Drone và Tàu dự phòng..

#### Dòng 210
```csharp
                picDroneSkill.Tag = "SKILL|DRONE";
```
**Giải thích:** Lưu dữ liệu phụ vào control. Game dùng Tag để nhớ tọa độ ô, mã tàu hoặc tên skill khi kéo thả/click. Ngữ cảnh: dòng này nằm trong `SetupSkillMenu`, tức phần thiết lập icon kỹ năng Drone và Tàu dự phòng..

#### Dòng 212
```csharp
                ToolTip tip = new ToolTip();
```
**Giải thích:** Tạo tooltip để khi rê chuột lên icon skill sẽ hiện mô tả ngắn. Ngữ cảnh: dòng này nằm trong `SetupSkillMenu`, tức phần thiết lập icon kỹ năng Drone và Tàu dự phòng..

#### Dòng 213
```csharp
                tip.SetToolTip(picDroneSkill, "Drone: kéo sang bàn cờ đối phương để dò 4 ô ngang.");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `SetupSkillMenu`, tức phần thiết lập icon kỹ năng Drone và Tàu dự phòng..

#### Dòng 216
```csharp
            if (picShipSpareSkill != null)
```
**Giải thích:** Kiểm tra điều kiện `(picShipSpareSkill != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `SetupSkillMenu`, tức phần thiết lập icon kỹ năng Drone và Tàu dự phòng..

#### Dòng 218
```csharp
                picShipSpareSkill.Image = spareImage;
```
**Giải thích:** Gán ảnh cho `picShipSpareSkill` để control hiển thị hình tương ứng. Ngữ cảnh: dòng này nằm trong `SetupSkillMenu`, tức phần thiết lập icon kỹ năng Drone và Tàu dự phòng..

#### Dòng 219
```csharp
                picShipSpareSkill.SizeMode = PictureBoxSizeMode.Zoom;
```
**Giải thích:** Gán `PictureBoxSizeMode.Zoom` cho `picShipSpareSkill.SizeMode`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `SetupSkillMenu`, tức phần thiết lập icon kỹ năng Drone và Tàu dự phòng..

#### Dòng 220
```csharp
                picShipSpareSkill.Tag = "SKILL|SPARE";
```
**Giải thích:** Lưu dữ liệu phụ vào control. Game dùng Tag để nhớ tọa độ ô, mã tàu hoặc tên skill khi kéo thả/click. Ngữ cảnh: dòng này nằm trong `SetupSkillMenu`, tức phần thiết lập icon kỹ năng Drone và Tàu dự phòng..

#### Dòng 222
```csharp
                ToolTip tip = new ToolTip();
```
**Giải thích:** Tạo tooltip để khi rê chuột lên icon skill sẽ hiện mô tả ngắn. Ngữ cảnh: dòng này nằm trong `SetupSkillMenu`, tức phần thiết lập icon kỹ năng Drone và Tàu dự phòng..

#### Dòng 223
```csharp
                tip.SetToolTip(picShipSpareSkill, "Tàu dự phòng: kéo sang bàn cờ của bạn để đặt thêm tàu 2 ô.");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `SetupSkillMenu`, tức phần thiết lập icon kỹ năng Drone và Tàu dự phòng..

#### Dòng 226
```csharp
            UpdateSkillMenuState();
```
**Giải thích:** Gọi hàm `UpdateSkillMenuState` để làm mới trạng thái icon skill. Ngữ cảnh: dòng này nằm trong `SetupSkillMenu`, tức phần thiết lập icon kỹ năng Drone và Tàu dự phòng..


---

### Nhóm hàm `UpdateSkillMenuState`

**Vai trò:** cập nhật icon skill bật/tắt.

#### Dòng 229
```csharp
        private void UpdateSkillMenuState()
```
**Giải thích:** Khai báo hàm `UpdateSkillMenuState()`. Công dụng chính: cập nhật icon skill bật/tắt.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 231
```csharp
            UpdateSkillPicture(picDroneSkill, "drone.png", CanUseDroneSkill());
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `UpdateSkillMenuState`, tức phần cập nhật icon skill bật/tắt..

#### Dòng 232
```csharp
            UpdateSkillPicture(picShipSpareSkill, "shipspare.png", CanUseShipSpareSkill());
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `UpdateSkillMenuState`, tức phần cập nhật icon skill bật/tắt..


---

### Nhóm hàm `UpdateSkillPicture`

**Vai trò:** đổi ảnh/icon skill theo trạng thái dùng được hay không.

#### Dòng 235
```csharp
        private void UpdateSkillPicture(PictureBox pictureBox, string imageFile, bool enabled)
```
**Giải thích:** Khai báo hàm `UpdateSkillPicture(PictureBox pictureBox, string imageFile, bool enabled)`. Công dụng chính: đổi ảnh/icon skill theo trạng thái dùng được hay không.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 237
```csharp
            if (pictureBox == null)
```
**Giải thích:** Kiểm tra điều kiện `(pictureBox == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `UpdateSkillPicture`, tức phần đổi ảnh/icon skill theo trạng thái dùng được hay không..

#### Dòng 239
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `UpdateSkillPicture`, tức phần đổi ảnh/icon skill theo trạng thái dùng được hay không..

#### Dòng 242
```csharp
            Image image = LoadImageSafe(imageFile);
```
**Giải thích:** Gán `LoadImageSafe(imageFile)` cho `Image image`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `UpdateSkillPicture`, tức phần đổi ảnh/icon skill theo trạng thái dùng được hay không..

#### Dòng 244
```csharp
            if (image == null)
```
**Giải thích:** Kiểm tra điều kiện `(image == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `UpdateSkillPicture`, tức phần đổi ảnh/icon skill theo trạng thái dùng được hay không..

#### Dòng 246
```csharp
                pictureBox.Enabled = enabled;
```
**Giải thích:** Bật/tắt tương tác `pictureBox`. Khi false người chơi không click/kéo được. Ngữ cảnh: dòng này nằm trong `UpdateSkillPicture`, tức phần đổi ảnh/icon skill theo trạng thái dùng được hay không..

#### Dòng 247
```csharp
                pictureBox.Cursor = enabled ? Cursors.Hand : Cursors.No;
```
**Giải thích:** Đổi hình con trỏ chuột, ví dụ Hand khi có thể dùng skill, No khi bị khóa. Ngữ cảnh: dòng này nằm trong `UpdateSkillPicture`, tức phần đổi ảnh/icon skill theo trạng thái dùng được hay không..

#### Dòng 248
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `UpdateSkillPicture`, tức phần đổi ảnh/icon skill theo trạng thái dùng được hay không..

#### Dòng 251
```csharp
            pictureBox.Image = enabled ? image : CreateOpacityImage(image, 0.35f);
```
**Giải thích:** Gán ảnh cho `pictureBox` để control hiển thị hình tương ứng. Ngữ cảnh: dòng này nằm trong `UpdateSkillPicture`, tức phần đổi ảnh/icon skill theo trạng thái dùng được hay không..

#### Dòng 252
```csharp
            pictureBox.Enabled = enabled;
```
**Giải thích:** Bật/tắt tương tác `pictureBox`. Khi false người chơi không click/kéo được. Ngữ cảnh: dòng này nằm trong `UpdateSkillPicture`, tức phần đổi ảnh/icon skill theo trạng thái dùng được hay không..

#### Dòng 253
```csharp
            pictureBox.Cursor = enabled ? Cursors.Hand : Cursors.No;
```
**Giải thích:** Đổi hình con trỏ chuột, ví dụ Hand khi có thể dùng skill, No khi bị khóa. Ngữ cảnh: dòng này nằm trong `UpdateSkillPicture`, tức phần đổi ảnh/icon skill theo trạng thái dùng được hay không..

#### Dòng 254
```csharp
            pictureBox.BackColor = enabled ? Color.White : Color.Gainsboro;
```
**Giải thích:** Đổi màu nền của `pictureBox`. Game dùng màu để thể hiện hiệu ứng, trạng thái hit/miss, skill hoặc thắng/thua. Ngữ cảnh: dòng này nằm trong `UpdateSkillPicture`, tức phần đổi ảnh/icon skill theo trạng thái dùng được hay không..


---

### Nhóm hàm `CanUseSkillBase`

**Vai trò:** kiểm tra điều kiện chung để dùng skill.

#### Dòng 257
```csharp
        private bool CanUseSkillBase()
```
**Giải thích:** Khai báo hàm `CanUseSkillBase()`. Công dụng chính: kiểm tra điều kiện chung để dùng skill.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 259
```csharp
            return connected && ready && myTurn && !gameEnded && !actionLocked && !turnTransitionRunning;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `CanUseSkillBase`, tức phần kiểm tra điều kiện chung để dùng skill..


---

### Nhóm hàm `CanUseDroneSkill`

**Vai trò:** kiểm tra riêng skill Drone.

#### Dòng 262
```csharp
        private bool CanUseDroneSkill()
```
**Giải thích:** Khai báo hàm `CanUseDroneSkill()`. Công dụng chính: kiểm tra riêng skill Drone.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 264
```csharp
            return CanUseSkillBase() && !droneUsed;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `CanUseDroneSkill`, tức phần kiểm tra riêng skill Drone..


---

### Nhóm hàm `CanUseShipSpareSkill`

**Vai trò:** kiểm tra riêng skill Tàu dự phòng.

#### Dòng 267
```csharp
        private bool CanUseShipSpareSkill()
```
**Giải thích:** Khai báo hàm `CanUseShipSpareSkill()`. Công dụng chính: kiểm tra riêng skill Tàu dự phòng.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 269
```csharp
            return CanUseSkillBase() && !shipSpareUsed;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `CanUseShipSpareSkill`, tức phần kiểm tra riêng skill Tàu dự phòng..


---

### Nhóm hàm `picDroneSkill_MouseDown`

**Vai trò:** bắt đầu kéo skill Drone.

#### Dòng 272
```csharp
        private void picDroneSkill_MouseDown(object sender, MouseEventArgs e)
```
**Giải thích:** Khai báo hàm `picDroneSkill_MouseDown(object sender, MouseEventArgs e)`. Công dụng chính: bắt đầu kéo skill Drone.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 274
```csharp
            if (e.Button != MouseButtons.Left)
```
**Giải thích:** Kiểm tra điều kiện `(e.Button != MouseButtons.Left`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `picDroneSkill_MouseDown`, tức phần bắt đầu kéo skill Drone..

#### Dòng 276
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `picDroneSkill_MouseDown`, tức phần bắt đầu kéo skill Drone..

#### Dòng 279
```csharp
            if (!CanUseDroneSkill())
```
**Giải thích:** Kiểm tra điều kiện `(!CanUseDroneSkill()`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `picDroneSkill_MouseDown`, tức phần bắt đầu kéo skill Drone..

#### Dòng 281
```csharp
                lblStatus.Text = droneUsed ? "TRẠNG THÁI: DRONE ĐÃ DÙNG" : "TRẠNG THÁI: CHƯA THỂ DÙNG DRONE";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `picDroneSkill_MouseDown`, tức phần bắt đầu kéo skill Drone..

#### Dòng 282
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `picDroneSkill_MouseDown`, tức phần bắt đầu kéo skill Drone..

#### Dòng 285
```csharp
            picDroneSkill.DoDragDrop("SKILL|DRONE", DragDropEffects.Copy);
```
**Giải thích:** Bắt đầu thao tác kéo thả. Dữ liệu trong DoDragDrop sẽ đi theo chuột đến nơi người chơi thả. Ngữ cảnh: dòng này nằm trong `picDroneSkill_MouseDown`, tức phần bắt đầu kéo skill Drone..


---

### Nhóm hàm `picShipSpareSkill_MouseDown`

**Vai trò:** bắt đầu kéo skill Tàu dự phòng.

#### Dòng 288
```csharp
        private void picShipSpareSkill_MouseDown(object sender, MouseEventArgs e)
```
**Giải thích:** Khai báo hàm `picShipSpareSkill_MouseDown(object sender, MouseEventArgs e)`. Công dụng chính: bắt đầu kéo skill Tàu dự phòng.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 290
```csharp
            if (e.Button != MouseButtons.Left)
```
**Giải thích:** Kiểm tra điều kiện `(e.Button != MouseButtons.Left`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `picShipSpareSkill_MouseDown`, tức phần bắt đầu kéo skill Tàu dự phòng..

#### Dòng 292
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `picShipSpareSkill_MouseDown`, tức phần bắt đầu kéo skill Tàu dự phòng..

#### Dòng 295
```csharp
            if (!CanUseShipSpareSkill())
```
**Giải thích:** Kiểm tra điều kiện `(!CanUseShipSpareSkill()`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `picShipSpareSkill_MouseDown`, tức phần bắt đầu kéo skill Tàu dự phòng..

#### Dòng 297
```csharp
                lblStatus.Text = shipSpareUsed ? "TRẠNG THÁI: TÀU DỰ PHÒNG ĐÃ DÙNG" : "TRẠNG THÁI: CHƯA THỂ DÙNG TÀU DỰ PHÒNG";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `picShipSpareSkill_MouseDown`, tức phần bắt đầu kéo skill Tàu dự phòng..

#### Dòng 298
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `picShipSpareSkill_MouseDown`, tức phần bắt đầu kéo skill Tàu dự phòng..

#### Dòng 301
```csharp
            picShipSpareSkill.DoDragDrop("SKILL|SPARE", DragDropEffects.Copy);
```
**Giải thích:** Bắt đầu thao tác kéo thả. Dữ liệu trong DoDragDrop sẽ đi theo chuột đến nơi người chơi thả. Ngữ cảnh: dòng này nằm trong `picShipSpareSkill_MouseDown`, tức phần bắt đầu kéo skill Tàu dự phòng..


---

### Nhóm hàm `TryGetDraggedSkill`

**Vai trò:** đọc dữ liệu kéo thả để biết skill nào được kéo.

#### Dòng 304
```csharp
        private bool TryGetDraggedSkill(DragEventArgs e, out string skillName)
```
**Giải thích:** Khai báo hàm `TryGetDraggedSkill(DragEventArgs e, out string skillName)`. Công dụng chính: đọc dữ liệu kéo thả để biết skill nào được kéo.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 306
```csharp
            skillName = "";
```
**Giải thích:** Gán `""` cho `skillName`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `TryGetDraggedSkill`, tức phần đọc dữ liệu kéo thả để biết skill nào được kéo..

#### Dòng 307
```csharp
            object data = e.Data.GetData(DataFormats.Text);
```
**Giải thích:** Gán `e.Data.GetData(DataFormats.Text)` cho `object data`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `TryGetDraggedSkill`, tức phần đọc dữ liệu kéo thả để biết skill nào được kéo..

#### Dòng 309
```csharp
            if (data == null)
```
**Giải thích:** Kiểm tra điều kiện `(data == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `TryGetDraggedSkill`, tức phần đọc dữ liệu kéo thả để biết skill nào được kéo..

#### Dòng 311
```csharp
                data = e.Data.GetData(DataFormats.StringFormat);
```
**Giải thích:** Gán `e.Data.GetData(DataFormats.StringFormat)` cho `data`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `TryGetDraggedSkill`, tức phần đọc dữ liệu kéo thả để biết skill nào được kéo..

#### Dòng 314
```csharp
            if (data == null)
```
**Giải thích:** Kiểm tra điều kiện `(data == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `TryGetDraggedSkill`, tức phần đọc dữ liệu kéo thả để biết skill nào được kéo..

#### Dòng 316
```csharp
                return false;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `TryGetDraggedSkill`, tức phần đọc dữ liệu kéo thả để biết skill nào được kéo..

#### Dòng 319
```csharp
            string text = data.ToString();
```
**Giải thích:** Gán `data.ToString()` cho `string text`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `TryGetDraggedSkill`, tức phần đọc dữ liệu kéo thả để biết skill nào được kéo..

#### Dòng 321
```csharp
            if (!text.StartsWith("SKILL|"))
```
**Giải thích:** Kiểm tra điều kiện `(!text.StartsWith("SKILL|")`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `TryGetDraggedSkill`, tức phần đọc dữ liệu kéo thả để biết skill nào được kéo..

#### Dòng 323
```csharp
                return false;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `TryGetDraggedSkill`, tức phần đọc dữ liệu kéo thả để biết skill nào được kéo..

#### Dòng 326
```csharp
            skillName = text.Replace("SKILL|", "").Trim().ToUpper();
```
**Giải thích:** Gán `text.Replace("SKILL|", "").Trim().ToUpper()` cho `skillName`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `TryGetDraggedSkill`, tức phần đọc dữ liệu kéo thả để biết skill nào được kéo..

#### Dòng 327
```csharp
            return skillName.Length > 0;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `TryGetDraggedSkill`, tức phần đọc dữ liệu kéo thả để biết skill nào được kéo..


---

### Nhóm hàm `EnemyCell_DragDrop`

**Vai trò:** nhận thao tác thả Drone vào ô địch.

#### Dòng 330
```csharp
        private void EnemyCell_DragDrop(object sender, DragEventArgs e)
```
**Giải thích:** Khai báo hàm `EnemyCell_DragDrop(object sender, DragEventArgs e)`. Công dụng chính: nhận thao tác thả Drone vào ô địch.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 332
```csharp
            Label cell = sender as Label;
```
**Giải thích:** Gán `sender as Label` cho `Label cell`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `EnemyCell_DragDrop`, tức phần nhận thao tác thả Drone vào ô địch..

#### Dòng 334
```csharp
            if (cell == null)
```
**Giải thích:** Kiểm tra điều kiện `(cell == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `EnemyCell_DragDrop`, tức phần nhận thao tác thả Drone vào ô địch..

#### Dòng 336
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `EnemyCell_DragDrop`, tức phần nhận thao tác thả Drone vào ô địch..

#### Dòng 339
```csharp
            string skillName;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `EnemyCell_DragDrop`, tức phần nhận thao tác thả Drone vào ô địch..

#### Dòng 341
```csharp
            if (!TryGetDraggedSkill(e, out skillName) || skillName != "DRONE")
```
**Giải thích:** Kiểm tra điều kiện `(!TryGetDraggedSkill(e, out skillName) || skillName != "DRONE"`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `EnemyCell_DragDrop`, tức phần nhận thao tác thả Drone vào ô địch..

#### Dòng 343
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `EnemyCell_DragDrop`, tức phần nhận thao tác thả Drone vào ô địch..

#### Dòng 346
```csharp
            Point p = (Point)cell.Tag;
```
**Giải thích:** Gán `(Point)cell.Tag` cho `Point p`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `EnemyCell_DragDrop`, tức phần nhận thao tác thả Drone vào ô địch..

#### Dòng 347
```csharp
            TryUseDrone(p.X, p.Y);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `EnemyCell_DragDrop`, tức phần nhận thao tác thả Drone vào ô địch..


---

### Nhóm hàm `EnemyBoard_DragDrop`

**Vai trò:** nhận thao tác thả Drone vào panel bàn địch.

#### Dòng 350
```csharp
        private void EnemyBoard_DragDrop(object sender, DragEventArgs e)
```
**Giải thích:** Khai báo hàm `EnemyBoard_DragDrop(object sender, DragEventArgs e)`. Công dụng chính: nhận thao tác thả Drone vào panel bàn địch.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 352
```csharp
            string skillName;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `EnemyBoard_DragDrop`, tức phần nhận thao tác thả Drone vào panel bàn địch..

#### Dòng 354
```csharp
            if (!TryGetDraggedSkill(e, out skillName) || skillName != "DRONE")
```
**Giải thích:** Kiểm tra điều kiện `(!TryGetDraggedSkill(e, out skillName) || skillName != "DRONE"`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `EnemyBoard_DragDrop`, tức phần nhận thao tác thả Drone vào panel bàn địch..

#### Dòng 356
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `EnemyBoard_DragDrop`, tức phần nhận thao tác thả Drone vào panel bàn địch..

#### Dòng 359
```csharp
            int cellWidth = panelEnemyBoard.ClientSize.Width / BOARD_SIZE;
```
**Giải thích:** Gán `panelEnemyBoard.ClientSize.Width / BOARD_SIZE` cho `int cellWidth`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `EnemyBoard_DragDrop`, tức phần nhận thao tác thả Drone vào panel bàn địch..

#### Dòng 360
```csharp
            int cellHeight = panelEnemyBoard.ClientSize.Height / BOARD_SIZE;
```
**Giải thích:** Gán `panelEnemyBoard.ClientSize.Height / BOARD_SIZE` cho `int cellHeight`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `EnemyBoard_DragDrop`, tức phần nhận thao tác thả Drone vào panel bàn địch..

#### Dòng 362
```csharp
            Point p = panelEnemyBoard.PointToClient(new Point(e.X, e.Y));
```
**Giải thích:** Gán `panelEnemyBoard.PointToClient(new Point(e.X, e.Y))` cho `Point p`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `EnemyBoard_DragDrop`, tức phần nhận thao tác thả Drone vào panel bàn địch..

#### Dòng 364
```csharp
            int col = p.X / cellWidth;
```
**Giải thích:** Gán `p.X / cellWidth` cho `int col`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `EnemyBoard_DragDrop`, tức phần nhận thao tác thả Drone vào panel bàn địch..

#### Dòng 365
```csharp
            int row = p.Y / cellHeight;
```
**Giải thích:** Gán `p.Y / cellHeight` cho `int row`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `EnemyBoard_DragDrop`, tức phần nhận thao tác thả Drone vào panel bàn địch..

#### Dòng 367
```csharp
            TryUseDrone(row, col);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `EnemyBoard_DragDrop`, tức phần nhận thao tác thả Drone vào panel bàn địch..


---

### Nhóm hàm `TryUseDrone`

**Vai trò:** gửi yêu cầu Drone scan sang Server.

#### Dòng 370
```csharp
        private void TryUseDrone(int row, int col)
```
**Giải thích:** Khai báo hàm `TryUseDrone(int row, int col)`. Công dụng chính: gửi yêu cầu Drone scan sang Server.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 372
```csharp
            if (!CanUseDroneSkill())
```
**Giải thích:** Kiểm tra điều kiện `(!CanUseDroneSkill()`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `TryUseDrone`, tức phần gửi yêu cầu Drone scan sang Server..

#### Dòng 374
```csharp
                lblStatus.Text = "TRẠNG THÁI: CHƯA THỂ DÙNG DRONE";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `TryUseDrone`, tức phần gửi yêu cầu Drone scan sang Server..

#### Dòng 375
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `TryUseDrone`, tức phần gửi yêu cầu Drone scan sang Server..

#### Dòng 378
```csharp
            if (!IsValidDroneStart(row, col))
```
**Giải thích:** Kiểm tra điều kiện `(!IsValidDroneStart(row, col)`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `TryUseDrone`, tức phần gửi yêu cầu Drone scan sang Server..

#### Dòng 380
```csharp
                MessageBox.Show("Drone cần đủ 4 ô ngang, hãy đặt từ cột 1 đến cột 4 của hàng cần dò.");
```
**Giải thích:** Hiện hộp thoại thông báo lỗi/cảnh báo cho người dùng. Ngữ cảnh: dòng này nằm trong `TryUseDrone`, tức phần gửi yêu cầu Drone scan sang Server..

#### Dòng 381
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `TryUseDrone`, tức phần gửi yêu cầu Drone scan sang Server..

#### Dòng 384
```csharp
            droneUsed = true;
```
**Giải thích:** Gán giá trị mới cho `droneUsed`. đánh dấu kỹ năng Drone đã dùng trong trận, vì mỗi trận chỉ dùng một lần. Giá trị `true` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `TryUseDrone`, tức phần gửi yêu cầu Drone scan sang Server..

#### Dòng 385
```csharp
            actionLocked = true;
```
**Giải thích:** Gán giá trị mới cho `actionLocked`. khóa thao tác trong lúc animation/delay/skill đang chạy để tránh click sai thời điểm. Giá trị `true` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `TryUseDrone`, tức phần gửi yêu cầu Drone scan sang Server..

#### Dòng 386
```csharp
            UpdateSkillMenuState();
```
**Giải thích:** Gọi hàm `UpdateSkillMenuState` để làm mới trạng thái icon skill. Ngữ cảnh: dòng này nằm trong `TryUseDrone`, tức phần gửi yêu cầu Drone scan sang Server..

#### Dòng 388
```csharp
            lblStatus.Text = "TRẠNG THÁI: DRONE ĐANG DÒ 4 Ô NGANG";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `TryUseDrone`, tức phần gửi yêu cầu Drone scan sang Server..

#### Dòng 389
```csharp
            SendLine("DRONE_SCAN|" + row + "|" + col);
```
**Giải thích:** Dòng này liên quan đến giao thức `DRONE_SCAN|`: lệnh yêu cầu đối thủ kiểm tra Drone. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `TryUseDrone`, tức phần gửi yêu cầu Drone scan sang Server..


---

### Nhóm hàm `IsValidDroneStart`

**Vai trò:** kiểm tra Drone có đủ 4 ô ngang để scan.

#### Dòng 392
```csharp
        private bool IsValidDroneStart(int row, int col)
```
**Giải thích:** Khai báo hàm `IsValidDroneStart(int row, int col)`. Công dụng chính: kiểm tra Drone có đủ 4 ô ngang để scan.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 394
```csharp
            return row >= 0 && row < BOARD_SIZE && col >= 0 && col + DRONE_SCAN_LENGTH - 1 < BOARD_SIZE;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `IsValidDroneStart`, tức phần kiểm tra Drone có đủ 4 ô ngang để scan..


---

### Nhóm hàm `HandleDroneRequest`

**Vai trò:** Client bị scan tự kiểm tra bàn mình.

#### Dòng 397
```csharp
        private void HandleDroneRequest(int row, int col)
```
**Giải thích:** Khai báo hàm `HandleDroneRequest(int row, int col)`. Công dụng chính: Client bị scan tự kiểm tra bàn mình.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 399
```csharp
            bool found = DoesDroneFindShip(row, col);
```
**Giải thích:** Gán `DoesDroneFindShip(row, col)` cho `bool found`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `HandleDroneRequest`, tức phần Client bị scan tự kiểm tra bàn mình..

#### Dòng 400
```csharp
            SendLine("DRONE_RESULT|" + row + "|" + col + "|" + (found ? "1" : "0"));
```
**Giải thích:** Dòng này liên quan đến giao thức `DRONE_RESULT|`: lệnh trả kết quả Drone. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `HandleDroneRequest`, tức phần Client bị scan tự kiểm tra bàn mình..


---

### Nhóm hàm `DoesDroneFindShip`

**Vai trò:** kiểm tra 4 ô ngang có chứa tàu không.

#### Dòng 403
```csharp
        private bool DoesDroneFindShip(int row, int col)
```
**Giải thích:** Khai báo hàm `DoesDroneFindShip(int row, int col)`. Công dụng chính: kiểm tra 4 ô ngang có chứa tàu không.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 405
```csharp
            if (!IsValidDroneStart(row, col))
```
**Giải thích:** Kiểm tra điều kiện `(!IsValidDroneStart(row, col)`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `DoesDroneFindShip`, tức phần kiểm tra 4 ô ngang có chứa tàu không..

#### Dòng 407
```csharp
                return false;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `DoesDroneFindShip`, tức phần kiểm tra 4 ô ngang có chứa tàu không..

#### Dòng 410
```csharp
            for (int i = 0; i < DRONE_SCAN_LENGTH; i++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `DoesDroneFindShip`, tức phần kiểm tra 4 ô ngang có chứa tàu không..

#### Dòng 412
```csharp
                if (ownShips[row, col + i])
```
**Giải thích:** Kiểm tra điều kiện `(ownShips[row, col + i]`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `DoesDroneFindShip`, tức phần kiểm tra 4 ô ngang có chứa tàu không..

#### Dòng 414
```csharp
                    return true;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `DoesDroneFindShip`, tức phần kiểm tra 4 ô ngang có chứa tàu không..

#### Dòng 418
```csharp
            return false;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `DoesDroneFindShip`, tức phần kiểm tra 4 ô ngang có chứa tàu không..


---

### Nhóm hàm `HandleDroneResult`

**Vai trò:** Client dùng Drone nhận kết quả và nhấp nháy màu.

#### Dòng 421
```csharp
        private void HandleDroneResult(int row, int col, bool found)
```
**Giải thích:** Khai báo hàm `HandleDroneResult(int row, int col, bool found)`. Công dụng chính: Client dùng Drone nhận kết quả và nhấp nháy màu.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 423
```csharp
            Color blinkColor = found ? Color.LimeGreen : Color.Red;
```
**Giải thích:** Gán `found ? Color.LimeGreen : Color.Red` cho `Color blinkColor`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `HandleDroneResult`, tức phần Client dùng Drone nhận kết quả và nhấp nháy màu..

#### Dòng 424
```csharp
            lblStatus.Text = found ? "TRẠNG THÁI: DRONE PHÁT HIỆN TÍN HIỆU TÀU" : "TRẠNG THÁI: DRONE KHÔNG PHÁT HIỆN TÀU";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `HandleDroneResult`, tức phần Client dùng Drone nhận kết quả và nhấp nháy màu..

#### Dòng 426
```csharp
            BlinkHorizontalCells(enemyCells, row, col, DRONE_SCAN_LENGTH, blinkColor, DRONE_BLINK_TOTAL_TIME, "drone.wav", () =>
```
**Giải thích:** Tạo lambda callback ngắn. Code này thường chạy khi Timer tick, thread chạy, hoặc event xảy ra. Ngữ cảnh: dòng này nằm trong `HandleDroneResult`, tức phần Client dùng Drone nhận kết quả và nhấp nháy màu..

#### Dòng 428
```csharp
                actionLocked = false;
```
**Giải thích:** Gán giá trị mới cho `actionLocked`. khóa thao tác trong lúc animation/delay/skill đang chạy để tránh click sai thời điểm. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `HandleDroneResult`, tức phần Client dùng Drone nhận kết quả và nhấp nháy màu..

#### Dòng 429
```csharp
                UpdateSkillMenuState();
```
**Giải thích:** Gọi hàm `UpdateSkillMenuState` để làm mới trạng thái icon skill. Ngữ cảnh: dòng này nằm trong `HandleDroneResult`, tức phần Client dùng Drone nhận kết quả và nhấp nháy màu..

#### Dòng 430
```csharp
            });
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `HandleDroneResult`, tức phần Client dùng Drone nhận kết quả và nhấp nháy màu..


---

### Nhóm hàm `TryUseShipSpare`

**Vai trò:** đặt tàu dự phòng 2 ô trên bàn mình.

#### Dòng 433
```csharp
        private void TryUseShipSpare(int row, int col)
```
**Giải thích:** Khai báo hàm `TryUseShipSpare(int row, int col)`. Công dụng chính: đặt tàu dự phòng 2 ô trên bàn mình.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 435
```csharp
            if (!CanUseShipSpareSkill())
```
**Giải thích:** Kiểm tra điều kiện `(!CanUseShipSpareSkill()`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `TryUseShipSpare`, tức phần đặt tàu dự phòng 2 ô trên bàn mình..

#### Dòng 437
```csharp
                lblStatus.Text = "TRẠNG THÁI: CHƯA THỂ DÙNG TÀU DỰ PHÒNG";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `TryUseShipSpare`, tức phần đặt tàu dự phòng 2 ô trên bàn mình..

#### Dòng 438
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `TryUseShipSpare`, tức phần đặt tàu dự phòng 2 ô trên bàn mình..

#### Dòng 441
```csharp
            if (!CanPlaceSpareShip(row, col, 2, isHorizontal))
```
**Giải thích:** Kiểm tra điều kiện `(!CanPlaceSpareShip(row, col, 2, isHorizontal)`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `TryUseShipSpare`, tức phần đặt tàu dự phòng 2 ô trên bàn mình..

#### Dòng 443
```csharp
                MessageBox.Show("Không đặt được tàu dự phòng ở vị trí này. Tàu cần 2 ô trống và chưa từng bị đối thủ bắn.");
```
**Giải thích:** Hiện hộp thoại thông báo lỗi/cảnh báo cho người dùng. Ngữ cảnh: dòng này nằm trong `TryUseShipSpare`, tức phần đặt tàu dự phòng 2 ô trên bàn mình..

#### Dòng 444
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `TryUseShipSpare`, tức phần đặt tàu dự phòng 2 ô trên bàn mình..

#### Dòng 447
```csharp
            shipSpareUsed = true;
```
**Giải thích:** Gán giá trị mới cho `shipSpareUsed`. đánh dấu kỹ năng tàu dự phòng đã dùng trong trận. Giá trị `true` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `TryUseShipSpare`, tức phần đặt tàu dự phòng 2 ô trên bàn mình..

#### Dòng 448
```csharp
            actionLocked = true;
```
**Giải thích:** Gán giá trị mới cho `actionLocked`. khóa thao tác trong lúc animation/delay/skill đang chạy để tránh click sai thời điểm. Giá trị `true` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `TryUseShipSpare`, tức phần đặt tàu dự phòng 2 ô trên bàn mình..

#### Dòng 449
```csharp
            shipPlaced[SPARE_SHIP_ID] = true;
```
**Giải thích:** Cập nhật phần tử mảng `shipPlaced[SPARE_SHIP_ID]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `TryUseShipSpare`, tức phần đặt tàu dự phòng 2 ô trên bàn mình..

#### Dòng 450
```csharp
            shipHP[SPARE_SHIP_ID] = 2;
```
**Giải thích:** Cập nhật phần tử mảng `shipHP[SPARE_SHIP_ID]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `TryUseShipSpare`, tức phần đặt tàu dự phòng 2 ô trên bàn mình..

#### Dòng 452
```csharp
            ApplyShipPlacement(row, col, 2, isHorizontal, SPARE_SHIP_ID);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `TryUseShipSpare`, tức phần đặt tàu dự phòng 2 ô trên bàn mình..

#### Dòng 453
```csharp
            SendLine("SPARE_PLACED|2");
```
**Giải thích:** Dòng này liên quan đến giao thức `SPARE_PLACED|`: lệnh báo người chơi đã thêm tàu dự phòng. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `TryUseShipSpare`, tức phần đặt tàu dự phòng 2 ô trên bàn mình..

#### Dòng 454
```csharp
            UpdateSkillMenuState();
```
**Giải thích:** Gọi hàm `UpdateSkillMenuState` để làm mới trạng thái icon skill. Ngữ cảnh: dòng này nằm trong `TryUseShipSpare`, tức phần đặt tàu dự phòng 2 ô trên bàn mình..

#### Dòng 456
```csharp
            lblStatus.Text = "TRẠNG THÁI: ĐÃ TRIỆU HỒI TÀU DỰ PHÒNG";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `TryUseShipSpare`, tức phần đặt tàu dự phòng 2 ô trên bàn mình..

#### Dòng 458
```csharp
            BlinkSpareShipCells(row, col, isHorizontal, () =>
```
**Giải thích:** Tạo lambda callback ngắn. Code này thường chạy khi Timer tick, thread chạy, hoặc event xảy ra. Ngữ cảnh: dòng này nằm trong `TryUseShipSpare`, tức phần đặt tàu dự phòng 2 ô trên bàn mình..

#### Dòng 460
```csharp
                actionLocked = false;
```
**Giải thích:** Gán giá trị mới cho `actionLocked`. khóa thao tác trong lúc animation/delay/skill đang chạy để tránh click sai thời điểm. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `TryUseShipSpare`, tức phần đặt tàu dự phòng 2 ô trên bàn mình..

#### Dòng 461
```csharp
                UpdateSkillMenuState();
```
**Giải thích:** Gọi hàm `UpdateSkillMenuState` để làm mới trạng thái icon skill. Ngữ cảnh: dòng này nằm trong `TryUseShipSpare`, tức phần đặt tàu dự phòng 2 ô trên bàn mình..

#### Dòng 462
```csharp
            });
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `TryUseShipSpare`, tức phần đặt tàu dự phòng 2 ô trên bàn mình..


---

### Nhóm hàm `CanPlaceSpareShip`

**Vai trò:** kiểm tra vị trí tàu dự phòng hợp lệ.

#### Dòng 465
```csharp
        private bool CanPlaceSpareShip(int row, int col, int length, bool horizontal)
```
**Giải thích:** Khai báo hàm `CanPlaceSpareShip(int row, int col, int length, bool horizontal)`. Công dụng chính: kiểm tra vị trí tàu dự phòng hợp lệ.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 467
```csharp
            for (int i = 0; i < length; i++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `CanPlaceSpareShip`, tức phần kiểm tra vị trí tàu dự phòng hợp lệ..

#### Dòng 469
```csharp
                int r = row;
```
**Giải thích:** Gán `row` cho `int r`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CanPlaceSpareShip`, tức phần kiểm tra vị trí tàu dự phòng hợp lệ..

#### Dòng 470
```csharp
                int c = col;
```
**Giải thích:** Gán `col` cho `int c`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CanPlaceSpareShip`, tức phần kiểm tra vị trí tàu dự phòng hợp lệ..

#### Dòng 472
```csharp
                if (horizontal)
```
**Giải thích:** Kiểm tra điều kiện `(horizontal`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `CanPlaceSpareShip`, tức phần kiểm tra vị trí tàu dự phòng hợp lệ..

#### Dòng 474
```csharp
                    c = col + i;
```
**Giải thích:** Gán `col + i` cho `c`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CanPlaceSpareShip`, tức phần kiểm tra vị trí tàu dự phòng hợp lệ..

#### Dòng 476
```csharp
                else
```
**Giải thích:** Nhánh xử lý còn lại khi các điều kiện phía trước đều sai. Ngữ cảnh: dòng này nằm trong `CanPlaceSpareShip`, tức phần kiểm tra vị trí tàu dự phòng hợp lệ..

#### Dòng 478
```csharp
                    r = row + i;
```
**Giải thích:** Gán `row + i` cho `r`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CanPlaceSpareShip`, tức phần kiểm tra vị trí tàu dự phòng hợp lệ..

#### Dòng 481
```csharp
                if (r < 0 || r >= BOARD_SIZE || c < 0 || c >= BOARD_SIZE)
```
**Giải thích:** Kiểm tra điều kiện `(r < 0 || r >= BOARD_SIZE || c < 0 || c >= BOARD_SIZE`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `CanPlaceSpareShip`, tức phần kiểm tra vị trí tàu dự phòng hợp lệ..

#### Dòng 483
```csharp
                    return false;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `CanPlaceSpareShip`, tức phần kiểm tra vị trí tàu dự phòng hợp lệ..

#### Dòng 486
```csharp
                if (ownShips[r, c] || ownShotsReceived[r, c])
```
**Giải thích:** Kiểm tra điều kiện `(ownShips[r, c] || ownShotsReceived[r, c]`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `CanPlaceSpareShip`, tức phần kiểm tra vị trí tàu dự phòng hợp lệ..

#### Dòng 488
```csharp
                    return false;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `CanPlaceSpareShip`, tức phần kiểm tra vị trí tàu dự phòng hợp lệ..

#### Dòng 492
```csharp
            return true;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `CanPlaceSpareShip`, tức phần kiểm tra vị trí tàu dự phòng hợp lệ..


---

### Nhóm hàm `BlinkSpareShipCells`

**Vai trò:** hiệu ứng xác nhận đặt tàu dự phòng.

#### Dòng 495
```csharp
        private void BlinkSpareShipCells(int row, int col, bool horizontal, Action finished)
```
**Giải thích:** Khai báo hàm `BlinkSpareShipCells(int row, int col, bool horizontal, Action finished)`. Công dụng chính: hiệu ứng xác nhận đặt tàu dự phòng.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 497
```csharp
            Label[] cells = new Label[BOARD_SIZE * BOARD_SIZE];
```
**Giải thích:** Tạo Label mới. Trong game, Label được dùng làm ô bàn cờ hoặc chữ hiển thị trạng thái. Ngữ cảnh: dòng này nằm trong `BlinkSpareShipCells`, tức phần hiệu ứng xác nhận đặt tàu dự phòng..

#### Dòng 498
```csharp
            int index = 0;
```
**Giải thích:** Gán `0` cho `int index`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `BlinkSpareShipCells`, tức phần hiệu ứng xác nhận đặt tàu dự phòng..

#### Dòng 500
```csharp
            for (int r = 0; r < BOARD_SIZE; r++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `BlinkSpareShipCells`, tức phần hiệu ứng xác nhận đặt tàu dự phòng..

#### Dòng 502
```csharp
                for (int c = 0; c < BOARD_SIZE; c++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `BlinkSpareShipCells`, tức phần hiệu ứng xác nhận đặt tàu dự phòng..

#### Dòng 504
```csharp
                    cells[index] = ownCells[r, c];
```
**Giải thích:** Cập nhật phần tử mảng `cells[index]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `BlinkSpareShipCells`, tức phần hiệu ứng xác nhận đặt tàu dự phòng..

#### Dòng 505
```csharp
                    index++;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `BlinkSpareShipCells`, tức phần hiệu ứng xác nhận đặt tàu dự phòng..

#### Dòng 509
```csharp
            BlinkCellsOnce(cells, Color.LimeGreen, SHIP_SPARE_CONFIRM_TIME, "heal.wav", finished);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `BlinkSpareShipCells`, tức phần hiệu ứng xác nhận đặt tàu dự phòng..


---

### Nhóm hàm `BlinkHorizontalCells`

**Vai trò:** hiệu ứng nhấp nháy hàng ngang.

#### Dòng 512
```csharp
        private void BlinkHorizontalCells(Label[,] boardCells, int row, int col, int count, Color color, int totalTime, string soundFile, Action finished)
```
**Giải thích:** Khai báo hàm `BlinkHorizontalCells(Label[,] boardCells, int row, int col, int count, Color color, int totalTime, string soundFile, Action finished)`. Công dụng chính: hiệu ứng nhấp nháy hàng ngang.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 514
```csharp
            Label[] cells = new Label[count];
```
**Giải thích:** Tạo Label mới. Trong game, Label được dùng làm ô bàn cờ hoặc chữ hiển thị trạng thái. Ngữ cảnh: dòng này nằm trong `BlinkHorizontalCells`, tức phần hiệu ứng nhấp nháy hàng ngang..

#### Dòng 516
```csharp
            for (int i = 0; i < count; i++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `BlinkHorizontalCells`, tức phần hiệu ứng nhấp nháy hàng ngang..

#### Dòng 518
```csharp
                cells[i] = boardCells[row, col + i];
```
**Giải thích:** Cập nhật phần tử mảng `cells[i]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `BlinkHorizontalCells`, tức phần hiệu ứng nhấp nháy hàng ngang..

#### Dòng 521
```csharp
            BlinkCells(cells, color, totalTime, soundFile, false, finished);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `BlinkHorizontalCells`, tức phần hiệu ứng nhấp nháy hàng ngang..


---

### Nhóm hàm `BlinkCellsOnce`

**Vai trò:** đổi màu một nhóm ô trong một khoảng thời gian.

#### Dòng 524
```csharp
        private void BlinkCellsOnce(Label[] cells, Color color, int totalTime, string soundFile, Action finished)
```
**Giải thích:** Khai báo hàm `BlinkCellsOnce(Label[] cells, Color color, int totalTime, string soundFile, Action finished)`. Công dụng chính: đổi màu một nhóm ô trong một khoảng thời gian.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 526
```csharp
            PlaySoundTimed(soundFile, totalTime);
```
**Giải thích:** Gọi hàm `PlaySoundTimed` để phát âm thanh và dừng theo thời gian nếu cần. Ngữ cảnh: dòng này nằm trong `BlinkCellsOnce`, tức phần đổi màu một nhóm ô trong một khoảng thời gian..

#### Dòng 528
```csharp
            Color[] oldColors = new Color[cells.Length];
```
**Giải thích:** Cập nhật phần tử mảng `Color[] oldColors`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `BlinkCellsOnce`, tức phần đổi màu một nhóm ô trong một khoảng thời gian..

#### Dòng 530
```csharp
            for (int i = 0; i < cells.Length; i++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `BlinkCellsOnce`, tức phần đổi màu một nhóm ô trong một khoảng thời gian..

#### Dòng 532
```csharp
                oldColors[i] = cells[i].BackColor;
```
**Giải thích:** Cập nhật phần tử mảng `oldColors[i]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `BlinkCellsOnce`, tức phần đổi màu một nhóm ô trong một khoảng thời gian..

#### Dòng 533
```csharp
                cells[i].BackColor = color;
```
**Giải thích:** Cập nhật phần tử mảng `cells[i].BackColor`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `BlinkCellsOnce`, tức phần đổi màu một nhóm ô trong một khoảng thời gian..

#### Dòng 536
```csharp
            System.Windows.Forms.Timer timer = new System.Windows.Forms.Timer();
```
**Giải thích:** Tạo Timer WinForms để chạy hiệu ứng theo thời gian, ví dụ nhấp nháy, fade, chuyển lượt. Ngữ cảnh: dòng này nằm trong `BlinkCellsOnce`, tức phần đổi màu một nhóm ô trong một khoảng thời gian..

#### Dòng 537
```csharp
            timer.Interval = totalTime;
```
**Giải thích:** Gán `totalTime` cho `timer.Interval`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `BlinkCellsOnce`, tức phần đổi màu một nhóm ô trong một khoảng thời gian..

#### Dòng 538
```csharp
            timer.Tick += (sender, e) =>
```
**Giải thích:** Tạo lambda callback ngắn. Code này thường chạy khi Timer tick, thread chạy, hoặc event xảy ra. Ngữ cảnh: dòng này nằm trong `BlinkCellsOnce`, tức phần đổi màu một nhóm ô trong một khoảng thời gian..

#### Dòng 540
```csharp
                timer.Stop();
```
**Giải thích:** Dừng timer hoặc âm thanh đang chạy. Ngữ cảnh: dòng này nằm trong `BlinkCellsOnce`, tức phần đổi màu một nhóm ô trong một khoảng thời gian..

#### Dòng 541
```csharp
                timer.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `BlinkCellsOnce`, tức phần đổi màu một nhóm ô trong một khoảng thời gian..

#### Dòng 543
```csharp
                for (int i = 0; i < cells.Length; i++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `BlinkCellsOnce`, tức phần đổi màu một nhóm ô trong một khoảng thời gian..

#### Dòng 545
```csharp
                    if (cells[i] != null && !cells[i].IsDisposed)
```
**Giải thích:** Kiểm tra điều kiện `(cells[i] != null && !cells[i].IsDisposed`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `BlinkCellsOnce`, tức phần đổi màu một nhóm ô trong một khoảng thời gian..

#### Dòng 547
```csharp
                        cells[i].BackColor = oldColors[i];
```
**Giải thích:** Cập nhật phần tử mảng `cells[i].BackColor`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `BlinkCellsOnce`, tức phần đổi màu một nhóm ô trong một khoảng thời gian..

#### Dòng 551
```csharp
                if (finished != null)
```
**Giải thích:** Kiểm tra điều kiện `(finished != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `BlinkCellsOnce`, tức phần đổi màu một nhóm ô trong một khoảng thời gian..

#### Dòng 553
```csharp
                    finished();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `BlinkCellsOnce`, tức phần đổi màu một nhóm ô trong một khoảng thời gian..

#### Dòng 556
```csharp
            timer.Start();
```
**Giải thích:** Bắt đầu chạy thread, timer hoặc listener đã được tạo trước đó. Ngữ cảnh: dòng này nằm trong `BlinkCellsOnce`, tức phần đổi màu một nhóm ô trong một khoảng thời gian..


---

### Nhóm hàm `BlinkCells`

**Vai trò:** nhấp nháy nhiều lần bằng Timer.

#### Dòng 559
```csharp
        private void BlinkCells(Label[] cells, Color color, int totalTime, string soundFile, bool stopSoundAfterDuration, Action finished)
```
**Giải thích:** Khai báo hàm `BlinkCells(Label[] cells, Color color, int totalTime, string soundFile, bool stopSoundAfterDuration, Action finished)`. Công dụng chính: nhấp nháy nhiều lần bằng Timer.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 561
```csharp
            PlaySoundTimed(soundFile, stopSoundAfterDuration ? totalTime : 0);
```
**Giải thích:** Gọi hàm `PlaySoundTimed` để phát âm thanh và dừng theo thời gian nếu cần. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 563
```csharp
            int interval = 160;
```
**Giải thích:** Gán `160` cho `int interval`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 564
```csharp
            int maxTicks = Math.Max(1, totalTime / interval);
```
**Giải thích:** Gán `Math.Max(1, totalTime / interval)` cho `int maxTicks`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 565
```csharp
            int tick = 0;
```
**Giải thích:** Gán `0` cho `int tick`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 566
```csharp
            Color[] oldColors = new Color[cells.Length];
```
**Giải thích:** Cập nhật phần tử mảng `Color[] oldColors`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 568
```csharp
            for (int i = 0; i < cells.Length; i++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 570
```csharp
                oldColors[i] = cells[i].BackColor;
```
**Giải thích:** Cập nhật phần tử mảng `oldColors[i]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 573
```csharp
            System.Windows.Forms.Timer timer = new System.Windows.Forms.Timer();
```
**Giải thích:** Tạo Timer WinForms để chạy hiệu ứng theo thời gian, ví dụ nhấp nháy, fade, chuyển lượt. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 574
```csharp
            timer.Interval = interval;
```
**Giải thích:** Gán `interval` cho `timer.Interval`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 575
```csharp
            timer.Tick += (sender, e) =>
```
**Giải thích:** Tạo lambda callback ngắn. Code này thường chạy khi Timer tick, thread chạy, hoặc event xảy ra. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 577
```csharp
                tick++;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 578
```csharp
                bool turnOn = tick % 2 == 1;
```
**Giải thích:** Gán `tick % 2 == 1` cho `bool turnOn`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 580
```csharp
                for (int i = 0; i < cells.Length; i++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 582
```csharp
                    if (cells[i] != null && !cells[i].IsDisposed)
```
**Giải thích:** Kiểm tra điều kiện `(cells[i] != null && !cells[i].IsDisposed`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 584
```csharp
                        cells[i].BackColor = turnOn ? color : Color.Transparent;
```
**Giải thích:** Cập nhật phần tử mảng `cells[i].BackColor`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 588
```csharp
                if (tick >= maxTicks)
```
**Giải thích:** Kiểm tra điều kiện `(tick >= maxTicks`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 590
```csharp
                    timer.Stop();
```
**Giải thích:** Dừng timer hoặc âm thanh đang chạy. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 591
```csharp
                    timer.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 593
```csharp
                    for (int i = 0; i < cells.Length; i++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 595
```csharp
                        if (cells[i] != null && !cells[i].IsDisposed)
```
**Giải thích:** Kiểm tra điều kiện `(cells[i] != null && !cells[i].IsDisposed`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 597
```csharp
                            cells[i].BackColor = oldColors[i];
```
**Giải thích:** Cập nhật phần tử mảng `cells[i].BackColor`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 601
```csharp
                    if (finished != null)
```
**Giải thích:** Kiểm tra điều kiện `(finished != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 603
```csharp
                        finished();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..

#### Dòng 608
```csharp
            timer.Start();
```
**Giải thích:** Bắt đầu chạy thread, timer hoặc listener đã được tạo trước đó. Ngữ cảnh: dòng này nằm trong `BlinkCells`, tức phần nhấp nháy nhiều lần bằng Timer..


---

### Nhóm hàm `PlaySoundTimed`

**Vai trò:** phát âm thanh và có thể dừng sau một khoảng thời gian.

#### Dòng 611
```csharp
        private void PlaySoundTimed(string fileName, int stopAfterMilliseconds)
```
**Giải thích:** Khai báo hàm `PlaySoundTimed(string fileName, int stopAfterMilliseconds)`. Công dụng chính: phát âm thanh và có thể dừng sau một khoảng thời gian.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 613
```csharp
            try
```
**Giải thích:** Bắt đầu khối có thể phát sinh lỗi như kết nối mạng, parse dữ liệu, đọc ảnh/âm thanh. Ngữ cảnh: dòng này nằm trong `PlaySoundTimed`, tức phần phát âm thanh và có thể dừng sau một khoảng thời gian..

#### Dòng 615
```csharp
                string soundPath = FindSoundFile(fileName);
```
**Giải thích:** Gán `FindSoundFile(fileName)` cho `string soundPath`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `PlaySoundTimed`, tức phần phát âm thanh và có thể dừng sau một khoảng thời gian..

#### Dòng 617
```csharp
                if (soundPath == null)
```
**Giải thích:** Kiểm tra điều kiện `(soundPath == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `PlaySoundTimed`, tức phần phát âm thanh và có thể dừng sau một khoảng thời gian..

#### Dòng 619
```csharp
                    return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `PlaySoundTimed`, tức phần phát âm thanh và có thể dừng sau một khoảng thời gian..

#### Dòng 622
```csharp
                SoundPlayer player = new SoundPlayer(soundPath);
```
**Giải thích:** Tạo SoundPlayer từ file wav. Đối tượng này dùng để phát âm thanh game. Ngữ cảnh: dòng này nằm trong `PlaySoundTimed`, tức phần phát âm thanh và có thể dừng sau một khoảng thời gian..

#### Dòng 623
```csharp
                player.Play();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `PlaySoundTimed`, tức phần phát âm thanh và có thể dừng sau một khoảng thời gian..

#### Dòng 625
```csharp
                if (stopAfterMilliseconds > 0)
```
**Giải thích:** Kiểm tra điều kiện `(stopAfterMilliseconds > 0`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `PlaySoundTimed`, tức phần phát âm thanh và có thể dừng sau một khoảng thời gian..

#### Dòng 627
```csharp
                    System.Windows.Forms.Timer stopTimer = new System.Windows.Forms.Timer();
```
**Giải thích:** Tạo Timer WinForms để chạy hiệu ứng theo thời gian, ví dụ nhấp nháy, fade, chuyển lượt. Ngữ cảnh: dòng này nằm trong `PlaySoundTimed`, tức phần phát âm thanh và có thể dừng sau một khoảng thời gian..

#### Dòng 628
```csharp
                    stopTimer.Interval = stopAfterMilliseconds;
```
**Giải thích:** Gán `stopAfterMilliseconds` cho `stopTimer.Interval`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `PlaySoundTimed`, tức phần phát âm thanh và có thể dừng sau một khoảng thời gian..

#### Dòng 629
```csharp
                    stopTimer.Tick += (sender, e) =>
```
**Giải thích:** Tạo lambda callback ngắn. Code này thường chạy khi Timer tick, thread chạy, hoặc event xảy ra. Ngữ cảnh: dòng này nằm trong `PlaySoundTimed`, tức phần phát âm thanh và có thể dừng sau một khoảng thời gian..

#### Dòng 631
```csharp
                        stopTimer.Stop();
```
**Giải thích:** Dừng timer hoặc âm thanh đang chạy. Ngữ cảnh: dòng này nằm trong `PlaySoundTimed`, tức phần phát âm thanh và có thể dừng sau một khoảng thời gian..

#### Dòng 632
```csharp
                        stopTimer.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `PlaySoundTimed`, tức phần phát âm thanh và có thể dừng sau một khoảng thời gian..

#### Dòng 633
```csharp
                        player.Stop();
```
**Giải thích:** Dừng timer hoặc âm thanh đang chạy. Ngữ cảnh: dòng này nằm trong `PlaySoundTimed`, tức phần phát âm thanh và có thể dừng sau một khoảng thời gian..

#### Dòng 634
```csharp
                        player.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `PlaySoundTimed`, tức phần phát âm thanh và có thể dừng sau một khoảng thời gian..

#### Dòng 636
```csharp
                    stopTimer.Start();
```
**Giải thích:** Bắt đầu chạy thread, timer hoặc listener đã được tạo trước đó. Ngữ cảnh: dòng này nằm trong `PlaySoundTimed`, tức phần phát âm thanh và có thể dừng sau một khoảng thời gian..

#### Dòng 639
```csharp
            catch
```
**Giải thích:** Bắt lỗi để chương trình không bị crash; thường hiển thị thông báo hoặc bỏ qua lỗi nhỏ. Ngữ cảnh: dòng này nằm trong `PlaySoundTimed`, tức phần phát âm thanh và có thể dừng sau một khoảng thời gian..


---

### Nhóm hàm `StartTurnTransition`

**Vai trò:** hiệu ứng chuyển lượt, khóa thao tác 2 giây.

#### Dòng 645
```csharp
        private void StartTurnTransition(bool isMyTurnNow)
```
**Giải thích:** Khai báo hàm `StartTurnTransition(bool isMyTurnNow)`. Công dụng chính: hiệu ứng chuyển lượt, khóa thao tác 2 giây.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 647
```csharp
            if (gameEnded)
```
**Giải thích:** Kiểm tra điều kiện `(gameEnded`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 649
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 652
```csharp
            actionLocked = true;
```
**Giải thích:** Gán giá trị mới cho `actionLocked`. khóa thao tác trong lúc animation/delay/skill đang chạy để tránh click sai thời điểm. Giá trị `true` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 653
```csharp
            turnTransitionRunning = true;
```
**Giải thích:** Gán giá trị mới cho `turnTransitionRunning`. cho biết animation chuyển lượt đang chạy. Giá trị `true` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 654
```csharp
            myTurn = false;
```
**Giải thích:** Gán giá trị mới cho `myTurn`. cho biết hiện tại có phải lượt bắn của mình. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 656
```csharp
            lblTurn.Text = "ĐANG CHUYỂN LƯỢT";
```
**Giải thích:** Đổi chữ hiển thị của `lblTurn`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 657
```csharp
            lblStatus.Text = isMyTurnNow ? "TRẠNG THÁI: SẮP ĐẾN LƯỢT BẠN" : "TRẠNG THÁI: SẮP ĐẾN LƯỢT ĐỐI THỦ";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 659
```csharp
            Label banner = lblTurnBanner;
```
**Giải thích:** Gán `lblTurnBanner` cho `Label banner`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 660
```csharp
            banner.Text = isMyTurnNow ? "LƯỢT CỦA BẠN" : "LƯỢT ĐỐI THỦ";
```
**Giải thích:** Đổi chữ hiển thị của `banner`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 661
```csharp
            banner.Visible = true;
```
**Giải thích:** Ẩn/hiện `banner`. Ví dụ nút Chơi lại chỉ hiện khi trận kết thúc. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 662
```csharp
            banner.Font = new Font("Segoe UI Black", 22F, FontStyle.Bold);
```
**Giải thích:** Tạo Font để chỉnh kiểu chữ, kích thước và độ đậm cho control. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 663
```csharp
            banner.ForeColor = Color.White;
```
**Giải thích:** Đổi màu chữ của `banner` để dễ nhìn trên nền hiện tại. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 664
```csharp
            banner.Left = (this.ClientSize.Width - banner.Width) / 2;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 665
```csharp
            banner.Top = (this.ClientSize.Height - banner.Height) / 2;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 666
```csharp
            banner.BringToFront();
```
**Giải thích:** Đưa control lên trên cùng để không bị panel/ảnh khác che. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 668
```csharp
            int step = 0;
```
**Giải thích:** Gán `0` cho `int step`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 669
```csharp
            System.Windows.Forms.Timer timer = new System.Windows.Forms.Timer();
```
**Giải thích:** Tạo Timer WinForms để chạy hiệu ứng theo thời gian, ví dụ nhấp nháy, fade, chuyển lượt. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 670
```csharp
            timer.Interval = TURN_TRANSITION_TOTAL_TIME / TURN_TRANSITION_STEPS;
```
**Giải thích:** Gán `TURN_TRANSITION_TOTAL_TIME / TURN_TRANSITION_STEPS` cho `timer.Interval`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 671
```csharp
            timer.Tick += (sender, e) =>
```
**Giải thích:** Tạo lambda callback ngắn. Code này thường chạy khi Timer tick, thread chạy, hoặc event xảy ra. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 673
```csharp
                if (banner.IsDisposed)
```
**Giải thích:** Kiểm tra điều kiện `(banner.IsDisposed`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 675
```csharp
                    timer.Stop();
```
**Giải thích:** Dừng timer hoặc âm thanh đang chạy. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 676
```csharp
                    timer.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 677
```csharp
                    actionLocked = false;
```
**Giải thích:** Gán giá trị mới cho `actionLocked`. khóa thao tác trong lúc animation/delay/skill đang chạy để tránh click sai thời điểm. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 678
```csharp
                    turnTransitionRunning = false;
```
**Giải thích:** Gán giá trị mới cho `turnTransitionRunning`. cho biết animation chuyển lượt đang chạy. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 679
```csharp
                    return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 682
```csharp
                step++;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 683
```csharp
                double t = step / (double)TURN_TRANSITION_STEPS;
```
**Giải thích:** Gán `step / (double)TURN_TRANSITION_STEPS` cho `double t`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 685
```csharp
                if (t > 1.0)
```
**Giải thích:** Kiểm tra điều kiện `(t > 1.0`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 687
```csharp
                    t = 1.0;
```
**Giải thích:** Gán `1.0` cho `t`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 690
```csharp
                if (t <= 0.5)
```
**Giải thích:** Kiểm tra điều kiện `(t <= 0.5`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 692
```csharp
                    double appear = t / 0.5;
```
**Giải thích:** Gán `t / 0.5` cho `double appear`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 693
```csharp
                    float fontSize = 22F + (float)(12F * appear);
```
**Giải thích:** Gán `22F + (float)(12F * appear)` cho `float fontSize`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 694
```csharp
                    banner.Font = new Font("Segoe UI Black", fontSize, FontStyle.Bold);
```
**Giải thích:** Tạo Font để chỉnh kiểu chữ, kích thước và độ đậm cho control. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 695
```csharp
                    banner.Top = (this.ClientSize.Height - banner.Height) / 2 - (int)(30 * (1.0 - appear));
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 696
```csharp
                    banner.ForeColor = Color.White;
```
**Giải thích:** Đổi màu chữ của `banner` để dễ nhìn trên nền hiện tại. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 698
```csharp
                else
```
**Giải thích:** Nhánh xử lý còn lại khi các điều kiện phía trước đều sai. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 700
```csharp
                    double disappear = (t - 0.5) / 0.5;
```
**Giải thích:** Gán `(t - 0.5) / 0.5` cho `double disappear`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 701
```csharp
                    int shade = 255 - (int)(190 * disappear);
```
**Giải thích:** Gán `255 - (int)(190 * disappear)` cho `int shade`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 702
```csharp
                    if (shade < 65)
```
**Giải thích:** Kiểm tra điều kiện `(shade < 65`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 704
```csharp
                        shade = 65;
```
**Giải thích:** Gán `65` cho `shade`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 707
```csharp
                    banner.ForeColor = Color.FromArgb(shade, shade, shade);
```
**Giải thích:** Đổi màu chữ của `banner` để dễ nhìn trên nền hiện tại. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 708
```csharp
                    banner.Top = (this.ClientSize.Height - banner.Height) / 2 + (int)(25 * disappear);
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 711
```csharp
                banner.Left = (this.ClientSize.Width - banner.Width) / 2;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 713
```csharp
                if (step >= TURN_TRANSITION_STEPS)
```
**Giải thích:** Kiểm tra điều kiện `(step >= TURN_TRANSITION_STEPS`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 715
```csharp
                    timer.Stop();
```
**Giải thích:** Dừng timer hoặc âm thanh đang chạy. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 716
```csharp
                    timer.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 718
```csharp
                    banner.Visible = false;
```
**Giải thích:** Ẩn/hiện `banner`. Ví dụ nút Chơi lại chỉ hiện khi trận kết thúc. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 719
```csharp
                    banner.ForeColor = Color.White;
```
**Giải thích:** Đổi màu chữ của `banner` để dễ nhìn trên nền hiện tại. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 721
```csharp
                    myTurn = isMyTurnNow;
```
**Giải thích:** Gán giá trị mới cho `myTurn`. cho biết hiện tại có phải lượt bắn của mình. Giá trị `isMyTurnNow` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 722
```csharp
                    actionLocked = false;
```
**Giải thích:** Gán giá trị mới cho `actionLocked`. khóa thao tác trong lúc animation/delay/skill đang chạy để tránh click sai thời điểm. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 723
```csharp
                    turnTransitionRunning = false;
```
**Giải thích:** Gán giá trị mới cho `turnTransitionRunning`. cho biết animation chuyển lượt đang chạy. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 725
```csharp
                    UpdateSkillMenuState();
```
**Giải thích:** Gọi hàm `UpdateSkillMenuState` để làm mới trạng thái icon skill. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 727
```csharp
                    if (isMyTurnNow)
```
**Giải thích:** Kiểm tra điều kiện `(isMyTurnNow`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 729
```csharp
                        lblTurn.Text = "LƯỢT: BẠN";
```
**Giải thích:** Đổi chữ hiển thị của `lblTurn`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 730
```csharp
                        lblStatus.Text = "TRẠNG THÁI: HÃY BẮN";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 732
```csharp
                    else
```
**Giải thích:** Nhánh xử lý còn lại khi các điều kiện phía trước đều sai. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 734
```csharp
                        lblTurn.Text = "LƯỢT: ĐỐI THỦ";
```
**Giải thích:** Đổi chữ hiển thị của `lblTurn`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 735
```csharp
                        lblStatus.Text = "TRẠNG THÁI: CHỜ ĐỐI THỦ";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..

#### Dòng 740
```csharp
            timer.Start();
```
**Giải thích:** Bắt đầu chạy thread, timer hoặc listener đã được tạo trước đó. Ngữ cảnh: dòng này nằm trong `StartTurnTransition`, tức phần hiệu ứng chuyển lượt, khóa thao tác 2 giây..


---

### Nhóm hàm `CreateShipDock`

**Vai trò:** tạo khu vực chọn/kéo tàu.

#### Dòng 743
```csharp
        private void CreateShipDock()
```
**Giải thích:** Khai báo hàm `CreateShipDock()`. Công dụng chính: tạo khu vực chọn/kéo tàu.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 746
```csharp
            panelShipDock.Controls.Clear();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 748
```csharp
            int[] slotWidths = new int[] { 65, 80, 100 };
```
**Giải thích:** Cập nhật phần tử mảng `int[] slotWidths`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 749
```csharp
            int x = 8;
```
**Giải thích:** Gán `8` cho `int x`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 751
```csharp
            for (int shipId = 0; shipId < BASE_SHIP_COUNT; shipId++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 753
```csharp
                PictureBox pic = new PictureBox();
```
**Giải thích:** Tạo PictureBox để hiển thị ảnh động/tàu/hit/miss trên form hoặc panel. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 754
```csharp
                pic.Tag = shipId;
```
**Giải thích:** Lưu dữ liệu phụ vào control. Game dùng Tag để nhớ tọa độ ô, mã tàu hoặc tên skill khi kéo thả/click. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 755
```csharp
                pic.Left = x;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 756
```csharp
                pic.Top = 6;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 757
```csharp
                pic.Width = slotWidths[shipId];
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 758
```csharp
                pic.Height = 38;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 759
```csharp
                pic.SizeMode = PictureBoxSizeMode.Zoom;
```
**Giải thích:** Gán `PictureBoxSizeMode.Zoom` cho `pic.SizeMode`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 760
```csharp
                pic.BorderStyle = BorderStyle.FixedSingle;
```
**Giải thích:** Gán `BorderStyle.FixedSingle` cho `pic.BorderStyle`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 761
```csharp
                pic.BackColor = Color.Transparent;
```
**Giải thích:** Đổi màu nền của `pic`. Game dùng màu để thể hiện hiệu ứng, trạng thái hit/miss, skill hoặc thắng/thua. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 762
```csharp
                pic.Cursor = Cursors.Hand;
```
**Giải thích:** Đổi hình con trỏ chuột, ví dụ Hand khi có thể dùng skill, No khi bị khóa. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 763
```csharp
                pic.Image = PrepareShipImage(shipId, isHorizontal);
```
**Giải thích:** Gán ảnh cho `pic` để control hiển thị hình tương ứng. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 764
```csharp
                pic.MouseDown += ShipDock_MouseDown;
```
**Giải thích:** Gán `ShipDock_MouseDown` cho `pic.MouseDown +`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 766
```csharp
                ToolTip tip = new ToolTip();
```
**Giải thích:** Tạo tooltip để khi rê chuột lên icon skill sẽ hiện mô tả ngắn. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 767
```csharp
                tip.SetToolTip(pic, "Tàu " + shipSizes[shipId] + " ô");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 769
```csharp
                panelShipDock.Controls.Add(pic);
```
**Giải thích:** Thêm control/dữ liệu vào collection. Với giao diện, control sẽ xuất hiện trên panel/form. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 770
```csharp
                shipDockPictures[shipId] = pic;
```
**Giải thích:** Cập nhật phần tử mảng `shipDockPictures[shipId]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 772
```csharp
                Label name = new Label();
```
**Giải thích:** Tạo Label mới. Trong game, Label được dùng làm ô bàn cờ hoặc chữ hiển thị trạng thái. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 773
```csharp
                name.Text = shipSizes[shipId] + " ô";
```
**Giải thích:** Đổi chữ hiển thị của `name`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 774
```csharp
                name.Left = pic.Left;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 775
```csharp
                name.Top = 42;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 776
```csharp
                name.Width = pic.Width;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 777
```csharp
                name.Height = 14;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 778
```csharp
                name.TextAlign = ContentAlignment.MiddleCenter;
```
**Giải thích:** Gán `ContentAlignment.MiddleCenter` cho `name.TextAlign`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 779
```csharp
                name.Font = new Font("Segoe UI", 8, FontStyle.Bold);
```
**Giải thích:** Tạo Font để chỉnh kiểu chữ, kích thước và độ đậm cho control. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 780
```csharp
                name.ForeColor = Color.Black;
```
**Giải thích:** Đổi màu chữ của `name` để dễ nhìn trên nền hiện tại. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 781
```csharp
                name.BackColor = Color.Transparent;
```
**Giải thích:** Đổi màu nền của `name`. Game dùng màu để thể hiện hiệu ứng, trạng thái hit/miss, skill hoặc thắng/thua. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 782
```csharp
                panelShipDock.Controls.Add(name);
```
**Giải thích:** Thêm control/dữ liệu vào collection. Với giao diện, control sẽ xuất hiện trên panel/form. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..

#### Dòng 784
```csharp
                x += slotWidths[shipId] + 12;
```
**Giải thích:** Gán `slotWidths[shipId] + 12` cho `x +`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateShipDock`, tức phần tạo khu vực chọn/kéo tàu..


---

### Nhóm hàm `CreateBoards`

**Vai trò:** tạo bàn mình và bàn địch bằng Label.

#### Dòng 788
```csharp
        private void CreateBoards()
```
**Giải thích:** Khai báo hàm `CreateBoards()`. Công dụng chính: tạo bàn mình và bàn địch bằng Label.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 790
```csharp
            panelEnemyBoard.AllowDrop = true;
```
**Giải thích:** Gán `true` cho `panelEnemyBoard.AllowDrop`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 791
```csharp
            panelEnemyBoard.DragEnter += Board_DragEnter;
```
**Giải thích:** Gán `Board_DragEnter` cho `panelEnemyBoard.DragEnter +`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 792
```csharp
            panelEnemyBoard.DragDrop += EnemyBoard_DragDrop;
```
**Giải thích:** Gán `EnemyBoard_DragDrop` cho `panelEnemyBoard.DragDrop +`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 794
```csharp
            int ownCellWidth = panelOwnBoard.ClientSize.Width / BOARD_SIZE;
```
**Giải thích:** Gán `panelOwnBoard.ClientSize.Width / BOARD_SIZE` cho `int ownCellWidth`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 795
```csharp
            int ownCellHeight = panelOwnBoard.ClientSize.Height / BOARD_SIZE;
```
**Giải thích:** Gán `panelOwnBoard.ClientSize.Height / BOARD_SIZE` cho `int ownCellHeight`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 797
```csharp
            int enemyCellWidth = panelEnemyBoard.ClientSize.Width / BOARD_SIZE;
```
**Giải thích:** Gán `panelEnemyBoard.ClientSize.Width / BOARD_SIZE` cho `int enemyCellWidth`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 798
```csharp
            int enemyCellHeight = panelEnemyBoard.ClientSize.Height / BOARD_SIZE;
```
**Giải thích:** Gán `panelEnemyBoard.ClientSize.Height / BOARD_SIZE` cho `int enemyCellHeight`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 800
```csharp
            for (int row = 0; row < BOARD_SIZE; row++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 802
```csharp
                for (int col = 0; col < BOARD_SIZE; col++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 804
```csharp
                    Label ownCell = new BoardCellLabel();
```
**Giải thích:** Tạo Label mới. Trong game, Label được dùng làm ô bàn cờ hoặc chữ hiển thị trạng thái. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 805
```csharp
                    ownCell.Width = ownCellWidth;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 806
```csharp
                    ownCell.Height = ownCellHeight;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 807
```csharp
                    ownCell.Left = col * ownCellWidth;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 808
```csharp
                    ownCell.Top = row * ownCellHeight;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 809
```csharp
                    ownCell.Tag = new Point(row, col);
```
**Giải thích:** Lưu dữ liệu phụ vào control. Game dùng Tag để nhớ tọa độ ô, mã tàu hoặc tên skill khi kéo thả/click. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 810
```csharp
                    ownCell.AllowDrop = true;
```
**Giải thích:** Gán `true` cho `ownCell.AllowDrop`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 811
```csharp
                    StyleBoardCell(ownCell);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 812
```csharp
                    ownCell.Click += OwnCell_Click;
```
**Giải thích:** Gán `OwnCell_Click` cho `ownCell.Click +`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 813
```csharp
                    ownCell.DragEnter += Board_DragEnter;
```
**Giải thích:** Gán `Board_DragEnter` cho `ownCell.DragEnter +`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 814
```csharp
                    ownCell.DragDrop += OwnCell_DragDrop;
```
**Giải thích:** Gán `OwnCell_DragDrop` cho `ownCell.DragDrop +`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 815
```csharp
                    panelOwnBoard.Controls.Add(ownCell);
```
**Giải thích:** Thêm control/dữ liệu vào collection. Với giao diện, control sẽ xuất hiện trên panel/form. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 816
```csharp
                    ownCells[row, col] = ownCell;
```
**Giải thích:** Cập nhật phần tử mảng `ownCells[row, col]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 818
```csharp
                    Label enemyCell = new BoardCellLabel();
```
**Giải thích:** Tạo Label mới. Trong game, Label được dùng làm ô bàn cờ hoặc chữ hiển thị trạng thái. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 819
```csharp
                    enemyCell.Width = enemyCellWidth;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 820
```csharp
                    enemyCell.Height = enemyCellHeight;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 821
```csharp
                    enemyCell.Left = col * enemyCellWidth;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 822
```csharp
                    enemyCell.Top = row * enemyCellHeight;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 823
```csharp
                    enemyCell.Tag = new Point(row, col);
```
**Giải thích:** Lưu dữ liệu phụ vào control. Game dùng Tag để nhớ tọa độ ô, mã tàu hoặc tên skill khi kéo thả/click. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 824
```csharp
                    enemyCell.AllowDrop = true;
```
**Giải thích:** Gán `true` cho `enemyCell.AllowDrop`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 825
```csharp
                    StyleBoardCell(enemyCell);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 826
```csharp
                    enemyCell.Click += EnemyCell_Click;
```
**Giải thích:** Gán `EnemyCell_Click` cho `enemyCell.Click +`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 827
```csharp
                    enemyCell.DragEnter += Board_DragEnter;
```
**Giải thích:** Gán `Board_DragEnter` cho `enemyCell.DragEnter +`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 828
```csharp
                    enemyCell.DragDrop += EnemyCell_DragDrop;
```
**Giải thích:** Gán `EnemyCell_DragDrop` cho `enemyCell.DragDrop +`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 829
```csharp
                    panelEnemyBoard.Controls.Add(enemyCell);
```
**Giải thích:** Thêm control/dữ liệu vào collection. Với giao diện, control sẽ xuất hiện trên panel/form. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..

#### Dòng 830
```csharp
                    enemyCells[row, col] = enemyCell;
```
**Giải thích:** Cập nhật phần tử mảng `enemyCells[row, col]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `CreateBoards`, tức phần tạo bàn mình và bàn địch bằng Label..


---

### Nhóm hàm `StyleBoardCell`

**Vai trò:** style một ô bàn cờ.

#### Dòng 835
```csharp
        private void StyleBoardCell(Label cell)
```
**Giải thích:** Khai báo hàm `StyleBoardCell(Label cell)`. Công dụng chính: style một ô bàn cờ.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 837
```csharp
            cell.BorderStyle = BorderStyle.None;
```
**Giải thích:** Gán `BorderStyle.None` cho `cell.BorderStyle`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `StyleBoardCell`, tức phần style một ô bàn cờ..

#### Dòng 838
```csharp
            cell.BackColor = Color.Transparent;
```
**Giải thích:** Đổi màu nền của `cell`. Game dùng màu để thể hiện hiệu ứng, trạng thái hit/miss, skill hoặc thắng/thua. Ngữ cảnh: dòng này nằm trong `StyleBoardCell`, tức phần style một ô bàn cờ..

#### Dòng 839
```csharp
            cell.Text = "";
```
**Giải thích:** Đổi chữ hiển thị của `cell`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `StyleBoardCell`, tức phần style một ô bàn cờ..

#### Dòng 840
```csharp
            cell.TextAlign = ContentAlignment.MiddleCenter;
```
**Giải thích:** Gán `ContentAlignment.MiddleCenter` cho `cell.TextAlign`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `StyleBoardCell`, tức phần style một ô bàn cờ..

#### Dòng 841
```csharp
            cell.Font = new Font("Segoe UI Emoji", 18, FontStyle.Bold);
```
**Giải thích:** Tạo Font để chỉnh kiểu chữ, kích thước và độ đậm cho control. Ngữ cảnh: dòng này nằm trong `StyleBoardCell`, tức phần style một ô bàn cờ..

#### Dòng 842
```csharp
            cell.ForeColor = Color.Black;
```
**Giải thích:** Đổi màu chữ của `cell` để dễ nhìn trên nền hiện tại. Ngữ cảnh: dòng này nằm trong `StyleBoardCell`, tức phần style một ô bàn cờ..


---

### Nhóm hàm `ShipDock_MouseDown`

**Vai trò:** kéo tàu từ dock.

#### Dòng 845
```csharp
        private void ShipDock_MouseDown(object sender, MouseEventArgs e)
```
**Giải thích:** Khai báo hàm `ShipDock_MouseDown(object sender, MouseEventArgs e)`. Công dụng chính: kéo tàu từ dock.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 847
```csharp
            if (ready)
```
**Giải thích:** Kiểm tra điều kiện `(ready`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ShipDock_MouseDown`, tức phần kéo tàu từ dock..

#### Dòng 849
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ShipDock_MouseDown`, tức phần kéo tàu từ dock..

#### Dòng 852
```csharp
            PictureBox pic = sender as PictureBox;
```
**Giải thích:** Gán `sender as PictureBox` cho `PictureBox pic`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ShipDock_MouseDown`, tức phần kéo tàu từ dock..

#### Dòng 854
```csharp
            if (pic == null)
```
**Giải thích:** Kiểm tra điều kiện `(pic == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ShipDock_MouseDown`, tức phần kéo tàu từ dock..

#### Dòng 856
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ShipDock_MouseDown`, tức phần kéo tàu từ dock..

#### Dòng 859
```csharp
            int shipId = (int)pic.Tag;
```
**Giải thích:** Gán `(int)pic.Tag` cho `int shipId`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ShipDock_MouseDown`, tức phần kéo tàu từ dock..

#### Dòng 861
```csharp
            if (e.Button == MouseButtons.Left)
```
**Giải thích:** Kiểm tra điều kiện `(e.Button == MouseButtons.Left`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ShipDock_MouseDown`, tức phần kéo tàu từ dock..

#### Dòng 863
```csharp
                pic.DoDragDrop("SHIP|" + shipId, DragDropEffects.Copy);
```
**Giải thích:** Bắt đầu thao tác kéo thả. Dữ liệu trong DoDragDrop sẽ đi theo chuột đến nơi người chơi thả. Ngữ cảnh: dòng này nằm trong `ShipDock_MouseDown`, tức phần kéo tàu từ dock..


---

### Nhóm hàm `PlacedShip_MouseDown`

**Vai trò:** kéo tàu đã đặt để đặt lại.

#### Dòng 867
```csharp
        private void PlacedShip_MouseDown(object sender, MouseEventArgs e)
```
**Giải thích:** Khai báo hàm `PlacedShip_MouseDown(object sender, MouseEventArgs e)`. Công dụng chính: kéo tàu đã đặt để đặt lại.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 869
```csharp
            if (ready)
```
**Giải thích:** Kiểm tra điều kiện `(ready`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `PlacedShip_MouseDown`, tức phần kéo tàu đã đặt để đặt lại..

#### Dòng 871
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `PlacedShip_MouseDown`, tức phần kéo tàu đã đặt để đặt lại..

#### Dòng 874
```csharp
            PictureBox pic = sender as PictureBox;
```
**Giải thích:** Gán `sender as PictureBox` cho `PictureBox pic`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `PlacedShip_MouseDown`, tức phần kéo tàu đã đặt để đặt lại..

#### Dòng 876
```csharp
            if (pic == null || pic.Tag == null)
```
**Giải thích:** Kiểm tra điều kiện `(pic == null || pic.Tag == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `PlacedShip_MouseDown`, tức phần kéo tàu đã đặt để đặt lại..

#### Dòng 878
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `PlacedShip_MouseDown`, tức phần kéo tàu đã đặt để đặt lại..

#### Dòng 881
```csharp
            int shipId = GetShipIdFromVisualTag(pic.Tag.ToString());
```
**Giải thích:** Gán `GetShipIdFromVisualTag(pic.Tag.ToString())` cho `int shipId`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `PlacedShip_MouseDown`, tức phần kéo tàu đã đặt để đặt lại..

#### Dòng 883
```csharp
            if (shipId < 0)
```
**Giải thích:** Kiểm tra điều kiện `(shipId < 0`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `PlacedShip_MouseDown`, tức phần kéo tàu đã đặt để đặt lại..

#### Dòng 885
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `PlacedShip_MouseDown`, tức phần kéo tàu đã đặt để đặt lại..

#### Dòng 889
```csharp
            if (e.Button == MouseButtons.Left)
```
**Giải thích:** Kiểm tra điều kiện `(e.Button == MouseButtons.Left`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `PlacedShip_MouseDown`, tức phần kéo tàu đã đặt để đặt lại..

#### Dòng 891
```csharp
                pic.DoDragDrop("SHIP|" + shipId, DragDropEffects.Move);
```
**Giải thích:** Bắt đầu thao tác kéo thả. Dữ liệu trong DoDragDrop sẽ đi theo chuột đến nơi người chơi thả. Ngữ cảnh: dòng này nằm trong `PlacedShip_MouseDown`, tức phần kéo tàu đã đặt để đặt lại..


---

### Nhóm hàm `GetShipIdFromVisualTag`

**Vai trò:** lấy mã tàu từ Tag của PictureBox.

#### Dòng 895
```csharp
        private int GetShipIdFromVisualTag(string tag)
```
**Giải thích:** Khai báo hàm `GetShipIdFromVisualTag(string tag)`. Công dụng chính: lấy mã tàu từ Tag của PictureBox.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 897
```csharp
            if (tag == null)
```
**Giải thích:** Kiểm tra điều kiện `(tag == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `GetShipIdFromVisualTag`, tức phần lấy mã tàu từ Tag của PictureBox..

#### Dòng 899
```csharp
                return -1;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `GetShipIdFromVisualTag`, tức phần lấy mã tàu từ Tag của PictureBox..

#### Dòng 902
```csharp
            if (tag.StartsWith("VISUAL_SHIP_"))
```
**Giải thích:** Kiểm tra điều kiện `(tag.StartsWith("VISUAL_SHIP_")`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `GetShipIdFromVisualTag`, tức phần lấy mã tàu từ Tag của PictureBox..

#### Dòng 904
```csharp
                string number = tag.Replace("VISUAL_SHIP_", "");
```
**Giải thích:** Gán `tag.Replace("VISUAL_SHIP_", "")` cho `string number`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `GetShipIdFromVisualTag`, tức phần lấy mã tàu từ Tag của PictureBox..

#### Dòng 905
```csharp
                int shipId;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `GetShipIdFromVisualTag`, tức phần lấy mã tàu từ Tag của PictureBox..

#### Dòng 907
```csharp
                if (int.TryParse(number, out shipId))
```
**Giải thích:** Kiểm tra điều kiện `(int.TryParse(number, out shipId)`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `GetShipIdFromVisualTag`, tức phần lấy mã tàu từ Tag của PictureBox..

#### Dòng 909
```csharp
                    return shipId;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `GetShipIdFromVisualTag`, tức phần lấy mã tàu từ Tag của PictureBox..

#### Dòng 913
```csharp
            return -1;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `GetShipIdFromVisualTag`, tức phần lấy mã tàu từ Tag của PictureBox..


---

### Nhóm hàm `Board_DragEnter`

**Vai trò:** cho phép kéo thả vào bàn cờ.

#### Dòng 916
```csharp
        private void Board_DragEnter(object sender, DragEventArgs e)
```
**Giải thích:** Khai báo hàm `Board_DragEnter(object sender, DragEventArgs e)`. Công dụng chính: cho phép kéo thả vào bàn cờ.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 918
```csharp
            int shipId;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `Board_DragEnter`, tức phần cho phép kéo thả vào bàn cờ..

#### Dòng 920
```csharp
            if (TryGetDraggedShipId(e, out shipId))
```
**Giải thích:** Kiểm tra điều kiện `(TryGetDraggedShipId(e, out shipId)`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `Board_DragEnter`, tức phần cho phép kéo thả vào bàn cờ..

#### Dòng 922
```csharp
                if (!ready && shipId >= 0 && shipId < BASE_SHIP_COUNT)
```
**Giải thích:** Kiểm tra điều kiện `(!ready && shipId >= 0 && shipId < BASE_SHIP_COUNT`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `Board_DragEnter`, tức phần cho phép kéo thả vào bàn cờ..

#### Dòng 924
```csharp
                    e.Effect = shipPlaced[shipId] ? DragDropEffects.Move : DragDropEffects.Copy;
```
**Giải thích:** Gán `shipPlaced[shipId] ? DragDropEffects.Move : DragDropEffects.Copy` cho `e.Effect`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `Board_DragEnter`, tức phần cho phép kéo thả vào bàn cờ..

#### Dòng 925
```csharp
                    return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `Board_DragEnter`, tức phần cho phép kéo thả vào bàn cờ..

#### Dòng 929
```csharp
            string skillName;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `Board_DragEnter`, tức phần cho phép kéo thả vào bàn cờ..

#### Dòng 931
```csharp
            if (TryGetDraggedSkill(e, out skillName))
```
**Giải thích:** Kiểm tra điều kiện `(TryGetDraggedSkill(e, out skillName)`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `Board_DragEnter`, tức phần cho phép kéo thả vào bàn cờ..

#### Dòng 933
```csharp
                Control targetControl = sender as Control;
```
**Giải thích:** Gán `sender as Control` cho `Control targetControl`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `Board_DragEnter`, tức phần cho phép kéo thả vào bàn cờ..

#### Dòng 934
```csharp
                bool targetEnemyBoard = targetControl != null && (targetControl == panelEnemyBoard || targetControl.Parent == panelEnemyBoard);
```
**Giải thích:** Gán `targetControl != null && (targetControl == panelEnemyBoard || targetControl.Parent == panelEnemyBoard)` cho `bool targetEnemyBoard`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `Board_DragEnter`, tức phần cho phép kéo thả vào bàn cờ..

#### Dòng 935
```csharp
                bool targetOwnBoard = targetControl != null && (targetControl == panelOwnBoard || targetControl.Parent == panelOwnBoard);
```
**Giải thích:** Gán `targetControl != null && (targetControl == panelOwnBoard || targetControl.Parent == panelOwnBoard)` cho `bool targetOwnBoard`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `Board_DragEnter`, tức phần cho phép kéo thả vào bàn cờ..

#### Dòng 937
```csharp
                if (skillName == "DRONE" && targetEnemyBoard && CanUseDroneSkill())
```
**Giải thích:** Kiểm tra điều kiện `(skillName == "DRONE" && targetEnemyBoard && CanUseDroneSkill()`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `Board_DragEnter`, tức phần cho phép kéo thả vào bàn cờ..

#### Dòng 939
```csharp
                    e.Effect = DragDropEffects.Copy;
```
**Giải thích:** Gán `DragDropEffects.Copy` cho `e.Effect`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `Board_DragEnter`, tức phần cho phép kéo thả vào bàn cờ..

#### Dòng 940
```csharp
                    return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `Board_DragEnter`, tức phần cho phép kéo thả vào bàn cờ..

#### Dòng 943
```csharp
                if (skillName == "SPARE" && targetOwnBoard && CanUseShipSpareSkill())
```
**Giải thích:** Kiểm tra điều kiện `(skillName == "SPARE" && targetOwnBoard && CanUseShipSpareSkill()`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `Board_DragEnter`, tức phần cho phép kéo thả vào bàn cờ..

#### Dòng 945
```csharp
                    e.Effect = DragDropEffects.Copy;
```
**Giải thích:** Gán `DragDropEffects.Copy` cho `e.Effect`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `Board_DragEnter`, tức phần cho phép kéo thả vào bàn cờ..

#### Dòng 946
```csharp
                    return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `Board_DragEnter`, tức phần cho phép kéo thả vào bàn cờ..

#### Dòng 950
```csharp
            e.Effect = DragDropEffects.None;
```
**Giải thích:** Gán `DragDropEffects.None` cho `e.Effect`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `Board_DragEnter`, tức phần cho phép kéo thả vào bàn cờ..


---

### Nhóm hàm `OwnCell_DragDrop`

**Vai trò:** thả tàu hoặc tàu dự phòng vào ô bàn mình.

#### Dòng 953
```csharp
        private void OwnCell_DragDrop(object sender, DragEventArgs e)
```
**Giải thích:** Khai báo hàm `OwnCell_DragDrop(object sender, DragEventArgs e)`. Công dụng chính: thả tàu hoặc tàu dự phòng vào ô bàn mình.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 955
```csharp
            Label cell = sender as Label;
```
**Giải thích:** Gán `sender as Label` cho `Label cell`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `OwnCell_DragDrop`, tức phần thả tàu hoặc tàu dự phòng vào ô bàn mình..

#### Dòng 957
```csharp
            if (cell == null)
```
**Giải thích:** Kiểm tra điều kiện `(cell == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `OwnCell_DragDrop`, tức phần thả tàu hoặc tàu dự phòng vào ô bàn mình..

#### Dòng 959
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `OwnCell_DragDrop`, tức phần thả tàu hoặc tàu dự phòng vào ô bàn mình..

#### Dòng 962
```csharp
            Point p = (Point)cell.Tag;
```
**Giải thích:** Gán `(Point)cell.Tag` cho `Point p`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `OwnCell_DragDrop`, tức phần thả tàu hoặc tàu dự phòng vào ô bàn mình..

#### Dòng 964
```csharp
            string skillName;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `OwnCell_DragDrop`, tức phần thả tàu hoặc tàu dự phòng vào ô bàn mình..

#### Dòng 965
```csharp
            if (TryGetDraggedSkill(e, out skillName) && skillName == "SPARE")
```
**Giải thích:** Kiểm tra điều kiện `(TryGetDraggedSkill(e, out skillName) && skillName == "SPARE"`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `OwnCell_DragDrop`, tức phần thả tàu hoặc tàu dự phòng vào ô bàn mình..

#### Dòng 967
```csharp
                TryUseShipSpare(p.X, p.Y);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `OwnCell_DragDrop`, tức phần thả tàu hoặc tàu dự phòng vào ô bàn mình..

#### Dòng 968
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `OwnCell_DragDrop`, tức phần thả tàu hoặc tàu dự phòng vào ô bàn mình..

#### Dòng 971
```csharp
            TryPlaceDraggedShip(e, p.X, p.Y);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `OwnCell_DragDrop`, tức phần thả tàu hoặc tàu dự phòng vào ô bàn mình..


---

### Nhóm hàm `OwnBoard_DragDrop`

**Vai trò:** tính ô khi thả lên panel bàn mình.

#### Dòng 974
```csharp
        private void OwnBoard_DragDrop(object sender, DragEventArgs e)
```
**Giải thích:** Khai báo hàm `OwnBoard_DragDrop(object sender, DragEventArgs e)`. Công dụng chính: tính ô khi thả lên panel bàn mình.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 976
```csharp
            int cellWidth = panelOwnBoard.ClientSize.Width / BOARD_SIZE;
```
**Giải thích:** Gán `panelOwnBoard.ClientSize.Width / BOARD_SIZE` cho `int cellWidth`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `OwnBoard_DragDrop`, tức phần tính ô khi thả lên panel bàn mình..

#### Dòng 977
```csharp
            int cellHeight = panelOwnBoard.ClientSize.Height / BOARD_SIZE;
```
**Giải thích:** Gán `panelOwnBoard.ClientSize.Height / BOARD_SIZE` cho `int cellHeight`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `OwnBoard_DragDrop`, tức phần tính ô khi thả lên panel bàn mình..

#### Dòng 979
```csharp
            Point p = panelOwnBoard.PointToClient(new Point(e.X, e.Y));
```
**Giải thích:** Gán `panelOwnBoard.PointToClient(new Point(e.X, e.Y))` cho `Point p`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `OwnBoard_DragDrop`, tức phần tính ô khi thả lên panel bàn mình..

#### Dòng 981
```csharp
            int col = p.X / cellWidth;
```
**Giải thích:** Gán `p.X / cellWidth` cho `int col`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `OwnBoard_DragDrop`, tức phần tính ô khi thả lên panel bàn mình..

#### Dòng 982
```csharp
            int row = p.Y / cellHeight;
```
**Giải thích:** Gán `p.Y / cellHeight` cho `int row`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `OwnBoard_DragDrop`, tức phần tính ô khi thả lên panel bàn mình..

#### Dòng 984
```csharp
            string skillName;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `OwnBoard_DragDrop`, tức phần tính ô khi thả lên panel bàn mình..

#### Dòng 985
```csharp
            if (TryGetDraggedSkill(e, out skillName) && skillName == "SPARE")
```
**Giải thích:** Kiểm tra điều kiện `(TryGetDraggedSkill(e, out skillName) && skillName == "SPARE"`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `OwnBoard_DragDrop`, tức phần tính ô khi thả lên panel bàn mình..

#### Dòng 987
```csharp
                TryUseShipSpare(row, col);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `OwnBoard_DragDrop`, tức phần tính ô khi thả lên panel bàn mình..

#### Dòng 988
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `OwnBoard_DragDrop`, tức phần tính ô khi thả lên panel bàn mình..

#### Dòng 991
```csharp
            TryPlaceDraggedShip(e, row, col);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `OwnBoard_DragDrop`, tức phần tính ô khi thả lên panel bàn mình..


---

### Nhóm hàm `TryGetDraggedShipId`

**Vai trò:** lấy mã tàu đang kéo.

#### Dòng 994
```csharp
        private bool TryGetDraggedShipId(DragEventArgs e, out int shipId)
```
**Giải thích:** Khai báo hàm `TryGetDraggedShipId(DragEventArgs e, out int shipId)`. Công dụng chính: lấy mã tàu đang kéo.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 996
```csharp
            shipId = -1;
```
**Giải thích:** Gán `-1` cho `shipId`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `TryGetDraggedShipId`, tức phần lấy mã tàu đang kéo..

#### Dòng 997
```csharp
            object data = e.Data.GetData(DataFormats.Text);
```
**Giải thích:** Gán `e.Data.GetData(DataFormats.Text)` cho `object data`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `TryGetDraggedShipId`, tức phần lấy mã tàu đang kéo..

#### Dòng 999
```csharp
            if (data == null)
```
**Giải thích:** Kiểm tra điều kiện `(data == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `TryGetDraggedShipId`, tức phần lấy mã tàu đang kéo..

#### Dòng 1001
```csharp
                data = e.Data.GetData(DataFormats.StringFormat);
```
**Giải thích:** Gán `e.Data.GetData(DataFormats.StringFormat)` cho `data`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `TryGetDraggedShipId`, tức phần lấy mã tàu đang kéo..

#### Dòng 1004
```csharp
            if (data == null)
```
**Giải thích:** Kiểm tra điều kiện `(data == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `TryGetDraggedShipId`, tức phần lấy mã tàu đang kéo..

#### Dòng 1006
```csharp
                return false;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `TryGetDraggedShipId`, tức phần lấy mã tàu đang kéo..

#### Dòng 1009
```csharp
            string text = data.ToString();
```
**Giải thích:** Gán `data.ToString()` cho `string text`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `TryGetDraggedShipId`, tức phần lấy mã tàu đang kéo..

#### Dòng 1011
```csharp
            if (text.StartsWith("SHIP|"))
```
**Giải thích:** Kiểm tra điều kiện `(text.StartsWith("SHIP|")`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `TryGetDraggedShipId`, tức phần lấy mã tàu đang kéo..

#### Dòng 1013
```csharp
                text = text.Replace("SHIP|", "");
```
**Giải thích:** Gán `text.Replace("SHIP|", "")` cho `text`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `TryGetDraggedShipId`, tức phần lấy mã tàu đang kéo..

#### Dòng 1016
```csharp
            return int.TryParse(text, out shipId);
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `TryGetDraggedShipId`, tức phần lấy mã tàu đang kéo..


---

### Nhóm hàm `TryPlaceDraggedShip`

**Vai trò:** đặt tàu kéo thả nếu hợp lệ.

#### Dòng 1019
```csharp
        private void TryPlaceDraggedShip(DragEventArgs e, int row, int col)
```
**Giải thích:** Khai báo hàm `TryPlaceDraggedShip(DragEventArgs e, int row, int col)`. Công dụng chính: đặt tàu kéo thả nếu hợp lệ.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1021
```csharp
            int shipId;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `TryPlaceDraggedShip`, tức phần đặt tàu kéo thả nếu hợp lệ..

#### Dòng 1023
```csharp
            if (!TryGetDraggedShipId(e, out shipId))
```
**Giải thích:** Kiểm tra điều kiện `(!TryGetDraggedShipId(e, out shipId)`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `TryPlaceDraggedShip`, tức phần đặt tàu kéo thả nếu hợp lệ..

#### Dòng 1025
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `TryPlaceDraggedShip`, tức phần đặt tàu kéo thả nếu hợp lệ..

#### Dòng 1028
```csharp
            PlaceShipById(shipId, row, col, isHorizontal);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `TryPlaceDraggedShip`, tức phần đặt tàu kéo thả nếu hợp lệ..


---

### Nhóm hàm `GetPlayerDisplayName`

**Vai trò:** lấy tên người chơi hiển thị.

#### Dòng 1031
```csharp
        private string GetPlayerDisplayName()
```
**Giải thích:** Khai báo hàm `GetPlayerDisplayName()`. Công dụng chính: lấy tên người chơi hiển thị.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1033
```csharp
            if (txtName == null)
```
**Giải thích:** Kiểm tra điều kiện `(txtName == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `GetPlayerDisplayName`, tức phần lấy tên người chơi hiển thị..

#### Dòng 1035
```csharp
                return "Người chơi";
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `GetPlayerDisplayName`, tức phần lấy tên người chơi hiển thị..

#### Dòng 1038
```csharp
            string name = txtName.Text.Trim();
```
**Giải thích:** Gán `txtName.Text.Trim()` cho `string name`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `GetPlayerDisplayName`, tức phần lấy tên người chơi hiển thị..

#### Dòng 1040
```csharp
            if (name.Length == 0)
```
**Giải thích:** Kiểm tra điều kiện `(name.Length == 0`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `GetPlayerDisplayName`, tức phần lấy tên người chơi hiển thị..

#### Dòng 1042
```csharp
                return "Người chơi";
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `GetPlayerDisplayName`, tức phần lấy tên người chơi hiển thị..

#### Dòng 1045
```csharp
            return name;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `GetPlayerDisplayName`, tức phần lấy tên người chơi hiển thị..


---

### Nhóm hàm `btnConnect_Click`

**Vai trò:** kết nối Client tới Server.

#### Dòng 1048
```csharp
        private void btnConnect_Click(object sender, EventArgs e)
```
**Giải thích:** Khai báo hàm `btnConnect_Click(object sender, EventArgs e)`. Công dụng chính: kết nối Client tới Server.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1050
```csharp
            if (connected)
```
**Giải thích:** Kiểm tra điều kiện `(connected`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `btnConnect_Click`, tức phần kết nối Client tới Server..

#### Dòng 1052
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `btnConnect_Click`, tức phần kết nối Client tới Server..

#### Dòng 1055
```csharp
            try
```
**Giải thích:** Bắt đầu khối có thể phát sinh lỗi như kết nối mạng, parse dữ liệu, đọc ảnh/âm thanh. Ngữ cảnh: dòng này nằm trong `btnConnect_Click`, tức phần kết nối Client tới Server..

#### Dòng 1057
```csharp
                string ip = txtIP.Text.Trim();
```
**Giải thích:** Gán `txtIP.Text.Trim()` cho `string ip`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `btnConnect_Click`, tức phần kết nối Client tới Server..

#### Dòng 1058
```csharp
                int port = int.Parse(txtPort.Text.Trim());
```
**Giải thích:** Gán `int.Parse(txtPort.Text.Trim())` cho `int port`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `btnConnect_Click`, tức phần kết nối Client tới Server..

#### Dòng 1060
```csharp
                client = new TcpClient();
```
**Giải thích:** Tạo đối tượng TCP Client để chuẩn bị kết nối đến Server qua IP/cổng người dùng nhập. Ngữ cảnh: dòng này nằm trong `btnConnect_Click`, tức phần kết nối Client tới Server..

#### Dòng 1061
```csharp
                client.Connect(ip, port);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `btnConnect_Click`, tức phần kết nối Client tới Server..

#### Dòng 1063
```csharp
                reader = new StreamReader(client.GetStream(), encoding);
```
**Giải thích:** Tạo luồng đọc dữ liệu dạng text từ socket. Mỗi lệnh mạng được đọc bằng ReadLine. Ngữ cảnh: dòng này nằm trong `btnConnect_Click`, tức phần kết nối Client tới Server..

#### Dòng 1064
```csharp
                writer = new StreamWriter(client.GetStream(), encoding);
```
**Giải thích:** Tạo luồng ghi dữ liệu dạng text vào socket. AutoFlush sẽ giúp gửi ngay sau WriteLine. Ngữ cảnh: dòng này nằm trong `btnConnect_Click`, tức phần kết nối Client tới Server..

#### Dòng 1065
```csharp
                writer.AutoFlush = true;
```
**Giải thích:** Bật tự động gửi dữ liệu ngay sau khi WriteLine, tránh lệnh bị giữ trong bộ đệm. Ngữ cảnh: dòng này nằm trong `btnConnect_Click`, tức phần kết nối Client tới Server..

#### Dòng 1067
```csharp
                connected = true;
```
**Giải thích:** Gán giá trị mới cho `connected`. trạng thái đã kết nối Server hay chưa. Giá trị `true` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `btnConnect_Click`, tức phần kết nối Client tới Server..

#### Dòng 1068
```csharp
                lblPlayer.Text = "Người chơi: " + GetPlayerDisplayName();
```
**Giải thích:** Đổi chữ hiển thị của `lblPlayer`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `btnConnect_Click`, tức phần kết nối Client tới Server..

#### Dòng 1069
```csharp
                lblStatus.Text = "TRẠNG THÁI: ĐÃ KẾT NỐI";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `btnConnect_Click`, tức phần kết nối Client tới Server..

#### Dòng 1071
```csharp
                receiveThread = new Thread(ReceiveLoop);
```
**Giải thích:** Tạo thread nền. Thread giúp nhận/gửi socket hoặc chờ client mà không làm treo giao diện WinForms. Ngữ cảnh: dòng này nằm trong `btnConnect_Click`, tức phần kết nối Client tới Server..

#### Dòng 1072
```csharp
                receiveThread.IsBackground = true;
```
**Giải thích:** Đặt thread là luồng nền để khi đóng form chương trình không bị kẹt vì thread phụ còn chạy. Ngữ cảnh: dòng này nằm trong `btnConnect_Click`, tức phần kết nối Client tới Server..

#### Dòng 1073
```csharp
                receiveThread.Start();
```
**Giải thích:** Bắt đầu chạy thread, timer hoặc listener đã được tạo trước đó. Ngữ cảnh: dòng này nằm trong `btnConnect_Click`, tức phần kết nối Client tới Server..

#### Dòng 1076
```csharp
            catch (Exception ex)
```
**Giải thích:** Bắt lỗi để chương trình không bị crash; thường hiển thị thông báo hoặc bỏ qua lỗi nhỏ. Ngữ cảnh: dòng này nằm trong `btnConnect_Click`, tức phần kết nối Client tới Server..

#### Dòng 1078
```csharp
                MessageBox.Show("Không kết nối được máy chủ: " + ex.Message);
```
**Giải thích:** Hiện hộp thoại thông báo lỗi/cảnh báo cho người dùng. Ngữ cảnh: dòng này nằm trong `btnConnect_Click`, tức phần kết nối Client tới Server..


---

### Nhóm hàm `btnReady_Click`

**Vai trò:** gửi READY khi đã đặt đủ tàu.

#### Dòng 1082
```csharp
        private void btnReady_Click(object sender, EventArgs e)
```
**Giải thích:** Khai báo hàm `btnReady_Click(object sender, EventArgs e)`. Công dụng chính: gửi READY khi đã đặt đủ tàu.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1084
```csharp
            if (!connected)
```
**Giải thích:** Kiểm tra điều kiện `(!connected`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `btnReady_Click`, tức phần gửi READY khi đã đặt đủ tàu..

#### Dòng 1086
```csharp
                MessageBox.Show("Bạn phải kết nối máy chủ trước.");
```
**Giải thích:** Hiện hộp thoại thông báo lỗi/cảnh báo cho người dùng. Ngữ cảnh: dòng này nằm trong `btnReady_Click`, tức phần gửi READY khi đã đặt đủ tàu..

#### Dòng 1087
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `btnReady_Click`, tức phần gửi READY khi đã đặt đủ tàu..

#### Dòng 1090
```csharp
            if (shipCount < BASE_SHIP_COUNT)
```
**Giải thích:** Kiểm tra điều kiện `(shipCount < BASE_SHIP_COUNT`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `btnReady_Click`, tức phần gửi READY khi đã đặt đủ tàu..

#### Dòng 1092
```csharp
                MessageBox.Show("Bạn phải đặt đủ 3 tàu trước khi sẵn sàng.");
```
**Giải thích:** Hiện hộp thoại thông báo lỗi/cảnh báo cho người dùng. Ngữ cảnh: dòng này nằm trong `btnReady_Click`, tức phần gửi READY khi đã đặt đủ tàu..

#### Dòng 1093
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `btnReady_Click`, tức phần gửi READY khi đã đặt đủ tàu..

#### Dòng 1096
```csharp
            if (ready)
```
**Giải thích:** Kiểm tra điều kiện `(ready`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `btnReady_Click`, tức phần gửi READY khi đã đặt đủ tàu..

#### Dòng 1098
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `btnReady_Click`, tức phần gửi READY khi đã đặt đủ tàu..

#### Dòng 1101
```csharp
            ready = true;
```
**Giải thích:** Gán giá trị mới cho `ready`. mảng trạng thái sẵn sàng của hai Client. Giá trị `true` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `btnReady_Click`, tức phần gửi READY khi đã đặt đủ tàu..

#### Dòng 1102
```csharp
            btnReady.Enabled = false;
```
**Giải thích:** Bật/tắt tương tác `btnReady`. Khi false người chơi không click/kéo được. Ngữ cảnh: dòng này nằm trong `btnReady_Click`, tức phần gửi READY khi đã đặt đủ tàu..

#### Dòng 1103
```csharp
            btnRotate.Enabled = false;
```
**Giải thích:** Bật/tắt tương tác `btnRotate`. Khi false người chơi không click/kéo được. Ngữ cảnh: dòng này nằm trong `btnReady_Click`, tức phần gửi READY khi đã đặt đủ tàu..

#### Dòng 1104
```csharp
            panelShipDock.Enabled = false;
```
**Giải thích:** Bật/tắt tương tác `panelShipDock`. Khi false người chơi không click/kéo được. Ngữ cảnh: dòng này nằm trong `btnReady_Click`, tức phần gửi READY khi đã đặt đủ tàu..

#### Dòng 1105
```csharp
            panelShipDock.Visible = false;
```
**Giải thích:** Ẩn/hiện `panelShipDock`. Ví dụ nút Chơi lại chỉ hiện khi trận kết thúc. Ngữ cảnh: dòng này nằm trong `btnReady_Click`, tức phần gửi READY khi đã đặt đủ tàu..

#### Dòng 1107
```csharp
            SendLine("READY");
```
**Giải thích:** Dòng này liên quan đến giao thức `READY`: lệnh Client báo đã sẵn sàng. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `btnReady_Click`, tức phần gửi READY khi đã đặt đủ tàu..

#### Dòng 1108
```csharp
            lblStatus.Text = "TRẠNG THÁI: ĐÃ SẴN SÀNG";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `btnReady_Click`, tức phần gửi READY khi đã đặt đủ tàu..


---

### Nhóm hàm `btnRotate_Click`

**Vai trò:** đổi hướng đặt tàu.

#### Dòng 1111
```csharp
        private void btnRotate_Click(object sender, EventArgs e)
```
**Giải thích:** Khai báo hàm `btnRotate_Click(object sender, EventArgs e)`. Công dụng chính: đổi hướng đặt tàu.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1113
```csharp
            if (ready)
```
**Giải thích:** Kiểm tra điều kiện `(ready`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `btnRotate_Click`, tức phần đổi hướng đặt tàu..

#### Dòng 1115
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `btnRotate_Click`, tức phần đổi hướng đặt tàu..

#### Dòng 1118
```csharp
            isHorizontal = !isHorizontal;
```
**Giải thích:** Gán giá trị mới cho `isHorizontal`. hướng đặt tàu hiện tại. Giá trị `!isHorizontal` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `btnRotate_Click`, tức phần đổi hướng đặt tàu..

#### Dòng 1119
```csharp
            UpdateDockShipImages();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `btnRotate_Click`, tức phần đổi hướng đặt tàu..


---

### Nhóm hàm `UpdateDockShipImages`

**Vai trò:** đổi ảnh dock theo hướng đặt.

#### Dòng 1122
```csharp
        private void UpdateDockShipImages()
```
**Giải thích:** Khai báo hàm `UpdateDockShipImages()`. Công dụng chính: đổi ảnh dock theo hướng đặt.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1124
```csharp
            for (int shipId = 0; shipId < BASE_SHIP_COUNT; shipId++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `UpdateDockShipImages`, tức phần đổi ảnh dock theo hướng đặt..

#### Dòng 1126
```csharp
                if (shipDockPictures[shipId] != null)
```
**Giải thích:** Kiểm tra điều kiện `(shipDockPictures[shipId] != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `UpdateDockShipImages`, tức phần đổi ảnh dock theo hướng đặt..

#### Dòng 1128
```csharp
                    shipDockPictures[shipId].Image = PrepareShipImage(shipId, isHorizontal);
```
**Giải thích:** Cập nhật phần tử mảng `shipDockPictures[shipId].Image`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `UpdateDockShipImages`, tức phần đổi ảnh dock theo hướng đặt..


---

### Nhóm hàm `PrepareShipImage`

**Vai trò:** chuẩn bị ảnh tàu đúng chiều.

#### Dòng 1133
```csharp
        private Image PrepareShipImage(int shipId, bool horizontal)
```
**Giải thích:** Khai báo hàm `PrepareShipImage(int shipId, bool horizontal)`. Công dụng chính: chuẩn bị ảnh tàu đúng chiều.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1135
```csharp
            Image original = LoadImageSafe(GetShipImageName(shipId));
```
**Giải thích:** Gán `LoadImageSafe(GetShipImageName(shipId))` cho `Image original`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `PrepareShipImage`, tức phần chuẩn bị ảnh tàu đúng chiều..

#### Dòng 1137
```csharp
            if (original == null)
```
**Giải thích:** Kiểm tra điều kiện `(original == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `PrepareShipImage`, tức phần chuẩn bị ảnh tàu đúng chiều..

#### Dòng 1139
```csharp
                return null;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `PrepareShipImage`, tức phần chuẩn bị ảnh tàu đúng chiều..

#### Dòng 1142
```csharp
            Bitmap img = new Bitmap(original);
```
**Giải thích:** Gán `new Bitmap(original)` cho `Bitmap img`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `PrepareShipImage`, tức phần chuẩn bị ảnh tàu đúng chiều..

#### Dòng 1144
```csharp
            if (!horizontal)
```
**Giải thích:** Kiểm tra điều kiện `(!horizontal`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `PrepareShipImage`, tức phần chuẩn bị ảnh tàu đúng chiều..

#### Dòng 1146
```csharp
                img.RotateFlip(RotateFlipType.Rotate90FlipNone);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `PrepareShipImage`, tức phần chuẩn bị ảnh tàu đúng chiều..

#### Dòng 1149
```csharp
            return img;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `PrepareShipImage`, tức phần chuẩn bị ảnh tàu đúng chiều..


---

### Nhóm hàm `OwnCell_Click`

**Vai trò:** đặt tàu bằng click.

#### Dòng 1152
```csharp
        private void OwnCell_Click(object sender, EventArgs e)
```
**Giải thích:** Khai báo hàm `OwnCell_Click(object sender, EventArgs e)`. Công dụng chính: đặt tàu bằng click.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1154
```csharp
            if (ready)
```
**Giải thích:** Kiểm tra điều kiện `(ready`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `OwnCell_Click`, tức phần đặt tàu bằng click..

#### Dòng 1156
```csharp
                MessageBox.Show("Bạn đã sẵn sàng rồi, không thể đặt lại tàu.");
```
**Giải thích:** Hiện hộp thoại thông báo lỗi/cảnh báo cho người dùng. Ngữ cảnh: dòng này nằm trong `OwnCell_Click`, tức phần đặt tàu bằng click..

#### Dòng 1157
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `OwnCell_Click`, tức phần đặt tàu bằng click..

#### Dòng 1160
```csharp
            Label cell = sender as Label;
```
**Giải thích:** Gán `sender as Label` cho `Label cell`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `OwnCell_Click`, tức phần đặt tàu bằng click..

#### Dòng 1162
```csharp
            if (cell == null)
```
**Giải thích:** Kiểm tra điều kiện `(cell == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `OwnCell_Click`, tức phần đặt tàu bằng click..

#### Dòng 1164
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `OwnCell_Click`, tức phần đặt tàu bằng click..

#### Dòng 1167
```csharp
            Point p = (Point)cell.Tag;
```
**Giải thích:** Gán `(Point)cell.Tag` cho `Point p`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `OwnCell_Click`, tức phần đặt tàu bằng click..

#### Dòng 1169
```csharp
            int nextShip = GetNextUnplacedShipId();
```
**Giải thích:** Gán `GetNextUnplacedShipId()` cho `int nextShip`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `OwnCell_Click`, tức phần đặt tàu bằng click..

#### Dòng 1171
```csharp
            if (nextShip < 0)
```
**Giải thích:** Kiểm tra điều kiện `(nextShip < 0`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `OwnCell_Click`, tức phần đặt tàu bằng click..

#### Dòng 1173
```csharp
                MessageBox.Show("Bạn đã đặt đủ 3 tàu. Có thể kéo tàu trên bàn cờ để đổi vị trí trước khi sẵn sàng.");
```
**Giải thích:** Hiện hộp thoại thông báo lỗi/cảnh báo cho người dùng. Ngữ cảnh: dòng này nằm trong `OwnCell_Click`, tức phần đặt tàu bằng click..

#### Dòng 1174
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `OwnCell_Click`, tức phần đặt tàu bằng click..

#### Dòng 1177
```csharp
            PlaceShipById(nextShip, p.X, p.Y, isHorizontal);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `OwnCell_Click`, tức phần đặt tàu bằng click..


---

### Nhóm hàm `GetNextUnplacedShipId`

**Vai trò:** tìm tàu chưa đặt tiếp theo.

#### Dòng 1180
```csharp
        private int GetNextUnplacedShipId()
```
**Giải thích:** Khai báo hàm `GetNextUnplacedShipId()`. Công dụng chính: tìm tàu chưa đặt tiếp theo.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1182
```csharp
            for (int i = 0; i < BASE_SHIP_COUNT; i++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `GetNextUnplacedShipId`, tức phần tìm tàu chưa đặt tiếp theo..

#### Dòng 1184
```csharp
                if (!shipPlaced[i])
```
**Giải thích:** Kiểm tra điều kiện `(!shipPlaced[i]`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `GetNextUnplacedShipId`, tức phần tìm tàu chưa đặt tiếp theo..

#### Dòng 1186
```csharp
                    return i;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `GetNextUnplacedShipId`, tức phần tìm tàu chưa đặt tiếp theo..

#### Dòng 1190
```csharp
            return -1;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `GetNextUnplacedShipId`, tức phần tìm tàu chưa đặt tiếp theo..


---

### Nhóm hàm `PlaceShipById`

**Vai trò:** đặt tàu theo id.

#### Dòng 1193
```csharp
        private void PlaceShipById(int shipId, int row, int col, bool horizontal)
```
**Giải thích:** Khai báo hàm `PlaceShipById(int shipId, int row, int col, bool horizontal)`. Công dụng chính: đặt tàu theo id.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1195
```csharp
            if (ready)
```
**Giải thích:** Kiểm tra điều kiện `(ready`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1197
```csharp
                MessageBox.Show("Bạn đã sẵn sàng rồi, không thể đặt lại tàu.");
```
**Giải thích:** Hiện hộp thoại thông báo lỗi/cảnh báo cho người dùng. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1198
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1201
```csharp
            if (shipId < 0 || shipId >= TOTAL_SHIP_COUNT)
```
**Giải thích:** Kiểm tra điều kiện `(shipId < 0 || shipId >= TOTAL_SHIP_COUNT`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1203
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1206
```csharp
            int length = shipSizes[shipId];
```
**Giải thích:** Gán `shipSizes[shipId]` cho `int length`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1208
```csharp
            bool wasPlaced = shipPlaced[shipId];
```
**Giải thích:** Gán `shipPlaced[shipId]` cho `bool wasPlaced`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1209
```csharp
            int oldRow = shipStartRow[shipId];
```
**Giải thích:** Gán `shipStartRow[shipId]` cho `int oldRow`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1210
```csharp
            int oldCol = shipStartCol[shipId];
```
**Giải thích:** Gán `shipStartCol[shipId]` cho `int oldCol`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1211
```csharp
            bool oldHorizontal = shipIsHorizontal[shipId];
```
**Giải thích:** Gán `shipIsHorizontal[shipId]` cho `bool oldHorizontal`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1213
```csharp
            if (wasPlaced)
```
**Giải thích:** Kiểm tra điều kiện `(wasPlaced`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1215
```csharp
                RemoveShipFromBoard(shipId);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1218
```csharp
            if (!CanPlaceShip(row, col, length, horizontal))
```
**Giải thích:** Kiểm tra điều kiện `(!CanPlaceShip(row, col, length, horizontal)`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1220
```csharp
                if (wasPlaced)
```
**Giải thích:** Kiểm tra điều kiện `(wasPlaced`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1222
```csharp
                    ApplyShipPlacement(oldRow, oldCol, length, oldHorizontal, shipId);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1225
```csharp
                MessageBox.Show("Không đặt được tàu ở vị trí này.");
```
**Giải thích:** Hiện hộp thoại thông báo lỗi/cảnh báo cho người dùng. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1226
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1229
```csharp
            ApplyShipPlacement(row, col, length, horizontal, shipId);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1231
```csharp
            if (!wasPlaced)
```
**Giải thích:** Kiểm tra điều kiện `(!wasPlaced`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1233
```csharp
                shipCount++;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1236
```csharp
            if (shipDockPictures[shipId] != null)
```
**Giải thích:** Kiểm tra điều kiện `(shipDockPictures[shipId] != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1238
```csharp
                shipDockPictures[shipId].Visible = false;
```
**Giải thích:** Cập nhật phần tử mảng `shipDockPictures[shipId].Visible`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1241
```csharp
            if (shipCount >= BASE_SHIP_COUNT)
```
**Giải thích:** Kiểm tra điều kiện `(shipCount >= BASE_SHIP_COUNT`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..

#### Dòng 1243
```csharp
                lblStatus.Text = "TRẠNG THÁI: ĐÃ ĐẶT ĐỦ TÀU";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `PlaceShipById`, tức phần đặt tàu theo id..


---

### Nhóm hàm `ApplyShipPlacement`

**Vai trò:** ghi dữ liệu tàu vào mảng logic.

#### Dòng 1247
```csharp
        private void ApplyShipPlacement(int row, int col, int length, bool horizontal, int shipId)
```
**Giải thích:** Khai báo hàm `ApplyShipPlacement(int row, int col, int length, bool horizontal, int shipId)`. Công dụng chính: ghi dữ liệu tàu vào mảng logic.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1249
```csharp
            shipStartRow[shipId] = row;
```
**Giải thích:** Cập nhật phần tử mảng `shipStartRow[shipId]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ApplyShipPlacement`, tức phần ghi dữ liệu tàu vào mảng logic..

#### Dòng 1250
```csharp
            shipStartCol[shipId] = col;
```
**Giải thích:** Cập nhật phần tử mảng `shipStartCol[shipId]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ApplyShipPlacement`, tức phần ghi dữ liệu tàu vào mảng logic..

#### Dòng 1251
```csharp
            shipIsHorizontal[shipId] = horizontal;
```
**Giải thích:** Cập nhật phần tử mảng `shipIsHorizontal[shipId]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ApplyShipPlacement`, tức phần ghi dữ liệu tàu vào mảng logic..

#### Dòng 1252
```csharp
            shipPlaced[shipId] = true;
```
**Giải thích:** Cập nhật phần tử mảng `shipPlaced[shipId]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ApplyShipPlacement`, tức phần ghi dữ liệu tàu vào mảng logic..

#### Dòng 1254
```csharp
            for (int i = 0; i < length; i++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `ApplyShipPlacement`, tức phần ghi dữ liệu tàu vào mảng logic..

#### Dòng 1256
```csharp
                int r = row;
```
**Giải thích:** Gán `row` cho `int r`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ApplyShipPlacement`, tức phần ghi dữ liệu tàu vào mảng logic..

#### Dòng 1257
```csharp
                int c = col;
```
**Giải thích:** Gán `col` cho `int c`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ApplyShipPlacement`, tức phần ghi dữ liệu tàu vào mảng logic..

#### Dòng 1259
```csharp
                if (horizontal)
```
**Giải thích:** Kiểm tra điều kiện `(horizontal`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ApplyShipPlacement`, tức phần ghi dữ liệu tàu vào mảng logic..

#### Dòng 1261
```csharp
                    c = col + i;
```
**Giải thích:** Gán `col + i` cho `c`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ApplyShipPlacement`, tức phần ghi dữ liệu tàu vào mảng logic..

#### Dòng 1263
```csharp
                else
```
**Giải thích:** Nhánh xử lý còn lại khi các điều kiện phía trước đều sai. Ngữ cảnh: dòng này nằm trong `ApplyShipPlacement`, tức phần ghi dữ liệu tàu vào mảng logic..

#### Dòng 1265
```csharp
                    r = row + i;
```
**Giải thích:** Gán `row + i` cho `r`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ApplyShipPlacement`, tức phần ghi dữ liệu tàu vào mảng logic..

#### Dòng 1268
```csharp
                ownShips[r, c] = true;
```
**Giải thích:** Cập nhật phần tử mảng `ownShips[r, c]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ApplyShipPlacement`, tức phần ghi dữ liệu tàu vào mảng logic..

#### Dòng 1269
```csharp
                ownShipId[r, c] = shipId;
```
**Giải thích:** Cập nhật phần tử mảng `ownShipId[r, c]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ApplyShipPlacement`, tức phần ghi dữ liệu tàu vào mảng logic..

#### Dòng 1272
```csharp
            DrawShipImage(panelOwnBoard, row, col, length, horizontal, shipId);
```
**Giải thích:** Gọi hàm `DrawShipImage` để vẽ ảnh tàu. Ngữ cảnh: dòng này nằm trong `ApplyShipPlacement`, tức phần ghi dữ liệu tàu vào mảng logic..


---

### Nhóm hàm `RemoveShipFromBoard`

**Vai trò:** xóa tàu khỏi mảng logic.

#### Dòng 1275
```csharp
        private void RemoveShipFromBoard(int shipId)
```
**Giải thích:** Khai báo hàm `RemoveShipFromBoard(int shipId)`. Công dụng chính: xóa tàu khỏi mảng logic.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1277
```csharp
            int length = shipSizes[shipId];
```
**Giải thích:** Gán `shipSizes[shipId]` cho `int length`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `RemoveShipFromBoard`, tức phần xóa tàu khỏi mảng logic..

#### Dòng 1279
```csharp
            for (int i = 0; i < length; i++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `RemoveShipFromBoard`, tức phần xóa tàu khỏi mảng logic..

#### Dòng 1281
```csharp
                int r = shipStartRow[shipId];
```
**Giải thích:** Gán `shipStartRow[shipId]` cho `int r`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `RemoveShipFromBoard`, tức phần xóa tàu khỏi mảng logic..

#### Dòng 1282
```csharp
                int c = shipStartCol[shipId];
```
**Giải thích:** Gán `shipStartCol[shipId]` cho `int c`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `RemoveShipFromBoard`, tức phần xóa tàu khỏi mảng logic..

#### Dòng 1284
```csharp
                if (shipIsHorizontal[shipId])
```
**Giải thích:** Kiểm tra điều kiện `(shipIsHorizontal[shipId]`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `RemoveShipFromBoard`, tức phần xóa tàu khỏi mảng logic..

#### Dòng 1286
```csharp
                    c = shipStartCol[shipId] + i;
```
**Giải thích:** Gán `shipStartCol[shipId] + i` cho `c`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `RemoveShipFromBoard`, tức phần xóa tàu khỏi mảng logic..

#### Dòng 1288
```csharp
                else
```
**Giải thích:** Nhánh xử lý còn lại khi các điều kiện phía trước đều sai. Ngữ cảnh: dòng này nằm trong `RemoveShipFromBoard`, tức phần xóa tàu khỏi mảng logic..

#### Dòng 1290
```csharp
                    r = shipStartRow[shipId] + i;
```
**Giải thích:** Gán `shipStartRow[shipId] + i` cho `r`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `RemoveShipFromBoard`, tức phần xóa tàu khỏi mảng logic..

#### Dòng 1293
```csharp
                if (r >= 0 && r < BOARD_SIZE && c >= 0 && c < BOARD_SIZE)
```
**Giải thích:** Kiểm tra điều kiện `(r >= 0 && r < BOARD_SIZE && c >= 0 && c < BOARD_SIZE`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `RemoveShipFromBoard`, tức phần xóa tàu khỏi mảng logic..

#### Dòng 1295
```csharp
                    ownShips[r, c] = false;
```
**Giải thích:** Cập nhật phần tử mảng `ownShips[r, c]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `RemoveShipFromBoard`, tức phần xóa tàu khỏi mảng logic..

#### Dòng 1296
```csharp
                    ownShipId[r, c] = -1;
```
**Giải thích:** Cập nhật phần tử mảng `ownShipId[r, c]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `RemoveShipFromBoard`, tức phần xóa tàu khỏi mảng logic..

#### Dòng 1300
```csharp
            shipPlaced[shipId] = false;
```
**Giải thích:** Cập nhật phần tử mảng `shipPlaced[shipId]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `RemoveShipFromBoard`, tức phần xóa tàu khỏi mảng logic..

#### Dòng 1301
```csharp
            RemoveShipVisual(shipId);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `RemoveShipFromBoard`, tức phần xóa tàu khỏi mảng logic..


---

### Nhóm hàm `RemoveShipVisual`

**Vai trò:** xóa ảnh tàu khỏi giao diện.

#### Dòng 1304
```csharp
        private void RemoveShipVisual(int shipId)
```
**Giải thích:** Khai báo hàm `RemoveShipVisual(int shipId)`. Công dụng chính: xóa ảnh tàu khỏi giao diện.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1306
```csharp
            string targetTag = "VISUAL_SHIP_" + shipId;
```
**Giải thích:** Gán `"VISUAL_SHIP_" + shipId` cho `string targetTag`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `RemoveShipVisual`, tức phần xóa ảnh tàu khỏi giao diện..

#### Dòng 1308
```csharp
            for (int i = panelOwnBoard.Controls.Count - 1; i >= 0; i--)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `RemoveShipVisual`, tức phần xóa ảnh tàu khỏi giao diện..

#### Dòng 1310
```csharp
                Control control = panelOwnBoard.Controls[i];
```
**Giải thích:** Gán `panelOwnBoard.Controls[i]` cho `Control control`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `RemoveShipVisual`, tức phần xóa ảnh tàu khỏi giao diện..

#### Dòng 1312
```csharp
                if (control.Tag is string && control.Tag.ToString() == targetTag)
```
**Giải thích:** Kiểm tra điều kiện `(control.Tag is string && control.Tag.ToString() == targetTag`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `RemoveShipVisual`, tức phần xóa ảnh tàu khỏi giao diện..

#### Dòng 1314
```csharp
                    panelOwnBoard.Controls.RemoveAt(i);
```
**Giải thích:** Xóa control/dữ liệu khỏi collection. Ví dụ xóa ảnh hit/miss hoặc overlay. Ngữ cảnh: dòng này nằm trong `RemoveShipVisual`, tức phần xóa ảnh tàu khỏi giao diện..

#### Dòng 1315
```csharp
                    control.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `RemoveShipVisual`, tức phần xóa ảnh tàu khỏi giao diện..


---

### Nhóm hàm `CanPlaceShip`

**Vai trò:** kiểm tra đặt tàu cơ bản hợp lệ.

#### Dòng 1320
```csharp
        private bool CanPlaceShip(int row, int col, int length, bool horizontal)
```
**Giải thích:** Khai báo hàm `CanPlaceShip(int row, int col, int length, bool horizontal)`. Công dụng chính: kiểm tra đặt tàu cơ bản hợp lệ.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1322
```csharp
            for (int i = 0; i < length; i++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `CanPlaceShip`, tức phần kiểm tra đặt tàu cơ bản hợp lệ..

#### Dòng 1324
```csharp
                int r = row;
```
**Giải thích:** Gán `row` cho `int r`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CanPlaceShip`, tức phần kiểm tra đặt tàu cơ bản hợp lệ..

#### Dòng 1325
```csharp
                int c = col;
```
**Giải thích:** Gán `col` cho `int c`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CanPlaceShip`, tức phần kiểm tra đặt tàu cơ bản hợp lệ..

#### Dòng 1327
```csharp
                if (horizontal)
```
**Giải thích:** Kiểm tra điều kiện `(horizontal`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `CanPlaceShip`, tức phần kiểm tra đặt tàu cơ bản hợp lệ..

#### Dòng 1329
```csharp
                    c = col + i;
```
**Giải thích:** Gán `col + i` cho `c`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CanPlaceShip`, tức phần kiểm tra đặt tàu cơ bản hợp lệ..

#### Dòng 1331
```csharp
                else
```
**Giải thích:** Nhánh xử lý còn lại khi các điều kiện phía trước đều sai. Ngữ cảnh: dòng này nằm trong `CanPlaceShip`, tức phần kiểm tra đặt tàu cơ bản hợp lệ..

#### Dòng 1333
```csharp
                    r = row + i;
```
**Giải thích:** Gán `row + i` cho `r`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CanPlaceShip`, tức phần kiểm tra đặt tàu cơ bản hợp lệ..

#### Dòng 1336
```csharp
                if (r < 0 || r >= BOARD_SIZE || c < 0 || c >= BOARD_SIZE)
```
**Giải thích:** Kiểm tra điều kiện `(r < 0 || r >= BOARD_SIZE || c < 0 || c >= BOARD_SIZE`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `CanPlaceShip`, tức phần kiểm tra đặt tàu cơ bản hợp lệ..

#### Dòng 1338
```csharp
                    return false;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `CanPlaceShip`, tức phần kiểm tra đặt tàu cơ bản hợp lệ..

#### Dòng 1341
```csharp
                if (ownShips[r, c])
```
**Giải thích:** Kiểm tra điều kiện `(ownShips[r, c]`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `CanPlaceShip`, tức phần kiểm tra đặt tàu cơ bản hợp lệ..

#### Dòng 1343
```csharp
                    return false;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `CanPlaceShip`, tức phần kiểm tra đặt tàu cơ bản hợp lệ..

#### Dòng 1347
```csharp
            return true;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `CanPlaceShip`, tức phần kiểm tra đặt tàu cơ bản hợp lệ..


---

### Nhóm hàm `EnemyCell_Click`

**Vai trò:** xử lý bắn vào bàn địch.

#### Dòng 1350
```csharp
        private void EnemyCell_Click(object sender, EventArgs e)
```
**Giải thích:** Khai báo hàm `EnemyCell_Click(object sender, EventArgs e)`. Công dụng chính: xử lý bắn vào bàn địch.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1352
```csharp
            if (!connected)
```
**Giải thích:** Kiểm tra điều kiện `(!connected`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1354
```csharp
                MessageBox.Show("Chưa kết nối máy chủ.");
```
**Giải thích:** Hiện hộp thoại thông báo lỗi/cảnh báo cho người dùng. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1355
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1358
```csharp
            if (!ready)
```
**Giải thích:** Kiểm tra điều kiện `(!ready`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1360
```csharp
                MessageBox.Show("Bạn phải đặt đủ tàu và nhấn sẵn sàng trước.");
```
**Giải thích:** Hiện hộp thoại thông báo lỗi/cảnh báo cho người dùng. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1361
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1364
```csharp
            if (gameEnded)
```
**Giải thích:** Kiểm tra điều kiện `(gameEnded`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1366
```csharp
                MessageBox.Show("Trận đấu đã kết thúc.");
```
**Giải thích:** Hiện hộp thoại thông báo lỗi/cảnh báo cho người dùng. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1367
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1370
```csharp
            if (actionLocked || turnTransitionRunning)
```
**Giải thích:** Kiểm tra điều kiện `(actionLocked || turnTransitionRunning`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1372
```csharp
                lblStatus.Text = "TRẠNG THÁI: ĐANG CHUYỂN LƯỢT";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1373
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1376
```csharp
            if (!myTurn)
```
**Giải thích:** Kiểm tra điều kiện `(!myTurn`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1378
```csharp
                MessageBox.Show("Chưa đến lượt bạn.");
```
**Giải thích:** Hiện hộp thoại thông báo lỗi/cảnh báo cho người dùng. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1379
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1382
```csharp
            Label cell = sender as Label;
```
**Giải thích:** Gán `sender as Label` cho `Label cell`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1383
```csharp
            Point p = (Point)cell.Tag;
```
**Giải thích:** Gán `(Point)cell.Tag` cho `Point p`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1385
```csharp
            int row = p.X;
```
**Giải thích:** Gán `p.X` cho `int row`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1386
```csharp
            int col = p.Y;
```
**Giải thích:** Gán `p.Y` cho `int col`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1388
```csharp
            if (enemyShots[row, col])
```
**Giải thích:** Kiểm tra điều kiện `(enemyShots[row, col]`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1390
```csharp
                MessageBox.Show("Bạn đã bắn ô này rồi.");
```
**Giải thích:** Hiện hộp thoại thông báo lỗi/cảnh báo cho người dùng. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1391
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1394
```csharp
            enemyShots[row, col] = true;
```
**Giải thích:** Cập nhật phần tử mảng `enemyShots[row, col]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1395
```csharp
            myTurn = false;
```
**Giải thích:** Gán giá trị mới cho `myTurn`. cho biết hiện tại có phải lượt bắn của mình. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1396
```csharp
            UpdateSkillMenuState();
```
**Giải thích:** Gọi hàm `UpdateSkillMenuState` để làm mới trạng thái icon skill. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1398
```csharp
            SendLine("FIRE|" + row + "|" + col);
```
**Giải thích:** Dòng này liên quan đến giao thức `FIRE|`: lệnh người chơi bắn vào tọa độ. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1400
```csharp
            lblTurn.Text = "LƯỢT: CHỜ";
```
**Giải thích:** Đổi chữ hiển thị của `lblTurn`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..

#### Dòng 1401
```csharp
            lblStatus.Text = "TRẠNG THÁI: ĐÃ BẮN";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `EnemyCell_Click`, tức phần xử lý bắn vào bàn địch..


---

### Nhóm hàm `ReceiveLoop`

**Vai trò:** đọc lệnh Server liên tục.

#### Dòng 1404
```csharp
        private void ReceiveLoop()
```
**Giải thích:** Khai báo hàm `ReceiveLoop()`. Công dụng chính: đọc lệnh Server liên tục.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1406
```csharp
            try
```
**Giải thích:** Bắt đầu khối có thể phát sinh lỗi như kết nối mạng, parse dữ liệu, đọc ảnh/âm thanh. Ngữ cảnh: dòng này nằm trong `ReceiveLoop`, tức phần đọc lệnh Server liên tục..

#### Dòng 1408
```csharp
                while (connected)
```
**Giải thích:** Vòng lặp `while` chạy lặp lại khi điều kiện còn đúng; trong socket dùng để nghe dữ liệu liên tục. Ngữ cảnh: dòng này nằm trong `ReceiveLoop`, tức phần đọc lệnh Server liên tục..

#### Dòng 1410
```csharp
                    string msg = reader.ReadLine();
```
**Giải thích:** Gán `reader.ReadLine()` cho `string msg`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ReceiveLoop`, tức phần đọc lệnh Server liên tục..

#### Dòng 1412
```csharp
                    if (msg == null)
```
**Giải thích:** Kiểm tra điều kiện `(msg == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ReceiveLoop`, tức phần đọc lệnh Server liên tục..

#### Dòng 1414
```csharp
                        break;
```
**Giải thích:** Kết thúc case hoặc vòng lặp hiện tại. Ngữ cảnh: dòng này nằm trong `ReceiveLoop`, tức phần đọc lệnh Server liên tục..

#### Dòng 1417
```csharp
                    SafeInvoke(() => ProcessMessage(msg));
```
**Giải thích:** Gán `> ProcessMessage(msg))` cho `SafeInvoke(()`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ReceiveLoop`, tức phần đọc lệnh Server liên tục..

#### Dòng 1420
```csharp
            catch
```
**Giải thích:** Bắt lỗi để chương trình không bị crash; thường hiển thị thông báo hoặc bỏ qua lỗi nhỏ. Ngữ cảnh: dòng này nằm trong `ReceiveLoop`, tức phần đọc lệnh Server liên tục..

#### Dòng 1425
```csharp
            SafeInvoke(() => MarkDisconnected());
```
**Giải thích:** Gán `> MarkDisconnected())` cho `SafeInvoke(()`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ReceiveLoop`, tức phần đọc lệnh Server liên tục..


---

### Nhóm hàm `ProcessMessage`

**Vai trò:** phân loại lệnh từ Client.

#### Dòng 1428
```csharp
        private void ProcessMessage(string msg)
```
**Giải thích:** Khai báo hàm `ProcessMessage(string msg)`. Công dụng chính: phân loại lệnh từ Client.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1431
```csharp
            string[] parts = msg.Split('|');
```
**Giải thích:** Cập nhật phần tử mảng `string[] parts`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1433
```csharp
            if (parts.Length == 0)
```
**Giải thích:** Kiểm tra điều kiện `(parts.Length == 0`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1435
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1438
```csharp
            if (parts[0] == "START")
```
**Giải thích:** Kiểm tra điều kiện `(parts[0] == "START"`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1440
```csharp
                if (parts.Length >= 2)
```
**Giải thích:** Kiểm tra điều kiện `(parts.Length >= 2`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1442
```csharp
                    playerNumber = int.Parse(parts[1]);
```
**Giải thích:** Gán giá trị mới cho `playerNumber`. số người chơi do Server cấp: 1 hoặc 2. Giá trị `int.Parse(parts[1])` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1443
```csharp
                    lblPlayer.Text = "Người chơi: " + GetPlayerDisplayName();
```
**Giải thích:** Đổi chữ hiển thị của `lblPlayer`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1444
```csharp
                    lblStatus.Text = "TRẠNG THÁI: " + GetPlayerDisplayName();
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1447
```csharp
            else if (parts[0] == "MESSAGE")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1449
```csharp
                if (parts.Length >= 2)
```
**Giải thích:** Kiểm tra điều kiện `(parts.Length >= 2`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1451
```csharp
                    lblStatus.Text = "THÔNG BÁO: " + parts[1];
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1454
```csharp
            else if (parts[0] == "BOTH_READY")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1456
```csharp
                lblStatus.Text = "TRẠNG THÁI: CẢ HAI ĐÃ SẴN SÀNG";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1458
```csharp
            else if (parts[0] == "TURN")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1460
```csharp
                if (parts.Length >= 2)
```
**Giải thích:** Kiểm tra điều kiện `(parts.Length >= 2`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1462
```csharp
                    int turnPlayer = int.Parse(parts[1]);
```
**Giải thích:** Gán `int.Parse(parts[1])` cho `int turnPlayer`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1463
```csharp
                    StartTurnTransition(turnPlayer == playerNumber);
```
**Giải thích:** Gán `= playerNumber)` cho `StartTurnTransition(turnPlayer`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1466
```csharp
            else if (parts[0] == "ENEMY_FIRE")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1468
```csharp
                if (parts.Length >= 3)
```
**Giải thích:** Kiểm tra điều kiện `(parts.Length >= 3`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1470
```csharp
                    int row = int.Parse(parts[1]);
```
**Giải thích:** Gán `int.Parse(parts[1])` cho `int row`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1471
```csharp
                    int col = int.Parse(parts[2]);
```
**Giải thích:** Gán `int.Parse(parts[2])` cho `int col`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1473
```csharp
                    HandleEnemyFire(row, col);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1476
```csharp
            else if (parts[0] == "FIRE_RESULT")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1478
```csharp
                if (parts.Length >= 4)
```
**Giải thích:** Kiểm tra điều kiện `(parts.Length >= 4`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1480
```csharp
                    string result = parts[1];
```
**Giải thích:** Gán `parts[1]` cho `string result`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1481
```csharp
                    int row = int.Parse(parts[2]);
```
**Giải thích:** Gán `int.Parse(parts[2])` cho `int row`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1482
```csharp
                    int col = int.Parse(parts[3]);
```
**Giải thích:** Gán `int.Parse(parts[3])` cho `int col`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1484
```csharp
                    if (result == "HIT")
```
**Giải thích:** Kiểm tra điều kiện `(result == "HIT"`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1486
```csharp
                        DrawPin(panelEnemyBoard, row, col, true);
```
**Giải thích:** Gọi hàm `DrawPin` để vẽ ảnh hit/miss. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1487
```csharp
                        lblStatus.Text = "TRẠNG THÁI: BẮN TRÚNG";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1489
```csharp
                    else
```
**Giải thích:** Nhánh xử lý còn lại khi các điều kiện phía trước đều sai. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1491
```csharp
                        DrawPin(panelEnemyBoard, row, col, false);
```
**Giải thích:** Gọi hàm `DrawPin` để vẽ ảnh hit/miss. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1492
```csharp
                        lblStatus.Text = "TRẠNG THÁI: BẮN HỤT";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1496
```csharp
            else if (parts[0] == "SUNK")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1498
```csharp
                if (parts.Length >= 5)
```
**Giải thích:** Kiểm tra điều kiện `(parts.Length >= 5`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1500
```csharp
                    int shipId = int.Parse(parts[1]);
```
**Giải thích:** Gán `int.Parse(parts[1])` cho `int shipId`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1501
```csharp
                    int row = int.Parse(parts[2]);
```
**Giải thích:** Gán `int.Parse(parts[2])` cho `int row`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1502
```csharp
                    int col = int.Parse(parts[3]);
```
**Giải thích:** Gán `int.Parse(parts[3])` cho `int col`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1503
```csharp
                    bool horizontal = parts[4] == "1";
```
**Giải thích:** Gán `parts[4] == "1"` cho `bool horizontal`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1505
```csharp
                    DrawShipImage(panelEnemyBoard, row, col, shipSizes[shipId], horizontal, shipId);
```
**Giải thích:** Gọi hàm `DrawShipImage` để vẽ ảnh tàu. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1506
```csharp
                    PlaySound("ship_sunk.wav");
```
**Giải thích:** Gọi hàm `PlaySound` để phát âm thanh trong thư mục Sounds. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1509
```csharp
            else if (parts[0] == "DRONE_REQUEST")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1511
```csharp
                if (parts.Length >= 3)
```
**Giải thích:** Kiểm tra điều kiện `(parts.Length >= 3`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1513
```csharp
                    int row = int.Parse(parts[1]);
```
**Giải thích:** Gán `int.Parse(parts[1])` cho `int row`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1514
```csharp
                    int col = int.Parse(parts[2]);
```
**Giải thích:** Gán `int.Parse(parts[2])` cho `int col`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1515
```csharp
                    HandleDroneRequest(row, col);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1518
```csharp
            else if (parts[0] == "DRONE_RESULT")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1520
```csharp
                if (parts.Length >= 4)
```
**Giải thích:** Kiểm tra điều kiện `(parts.Length >= 4`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1522
```csharp
                    int row = int.Parse(parts[1]);
```
**Giải thích:** Gán `int.Parse(parts[1])` cho `int row`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1523
```csharp
                    int col = int.Parse(parts[2]);
```
**Giải thích:** Gán `int.Parse(parts[2])` cho `int col`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1524
```csharp
                    bool found = parts[3] == "1";
```
**Giải thích:** Gán `parts[3] == "1"` cho `bool found`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1525
```csharp
                    HandleDroneResult(row, col, found);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1528
```csharp
            else if (parts[0] == "GAME_OVER")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1530
```csharp
                gameEnded = true;
```
**Giải thích:** Gán giá trị mới cho `gameEnded`. cho biết trận đã kết thúc; khi true thì khóa bắn và khóa skill. Giá trị `true` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1531
```csharp
                myTurn = false;
```
**Giải thích:** Gán giá trị mới cho `myTurn`. cho biết hiện tại có phải lượt bắn của mình. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1532
```csharp
                UpdateSkillMenuState();
```
**Giải thích:** Gọi hàm `UpdateSkillMenuState` để làm mới trạng thái icon skill. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1533
```csharp
                lblTurn.Text = "KẾT THÚC";
```
**Giải thích:** Đổi chữ hiển thị của `lblTurn`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1535
```csharp
            else if (parts[0] == "RESTART")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1537
```csharp
                ResetForNewGame();
```
**Giải thích:** Gọi hàm `ResetForNewGame` để reset trận đấu. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1539
```csharp
            else if (parts[0] == "WIN")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1541
```csharp
                PlaySound("win.wav");
```
**Giải thích:** Gọi hàm `PlaySound` để phát âm thanh trong thư mục Sounds. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1542
```csharp
                ShowEndGameOptions("Bạn thắng!");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1544
```csharp
            else if (parts[0] == "LOSE")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1546
```csharp
                PlaySound("lose.wav");
```
**Giải thích:** Gọi hàm `PlaySound` để phát âm thanh trong thư mục Sounds. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1547
```csharp
                ShowEndGameOptions("Bạn thua!");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1549
```csharp
            else if (parts[0] == "DISCONNECT")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1551
```csharp
                lblStatus.Text = "TRẠNG THÁI: ĐỐI THỦ ĐÃ THOÁT";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1552
```csharp
                gameEnded = true;
```
**Giải thích:** Gán giá trị mới cho `gameEnded`. cho biết trận đã kết thúc; khi true thì khóa bắn và khóa skill. Giá trị `true` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1553
```csharp
                myTurn = false;
```
**Giải thích:** Gán giá trị mới cho `myTurn`. cho biết hiện tại có phải lượt bắn của mình. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1554
```csharp
                UpdateSkillMenuState();
```
**Giải thích:** Gọi hàm `UpdateSkillMenuState` để làm mới trạng thái icon skill. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 1555
```csharp
                DisableAllBoardCells();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..


---

### Nhóm hàm `HandleEnemyFire`

**Vai trò:** nhận tọa độ đối thủ bắn.

#### Dòng 1559
```csharp
        private void HandleEnemyFire(int row, int col)
```
**Giải thích:** Khai báo hàm `HandleEnemyFire(int row, int col)`. Công dụng chính: nhận tọa độ đối thủ bắn.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1561
```csharp
            actionLocked = true;
```
**Giải thích:** Gán giá trị mới cho `actionLocked`. khóa thao tác trong lúc animation/delay/skill đang chạy để tránh click sai thời điểm. Giá trị `true` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `HandleEnemyFire`, tức phần nhận tọa độ đối thủ bắn..

#### Dòng 1562
```csharp
            lblStatus.Text = "TRẠNG THÁI: ĐỐI THỦ ĐANG BẮN";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `HandleEnemyFire`, tức phần nhận tọa độ đối thủ bắn..

#### Dòng 1564
```csharp
            System.Windows.Forms.Timer impactTimer = new System.Windows.Forms.Timer();
```
**Giải thích:** Tạo Timer WinForms để chạy hiệu ứng theo thời gian, ví dụ nhấp nháy, fade, chuyển lượt. Ngữ cảnh: dòng này nằm trong `HandleEnemyFire`, tức phần nhận tọa độ đối thủ bắn..

#### Dòng 1565
```csharp
            impactTimer.Interval = SHOT_IMPACT_DELAY;
```
**Giải thích:** Gán `SHOT_IMPACT_DELAY` cho `impactTimer.Interval`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `HandleEnemyFire`, tức phần nhận tọa độ đối thủ bắn..

#### Dòng 1566
```csharp
            impactTimer.Tick += (sender, e) =>
```
**Giải thích:** Tạo lambda callback ngắn. Code này thường chạy khi Timer tick, thread chạy, hoặc event xảy ra. Ngữ cảnh: dòng này nằm trong `HandleEnemyFire`, tức phần nhận tọa độ đối thủ bắn..

#### Dòng 1568
```csharp
                impactTimer.Stop();
```
**Giải thích:** Dừng timer hoặc âm thanh đang chạy. Ngữ cảnh: dòng này nằm trong `HandleEnemyFire`, tức phần nhận tọa độ đối thủ bắn..

#### Dòng 1569
```csharp
                impactTimer.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `HandleEnemyFire`, tức phần nhận tọa độ đối thủ bắn..

#### Dòng 1570
```csharp
                ResolveEnemyFire(row, col);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `HandleEnemyFire`, tức phần nhận tọa độ đối thủ bắn..

#### Dòng 1572
```csharp
            impactTimer.Start();
```
**Giải thích:** Bắt đầu chạy thread, timer hoặc listener đã được tạo trước đó. Ngữ cảnh: dòng này nằm trong `HandleEnemyFire`, tức phần nhận tọa độ đối thủ bắn..


---

### Nhóm hàm `ResolveEnemyFire`

**Vai trò:** tính hit/miss trên bàn mình.

#### Dòng 1575
```csharp
        private void ResolveEnemyFire(int row, int col)
```
**Giải thích:** Khai báo hàm `ResolveEnemyFire(int row, int col)`. Công dụng chính: tính hit/miss trên bàn mình.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1577
```csharp
            bool hit = false;
```
**Giải thích:** Gán `false` cho `bool hit`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1578
```csharp
            int hitShipId = -1;
```
**Giải thích:** Gán `-1` cho `int hitShipId`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1580
```csharp
            ownShotsReceived[row, col] = true;
```
**Giải thích:** Cập nhật phần tử mảng `ownShotsReceived[row, col]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1582
```csharp
            if (ownShips[row, col] && !ownHits[row, col])
```
**Giải thích:** Kiểm tra điều kiện `(ownShips[row, col] && !ownHits[row, col]`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1584
```csharp
                hit = true;
```
**Giải thích:** Gán `true` cho `hit`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1585
```csharp
                ownHits[row, col] = true;
```
**Giải thích:** Cập nhật phần tử mảng `ownHits[row, col]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1586
```csharp
                hitShipId = ownShipId[row, col];
```
**Giải thích:** Gán `ownShipId[row, col]` cho `hitShipId`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1588
```csharp
                if (hitShipId >= 0)
```
**Giải thích:** Kiểm tra điều kiện `(hitShipId >= 0`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1590
```csharp
                    shipHP[hitShipId]--;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1593
```csharp
                DrawPin(panelOwnBoard, row, col, true);
```
**Giải thích:** Gọi hàm `DrawPin` để vẽ ảnh hit/miss. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1596
```csharp
            else
```
**Giải thích:** Nhánh xử lý còn lại khi các điều kiện phía trước đều sai. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1598
```csharp
                DrawPin(panelOwnBoard, row, col, false);
```
**Giải thích:** Gọi hàm `DrawPin` để vẽ ảnh hit/miss. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1601
```csharp
            if (hit)
```
**Giải thích:** Kiểm tra điều kiện `(hit`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1603
```csharp
                SendLine("FIRE_RESULT|HIT|" + row + "|" + col);
```
**Giải thích:** Dòng này liên quan đến giao thức `FIRE_RESULT|`: lệnh trả kết quả bắn HIT/MISS. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1605
```csharp
                if (hitShipId >= 0 && shipHP[hitShipId] == 0)
```
**Giải thích:** Kiểm tra điều kiện `(hitShipId >= 0 && shipHP[hitShipId] == 0`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1607
```csharp
                    int horizontalValue = shipIsHorizontal[hitShipId] ? 1 : 0;
```
**Giải thích:** Gán `shipIsHorizontal[hitShipId] ? 1 : 0` cho `int horizontalValue`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1608
```csharp
                    SendLine("SUNK|" + hitShipId + "|" + shipStartRow[hitShipId] + "|" + shipStartCol[hitShipId] + "|" + horizontalValue);
```
**Giải thích:** Dòng này liên quan đến giao thức `SUNK|`: lệnh báo một tàu bị chìm. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1609
```csharp
                    PlaySound("ship_sunk.wav");
```
**Giải thích:** Gọi hàm `PlaySound` để phát âm thanh trong thư mục Sounds. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1612
```csharp
            else
```
**Giải thích:** Nhánh xử lý còn lại khi các điều kiện phía trước đều sai. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1614
```csharp
                SendLine("FIRE_RESULT|MISS|" + row + "|" + col);
```
**Giải thích:** Dòng này liên quan đến giao thức `FIRE_RESULT|`: lệnh trả kết quả bắn HIT/MISS. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1617
```csharp
            actionLocked = false;
```
**Giải thích:** Gán giá trị mới cho `actionLocked`. khóa thao tác trong lúc animation/delay/skill đang chạy để tránh click sai thời điểm. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..

#### Dòng 1618
```csharp
            UpdateSkillMenuState();
```
**Giải thích:** Gọi hàm `UpdateSkillMenuState` để làm mới trạng thái icon skill. Ngữ cảnh: dòng này nằm trong `ResolveEnemyFire`, tức phần tính hit/miss trên bàn mình..


---

### Nhóm hàm `DrawPin`

**Vai trò:** vẽ hit/miss lên ô.

#### Dòng 1621
```csharp
        private void DrawPin(Panel board, int row, int col, bool hit)
```
**Giải thích:** Khai báo hàm `DrawPin(Panel board, int row, int col, bool hit)`. Công dụng chính: vẽ hit/miss lên ô.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1623
```csharp
            int cellWidth = board.ClientSize.Width / BOARD_SIZE;
```
**Giải thích:** Gán `board.ClientSize.Width / BOARD_SIZE` cho `int cellWidth`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1624
```csharp
            int cellHeight = board.ClientSize.Height / BOARD_SIZE;
```
**Giải thích:** Gán `board.ClientSize.Height / BOARD_SIZE` cho `int cellHeight`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1626
```csharp
            string imageFile = hit ? "hit.png" : "miss.png";
```
**Giải thích:** Gán `hit ? "hit.png" : "miss.png"` cho `string imageFile`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1627
```csharp
            Image shotImage = LoadImageSafe(imageFile);
```
**Giải thích:** Gán `LoadImageSafe(imageFile)` cho `Image shotImage`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1629
```csharp
            PlaySound(hit ? "shoot_hit.wav" : "shoot_miss.wav");
```
**Giải thích:** Gọi hàm `PlaySound` để phát âm thanh trong thư mục Sounds. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1631
```csharp
            if (shotImage != null)
```
**Giải thích:** Kiểm tra điều kiện `(shotImage != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1633
```csharp
                PictureBox shotPic = new PictureBox();
```
**Giải thích:** Tạo PictureBox để hiển thị ảnh động/tàu/hit/miss trên form hoặc panel. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1634
```csharp
                shotPic.Image = CreateOpacityImage(shotImage, 0.05f);
```
**Giải thích:** Gán ảnh cho `shotPic` để control hiển thị hình tương ứng. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1635
```csharp
                shotPic.SizeMode = PictureBoxSizeMode.Zoom;
```
**Giải thích:** Gán `PictureBoxSizeMode.Zoom` cho `shotPic.SizeMode`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1636
```csharp
                shotPic.BackColor = Color.Transparent;
```
**Giải thích:** Đổi màu nền của `shotPic`. Game dùng màu để thể hiện hiệu ứng, trạng thái hit/miss, skill hoặc thắng/thua. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1638
```csharp
                int finalLeft;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1639
```csharp
                int finalTop;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1641
```csharp
                if (hit)
```
**Giải thích:** Kiểm tra điều kiện `(hit`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1643
```csharp
                    shotPic.Width = cellWidth;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1644
```csharp
                    shotPic.Height = cellHeight;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1645
```csharp
                    finalLeft = col * cellWidth;
```
**Giải thích:** Gán `col * cellWidth` cho `finalLeft`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1646
```csharp
                    finalTop = row * cellHeight;
```
**Giải thích:** Gán `row * cellHeight` cho `finalTop`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1648
```csharp
                else
```
**Giải thích:** Nhánh xử lý còn lại khi các điều kiện phía trước đều sai. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1650
```csharp
                    shotPic.Width = (int)(cellWidth * 0.55);
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1651
```csharp
                    shotPic.Height = (int)(cellHeight * 0.55);
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1652
```csharp
                    finalLeft = col * cellWidth + (cellWidth - shotPic.Width) / 2;
```
**Giải thích:** Gán `col * cellWidth + (cellWidth - shotPic.Width) / 2` cho `finalLeft`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1653
```csharp
                    finalTop = row * cellHeight + (cellHeight - shotPic.Height) / 2;
```
**Giải thích:** Gán `row * cellHeight + (cellHeight - shotPic.Height) / 2` cho `finalTop`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1656
```csharp
                shotPic.Left = finalLeft;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1657
```csharp
                shotPic.Top = finalTop - Math.Max(18, cellHeight / 2);
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1658
```csharp
                shotPic.Tag = "VISUAL_PIN";
```
**Giải thích:** Lưu dữ liệu phụ vào control. Game dùng Tag để nhớ tọa độ ô, mã tàu hoặc tên skill khi kéo thả/click. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1660
```csharp
                board.Controls.Add(shotPic);
```
**Giải thích:** Thêm control/dữ liệu vào collection. Với giao diện, control sẽ xuất hiện trên panel/form. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1661
```csharp
                shotPic.BringToFront();
```
**Giải thích:** Đưa control lên trên cùng để không bị panel/ảnh khác che. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1663
```csharp
                AnimateShotPictureBox(shotPic, shotImage, finalLeft, finalTop);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1664
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1667
```csharp
            Label fallback = new Label();
```
**Giải thích:** Tạo Label mới. Trong game, Label được dùng làm ô bàn cờ hoặc chữ hiển thị trạng thái. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1669
```csharp
            if (hit)
```
**Giải thích:** Kiểm tra điều kiện `(hit`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1671
```csharp
                fallback.Text = "X";
```
**Giải thích:** Đổi chữ hiển thị của `fallback`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1672
```csharp
                fallback.ForeColor = Color.Red;
```
**Giải thích:** Đổi màu chữ của `fallback` để dễ nhìn trên nền hiện tại. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1674
```csharp
            else
```
**Giải thích:** Nhánh xử lý còn lại khi các điều kiện phía trước đều sai. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1676
```csharp
                fallback.Text = "O";
```
**Giải thích:** Đổi chữ hiển thị của `fallback`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1677
```csharp
                fallback.ForeColor = Color.DimGray;
```
**Giải thích:** Đổi màu chữ của `fallback` để dễ nhìn trên nền hiện tại. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1680
```csharp
            fallback.Font = new Font("Segoe UI", 18, FontStyle.Bold);
```
**Giải thích:** Tạo Font để chỉnh kiểu chữ, kích thước và độ đậm cho control. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1681
```csharp
            fallback.TextAlign = ContentAlignment.MiddleCenter;
```
**Giải thích:** Gán `ContentAlignment.MiddleCenter` cho `fallback.TextAlign`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1682
```csharp
            fallback.BackColor = Color.Transparent;
```
**Giải thích:** Đổi màu nền của `fallback`. Game dùng màu để thể hiện hiệu ứng, trạng thái hit/miss, skill hoặc thắng/thua. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1683
```csharp
            fallback.Width = cellWidth;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1684
```csharp
            fallback.Height = cellHeight;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1685
```csharp
            fallback.Left = col * cellWidth;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1686
```csharp
            fallback.Top = row * cellHeight;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1687
```csharp
            fallback.Tag = "VISUAL_PIN";
```
**Giải thích:** Lưu dữ liệu phụ vào control. Game dùng Tag để nhớ tọa độ ô, mã tàu hoặc tên skill khi kéo thả/click. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1689
```csharp
            board.Controls.Add(fallback);
```
**Giải thích:** Thêm control/dữ liệu vào collection. Với giao diện, control sẽ xuất hiện trên panel/form. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..

#### Dòng 1690
```csharp
            fallback.BringToFront();
```
**Giải thích:** Đưa control lên trên cùng để không bị panel/ảnh khác che. Ngữ cảnh: dòng này nằm trong `DrawPin`, tức phần vẽ hit/miss lên ô..


---

### Nhóm hàm `DrawShipImage`

**Vai trò:** vẽ ảnh tàu lên panel.

#### Dòng 1693
```csharp
        private void DrawShipImage(Panel board, int row, int col, int length, bool horizontal, int shipId)
```
**Giải thích:** Khai báo hàm `DrawShipImage(Panel board, int row, int col, int length, bool horizontal, int shipId)`. Công dụng chính: vẽ ảnh tàu lên panel.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1695
```csharp
            Image shipImage = PrepareShipImage(shipId, horizontal);
```
**Giải thích:** Gán `PrepareShipImage(shipId, horizontal)` cho `Image shipImage`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1697
```csharp
            if (shipImage == null)
```
**Giải thích:** Kiểm tra điều kiện `(shipImage == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1699
```csharp
                Label[,] cells = board == panelOwnBoard ? ownCells : enemyCells;
```
**Giải thích:** Cập nhật phần tử mảng `Label[,] cells`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1701
```csharp
                for (int i = 0; i < length; i++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1703
```csharp
                    int r = row;
```
**Giải thích:** Gán `row` cho `int r`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1704
```csharp
                    int c = col;
```
**Giải thích:** Gán `col` cho `int c`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1706
```csharp
                    if (horizontal)
```
**Giải thích:** Kiểm tra điều kiện `(horizontal`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1708
```csharp
                        c = col + i;
```
**Giải thích:** Gán `col + i` cho `c`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1710
```csharp
                    else
```
**Giải thích:** Nhánh xử lý còn lại khi các điều kiện phía trước đều sai. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1712
```csharp
                        r = row + i;
```
**Giải thích:** Gán `row + i` cho `r`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1715
```csharp
                    if (r >= 0 && r < BOARD_SIZE && c >= 0 && c < BOARD_SIZE)
```
**Giải thích:** Kiểm tra điều kiện `(r >= 0 && r < BOARD_SIZE && c >= 0 && c < BOARD_SIZE`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1717
```csharp
                        cells[r, c].Text = "🚢";
```
**Giải thích:** Cập nhật phần tử mảng `cells[r, c].Text`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1718
```csharp
                        cells[r, c].ForeColor = board == panelOwnBoard ? Color.DarkGreen : Color.DarkBlue;
```
**Giải thích:** Cập nhật phần tử mảng `cells[r, c].ForeColor`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1722
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1725
```csharp
            int cellWidth = board.ClientSize.Width / BOARD_SIZE;
```
**Giải thích:** Gán `board.ClientSize.Width / BOARD_SIZE` cho `int cellWidth`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1726
```csharp
            int cellHeight = board.ClientSize.Height / BOARD_SIZE;
```
**Giải thích:** Gán `board.ClientSize.Height / BOARD_SIZE` cho `int cellHeight`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1728
```csharp
            PictureBox ship = new PictureBox();
```
**Giải thích:** Tạo PictureBox để hiển thị ảnh động/tàu/hit/miss trên form hoặc panel. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1729
```csharp
            ship.Image = shipImage;
```
**Giải thích:** Gán ảnh cho `ship` để control hiển thị hình tương ứng. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1730
```csharp
            ship.SizeMode = PictureBoxSizeMode.StretchImage;
```
**Giải thích:** Gán `PictureBoxSizeMode.StretchImage` cho `ship.SizeMode`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1731
```csharp
            ship.BackColor = Color.Transparent;
```
**Giải thích:** Đổi màu nền của `ship`. Game dùng màu để thể hiện hiệu ứng, trạng thái hit/miss, skill hoặc thắng/thua. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1732
```csharp
            ship.Left = col * cellWidth;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1733
```csharp
            ship.Top = row * cellHeight;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1734
```csharp
            ship.Tag = "VISUAL_SHIP_" + shipId;
```
**Giải thích:** Lưu dữ liệu phụ vào control. Game dùng Tag để nhớ tọa độ ô, mã tàu hoặc tên skill khi kéo thả/click. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1735
```csharp
            ship.AllowDrop = true;
```
**Giải thích:** Gán `true` cho `ship.AllowDrop`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1736
```csharp
            ship.DragEnter += Board_DragEnter;
```
**Giải thích:** Gán `Board_DragEnter` cho `ship.DragEnter +`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1738
```csharp
            if (board == panelOwnBoard)
```
**Giải thích:** Kiểm tra điều kiện `(board == panelOwnBoard`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1740
```csharp
                ship.DragDrop += OwnBoard_DragDrop;
```
**Giải thích:** Gán `OwnBoard_DragDrop` cho `ship.DragDrop +`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1741
```csharp
                ship.MouseDown += PlacedShip_MouseDown;
```
**Giải thích:** Gán `PlacedShip_MouseDown` cho `ship.MouseDown +`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1742
```csharp
                ship.MouseDown += Control_MouseDownSound;
```
**Giải thích:** Gán `Control_MouseDownSound` cho `ship.MouseDown +`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1743
```csharp
                ship.Cursor = Cursors.Hand;
```
**Giải thích:** Đổi hình con trỏ chuột, ví dụ Hand khi có thể dùng skill, No khi bị khóa. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1745
```csharp
            else
```
**Giải thích:** Nhánh xử lý còn lại khi các điều kiện phía trước đều sai. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1747
```csharp
                ship.DragDrop += EnemyBoard_DragDrop;
```
**Giải thích:** Gán `EnemyBoard_DragDrop` cho `ship.DragDrop +`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1750
```csharp
            if (horizontal)
```
**Giải thích:** Kiểm tra điều kiện `(horizontal`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1752
```csharp
                ship.Width = cellWidth * length;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1753
```csharp
                ship.Height = cellHeight;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1755
```csharp
            else
```
**Giải thích:** Nhánh xử lý còn lại khi các điều kiện phía trước đều sai. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1757
```csharp
                ship.Width = cellWidth;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1758
```csharp
                ship.Height = cellHeight * length;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1761
```csharp
            board.Controls.Add(ship);
```
**Giải thích:** Thêm control/dữ liệu vào collection. Với giao diện, control sẽ xuất hiện trên panel/form. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1763
```csharp
            if (board == panelEnemyBoard)
```
**Giải thích:** Kiểm tra điều kiện `(board == panelEnemyBoard`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1765
```csharp
                ship.Image = CreateOpacityImage(shipImage, 0.05f);
```
**Giải thích:** Gán ảnh cho `ship` để control hiển thị hình tương ứng. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1766
```csharp
                FadeInPictureBox(ship, shipImage, 10, 35);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..

#### Dòng 1769
```csharp
            ship.BringToFront();
```
**Giải thích:** Đưa control lên trên cùng để không bị panel/ảnh khác che. Ngữ cảnh: dòng này nằm trong `DrawShipImage`, tức phần vẽ ảnh tàu lên panel..


---

### Nhóm hàm `HookClickSounds`

**Vai trò:** gắn âm click cho control.

#### Dòng 1772
```csharp
        private void HookClickSounds(Control parent)
```
**Giải thích:** Khai báo hàm `HookClickSounds(Control parent)`. Công dụng chính: gắn âm click cho control.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1774
```csharp
            foreach (Control control in parent.Controls)
```
**Giải thích:** Vòng lặp `foreach` duyệt từng control hoặc phần tử trong danh sách mà không cần tự quản lý chỉ số. Ngữ cảnh: dòng này nằm trong `HookClickSounds`, tức phần gắn âm click cho control..

#### Dòng 1776
```csharp
                bool shouldHook =
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `HookClickSounds`, tức phần gắn âm click cho control..

#### Dòng 1777
```csharp
                    control is Button ||
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `HookClickSounds`, tức phần gắn âm click cho control..

#### Dòng 1778
```csharp
                    control is TextBox ||
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `HookClickSounds`, tức phần gắn âm click cho control..

#### Dòng 1779
```csharp
                    control is PictureBox ||
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `HookClickSounds`, tức phần gắn âm click cho control..

#### Dòng 1780
```csharp
                    (control is Label && control.Tag is Point);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `HookClickSounds`, tức phần gắn âm click cho control..

#### Dòng 1782
```csharp
                if (shouldHook)
```
**Giải thích:** Kiểm tra điều kiện `(shouldHook`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `HookClickSounds`, tức phần gắn âm click cho control..

#### Dòng 1784
```csharp
                    control.MouseDown -= Control_MouseDownSound;
```
**Giải thích:** Gán `Control_MouseDownSound` cho `control.MouseDown -`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `HookClickSounds`, tức phần gắn âm click cho control..

#### Dòng 1785
```csharp
                    control.MouseDown += Control_MouseDownSound;
```
**Giải thích:** Gán `Control_MouseDownSound` cho `control.MouseDown +`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `HookClickSounds`, tức phần gắn âm click cho control..

#### Dòng 1788
```csharp
                if (control.HasChildren)
```
**Giải thích:** Kiểm tra điều kiện `(control.HasChildren`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `HookClickSounds`, tức phần gắn âm click cho control..

#### Dòng 1790
```csharp
                    HookClickSounds(control);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `HookClickSounds`, tức phần gắn âm click cho control..


---

### Nhóm hàm `Control_MouseDownSound`

**Vai trò:** phát click.wav khi nhấn chuột.

#### Dòng 1795
```csharp
        private void Control_MouseDownSound(object sender, MouseEventArgs e)
```
**Giải thích:** Khai báo hàm `Control_MouseDownSound(object sender, MouseEventArgs e)`. Công dụng chính: phát click.wav khi nhấn chuột.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1797
```csharp
            PlaySound("click.wav");
```
**Giải thích:** Gọi hàm `PlaySound` để phát âm thanh trong thư mục Sounds. Ngữ cảnh: dòng này nằm trong `Control_MouseDownSound`, tức phần phát click.wav khi nhấn chuột..


---

### Nhóm hàm `PlaySound`

**Vai trò:** phát file âm thanh.

#### Dòng 1800
```csharp
        private void PlaySound(string fileName)
```
**Giải thích:** Khai báo hàm `PlaySound(string fileName)`. Công dụng chính: phát file âm thanh.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1802
```csharp
            try
```
**Giải thích:** Bắt đầu khối có thể phát sinh lỗi như kết nối mạng, parse dữ liệu, đọc ảnh/âm thanh. Ngữ cảnh: dòng này nằm trong `PlaySound`, tức phần phát file âm thanh..

#### Dòng 1804
```csharp
                string soundPath = FindSoundFile(fileName);
```
**Giải thích:** Gán `FindSoundFile(fileName)` cho `string soundPath`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `PlaySound`, tức phần phát file âm thanh..

#### Dòng 1806
```csharp
                if (soundPath == null)
```
**Giải thích:** Kiểm tra điều kiện `(soundPath == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `PlaySound`, tức phần phát file âm thanh..

#### Dòng 1808
```csharp
                    return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `PlaySound`, tức phần phát file âm thanh..

#### Dòng 1811
```csharp
                SoundPlayer player = new SoundPlayer(soundPath);
```
**Giải thích:** Tạo SoundPlayer từ file wav. Đối tượng này dùng để phát âm thanh game. Ngữ cảnh: dòng này nằm trong `PlaySound`, tức phần phát file âm thanh..

#### Dòng 1812
```csharp
                player.Play();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `PlaySound`, tức phần phát file âm thanh..

#### Dòng 1814
```csharp
            catch
```
**Giải thích:** Bắt lỗi để chương trình không bị crash; thường hiển thị thông báo hoặc bỏ qua lỗi nhỏ. Ngữ cảnh: dòng này nằm trong `PlaySound`, tức phần phát file âm thanh..


---

### Nhóm hàm `FindSoundFile`

**Vai trò:** tìm đường dẫn file âm thanh.

#### Dòng 1820
```csharp
        private string FindSoundFile(string fileName)
```
**Giải thích:** Khai báo hàm `FindSoundFile(string fileName)`. Công dụng chính: tìm đường dẫn file âm thanh.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1822
```csharp
            try
```
**Giải thích:** Bắt đầu khối có thể phát sinh lỗi như kết nối mạng, parse dữ liệu, đọc ảnh/âm thanh. Ngữ cảnh: dòng này nằm trong `FindSoundFile`, tức phần tìm đường dẫn file âm thanh..

#### Dòng 1824
```csharp
                string folder = Application.StartupPath;
```
**Giải thích:** Gán `Application.StartupPath` cho `string folder`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `FindSoundFile`, tức phần tìm đường dẫn file âm thanh..

#### Dòng 1826
```csharp
                for (int i = 0; i < 8; i++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `FindSoundFile`, tức phần tìm đường dẫn file âm thanh..

#### Dòng 1828
```csharp
                    string path1 = Path.Combine(folder, "Sounds", fileName);
```
**Giải thích:** Gán `Path.Combine(folder, "Sounds", fileName)` cho `string path1`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `FindSoundFile`, tức phần tìm đường dẫn file âm thanh..

#### Dòng 1829
```csharp
                    string path2 = Path.Combine(folder, fileName);
```
**Giải thích:** Gán `Path.Combine(folder, fileName)` cho `string path2`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `FindSoundFile`, tức phần tìm đường dẫn file âm thanh..

#### Dòng 1831
```csharp
                    if (File.Exists(path1))
```
**Giải thích:** Kiểm tra điều kiện `(File.Exists(path1)`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `FindSoundFile`, tức phần tìm đường dẫn file âm thanh..

#### Dòng 1833
```csharp
                        return path1;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `FindSoundFile`, tức phần tìm đường dẫn file âm thanh..

#### Dòng 1836
```csharp
                    if (File.Exists(path2))
```
**Giải thích:** Kiểm tra điều kiện `(File.Exists(path2)`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `FindSoundFile`, tức phần tìm đường dẫn file âm thanh..

#### Dòng 1838
```csharp
                        return path2;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `FindSoundFile`, tức phần tìm đường dẫn file âm thanh..

#### Dòng 1841
```csharp
                    DirectoryInfo parent = Directory.GetParent(folder);
```
**Giải thích:** Gán `Directory.GetParent(folder)` cho `DirectoryInfo parent`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `FindSoundFile`, tức phần tìm đường dẫn file âm thanh..

#### Dòng 1843
```csharp
                    if (parent == null)
```
**Giải thích:** Kiểm tra điều kiện `(parent == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `FindSoundFile`, tức phần tìm đường dẫn file âm thanh..

#### Dòng 1845
```csharp
                        break;
```
**Giải thích:** Kết thúc case hoặc vòng lặp hiện tại. Ngữ cảnh: dòng này nằm trong `FindSoundFile`, tức phần tìm đường dẫn file âm thanh..

#### Dòng 1848
```csharp
                    folder = parent.FullName;
```
**Giải thích:** Gán `parent.FullName` cho `folder`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `FindSoundFile`, tức phần tìm đường dẫn file âm thanh..

#### Dòng 1851
```csharp
            catch
```
**Giải thích:** Bắt lỗi để chương trình không bị crash; thường hiển thị thông báo hoặc bỏ qua lỗi nhỏ. Ngữ cảnh: dòng này nằm trong `FindSoundFile`, tức phần tìm đường dẫn file âm thanh..

#### Dòng 1856
```csharp
            return null;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `FindSoundFile`, tức phần tìm đường dẫn file âm thanh..


---

### Nhóm hàm `CreateOpacityImage`

**Vai trò:** tạo ảnh mờ cho hiệu ứng fade.

#### Dòng 1859
```csharp
        private Image CreateOpacityImage(Image source, float opacity)
```
**Giải thích:** Khai báo hàm `CreateOpacityImage(Image source, float opacity)`. Công dụng chính: tạo ảnh mờ cho hiệu ứng fade.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1861
```csharp
            Bitmap bitmap = new Bitmap(source.Width, source.Height);
```
**Giải thích:** Gán `new Bitmap(source.Width, source.Height)` cho `Bitmap bitmap`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateOpacityImage`, tức phần tạo ảnh mờ cho hiệu ứng fade..

#### Dòng 1865
```csharp
                ColorMatrix matrix = new ColorMatrix();
```
**Giải thích:** Tạo ma trận màu để chỉnh độ trong suốt của ảnh, phục vụ hiệu ứng mờ/rõ. Ngữ cảnh: dòng này nằm trong `CreateOpacityImage`, tức phần tạo ảnh mờ cho hiệu ứng fade..

#### Dòng 1866
```csharp
                matrix.Matrix33 = opacity;
```
**Giải thích:** Gán `opacity` cho `matrix.Matrix33`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateOpacityImage`, tức phần tạo ảnh mờ cho hiệu ứng fade..

#### Dòng 1870
```csharp
                    attributes.SetColorMatrix(matrix, ColorMatrixFlag.Default, ColorAdjustType.Bitmap);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `CreateOpacityImage`, tức phần tạo ảnh mờ cho hiệu ứng fade..

#### Dòng 1872
```csharp
                    graphics.DrawImage(
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `CreateOpacityImage`, tức phần tạo ảnh mờ cho hiệu ứng fade..

#### Dòng 1873
```csharp
                        source,
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `CreateOpacityImage`, tức phần tạo ảnh mờ cho hiệu ứng fade..

#### Dòng 1874
```csharp
                        new Rectangle(0, 0, bitmap.Width, bitmap.Height),
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `CreateOpacityImage`, tức phần tạo ảnh mờ cho hiệu ứng fade..

#### Dòng 1875
```csharp
                        0,
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `CreateOpacityImage`, tức phần tạo ảnh mờ cho hiệu ứng fade..

#### Dòng 1876
```csharp
                        0,
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `CreateOpacityImage`, tức phần tạo ảnh mờ cho hiệu ứng fade..

#### Dòng 1877
```csharp
                        source.Width,
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `CreateOpacityImage`, tức phần tạo ảnh mờ cho hiệu ứng fade..

#### Dòng 1878
```csharp
                        source.Height,
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `CreateOpacityImage`, tức phần tạo ảnh mờ cho hiệu ứng fade..

#### Dòng 1879
```csharp
                        GraphicsUnit.Pixel,
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `CreateOpacityImage`, tức phần tạo ảnh mờ cho hiệu ứng fade..

#### Dòng 1880
```csharp
                        attributes
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `CreateOpacityImage`, tức phần tạo ảnh mờ cho hiệu ứng fade..

#### Dòng 1881
```csharp
                    );
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `CreateOpacityImage`, tức phần tạo ảnh mờ cho hiệu ứng fade..

#### Dòng 1885
```csharp
            return bitmap;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `CreateOpacityImage`, tức phần tạo ảnh mờ cho hiệu ứng fade..


---

### Nhóm hàm `AnimateShotPictureBox`

**Vai trò:** animation ảnh hit/miss.

#### Dòng 1888
```csharp
        private void AnimateShotPictureBox(PictureBox pictureBox, Image originalImage, int finalLeft, int finalTop)
```
**Giải thích:** Khai báo hàm `AnimateShotPictureBox(PictureBox pictureBox, Image originalImage, int finalLeft, int finalTop)`. Công dụng chính: animation ảnh hit/miss.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1890
```csharp
            int currentStep = 1;
```
**Giải thích:** Gán `1` cho `int currentStep`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1891
```csharp
            int startTop = pictureBox.Top;
```
**Giải thích:** Gán `pictureBox.Top` cho `int startTop`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1892
```csharp
            int startLeft = pictureBox.Left;
```
**Giải thích:** Gán `pictureBox.Left` cho `int startLeft`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1894
```csharp
            System.Windows.Forms.Timer animationTimer = new System.Windows.Forms.Timer();
```
**Giải thích:** Tạo Timer WinForms để chạy hiệu ứng theo thời gian, ví dụ nhấp nháy, fade, chuyển lượt. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1895
```csharp
            animationTimer.Interval = SHOT_ANIMATION_INTERVAL;
```
**Giải thích:** Gán `SHOT_ANIMATION_INTERVAL` cho `animationTimer.Interval`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1897
```csharp
            animationTimer.Tick += (sender, e) =>
```
**Giải thích:** Tạo lambda callback ngắn. Code này thường chạy khi Timer tick, thread chạy, hoặc event xảy ra. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1899
```csharp
                if (pictureBox.IsDisposed)
```
**Giải thích:** Kiểm tra điều kiện `(pictureBox.IsDisposed`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1901
```csharp
                    animationTimer.Stop();
```
**Giải thích:** Dừng timer hoặc âm thanh đang chạy. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1902
```csharp
                    animationTimer.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1903
```csharp
                    return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1906
```csharp
                currentStep++;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1907
```csharp
                double t = currentStep / (double)SHOT_ANIMATION_STEPS;
```
**Giải thích:** Gán `currentStep / (double)SHOT_ANIMATION_STEPS` cho `double t`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1909
```csharp
                if (t > 1.0)
```
**Giải thích:** Kiểm tra điều kiện `(t > 1.0`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1911
```csharp
                    t = 1.0;
```
**Giải thích:** Gán `1.0` cho `t`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1914
```csharp
                double eased = 1.0 - Math.Pow(1.0 - t, 3.0);
```
**Giải thích:** Gán `1.0 - Math.Pow(1.0 - t, 3.0)` cho `double eased`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1915
```csharp
                float opacity = (float)t;
```
**Giải thích:** Gán `(float)t` cho `float opacity`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1917
```csharp
                Image oldImage = pictureBox.Image;
```
**Giải thích:** Gán `pictureBox.Image` cho `Image oldImage`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1918
```csharp
                pictureBox.Image = CreateOpacityImage(originalImage, opacity);
```
**Giải thích:** Gán ảnh cho `pictureBox` để control hiển thị hình tương ứng. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1920
```csharp
                if (oldImage != null)
```
**Giải thích:** Kiểm tra điều kiện `(oldImage != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1922
```csharp
                    oldImage.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1925
```csharp
                pictureBox.Left = startLeft + (int)((finalLeft - startLeft) * eased);
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1926
```csharp
                pictureBox.Top = startTop + (int)((finalTop - startTop) * eased);
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1928
```csharp
                if (currentStep >= SHOT_ANIMATION_STEPS)
```
**Giải thích:** Kiểm tra điều kiện `(currentStep >= SHOT_ANIMATION_STEPS`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1930
```csharp
                    pictureBox.Left = finalLeft;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1931
```csharp
                    pictureBox.Top = finalTop;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1932
```csharp
                    animationTimer.Stop();
```
**Giải thích:** Dừng timer hoặc âm thanh đang chạy. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1933
```csharp
                    animationTimer.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..

#### Dòng 1937
```csharp
            animationTimer.Start();
```
**Giải thích:** Bắt đầu chạy thread, timer hoặc listener đã được tạo trước đó. Ngữ cảnh: dòng này nằm trong `AnimateShotPictureBox`, tức phần animation ảnh hit/miss..


---

### Nhóm hàm `FadeInPictureBox`

**Vai trò:** animation rõ dần.

#### Dòng 1940
```csharp
        private void FadeInPictureBox(PictureBox pictureBox, Image originalImage, int steps, int interval)
```
**Giải thích:** Khai báo hàm `FadeInPictureBox(PictureBox pictureBox, Image originalImage, int steps, int interval)`. Công dụng chính: animation rõ dần.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1942
```csharp
            int currentStep = 1;
```
**Giải thích:** Gán `1` cho `int currentStep`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1944
```csharp
            System.Windows.Forms.Timer fadeTimer = new System.Windows.Forms.Timer();
```
**Giải thích:** Tạo Timer WinForms để chạy hiệu ứng theo thời gian, ví dụ nhấp nháy, fade, chuyển lượt. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1945
```csharp
            fadeTimer.Interval = interval;
```
**Giải thích:** Gán `interval` cho `fadeTimer.Interval`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1947
```csharp
            fadeTimer.Tick += (sender, e) =>
```
**Giải thích:** Tạo lambda callback ngắn. Code này thường chạy khi Timer tick, thread chạy, hoặc event xảy ra. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1949
```csharp
                if (pictureBox.IsDisposed)
```
**Giải thích:** Kiểm tra điều kiện `(pictureBox.IsDisposed`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1951
```csharp
                    fadeTimer.Stop();
```
**Giải thích:** Dừng timer hoặc âm thanh đang chạy. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1952
```csharp
                    fadeTimer.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1953
```csharp
                    return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1956
```csharp
                currentStep++;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1958
```csharp
                float opacity = currentStep / (float)steps;
```
**Giải thích:** Gán `currentStep / (float)steps` cho `float opacity`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1960
```csharp
                if (opacity > 1f)
```
**Giải thích:** Kiểm tra điều kiện `(opacity > 1f`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1962
```csharp
                    opacity = 1f;
```
**Giải thích:** Gán `1f` cho `opacity`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1965
```csharp
                Image oldImage = pictureBox.Image;
```
**Giải thích:** Gán `pictureBox.Image` cho `Image oldImage`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1966
```csharp
                pictureBox.Image = CreateOpacityImage(originalImage, opacity);
```
**Giải thích:** Gán ảnh cho `pictureBox` để control hiển thị hình tương ứng. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1968
```csharp
                if (oldImage != null)
```
**Giải thích:** Kiểm tra điều kiện `(oldImage != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1970
```csharp
                    oldImage.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1973
```csharp
                if (currentStep >= steps)
```
**Giải thích:** Kiểm tra điều kiện `(currentStep >= steps`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1975
```csharp
                    fadeTimer.Stop();
```
**Giải thích:** Dừng timer hoặc âm thanh đang chạy. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1976
```csharp
                    fadeTimer.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..

#### Dòng 1980
```csharp
            fadeTimer.Start();
```
**Giải thích:** Bắt đầu chạy thread, timer hoặc listener đã được tạo trước đó. Ngữ cảnh: dòng này nằm trong `FadeInPictureBox`, tức phần animation rõ dần..


---

### Nhóm hàm `GetShipImageName`

**Vai trò:** trả tên file ảnh tàu.

#### Dòng 1983
```csharp
        private string GetShipImageName(int shipId)
```
**Giải thích:** Khai báo hàm `GetShipImageName(int shipId)`. Công dụng chính: trả tên file ảnh tàu.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 1985
```csharp
            if (shipId == 0)
```
**Giải thích:** Kiểm tra điều kiện `(shipId == 0`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `GetShipImageName`, tức phần trả tên file ảnh tàu..

#### Dòng 1987
```csharp
                return "ship2.png";
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `GetShipImageName`, tức phần trả tên file ảnh tàu..

#### Dòng 1990
```csharp
            if (shipId == 1)
```
**Giải thích:** Kiểm tra điều kiện `(shipId == 1`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `GetShipImageName`, tức phần trả tên file ảnh tàu..

#### Dòng 1992
```csharp
                return "ship3.png";
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `GetShipImageName`, tức phần trả tên file ảnh tàu..

#### Dòng 1995
```csharp
            if (shipId == 2)
```
**Giải thích:** Kiểm tra điều kiện `(shipId == 2`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `GetShipImageName`, tức phần trả tên file ảnh tàu..

#### Dòng 1997
```csharp
                return "ship4.png";
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `GetShipImageName`, tức phần trả tên file ảnh tàu..

#### Dòng 2000
```csharp
            return "shipspare.png";
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `GetShipImageName`, tức phần trả tên file ảnh tàu..


---

### Nhóm hàm `LoadImageSafe`

**Vai trò:** nạp ảnh an toàn.

#### Dòng 2003
```csharp
        private Image LoadImageSafe(string fileName)
```
**Giải thích:** Khai báo hàm `LoadImageSafe(string fileName)`. Công dụng chính: nạp ảnh an toàn.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 2005
```csharp
            try
```
**Giải thích:** Bắt đầu khối có thể phát sinh lỗi như kết nối mạng, parse dữ liệu, đọc ảnh/âm thanh. Ngữ cảnh: dòng này nằm trong `LoadImageSafe`, tức phần nạp ảnh an toàn..

#### Dòng 2007
```csharp
                string folder = Application.StartupPath;
```
**Giải thích:** Gán `Application.StartupPath` cho `string folder`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `LoadImageSafe`, tức phần nạp ảnh an toàn..

#### Dòng 2009
```csharp
                for (int i = 0; i < 8; i++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `LoadImageSafe`, tức phần nạp ảnh an toàn..

#### Dòng 2011
```csharp
                    string path1 = Path.Combine(folder, "Images", fileName);
```
**Giải thích:** Gán `Path.Combine(folder, "Images", fileName)` cho `string path1`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `LoadImageSafe`, tức phần nạp ảnh an toàn..

#### Dòng 2012
```csharp
                    string path2 = Path.Combine(folder, fileName);
```
**Giải thích:** Gán `Path.Combine(folder, fileName)` cho `string path2`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `LoadImageSafe`, tức phần nạp ảnh an toàn..

#### Dòng 2014
```csharp
                    if (File.Exists(path1))
```
**Giải thích:** Kiểm tra điều kiện `(File.Exists(path1)`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `LoadImageSafe`, tức phần nạp ảnh an toàn..

#### Dòng 2018
```csharp
                            return new Bitmap(fs);
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `LoadImageSafe`, tức phần nạp ảnh an toàn..

#### Dòng 2022
```csharp
                    if (File.Exists(path2))
```
**Giải thích:** Kiểm tra điều kiện `(File.Exists(path2)`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `LoadImageSafe`, tức phần nạp ảnh an toàn..

#### Dòng 2026
```csharp
                            return new Bitmap(fs);
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `LoadImageSafe`, tức phần nạp ảnh an toàn..

#### Dòng 2030
```csharp
                    DirectoryInfo parent = Directory.GetParent(folder);
```
**Giải thích:** Gán `Directory.GetParent(folder)` cho `DirectoryInfo parent`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `LoadImageSafe`, tức phần nạp ảnh an toàn..

#### Dòng 2032
```csharp
                    if (parent == null)
```
**Giải thích:** Kiểm tra điều kiện `(parent == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `LoadImageSafe`, tức phần nạp ảnh an toàn..

#### Dòng 2034
```csharp
                        break;
```
**Giải thích:** Kết thúc case hoặc vòng lặp hiện tại. Ngữ cảnh: dòng này nằm trong `LoadImageSafe`, tức phần nạp ảnh an toàn..

#### Dòng 2037
```csharp
                    folder = parent.FullName;
```
**Giải thích:** Gán `parent.FullName` cho `folder`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `LoadImageSafe`, tức phần nạp ảnh an toàn..

#### Dòng 2040
```csharp
            catch
```
**Giải thích:** Bắt lỗi để chương trình không bị crash; thường hiển thị thông báo hoặc bỏ qua lỗi nhỏ. Ngữ cảnh: dòng này nằm trong `LoadImageSafe`, tức phần nạp ảnh an toàn..

#### Dòng 2045
```csharp
            return null;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `LoadImageSafe`, tức phần nạp ảnh an toàn..


---

### Nhóm hàm `ShowEndGameOptions`

**Vai trò:** gọi màn hình kết thúc.

#### Dòng 2048
```csharp
        private void ShowEndGameOptions(string result)
```
**Giải thích:** Khai báo hàm `ShowEndGameOptions(string result)`. Công dụng chính: gọi màn hình kết thúc.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 2050
```csharp
            gameEnded = true;
```
**Giải thích:** Gán giá trị mới cho `gameEnded`. cho biết trận đã kết thúc; khi true thì khóa bắn và khóa skill. Giá trị `true` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ShowEndGameOptions`, tức phần gọi màn hình kết thúc..

#### Dòng 2051
```csharp
            myTurn = false;
```
**Giải thích:** Gán giá trị mới cho `myTurn`. cho biết hiện tại có phải lượt bắn của mình. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ShowEndGameOptions`, tức phần gọi màn hình kết thúc..

#### Dòng 2052
```csharp
            actionLocked = true;
```
**Giải thích:** Gán giá trị mới cho `actionLocked`. khóa thao tác trong lúc animation/delay/skill đang chạy để tránh click sai thời điểm. Giá trị `true` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ShowEndGameOptions`, tức phần gọi màn hình kết thúc..

#### Dòng 2053
```csharp
            turnTransitionRunning = false;
```
**Giải thích:** Gán giá trị mới cho `turnTransitionRunning`. cho biết animation chuyển lượt đang chạy. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ShowEndGameOptions`, tức phần gọi màn hình kết thúc..

#### Dòng 2055
```csharp
            lblTurn.Text = "KẾT THÚC";
```
**Giải thích:** Đổi chữ hiển thị của `lblTurn`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `ShowEndGameOptions`, tức phần gọi màn hình kết thúc..

#### Dòng 2056
```csharp
            lblStatus.Text = "TRẠNG THÁI: " + result.ToUpper();
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `ShowEndGameOptions`, tức phần gọi màn hình kết thúc..

#### Dòng 2058
```csharp
            DisableAllBoardCells();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ShowEndGameOptions`, tức phần gọi màn hình kết thúc..

#### Dòng 2060
```csharp
            btnPlayAgain.Visible = false;
```
**Giải thích:** Ẩn/hiện `btnPlayAgain`. Ví dụ nút Chơi lại chỉ hiện khi trận kết thúc. Ngữ cảnh: dòng này nằm trong `ShowEndGameOptions`, tức phần gọi màn hình kết thúc..

#### Dòng 2061
```csharp
            btnExitGame.Visible = false;
```
**Giải thích:** Ẩn/hiện `btnExitGame`. Ví dụ nút Chơi lại chỉ hiện khi trận kết thúc. Ngữ cảnh: dòng này nằm trong `ShowEndGameOptions`, tức phần gọi màn hình kết thúc..

#### Dòng 2062
```csharp
            btnPlayAgain.Enabled = true;
```
**Giải thích:** Bật/tắt tương tác `btnPlayAgain`. Khi false người chơi không click/kéo được. Ngữ cảnh: dòng này nằm trong `ShowEndGameOptions`, tức phần gọi màn hình kết thúc..

#### Dòng 2063
```csharp
            btnExitGame.Enabled = true;
```
**Giải thích:** Bật/tắt tương tác `btnExitGame`. Khi false người chơi không click/kéo được. Ngữ cảnh: dòng này nằm trong `ShowEndGameOptions`, tức phần gọi màn hình kết thúc..

#### Dòng 2065
```csharp
            bool isWin = result.ToLower().Contains("thắng");
```
**Giải thích:** Gán `result.ToLower().Contains("thắng")` cho `bool isWin`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ShowEndGameOptions`, tức phần gọi màn hình kết thúc..

#### Dòng 2066
```csharp
            ShowEndGameOverlay(isWin);
```
**Giải thích:** Gọi hàm `ShowEndGameOverlay` để hiện màn hình thắng/thua. Ngữ cảnh: dòng này nằm trong `ShowEndGameOptions`, tức phần gọi màn hình kết thúc..


---

### Nhóm hàm `ShowEndGameOverlay`

**Vai trò:** tạo overlay thắng/thua.

#### Dòng 2069
```csharp
        private void ShowEndGameOverlay(bool isWin)
```
**Giải thích:** Khai báo hàm `ShowEndGameOverlay(bool isWin)`. Công dụng chính: tạo overlay thắng/thua.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 2071
```csharp
            RemoveEndGameOverlay();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2073
```csharp
            Color endColor = isWin ? Color.FromArgb(248, 222, 120) : Color.FromArgb(135, 135, 135);
```
**Giải thích:** Gán `isWin ? Color.FromArgb(248, 222, 120) : Color.FromArgb(135, 135, 135)` cho `Color endColor`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2074
```csharp
            string resultText = isWin ? "BẠN THẮNG!" : "BẠN THUA!";
```
**Giải thích:** Gán `isWin ? "BẠN THẮNG!" : "BẠN THUA!"` cho `string resultText`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2076
```csharp
            this.BackgroundImage = null;
```
**Giải thích:** Gán `null` cho `this.BackgroundImage`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2077
```csharp
            this.BackColor = endColor;
```
**Giải thích:** Đổi màu nền của `this`. Game dùng màu để thể hiện hiệu ứng, trạng thái hit/miss, skill hoặc thắng/thua. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2079
```csharp
            panelEndOverlay = new Panel();
```
**Giải thích:** Tạo Panel mới để chứa control, ví dụ overlay thắng/thua hoặc bàn cờ. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2080
```csharp
            panelEndOverlay.Name = "panelEndOverlay";
```
**Giải thích:** Gán `"panelEndOverlay"` cho `panelEndOverlay.Name`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2081
```csharp
            panelEndOverlay.BackColor = endColor;
```
**Giải thích:** Đổi màu nền của `panelEndOverlay`. Game dùng màu để thể hiện hiệu ứng, trạng thái hit/miss, skill hoặc thắng/thua. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2082
```csharp
            panelEndOverlay.Left = 0;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2083
```csharp
            panelEndOverlay.Top = 0;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2084
```csharp
            panelEndOverlay.Width = this.ClientSize.Width;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2085
```csharp
            panelEndOverlay.Height = this.ClientSize.Height;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2086
```csharp
            panelEndOverlay.Anchor = AnchorStyles.Top | AnchorStyles.Bottom | AnchorStyles.Left | AnchorStyles.Right;
```
**Giải thích:** Gán `AnchorStyles.Top | AnchorStyles.Bottom | AnchorStyles.Left | AnchorStyles.Right` cho `panelEndOverlay.Anchor`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2088
```csharp
            lblEndResult = new Label();
```
**Giải thích:** Tạo Label mới. Trong game, Label được dùng làm ô bàn cờ hoặc chữ hiển thị trạng thái. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2089
```csharp
            lblEndResult.Text = resultText;
```
**Giải thích:** Đổi chữ hiển thị của `lblEndResult`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2090
```csharp
            lblEndResult.ForeColor = Color.White;
```
**Giải thích:** Đổi màu chữ của `lblEndResult` để dễ nhìn trên nền hiện tại. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2091
```csharp
            lblEndResult.BackColor = Color.Transparent;
```
**Giải thích:** Đổi màu nền của `lblEndResult`. Game dùng màu để thể hiện hiệu ứng, trạng thái hit/miss, skill hoặc thắng/thua. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2092
```csharp
            lblEndResult.Font = new Font("Segoe UI Black", 14, FontStyle.Bold);
```
**Giải thích:** Tạo Font để chỉnh kiểu chữ, kích thước và độ đậm cho control. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2093
```csharp
            lblEndResult.TextAlign = ContentAlignment.MiddleCenter;
```
**Giải thích:** Gán `ContentAlignment.MiddleCenter` cho `lblEndResult.TextAlign`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2094
```csharp
            lblEndResult.Width = this.ClientSize.Width;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2095
```csharp
            lblEndResult.Height = 140;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2096
```csharp
            lblEndResult.Left = 0;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2097
```csharp
            lblEndResult.Top = (this.ClientSize.Height - lblEndResult.Height) / 2 - 90;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2099
```csharp
            lblEndReplay = CreateEndActionLabel("CHƠI LẠI");
```
**Giải thích:** Gán giá trị mới cho `lblEndReplay`. label dạng nút Chơi lại trên màn hình kết thúc. Giá trị `CreateEndActionLabel("CHƠI LẠI")` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2100
```csharp
            lblEndExit = CreateEndActionLabel("THOÁT");
```
**Giải thích:** Gán giá trị mới cho `lblEndExit`. label dạng nút Thoát trên màn hình kết thúc. Giá trị `CreateEndActionLabel("THOÁT")` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2102
```csharp
            lblEndReplay.Left = this.ClientSize.Width / 2 - lblEndReplay.Width - 25;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2103
```csharp
            lblEndExit.Left = this.ClientSize.Width / 2 + 25;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2104
```csharp
            lblEndReplay.Top = this.ClientSize.Height + 60;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2105
```csharp
            lblEndExit.Top = this.ClientSize.Height + 60;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2106
```csharp
            lblEndReplay.Visible = false;
```
**Giải thích:** Ẩn/hiện `lblEndReplay`. Ví dụ nút Chơi lại chỉ hiện khi trận kết thúc. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2107
```csharp
            lblEndExit.Visible = false;
```
**Giải thích:** Ẩn/hiện `lblEndExit`. Ví dụ nút Chơi lại chỉ hiện khi trận kết thúc. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2109
```csharp
            lblEndReplay.Click += (sender, e) =>
```
**Giải thích:** Tạo lambda callback ngắn. Code này thường chạy khi Timer tick, thread chạy, hoặc event xảy ra. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2111
```csharp
                lblEndReplay.Enabled = false;
```
**Giải thích:** Bật/tắt tương tác `lblEndReplay`. Khi false người chơi không click/kéo được. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2112
```csharp
                lblEndReplay.Text = "ĐANG CHỜ...";
```
**Giải thích:** Đổi chữ hiển thị của `lblEndReplay`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2113
```csharp
                btnPlayAgain_Click(sender, e);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2116
```csharp
            lblEndExit.Click += (sender, e) =>
```
**Giải thích:** Tạo lambda callback ngắn. Code này thường chạy khi Timer tick, thread chạy, hoặc event xảy ra. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2118
```csharp
                btnExitGame_Click(sender, e);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2121
```csharp
            panelEndOverlay.Controls.Add(lblEndResult);
```
**Giải thích:** Thêm control/dữ liệu vào collection. Với giao diện, control sẽ xuất hiện trên panel/form. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2122
```csharp
            panelEndOverlay.Controls.Add(lblEndReplay);
```
**Giải thích:** Thêm control/dữ liệu vào collection. Với giao diện, control sẽ xuất hiện trên panel/form. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2123
```csharp
            panelEndOverlay.Controls.Add(lblEndExit);
```
**Giải thích:** Thêm control/dữ liệu vào collection. Với giao diện, control sẽ xuất hiện trên panel/form. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2124
```csharp
            this.Controls.Add(panelEndOverlay);
```
**Giải thích:** Thêm control/dữ liệu vào collection. Với giao diện, control sẽ xuất hiện trên panel/form. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2125
```csharp
            panelEndOverlay.BringToFront();
```
**Giải thích:** Đưa control lên trên cùng để không bị panel/ảnh khác che. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..

#### Dòng 2127
```csharp
            AnimateEndResultThenOptions();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ShowEndGameOverlay`, tức phần tạo overlay thắng/thua..


---

### Nhóm hàm `CreateEndActionLabel`

**Vai trò:** tạo chữ Chơi lại/Thoát.

#### Dòng 2130
```csharp
        private Label CreateEndActionLabel(string text)
```
**Giải thích:** Khai báo hàm `CreateEndActionLabel(string text)`. Công dụng chính: tạo chữ Chơi lại/Thoát.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 2132
```csharp
            Label label = new Label();
```
**Giải thích:** Tạo Label mới. Trong game, Label được dùng làm ô bàn cờ hoặc chữ hiển thị trạng thái. Ngữ cảnh: dòng này nằm trong `CreateEndActionLabel`, tức phần tạo chữ Chơi lại/Thoát..

#### Dòng 2133
```csharp
            label.Text = text;
```
**Giải thích:** Đổi chữ hiển thị của `label`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `CreateEndActionLabel`, tức phần tạo chữ Chơi lại/Thoát..

#### Dòng 2134
```csharp
            label.ForeColor = Color.White;
```
**Giải thích:** Đổi màu chữ của `label` để dễ nhìn trên nền hiện tại. Ngữ cảnh: dòng này nằm trong `CreateEndActionLabel`, tức phần tạo chữ Chơi lại/Thoát..

#### Dòng 2135
```csharp
            label.BackColor = Color.Transparent;
```
**Giải thích:** Đổi màu nền của `label`. Game dùng màu để thể hiện hiệu ứng, trạng thái hit/miss, skill hoặc thắng/thua. Ngữ cảnh: dòng này nằm trong `CreateEndActionLabel`, tức phần tạo chữ Chơi lại/Thoát..

#### Dòng 2136
```csharp
            label.BorderStyle = BorderStyle.FixedSingle;
```
**Giải thích:** Gán `BorderStyle.FixedSingle` cho `label.BorderStyle`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateEndActionLabel`, tức phần tạo chữ Chơi lại/Thoát..

#### Dòng 2137
```csharp
            label.Cursor = Cursors.Hand;
```
**Giải thích:** Đổi hình con trỏ chuột, ví dụ Hand khi có thể dùng skill, No khi bị khóa. Ngữ cảnh: dòng này nằm trong `CreateEndActionLabel`, tức phần tạo chữ Chơi lại/Thoát..

#### Dòng 2138
```csharp
            label.Font = new Font("Segoe UI", 18, FontStyle.Bold);
```
**Giải thích:** Tạo Font để chỉnh kiểu chữ, kích thước và độ đậm cho control. Ngữ cảnh: dòng này nằm trong `CreateEndActionLabel`, tức phần tạo chữ Chơi lại/Thoát..

#### Dòng 2139
```csharp
            label.TextAlign = ContentAlignment.MiddleCenter;
```
**Giải thích:** Gán `ContentAlignment.MiddleCenter` cho `label.TextAlign`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `CreateEndActionLabel`, tức phần tạo chữ Chơi lại/Thoát..

#### Dòng 2140
```csharp
            label.Width = 190;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `CreateEndActionLabel`, tức phần tạo chữ Chơi lại/Thoát..

#### Dòng 2141
```csharp
            label.Height = 56;
```
**Giải thích:** Cập nhật kích thước control, thường dùng khi vẽ hoặc căn giữa. Ngữ cảnh: dòng này nằm trong `CreateEndActionLabel`, tức phần tạo chữ Chơi lại/Thoát..

#### Dòng 2142
```csharp
            return label;
```
**Giải thích:** Trả kết quả về nơi gọi hàm. Với hàm bool, dòng này quyết định đúng/sai cho điều kiện kiểm tra. Ngữ cảnh: dòng này nằm trong `CreateEndActionLabel`, tức phần tạo chữ Chơi lại/Thoát..


---

### Nhóm hàm `AnimateEndResultThenOptions`

**Vai trò:** animation chữ thắng/thua.

#### Dòng 2145
```csharp
        private void AnimateEndResultThenOptions()
```
**Giải thích:** Khai báo hàm `AnimateEndResultThenOptions()`. Công dụng chính: animation chữ thắng/thua.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 2147
```csharp
            int step = 0;
```
**Giải thích:** Gán `0` cho `int step`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2148
```csharp
            int steps = 18;
```
**Giải thích:** Gán `18` cho `int steps`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2149
```csharp
            int centerTop = (this.ClientSize.Height - lblEndResult.Height) / 2 - 90;
```
**Giải thích:** Gán `(this.ClientSize.Height - lblEndResult.Height) / 2 - 90` cho `int centerTop`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2151
```csharp
            System.Windows.Forms.Timer timer = new System.Windows.Forms.Timer();
```
**Giải thích:** Tạo Timer WinForms để chạy hiệu ứng theo thời gian, ví dụ nhấp nháy, fade, chuyển lượt. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2152
```csharp
            timer.Interval = 24;
```
**Giải thích:** Gán `24` cho `timer.Interval`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2153
```csharp
            timer.Tick += (sender, e) =>
```
**Giải thích:** Tạo lambda callback ngắn. Code này thường chạy khi Timer tick, thread chạy, hoặc event xảy ra. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2155
```csharp
                if (lblEndResult == null || lblEndResult.IsDisposed)
```
**Giải thích:** Kiểm tra điều kiện `(lblEndResult == null || lblEndResult.IsDisposed`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2157
```csharp
                    timer.Stop();
```
**Giải thích:** Dừng timer hoặc âm thanh đang chạy. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2158
```csharp
                    timer.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2159
```csharp
                    return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2162
```csharp
                step++;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2163
```csharp
                double t = step / (double)steps;
```
**Giải thích:** Gán `step / (double)steps` cho `double t`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2165
```csharp
                if (t > 1.0)
```
**Giải thích:** Kiểm tra điều kiện `(t > 1.0`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2167
```csharp
                    t = 1.0;
```
**Giải thích:** Gán `1.0` cho `t`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2170
```csharp
                float fontSize = 14f + (float)(42f * t);
```
**Giải thích:** Gán `14f + (float)(42f * t)` cho `float fontSize`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2171
```csharp
                Font oldFont = lblEndResult.Font;
```
**Giải thích:** Gán `lblEndResult.Font` cho `Font oldFont`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2172
```csharp
                lblEndResult.Font = new Font("Segoe UI Black", fontSize, FontStyle.Bold);
```
**Giải thích:** Tạo Font để chỉnh kiểu chữ, kích thước và độ đậm cho control. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2173
```csharp
                oldFont.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2174
```csharp
                lblEndResult.Top = centerTop - (int)(40 * (1.0 - t));
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2176
```csharp
                if (step >= steps)
```
**Giải thích:** Kiểm tra điều kiện `(step >= steps`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2178
```csharp
                    timer.Stop();
```
**Giải thích:** Dừng timer hoặc âm thanh đang chạy. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2179
```csharp
                    timer.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2180
```csharp
                    AnimateEndActionTexts();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..

#### Dòng 2183
```csharp
            timer.Start();
```
**Giải thích:** Bắt đầu chạy thread, timer hoặc listener đã được tạo trước đó. Ngữ cảnh: dòng này nằm trong `AnimateEndResultThenOptions`, tức phần animation chữ thắng/thua..


---

### Nhóm hàm `AnimateEndActionTexts`

**Vai trò:** animation chữ Chơi lại/Thoát.

#### Dòng 2186
```csharp
        private void AnimateEndActionTexts()
```
**Giải thích:** Khai báo hàm `AnimateEndActionTexts()`. Công dụng chính: animation chữ Chơi lại/Thoát.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 2188
```csharp
            lblEndReplay.Visible = true;
```
**Giải thích:** Ẩn/hiện `lblEndReplay`. Ví dụ nút Chơi lại chỉ hiện khi trận kết thúc. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2189
```csharp
            lblEndExit.Visible = true;
```
**Giải thích:** Ẩn/hiện `lblEndExit`. Ví dụ nút Chơi lại chỉ hiện khi trận kết thúc. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2191
```csharp
            int step = 0;
```
**Giải thích:** Gán `0` cho `int step`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2192
```csharp
            int steps = 18;
```
**Giải thích:** Gán `18` cho `int steps`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2193
```csharp
            int startTop = this.ClientSize.Height + 60;
```
**Giải thích:** Gán `this.ClientSize.Height + 60` cho `int startTop`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2194
```csharp
            int finalTop = (this.ClientSize.Height / 2) + 80;
```
**Giải thích:** Gán `(this.ClientSize.Height / 2) + 80` cho `int finalTop`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2196
```csharp
            System.Windows.Forms.Timer timer = new System.Windows.Forms.Timer();
```
**Giải thích:** Tạo Timer WinForms để chạy hiệu ứng theo thời gian, ví dụ nhấp nháy, fade, chuyển lượt. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2197
```csharp
            timer.Interval = 22;
```
**Giải thích:** Gán `22` cho `timer.Interval`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2198
```csharp
            timer.Tick += (sender, e) =>
```
**Giải thích:** Tạo lambda callback ngắn. Code này thường chạy khi Timer tick, thread chạy, hoặc event xảy ra. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2200
```csharp
                if (lblEndReplay == null || lblEndReplay.IsDisposed || lblEndExit == null || lblEndExit.IsDisposed)
```
**Giải thích:** Kiểm tra điều kiện `(lblEndReplay == null || lblEndReplay.IsDisposed || lblEndExit == null || lblEndExit.IsDisposed`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2202
```csharp
                    timer.Stop();
```
**Giải thích:** Dừng timer hoặc âm thanh đang chạy. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2203
```csharp
                    timer.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2204
```csharp
                    return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2207
```csharp
                step++;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2208
```csharp
                double t = step / (double)steps;
```
**Giải thích:** Gán `step / (double)steps` cho `double t`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2209
```csharp
                if (t > 1.0)
```
**Giải thích:** Kiểm tra điều kiện `(t > 1.0`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2211
```csharp
                    t = 1.0;
```
**Giải thích:** Gán `1.0` cho `t`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2214
```csharp
                double eased = 1.0 - Math.Pow(1.0 - t, 3.0);
```
**Giải thích:** Gán `1.0 - Math.Pow(1.0 - t, 3.0)` cho `double eased`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2215
```csharp
                int top = startTop + (int)((finalTop - startTop) * eased);
```
**Giải thích:** Gán `startTop + (int)((finalTop - startTop) * eased)` cho `int top`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2217
```csharp
                lblEndReplay.Top = top;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2218
```csharp
                lblEndExit.Top = top;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2220
```csharp
                if (step >= steps)
```
**Giải thích:** Kiểm tra điều kiện `(step >= steps`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2222
```csharp
                    lblEndReplay.Top = finalTop;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2223
```csharp
                    lblEndExit.Top = finalTop;
```
**Giải thích:** Cập nhật tọa độ control trong animation hoặc layout. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2224
```csharp
                    timer.Stop();
```
**Giải thích:** Dừng timer hoặc âm thanh đang chạy. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2225
```csharp
                    timer.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..

#### Dòng 2228
```csharp
            timer.Start();
```
**Giải thích:** Bắt đầu chạy thread, timer hoặc listener đã được tạo trước đó. Ngữ cảnh: dòng này nằm trong `AnimateEndActionTexts`, tức phần animation chữ Chơi lại/Thoát..


---

### Nhóm hàm `RemoveEndGameOverlay`

**Vai trò:** xóa overlay kết thúc.

#### Dòng 2231
```csharp
        private void RemoveEndGameOverlay()
```
**Giải thích:** Khai báo hàm `RemoveEndGameOverlay()`. Công dụng chính: xóa overlay kết thúc.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 2233
```csharp
            if (panelEndOverlay != null)
```
**Giải thích:** Kiểm tra điều kiện `(panelEndOverlay != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `RemoveEndGameOverlay`, tức phần xóa overlay kết thúc..

#### Dòng 2235
```csharp
                this.Controls.Remove(panelEndOverlay);
```
**Giải thích:** Xóa control/dữ liệu khỏi collection. Ví dụ xóa ảnh hit/miss hoặc overlay. Ngữ cảnh: dòng này nằm trong `RemoveEndGameOverlay`, tức phần xóa overlay kết thúc..

#### Dòng 2236
```csharp
                panelEndOverlay.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `RemoveEndGameOverlay`, tức phần xóa overlay kết thúc..

#### Dòng 2237
```csharp
                panelEndOverlay = null;
```
**Giải thích:** Gán giá trị mới cho `panelEndOverlay`. panel phủ toàn màn hình khi thắng/thua. Giá trị `null` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `RemoveEndGameOverlay`, tức phần xóa overlay kết thúc..

#### Dòng 2238
```csharp
                lblEndResult = null;
```
**Giải thích:** Gán giá trị mới cho `lblEndResult`. label chữ thắng/thua lớn. Giá trị `null` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `RemoveEndGameOverlay`, tức phần xóa overlay kết thúc..

#### Dòng 2239
```csharp
                lblEndReplay = null;
```
**Giải thích:** Gán giá trị mới cho `lblEndReplay`. label dạng nút Chơi lại trên màn hình kết thúc. Giá trị `null` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `RemoveEndGameOverlay`, tức phần xóa overlay kết thúc..

#### Dòng 2240
```csharp
                lblEndExit = null;
```
**Giải thích:** Gán giá trị mới cho `lblEndExit`. label dạng nút Thoát trên màn hình kết thúc. Giá trị `null` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `RemoveEndGameOverlay`, tức phần xóa overlay kết thúc..


---

### Nhóm hàm `RestoreNormalTheme`

**Vai trò:** khôi phục giao diện bình thường.

#### Dòng 2244
```csharp
        private void RestoreNormalTheme()
```
**Giải thích:** Khai báo hàm `RestoreNormalTheme()`. Công dụng chính: khôi phục giao diện bình thường.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 2246
```csharp
            this.BackColor = Color.FromArgb(205, 225, 240);
```
**Giải thích:** Đổi màu nền của `this`. Game dùng màu để thể hiện hiệu ứng, trạng thái hit/miss, skill hoặc thắng/thua. Ngữ cảnh: dòng này nằm trong `RestoreNormalTheme`, tức phần khôi phục giao diện bình thường..

#### Dòng 2247
```csharp
            ApplySeaBackground();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `RestoreNormalTheme`, tức phần khôi phục giao diện bình thường..

#### Dòng 2249
```csharp
            if (panelOwnBoard != null)
```
**Giải thích:** Kiểm tra điều kiện `(panelOwnBoard != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `RestoreNormalTheme`, tức phần khôi phục giao diện bình thường..

#### Dòng 2251
```csharp
                panelOwnBoard.BackColor = Color.FromArgb(255, 224, 192);
```
**Giải thích:** Đổi màu nền của `panelOwnBoard`. Game dùng màu để thể hiện hiệu ứng, trạng thái hit/miss, skill hoặc thắng/thua. Ngữ cảnh: dòng này nằm trong `RestoreNormalTheme`, tức phần khôi phục giao diện bình thường..

#### Dòng 2254
```csharp
            if (panelEnemyBoard != null)
```
**Giải thích:** Kiểm tra điều kiện `(panelEnemyBoard != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `RestoreNormalTheme`, tức phần khôi phục giao diện bình thường..

#### Dòng 2256
```csharp
                panelEnemyBoard.BackColor = Color.FromArgb(255, 224, 192);
```
**Giải thích:** Đổi màu nền của `panelEnemyBoard`. Game dùng màu để thể hiện hiệu ứng, trạng thái hit/miss, skill hoặc thắng/thua. Ngữ cảnh: dòng này nằm trong `RestoreNormalTheme`, tức phần khôi phục giao diện bình thường..


---

### Nhóm hàm `DisableAllBoardCells`

**Vai trò:** khóa bàn cờ.

#### Dòng 2260
```csharp
        private void DisableAllBoardCells()
```
**Giải thích:** Khai báo hàm `DisableAllBoardCells()`. Công dụng chính: khóa bàn cờ.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 2262
```csharp
            for (int row = 0; row < BOARD_SIZE; row++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `DisableAllBoardCells`, tức phần khóa bàn cờ..

#### Dòng 2264
```csharp
                for (int col = 0; col < BOARD_SIZE; col++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `DisableAllBoardCells`, tức phần khóa bàn cờ..

#### Dòng 2266
```csharp
                    ownCells[row, col].Enabled = false;
```
**Giải thích:** Cập nhật phần tử mảng `ownCells[row, col].Enabled`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `DisableAllBoardCells`, tức phần khóa bàn cờ..

#### Dòng 2267
```csharp
                    enemyCells[row, col].Enabled = false;
```
**Giải thích:** Cập nhật phần tử mảng `enemyCells[row, col].Enabled`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `DisableAllBoardCells`, tức phần khóa bàn cờ..

#### Dòng 2271
```csharp
            btnReady.Enabled = false;
```
**Giải thích:** Bật/tắt tương tác `btnReady`. Khi false người chơi không click/kéo được. Ngữ cảnh: dòng này nằm trong `DisableAllBoardCells`, tức phần khóa bàn cờ..

#### Dòng 2272
```csharp
            btnRotate.Enabled = false;
```
**Giải thích:** Bật/tắt tương tác `btnRotate`. Khi false người chơi không click/kéo được. Ngữ cảnh: dòng này nằm trong `DisableAllBoardCells`, tức phần khóa bàn cờ..

#### Dòng 2274
```csharp
            if (panelShipDock != null)
```
**Giải thích:** Kiểm tra điều kiện `(panelShipDock != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `DisableAllBoardCells`, tức phần khóa bàn cờ..

#### Dòng 2276
```csharp
                panelShipDock.Enabled = false;
```
**Giải thích:** Bật/tắt tương tác `panelShipDock`. Khi false người chơi không click/kéo được. Ngữ cảnh: dòng này nằm trong `DisableAllBoardCells`, tức phần khóa bàn cờ..

#### Dòng 2279
```csharp
            UpdateSkillMenuState();
```
**Giải thích:** Gọi hàm `UpdateSkillMenuState` để làm mới trạng thái icon skill. Ngữ cảnh: dòng này nằm trong `DisableAllBoardCells`, tức phần khóa bàn cờ..


---

### Nhóm hàm `btnPlayAgain_Click`

**Vai trò:** gửi yêu cầu chơi lại.

#### Dòng 2282
```csharp
        private void btnPlayAgain_Click(object sender, EventArgs e)
```
**Giải thích:** Khai báo hàm `btnPlayAgain_Click(object sender, EventArgs e)`. Công dụng chính: gửi yêu cầu chơi lại.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 2284
```csharp
            if (!connected)
```
**Giải thích:** Kiểm tra điều kiện `(!connected`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `btnPlayAgain_Click`, tức phần gửi yêu cầu chơi lại..

#### Dòng 2286
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `btnPlayAgain_Click`, tức phần gửi yêu cầu chơi lại..

#### Dòng 2289
```csharp
            SendLine("PLAY_AGAIN");
```
**Giải thích:** Dòng này liên quan đến giao thức `PLAY_AGAIN`: lệnh xin chơi lại. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `btnPlayAgain_Click`, tức phần gửi yêu cầu chơi lại..

#### Dòng 2291
```csharp
            btnPlayAgain.Enabled = false;
```
**Giải thích:** Bật/tắt tương tác `btnPlayAgain`. Khi false người chơi không click/kéo được. Ngữ cảnh: dòng này nằm trong `btnPlayAgain_Click`, tức phần gửi yêu cầu chơi lại..

#### Dòng 2292
```csharp
            lblStatus.Text = "TRẠNG THÁI: CHỜ CHƠI LẠI";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `btnPlayAgain_Click`, tức phần gửi yêu cầu chơi lại..


---

### Nhóm hàm `btnExitGame_Click`

**Vai trò:** thoát game.

#### Dòng 2295
```csharp
        private void btnExitGame_Click(object sender, EventArgs e)
```
**Giải thích:** Khai báo hàm `btnExitGame_Click(object sender, EventArgs e)`. Công dụng chính: thoát game.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 2297
```csharp
            this.Close();
```
**Giải thích:** Đóng kết nối hoặc tài nguyên socket/file hiện tại. Ngữ cảnh: dòng này nằm trong `btnExitGame_Click`, tức phần thoát game..


---

### Nhóm hàm `ResetForNewGame`

**Vai trò:** reset toàn bộ ván mới.

#### Dòng 2300
```csharp
        private void ResetForNewGame()
```
**Giải thích:** Khai báo hàm `ResetForNewGame()`. Công dụng chính: reset toàn bộ ván mới.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 2302
```csharp
            shipCount = 0;
```
**Giải thích:** Gán giá trị mới cho `shipCount`. số tàu cơ bản đã đặt xong. Giá trị `0` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2303
```csharp
            ready = false;
```
**Giải thích:** Gán giá trị mới cho `ready`. mảng trạng thái sẵn sàng của hai Client. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2304
```csharp
            myTurn = false;
```
**Giải thích:** Gán giá trị mới cho `myTurn`. cho biết hiện tại có phải lượt bắn của mình. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2305
```csharp
            gameEnded = false;
```
**Giải thích:** Gán giá trị mới cho `gameEnded`. cho biết trận đã kết thúc; khi true thì khóa bắn và khóa skill. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2306
```csharp
            isHorizontal = true;
```
**Giải thích:** Gán giá trị mới cho `isHorizontal`. hướng đặt tàu hiện tại. Giá trị `true` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2307
```csharp
            droneUsed = false;
```
**Giải thích:** Gán giá trị mới cho `droneUsed`. đánh dấu kỹ năng Drone đã dùng trong trận, vì mỗi trận chỉ dùng một lần. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2308
```csharp
            shipSpareUsed = false;
```
**Giải thích:** Gán giá trị mới cho `shipSpareUsed`. đánh dấu kỹ năng tàu dự phòng đã dùng trong trận. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2310
```csharp
            shipHP = new int[] { 2, 3, 4, 2 };
```
**Giải thích:** Gán giá trị mới cho `shipHP`. mảng máu còn lại của từng tàu; mỗi hit vào tàu sẽ trừ 1. Giá trị `new int[] { 2, 3, 4, 2 }` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2312
```csharp
            RemoveEndGameOverlay();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2313
```csharp
            RestoreNormalTheme();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2314
```csharp
            actionLocked = false;
```
**Giải thích:** Gán giá trị mới cho `actionLocked`. khóa thao tác trong lúc animation/delay/skill đang chạy để tránh click sai thời điểm. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2315
```csharp
            turnTransitionRunning = false;
```
**Giải thích:** Gán giá trị mới cho `turnTransitionRunning`. cho biết animation chuyển lượt đang chạy. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2317
```csharp
            ClearBoardVisuals(panelOwnBoard);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2318
```csharp
            ClearBoardVisuals(panelEnemyBoard);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2320
```csharp
            for (int i = 0; i < TOTAL_SHIP_COUNT; i++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2322
```csharp
                shipPlaced[i] = false;
```
**Giải thích:** Cập nhật phần tử mảng `shipPlaced[i]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2325
```csharp
            for (int i = 0; i < BASE_SHIP_COUNT; i++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2327
```csharp
                if (shipDockPictures[i] != null)
```
**Giải thích:** Kiểm tra điều kiện `(shipDockPictures[i] != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2329
```csharp
                    shipDockPictures[i].Visible = true;
```
**Giải thích:** Cập nhật phần tử mảng `shipDockPictures[i].Visible`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2330
```csharp
                    shipDockPictures[i].Enabled = true;
```
**Giải thích:** Cập nhật phần tử mảng `shipDockPictures[i].Enabled`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2331
```csharp
                    shipDockPictures[i].Image = PrepareShipImage(i, isHorizontal);
```
**Giải thích:** Cập nhật phần tử mảng `shipDockPictures[i].Image`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2335
```csharp
            for (int row = 0; row < BOARD_SIZE; row++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2337
```csharp
                for (int col = 0; col < BOARD_SIZE; col++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2339
```csharp
                    ownShips[row, col] = false;
```
**Giải thích:** Cập nhật phần tử mảng `ownShips[row, col]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2340
```csharp
                    ownHits[row, col] = false;
```
**Giải thích:** Cập nhật phần tử mảng `ownHits[row, col]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2341
```csharp
                    enemyShots[row, col] = false;
```
**Giải thích:** Cập nhật phần tử mảng `enemyShots[row, col]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2342
```csharp
                    ownShotsReceived[row, col] = false;
```
**Giải thích:** Cập nhật phần tử mảng `ownShotsReceived[row, col]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2343
```csharp
                    ownShipId[row, col] = -1;
```
**Giải thích:** Cập nhật phần tử mảng `ownShipId[row, col]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2345
```csharp
                    ownCells[row, col].Enabled = true;
```
**Giải thích:** Cập nhật phần tử mảng `ownCells[row, col].Enabled`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2346
```csharp
                    ownCells[row, col].Text = "";
```
**Giải thích:** Cập nhật phần tử mảng `ownCells[row, col].Text`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2347
```csharp
                    StyleBoardCell(ownCells[row, col]);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2348
```csharp
                    ownCells[row, col].Invalidate();
```
**Giải thích:** Yêu cầu control vẽ lại. Sau đó Paint/OnPaint có thể chạy để cập nhật viền hoặc giao diện. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2350
```csharp
                    enemyCells[row, col].Enabled = true;
```
**Giải thích:** Cập nhật phần tử mảng `enemyCells[row, col].Enabled`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2351
```csharp
                    enemyCells[row, col].Text = "";
```
**Giải thích:** Cập nhật phần tử mảng `enemyCells[row, col].Text`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2352
```csharp
                    StyleBoardCell(enemyCells[row, col]);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2353
```csharp
                    enemyCells[row, col].Invalidate();
```
**Giải thích:** Yêu cầu control vẽ lại. Sau đó Paint/OnPaint có thể chạy để cập nhật viền hoặc giao diện. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2357
```csharp
            lblTurn.Text = "LƯỢT: ?";
```
**Giải thích:** Đổi chữ hiển thị của `lblTurn`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2358
```csharp
            lblStatus.Text = "TRẠNG THÁI: TRẬN MỚI";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2360
```csharp
            btnReady.Enabled = true;
```
**Giải thích:** Bật/tắt tương tác `btnReady`. Khi false người chơi không click/kéo được. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2361
```csharp
            btnRotate.Enabled = true;
```
**Giải thích:** Bật/tắt tương tác `btnRotate`. Khi false người chơi không click/kéo được. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2363
```csharp
            if (panelShipDock != null)
```
**Giải thích:** Kiểm tra điều kiện `(panelShipDock != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2365
```csharp
                panelShipDock.Enabled = true;
```
**Giải thích:** Bật/tắt tương tác `panelShipDock`. Khi false người chơi không click/kéo được. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2366
```csharp
                panelShipDock.Visible = true;
```
**Giải thích:** Ẩn/hiện `panelShipDock`. Ví dụ nút Chơi lại chỉ hiện khi trận kết thúc. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2370
```csharp
            btnPlayAgain.Visible = false;
```
**Giải thích:** Ẩn/hiện `btnPlayAgain`. Ví dụ nút Chơi lại chỉ hiện khi trận kết thúc. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2371
```csharp
            btnExitGame.Visible = false;
```
**Giải thích:** Ẩn/hiện `btnExitGame`. Ví dụ nút Chơi lại chỉ hiện khi trận kết thúc. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2372
```csharp
            btnPlayAgain.Enabled = true;
```
**Giải thích:** Bật/tắt tương tác `btnPlayAgain`. Khi false người chơi không click/kéo được. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2373
```csharp
            btnExitGame.Enabled = true;
```
**Giải thích:** Bật/tắt tương tác `btnExitGame`. Khi false người chơi không click/kéo được. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..

#### Dòng 2375
```csharp
            UpdateSkillMenuState();
```
**Giải thích:** Gọi hàm `UpdateSkillMenuState` để làm mới trạng thái icon skill. Ngữ cảnh: dòng này nằm trong `ResetForNewGame`, tức phần reset toàn bộ ván mới..


---

### Nhóm hàm `ClearBoardVisuals`

**Vai trò:** xóa control hình ảnh trên bàn.

#### Dòng 2378
```csharp
        private void ClearBoardVisuals(Panel board)
```
**Giải thích:** Khai báo hàm `ClearBoardVisuals(Panel board)`. Công dụng chính: xóa control hình ảnh trên bàn.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 2380
```csharp
            for (int i = board.Controls.Count - 1; i >= 0; i--)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `ClearBoardVisuals`, tức phần xóa control hình ảnh trên bàn..

#### Dòng 2382
```csharp
                Control control = board.Controls[i];
```
**Giải thích:** Gán `board.Controls[i]` cho `Control control`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ClearBoardVisuals`, tức phần xóa control hình ảnh trên bàn..

#### Dòng 2384
```csharp
                if (control.Tag is string)
```
**Giải thích:** Kiểm tra điều kiện `(control.Tag is string`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ClearBoardVisuals`, tức phần xóa control hình ảnh trên bàn..

#### Dòng 2386
```csharp
                    string tag = control.Tag.ToString();
```
**Giải thích:** Gán `control.Tag.ToString()` cho `string tag`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ClearBoardVisuals`, tức phần xóa control hình ảnh trên bàn..

#### Dòng 2388
```csharp
                    if (tag.StartsWith("VISUAL_SHIP_") || tag == "VISUAL_PIN")
```
**Giải thích:** Kiểm tra điều kiện `(tag.StartsWith("VISUAL_SHIP_") || tag == "VISUAL_PIN"`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ClearBoardVisuals`, tức phần xóa control hình ảnh trên bàn..

#### Dòng 2390
```csharp
                        board.Controls.RemoveAt(i);
```
**Giải thích:** Xóa control/dữ liệu khỏi collection. Ví dụ xóa ảnh hit/miss hoặc overlay. Ngữ cảnh: dòng này nằm trong `ClearBoardVisuals`, tức phần xóa control hình ảnh trên bàn..

#### Dòng 2391
```csharp
                        control.Dispose();
```
**Giải thích:** Giải phóng tài nguyên như ảnh, timer, sound hoặc form để tránh rò rỉ bộ nhớ. Ngữ cảnh: dòng này nằm trong `ClearBoardVisuals`, tức phần xóa control hình ảnh trên bàn..


---

### Nhóm hàm `SendLine`

**Vai trò:** gửi lệnh TCP.

#### Dòng 2397
```csharp
        private void SendLine(string msg)
```
**Giải thích:** Khai báo hàm `SendLine(string msg)`. Công dụng chính: gửi lệnh TCP.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 2399
```csharp
            try
```
**Giải thích:** Bắt đầu khối có thể phát sinh lỗi như kết nối mạng, parse dữ liệu, đọc ảnh/âm thanh. Ngữ cảnh: dòng này nằm trong `SendLine`, tức phần gửi lệnh TCP..

#### Dòng 2401
```csharp
                if (writer != null)
```
**Giải thích:** Kiểm tra điều kiện `(writer != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `SendLine`, tức phần gửi lệnh TCP..

#### Dòng 2403
```csharp
                    writer.WriteLine(msg);
```
**Giải thích:** Gửi một dòng lệnh text qua TCP. Đây là lệnh thật đi giữa Client và Server. Ngữ cảnh: dòng này nằm trong `SendLine`, tức phần gửi lệnh TCP..

#### Dòng 2406
```csharp
            catch
```
**Giải thích:** Bắt lỗi để chương trình không bị crash; thường hiển thị thông báo hoặc bỏ qua lỗi nhỏ. Ngữ cảnh: dòng này nằm trong `SendLine`, tức phần gửi lệnh TCP..


---

### Nhóm hàm `MarkDisconnected`

**Vai trò:** xử lý mất kết nối.

#### Dòng 2411
```csharp
        private void MarkDisconnected()
```
**Giải thích:** Khai báo hàm `MarkDisconnected()`. Công dụng chính: xử lý mất kết nối.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 2413
```csharp
            if (!connected)
```
**Giải thích:** Kiểm tra điều kiện `(!connected`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `MarkDisconnected`, tức phần xử lý mất kết nối..

#### Dòng 2415
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `MarkDisconnected`, tức phần xử lý mất kết nối..

#### Dòng 2418
```csharp
            connected = false;
```
**Giải thích:** Gán giá trị mới cho `connected`. trạng thái đã kết nối Server hay chưa. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `MarkDisconnected`, tức phần xử lý mất kết nối..

#### Dòng 2419
```csharp
            myTurn = false;
```
**Giải thích:** Gán giá trị mới cho `myTurn`. cho biết hiện tại có phải lượt bắn của mình. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `MarkDisconnected`, tức phần xử lý mất kết nối..

#### Dòng 2420
```csharp
            UpdateSkillMenuState();
```
**Giải thích:** Gọi hàm `UpdateSkillMenuState` để làm mới trạng thái icon skill. Ngữ cảnh: dòng này nằm trong `MarkDisconnected`, tức phần xử lý mất kết nối..

#### Dòng 2421
```csharp
            lblStatus.Text = "TRẠNG THÁI: MẤT KẾT NỐI";
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `MarkDisconnected`, tức phần xử lý mất kết nối..


---

### Nhóm hàm `SafeInvoke`

**Vai trò:** cập nhật UI an toàn từ thread nền.

#### Dòng 2424
```csharp
        private void SafeInvoke(Action action)
```
**Giải thích:** Khai báo hàm `SafeInvoke(Action action)`. Công dụng chính: cập nhật UI an toàn từ thread nền.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 2426
```csharp
            if (this.IsDisposed)
```
**Giải thích:** Kiểm tra điều kiện `(this.IsDisposed`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `SafeInvoke`, tức phần cập nhật UI an toàn từ thread nền..

#### Dòng 2428
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `SafeInvoke`, tức phần cập nhật UI an toàn từ thread nền..

#### Dòng 2431
```csharp
            if (this.InvokeRequired)
```
**Giải thích:** Kiểm tra điều kiện `(this.InvokeRequired`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `SafeInvoke`, tức phần cập nhật UI an toàn từ thread nền..

#### Dòng 2433
```csharp
                this.Invoke(action);
```
**Giải thích:** Đưa code cập nhật UI về UI thread. Đây là bước quan trọng vì thread socket không được sửa control trực tiếp. Ngữ cảnh: dòng này nằm trong `SafeInvoke`, tức phần cập nhật UI an toàn từ thread nền..

#### Dòng 2435
```csharp
            else
```
**Giải thích:** Nhánh xử lý còn lại khi các điều kiện phía trước đều sai. Ngữ cảnh: dòng này nằm trong `SafeInvoke`, tức phần cập nhật UI an toàn từ thread nền..

#### Dòng 2437
```csharp
                action();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `SafeInvoke`, tức phần cập nhật UI an toàn từ thread nền..


---

### Nhóm hàm `Form1_FormClosing`

**Vai trò:** dọn tài nguyên server khi đóng.

#### Dòng 2441
```csharp
        private void Form1_FormClosing(object sender, FormClosingEventArgs e)
```
**Giải thích:** Khai báo hàm `Form1_FormClosing(object sender, FormClosingEventArgs e)`. Công dụng chính: dọn tài nguyên server khi đóng.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 2443
```csharp
            try
```
**Giải thích:** Bắt đầu khối có thể phát sinh lỗi như kết nối mạng, parse dữ liệu, đọc ảnh/âm thanh. Ngữ cảnh: dòng này nằm trong `Form1_FormClosing`, tức phần dọn tài nguyên server khi đóng..

#### Dòng 2445
```csharp
                if (connected)
```
**Giải thích:** Kiểm tra điều kiện `(connected`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `Form1_FormClosing`, tức phần dọn tài nguyên server khi đóng..

#### Dòng 2447
```csharp
                    SendLine("DISCONNECT");
```
**Giải thích:** Dòng này liên quan đến giao thức `DISCONNECT`: lệnh báo mất kết nối. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `Form1_FormClosing`, tức phần dọn tài nguyên server khi đóng..

#### Dòng 2450
```csharp
                connected = false;
```
**Giải thích:** Gán giá trị mới cho `connected`. trạng thái đã kết nối Server hay chưa. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `Form1_FormClosing`, tức phần dọn tài nguyên server khi đóng..

#### Dòng 2451
```csharp
                myTurn = false;
```
**Giải thích:** Gán giá trị mới cho `myTurn`. cho biết hiện tại có phải lượt bắn của mình. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `Form1_FormClosing`, tức phần dọn tài nguyên server khi đóng..

#### Dòng 2453
```csharp
                if (client != null)
```
**Giải thích:** Kiểm tra điều kiện `(client != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `Form1_FormClosing`, tức phần dọn tài nguyên server khi đóng..

#### Dòng 2455
```csharp
                    client.Close();
```
**Giải thích:** Đóng kết nối hoặc tài nguyên socket/file hiện tại. Ngữ cảnh: dòng này nằm trong `Form1_FormClosing`, tức phần dọn tài nguyên server khi đóng..

#### Dòng 2458
```csharp
            catch
```
**Giải thích:** Bắt lỗi để chương trình không bị crash; thường hiển thị thông báo hoặc bỏ qua lỗi nhỏ. Ngữ cảnh: dòng này nằm trong `Form1_FormClosing`, tức phần dọn tài nguyên server khi đóng..


---

### Nhóm hàm `Form1_Load`

**Vai trò:** sự kiện load form.

#### Dòng 2464
```csharp
        private void Form1_Load(object sender, EventArgs e)
```
**Giải thích:** Khai báo hàm `Form1_Load(object sender, EventArgs e)`. Công dụng chính: sự kiện load form.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.


---
## Các file phụ

- `AssemblyInfo.cs`: chứa thông tin metadata của project như tên assembly, version. Không ảnh hưởng trực tiếp gameplay.

- `Resources.Designer.cs`: file tự sinh để truy cập resource nếu project dùng Resources. Bản này chủ yếu dùng ảnh/âm thanh qua thư mục `Images` và `Sounds`.

- `Settings.Designer.cs`: file tự sinh cho cấu hình ứng dụng. Không phải phần logic chính của đồ án.
