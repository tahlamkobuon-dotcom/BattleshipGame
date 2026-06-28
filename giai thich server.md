# SERVER - GIẢI THÍCH CODE CHÍNH THEO TỪNG DÒNG

Nhóm: **Lâm - Minh**

Tài liệu này dùng để đưa lên GitHub làm phần giải thích code. Nội dung tập trung vào **code chính** của đồ án Battleship: giao diện tự viết trong runtime, logic đặt tàu/bắn tàu, hiệu ứng, âm thanh, kỹ năng Drone/Tàu dự phòng và giao tiếp Client-Server.

Các phần phụ như `using`, comment, dấu ngoặc nhọn, `AssemblyInfo`, `Resources.Designer`, `Settings.Designer` chỉ được giải thích sơ vì đa số là code tự sinh hoặc metadata, không phải logic đồ án.


## File `Program.cs` — điểm khởi động Server

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
| 33 | `lblPortCaption` | `Label` | Control giao diện hỗ trợ form hoạt động. |
| 34 | `txtPort` | `TextBox` | TextBox nhập cổng TCP. |
| 35 | `btnStart` | `Button` | nút chạy máy chủ ở Server. |
| 36 | `lblStatus` | `Label` | Label hiển thị trạng thái game. |
| 37 | `listBoxLog` | `ListBox` | ListBox log phía Server. |

Các dòng event quan trọng trong Designer:

| Dòng | Code | Ý nghĩa |
|---:|---|---|
| 82 | `this.btnStart.Click += new System.EventHandler(this.btnStart_Click);` | Gắn control với hàm xử lý trong `Form1.cs`; khi người dùng thao tác thì logic chính được gọi. |
| 118 | `this.FormClosing += new System.Windows.Forms.FormClosingEventHandler(this.Form1_FormClosing);` | Gắn control với hàm xử lý trong `Form1.cs`; khi người dùng thao tác thì logic chính được gọi. |

> Tóm lại: Designer giữ bố cục kéo thả; logic thật nằm trong `Form1.cs`.


---

## File `Form1.cs` — giải thích từng dòng code chính

Mình bỏ qua dòng trắng, `using`, `namespace`, comment và dấu ngoặc nhọn đơn lẻ. Các dòng còn lại là phần chính: biến trạng thái, GUI runtime, socket, game logic, hiệu ứng, âm thanh và skill.

#### Dòng 12
```csharp
    public partial class Form1 : Form
```
**Giải thích:** Khai báo cửa sổ chính `Form1`, kế thừa `Form` của WinForms. Toàn bộ giao diện và logic chính của chương trình nằm trong class này.

#### Dòng 14
```csharp
        const int DEFAULT_SHIP_CELLS = 9;
```
**Giải thích:** Khai báo hằng số `DEFAULT_SHIP_CELLS`. tổng số ô tàu mặc định mỗi người có trước khi dùng tàu dự phòng; Server dùng số này để tính thắng thua. Vì là `const`, giá trị không đổi khi chương trình chạy.

#### Dòng 18
```csharp
        TcpListener listener;
```
**Giải thích:** Khai báo biến `listener`. TcpListener của Server, dùng để mở cổng và lắng nghe Client.

#### Dòng 19
```csharp
        Thread listenThread;
```
**Giải thích:** Khai báo biến `listenThread`. thread nền của Server để chờ Client kết nối.

#### Dòng 20
```csharp
        bool isRunning = false;
```
**Giải thích:** Khai báo và/hoặc khởi tạo `isRunning`. trạng thái Server đang chạy. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 22
```csharp
        TcpClient[] players = new TcpClient[3];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `players`. mảng lưu 2 TcpClient, dùng chỉ số 1 và 2 cho đúng số người chơi. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 23
```csharp
        StreamReader[] readers = new StreamReader[3];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `readers`. mảng luồng đọc dữ liệu từ từng Client. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 24
```csharp
        StreamWriter[] writers = new StreamWriter[3];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `writers`. mảng luồng gửi dữ liệu đến từng Client. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 26
```csharp
        bool[] ready = new bool[3];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `ready`. mảng trạng thái sẵn sàng của hai Client. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 27
```csharp
        bool[] playAgain = new bool[3];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `playAgain`. mảng trạng thái muốn chơi lại của hai người. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 28
```csharp
        int[] hitsOnPlayer = new int[3];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `hitsOnPlayer`. đếm số ô tàu mỗi người đã bị bắn trúng. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 29
```csharp
        int[] totalShipCellsOnPlayer = new int[3];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `totalShipCellsOnPlayer`. tổng số ô tàu của từng người; tăng khi người đó đặt tàu dự phòng. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 30
```csharp
        bool[] sparePlaced = new bool[3];
```
**Giải thích:** Khai báo và/hoặc khởi tạo `sparePlaced`. đánh dấu người chơi nào đã dùng tàu dự phòng để Server không cộng máu nhiều lần. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 32
```csharp
        int playerCount = 0;
```
**Giải thích:** Khai báo và/hoặc khởi tạo `playerCount`. số Client đang kết nối. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 33
```csharp
        int currentTurn = 0;
```
**Giải thích:** Khai báo và/hoặc khởi tạo `currentTurn`. người chơi đang được quyền bắn. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 34
```csharp
        bool gameRunning = false;
```
**Giải thích:** Khai báo và/hoặc khởi tạo `gameRunning`. trạng thái ván đấu đang diễn ra. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 36
```csharp
        object lockObj = new object();
```
**Giải thích:** Khai báo và/hoặc khởi tạo `lockObj`. object dùng lock để chống nhiều thread sửa dữ liệu Server cùng lúc. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.

#### Dòng 37
```csharp
        Encoding encoding = new UTF8Encoding(false);
```
**Giải thích:** Khai báo và/hoặc khởi tạo `encoding`. mã hóa UTF-8 không BOM, giúp gửi tiếng Việt qua socket ổn hơn. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý.


---

### Nhóm hàm `Form1`

**Vai trò:** khởi tạo form và gọi các hàm chuẩn bị game.

#### Dòng 39
```csharp
        public Form1()
```
**Giải thích:** Hàm khởi tạo `Form1`. Khi cửa sổ mở, hàm này chạy đầu tiên để tạo control, khởi tạo dữ liệu, tạo bàn cờ, dock tàu, menu skill và âm thanh click.

#### Dòng 41
```csharp
            InitializeComponent();
```
**Giải thích:** Gọi hàm `InitializeComponent` để tạo các control từ Designer trước khi code truy cập chúng. Ngữ cảnh: dòng này nằm trong `Form1`, tức phần khởi tạo form và gọi các hàm chuẩn bị game..


---

### Nhóm hàm `btnStart_Click`

**Vai trò:** chạy Server.

#### Dòng 45
```csharp
        private void btnStart_Click(object sender, EventArgs e)
```
**Giải thích:** Khai báo hàm `btnStart_Click(object sender, EventArgs e)`. Công dụng chính: chạy Server.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 47
```csharp
            if (isRunning)
```
**Giải thích:** Kiểm tra điều kiện `(isRunning`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `btnStart_Click`, tức phần chạy Server..

#### Dòng 49
```csharp
                AddLog("Máy chủ đang chạy rồi.");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `btnStart_Click`, tức phần chạy Server..

#### Dòng 50
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `btnStart_Click`, tức phần chạy Server..

#### Dòng 53
```csharp
            try
```
**Giải thích:** Bắt đầu khối có thể phát sinh lỗi như kết nối mạng, parse dữ liệu, đọc ảnh/âm thanh. Ngữ cảnh: dòng này nằm trong `btnStart_Click`, tức phần chạy Server..

#### Dòng 55
```csharp
                string ipText = txtIP.Text.Trim();
```
**Giải thích:** Gán `txtIP.Text.Trim()` cho `string ipText`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `btnStart_Click`, tức phần chạy Server..

#### Dòng 56
```csharp
                int port = int.Parse(txtPort.Text.Trim());
```
**Giải thích:** Gán `int.Parse(txtPort.Text.Trim())` cho `int port`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `btnStart_Click`, tức phần chạy Server..

#### Dòng 58
```csharp
                IPAddress ip = IPAddress.Parse(ipText);
```
**Giải thích:** Gán `IPAddress.Parse(ipText)` cho `IPAddress ip`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `btnStart_Click`, tức phần chạy Server..

#### Dòng 60
```csharp
                listener = new TcpListener(ip, port);
```
**Giải thích:** Tạo TCP Listener cho Server, mở cổng để chờ Client kết nối. Ngữ cảnh: dòng này nằm trong `btnStart_Click`, tức phần chạy Server..

#### Dòng 61
```csharp
                listener.Start();
```
**Giải thích:** Bắt đầu chạy thread, timer hoặc listener đã được tạo trước đó. Ngữ cảnh: dòng này nằm trong `btnStart_Click`, tức phần chạy Server..

#### Dòng 63
```csharp
                isRunning = true;
```
**Giải thích:** Gán giá trị mới cho `isRunning`. trạng thái Server đang chạy. Giá trị `true` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `btnStart_Click`, tức phần chạy Server..

#### Dòng 65
```csharp
                listenThread = new Thread(ListenClients);
```
**Giải thích:** Tạo thread nền. Thread giúp nhận/gửi socket hoặc chờ client mà không làm treo giao diện WinForms. Ngữ cảnh: dòng này nằm trong `btnStart_Click`, tức phần chạy Server..

#### Dòng 66
```csharp
                listenThread.IsBackground = true;
```
**Giải thích:** Đặt thread là luồng nền để khi đóng form chương trình không bị kẹt vì thread phụ còn chạy. Ngữ cảnh: dòng này nằm trong `btnStart_Click`, tức phần chạy Server..

#### Dòng 67
```csharp
                listenThread.Start();
```
**Giải thích:** Bắt đầu chạy thread, timer hoặc listener đã được tạo trước đó. Ngữ cảnh: dòng này nằm trong `btnStart_Click`, tức phần chạy Server..

#### Dòng 69
```csharp
                UpdateStatus("Trạng thái: Máy chủ đang chạy...");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `btnStart_Click`, tức phần chạy Server..

#### Dòng 70
```csharp
                AddLog("Máy chủ đã chạy tại " + ipText + ":" + port);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `btnStart_Click`, tức phần chạy Server..

#### Dòng 71
```csharp
                AddLog("Đang chờ tối đa 2 máy khách kết nối...");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `btnStart_Click`, tức phần chạy Server..

#### Dòng 73
```csharp
            catch (Exception ex)
```
**Giải thích:** Bắt lỗi để chương trình không bị crash; thường hiển thị thông báo hoặc bỏ qua lỗi nhỏ. Ngữ cảnh: dòng này nằm trong `btnStart_Click`, tức phần chạy Server..

#### Dòng 75
```csharp
                MessageBox.Show("Lỗi khi chạy máy chủ: " + ex.Message);
```
**Giải thích:** Hiện hộp thoại thông báo lỗi/cảnh báo cho người dùng. Ngữ cảnh: dòng này nằm trong `btnStart_Click`, tức phần chạy Server..


---

### Nhóm hàm `ListenClients`

**Vai trò:** nhận tối đa 2 Client.

#### Dòng 79
```csharp
        private void ListenClients()
```
**Giải thích:** Khai báo hàm `ListenClients()`. Công dụng chính: nhận tối đa 2 Client.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 81
```csharp
            while (isRunning)
```
**Giải thích:** Vòng lặp `while` chạy lặp lại khi điều kiện còn đúng; trong socket dùng để nghe dữ liệu liên tục. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 83
```csharp
                try
```
**Giải thích:** Bắt đầu khối có thể phát sinh lỗi như kết nối mạng, parse dữ liệu, đọc ảnh/âm thanh. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 85
```csharp
                    TcpClient tcpClient = listener.AcceptTcpClient();
```
**Giải thích:** Gán `listener.AcceptTcpClient()` cho `TcpClient tcpClient`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 87
```csharp
                    lock (lockObj)
```
**Giải thích:** Khóa dữ liệu dùng chung để tránh nhiều thread Server sửa cùng lúc, đặc biệt khi nhiều Client gửi lệnh. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 89
```csharp
                        if (playerCount >= 2)
```
**Giải thích:** Kiểm tra điều kiện `(playerCount >= 2`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 91
```csharp
                            StreamWriter tempWriter = new StreamWriter(tcpClient.GetStream(), encoding);
```
**Giải thích:** Tạo luồng ghi dữ liệu dạng text vào socket. AutoFlush sẽ giúp gửi ngay sau WriteLine. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 92
```csharp
                            tempWriter.AutoFlush = true;
```
**Giải thích:** Bật tự động gửi dữ liệu ngay sau khi WriteLine, tránh lệnh bị giữ trong bộ đệm. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 93
```csharp
                            tempWriter.WriteLine("MESSAGE|Máy chủ đã đủ 2 người chơi.");
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 94
```csharp
                            tcpClient.Close();
```
**Giải thích:** Đóng kết nối hoặc tài nguyên socket/file hiện tại. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 95
```csharp
                            AddLog("Từ chối máy khách thứ 3.");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 96
```csharp
                            continue;
```
**Giải thích:** Bỏ qua phần còn lại của vòng lặp hiện tại và chuyển sang lần lặp tiếp theo. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 99
```csharp
                        int playerNumber = players[1] == null ? 1 : 2;
```
**Giải thích:** Khai báo và/hoặc khởi tạo `playerNumber`. số người chơi do Server cấp: 1 hoặc 2. Dữ liệu này là trạng thái nền để các hàm gameplay xử lý. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 101
```csharp
                        players[playerNumber] = tcpClient;
```
**Giải thích:** Cập nhật phần tử mảng `players[playerNumber]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 102
```csharp
                        readers[playerNumber] = new StreamReader(tcpClient.GetStream(), encoding);
```
**Giải thích:** Tạo luồng đọc dữ liệu dạng text từ socket. Mỗi lệnh mạng được đọc bằng ReadLine. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 103
```csharp
                        writers[playerNumber] = new StreamWriter(tcpClient.GetStream(), encoding);
```
**Giải thích:** Tạo luồng ghi dữ liệu dạng text vào socket. AutoFlush sẽ giúp gửi ngay sau WriteLine. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 104
```csharp
                        writers[playerNumber].AutoFlush = true;
```
**Giải thích:** Cập nhật phần tử mảng `writers[playerNumber].AutoFlush`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 106
```csharp
                        playerCount++;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 108
```csharp
                        ready[playerNumber] = false;
```
**Giải thích:** Cập nhật phần tử mảng `ready[playerNumber]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 109
```csharp
                        playAgain[playerNumber] = false;
```
**Giải thích:** Cập nhật phần tử mảng `playAgain[playerNumber]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 110
```csharp
                        hitsOnPlayer[playerNumber] = 0;
```
**Giải thích:** Cập nhật phần tử mảng `hitsOnPlayer[playerNumber]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 111
```csharp
                        totalShipCellsOnPlayer[playerNumber] = DEFAULT_SHIP_CELLS;
```
**Giải thích:** Cập nhật phần tử mảng `totalShipCellsOnPlayer[playerNumber]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 112
```csharp
                        sparePlaced[playerNumber] = false;
```
**Giải thích:** Cập nhật phần tử mảng `sparePlaced[playerNumber]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 114
```csharp
                        Thread clientThread = new Thread(() => HandleClient(playerNumber));
```
**Giải thích:** Tạo thread nền. Thread giúp nhận/gửi socket hoặc chờ client mà không làm treo giao diện WinForms. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 115
```csharp
                        clientThread.IsBackground = true;
```
**Giải thích:** Đặt thread là luồng nền để khi đóng form chương trình không bị kẹt vì thread phụ còn chạy. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 116
```csharp
                        clientThread.Start();
```
**Giải thích:** Bắt đầu chạy thread, timer hoặc listener đã được tạo trước đó. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 118
```csharp
                        SendTo(playerNumber, "START|" + playerNumber);
```
**Giải thích:** Dòng này liên quan đến giao thức `START|`: lệnh Server cấp số người chơi cho Client. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 119
```csharp
                        SendTo(playerNumber, "MESSAGE|Bạn là người chơi " + playerNumber);
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 121
```csharp
                        AddLog("Người chơi " + playerNumber + " đã kết nối.");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 123
```csharp
                        if (playerCount == 2)
```
**Giải thích:** Kiểm tra điều kiện `(playerCount == 2`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 125
```csharp
                            Broadcast("MESSAGE|Đã đủ 2 người chơi. Hai máy khách hãy đặt tàu và nhấn sẵn sàng.");
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 126
```csharp
                            AddLog("Đã đủ 2 máy khách.");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 128
```csharp
                        else
```
**Giải thích:** Nhánh xử lý còn lại khi các điều kiện phía trước đều sai. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 130
```csharp
                            SendTo(playerNumber, "MESSAGE|Đang chờ người chơi thứ 2...");
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 134
```csharp
                catch
```
**Giải thích:** Bắt lỗi để chương trình không bị crash; thường hiển thị thông báo hoặc bỏ qua lỗi nhỏ. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..

#### Dòng 136
```csharp
                    break;
```
**Giải thích:** Kết thúc case hoặc vòng lặp hiện tại. Ngữ cảnh: dòng này nằm trong `ListenClients`, tức phần nhận tối đa 2 Client..


---

### Nhóm hàm `HandleClient`

**Vai trò:** đọc lệnh từ một Client.

#### Dòng 141
```csharp
        private void HandleClient(int playerNumber)
```
**Giải thích:** Khai báo hàm `HandleClient(int playerNumber)`. Công dụng chính: đọc lệnh từ một Client.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 143
```csharp
            try
```
**Giải thích:** Bắt đầu khối có thể phát sinh lỗi như kết nối mạng, parse dữ liệu, đọc ảnh/âm thanh. Ngữ cảnh: dòng này nằm trong `HandleClient`, tức phần đọc lệnh từ một Client..

#### Dòng 145
```csharp
                while (isRunning && players[playerNumber] != null)
```
**Giải thích:** Vòng lặp `while` chạy lặp lại khi điều kiện còn đúng; trong socket dùng để nghe dữ liệu liên tục. Ngữ cảnh: dòng này nằm trong `HandleClient`, tức phần đọc lệnh từ một Client..

#### Dòng 147
```csharp
                    string msg = readers[playerNumber].ReadLine();
```
**Giải thích:** Gán `readers[playerNumber].ReadLine()` cho `string msg`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `HandleClient`, tức phần đọc lệnh từ một Client..

#### Dòng 149
```csharp
                    if (msg == null)
```
**Giải thích:** Kiểm tra điều kiện `(msg == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `HandleClient`, tức phần đọc lệnh từ một Client..

#### Dòng 151
```csharp
                        break;
```
**Giải thích:** Kết thúc case hoặc vòng lặp hiện tại. Ngữ cảnh: dòng này nằm trong `HandleClient`, tức phần đọc lệnh từ một Client..

#### Dòng 154
```csharp
                    AddLog("Người chơi " + playerNumber + " gửi: " + msg);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `HandleClient`, tức phần đọc lệnh từ một Client..

#### Dòng 155
```csharp
                    ProcessMessage(playerNumber, msg);
```
**Giải thích:** Gọi hàm `ProcessMessage` để xử lý lệnh nhận được. Ngữ cảnh: dòng này nằm trong `HandleClient`, tức phần đọc lệnh từ một Client..

#### Dòng 158
```csharp
            catch
```
**Giải thích:** Bắt lỗi để chương trình không bị crash; thường hiển thị thông báo hoặc bỏ qua lỗi nhỏ. Ngữ cảnh: dòng này nằm trong `HandleClient`, tức phần đọc lệnh từ một Client..

#### Dòng 160
```csharp
                AddLog("Người chơi " + playerNumber + " mất kết nối.");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `HandleClient`, tức phần đọc lệnh từ một Client..

#### Dòng 162
```csharp
            finally
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `HandleClient`, tức phần đọc lệnh từ một Client..

#### Dòng 164
```csharp
                DisconnectPlayer(playerNumber);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `HandleClient`, tức phần đọc lệnh từ một Client..


---

### Nhóm hàm `ProcessMessage`

**Vai trò:** phân loại lệnh từ Client.

#### Dòng 168
```csharp
        private void ProcessMessage(int playerNumber, string msg)
```
**Giải thích:** Khai báo hàm `ProcessMessage(int playerNumber, string msg)`. Công dụng chính: phân loại lệnh từ Client.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 170
```csharp
            string[] parts = msg.Split('|');
```
**Giải thích:** Cập nhật phần tử mảng `string[] parts`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 172
```csharp
            if (parts.Length == 0)
```
**Giải thích:** Kiểm tra điều kiện `(parts.Length == 0`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 174
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 177
```csharp
            if (parts[0] == "READY")
```
**Giải thích:** Kiểm tra điều kiện `(parts[0] == "READY"`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 179
```csharp
                ready[playerNumber] = true;
```
**Giải thích:** Cập nhật phần tử mảng `ready[playerNumber]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 180
```csharp
                AddLog("Người chơi " + playerNumber + " đã sẵn sàng.");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 181
```csharp
                Broadcast("MESSAGE|Người chơi " + playerNumber + " đã sẵn sàng.");
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 183
```csharp
                if (players[1] != null && players[2] != null && ready[1] && ready[2] && !gameRunning)
```
**Giải thích:** Kiểm tra điều kiện `(players[1] != null && players[2] != null && ready[1] && ready[2] && !gameRunning`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 185
```csharp
                    StartGame();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 188
```csharp
            else if (parts[0] == "FIRE")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 190
```csharp
                ProcessFire(playerNumber, parts);
```
**Giải thích:** Gọi hàm `ProcessFire` để xử lý lệnh bắn. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 192
```csharp
            else if (parts[0] == "FIRE_RESULT")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 194
```csharp
                ProcessFireResult(playerNumber, parts);
```
**Giải thích:** Gọi hàm `ProcessFireResult` để xử lý kết quả bắn. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 196
```csharp
            else if (parts[0] == "SUNK")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 198
```csharp
                ProcessSunk(playerNumber, msg, parts);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 200
```csharp
            else if (parts[0] == "DRONE_SCAN")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 202
```csharp
                ProcessDroneScan(playerNumber, parts);
```
**Giải thích:** Gọi hàm `ProcessDroneScan` để xử lý lệnh Drone scan. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 204
```csharp
            else if (parts[0] == "DRONE_RESULT")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 206
```csharp
                ProcessDroneResult(playerNumber, parts);
```
**Giải thích:** Gọi hàm `ProcessDroneResult` để xử lý kết quả Drone. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 208
```csharp
            else if (parts[0] == "SPARE_PLACED")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 210
```csharp
                ProcessSparePlaced(playerNumber);
```
**Giải thích:** Gọi hàm `ProcessSparePlaced` để xử lý người chơi đặt tàu dự phòng. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 212
```csharp
            else if (parts[0] == "PLAY_AGAIN")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 214
```csharp
                ProcessPlayAgain(playerNumber);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 216
```csharp
            else if (parts[0] == "DISCONNECT" || parts[0] == "EXIT_GAME")
```
**Giải thích:** Kiểm tra điều kiện phụ khi điều kiện trước đó sai. Dùng để tách nhiều trường hợp xử lý khác nhau. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 218
```csharp
                DisconnectPlayer(playerNumber);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 220
```csharp
            else
```
**Giải thích:** Nhánh xử lý còn lại khi các điều kiện phía trước đều sai. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..

#### Dòng 222
```csharp
                SendTo(playerNumber, "MESSAGE|Máy chủ không hiểu lệnh: " + msg);
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessMessage`, tức phần phân loại lệnh từ Client..


---

### Nhóm hàm `StartGame`

**Vai trò:** bắt đầu trận.

#### Dòng 226
```csharp
        private void StartGame()
```
**Giải thích:** Khai báo hàm `StartGame()`. Công dụng chính: bắt đầu trận.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 228
```csharp
            gameRunning = true;
```
**Giải thích:** Gán giá trị mới cho `gameRunning`. trạng thái ván đấu đang diễn ra. Giá trị `true` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `StartGame`, tức phần bắt đầu trận..

#### Dòng 229
```csharp
            currentTurn = 1;
```
**Giải thích:** Gán giá trị mới cho `currentTurn`. người chơi đang được quyền bắn. Giá trị `1` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `StartGame`, tức phần bắt đầu trận..

#### Dòng 231
```csharp
            hitsOnPlayer[1] = 0;
```
**Giải thích:** Cập nhật phần tử mảng `hitsOnPlayer[1]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `StartGame`, tức phần bắt đầu trận..

#### Dòng 232
```csharp
            hitsOnPlayer[2] = 0;
```
**Giải thích:** Cập nhật phần tử mảng `hitsOnPlayer[2]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `StartGame`, tức phần bắt đầu trận..

#### Dòng 234
```csharp
            totalShipCellsOnPlayer[1] = DEFAULT_SHIP_CELLS;
```
**Giải thích:** Cập nhật phần tử mảng `totalShipCellsOnPlayer[1]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `StartGame`, tức phần bắt đầu trận..

#### Dòng 235
```csharp
            totalShipCellsOnPlayer[2] = DEFAULT_SHIP_CELLS;
```
**Giải thích:** Cập nhật phần tử mảng `totalShipCellsOnPlayer[2]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `StartGame`, tức phần bắt đầu trận..

#### Dòng 236
```csharp
            sparePlaced[1] = false;
```
**Giải thích:** Cập nhật phần tử mảng `sparePlaced[1]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `StartGame`, tức phần bắt đầu trận..

#### Dòng 237
```csharp
            sparePlaced[2] = false;
```
**Giải thích:** Cập nhật phần tử mảng `sparePlaced[2]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `StartGame`, tức phần bắt đầu trận..

#### Dòng 239
```csharp
            playAgain[1] = false;
```
**Giải thích:** Cập nhật phần tử mảng `playAgain[1]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `StartGame`, tức phần bắt đầu trận..

#### Dòng 240
```csharp
            playAgain[2] = false;
```
**Giải thích:** Cập nhật phần tử mảng `playAgain[2]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `StartGame`, tức phần bắt đầu trận..

#### Dòng 242
```csharp
            Broadcast("BOTH_READY");
```
**Giải thích:** Dòng này liên quan đến giao thức `READY`: lệnh Client báo đã sẵn sàng. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `StartGame`, tức phần bắt đầu trận..

#### Dòng 243
```csharp
            Broadcast("MESSAGE|Cả hai người chơi đã sẵn sàng. Người chơi 1 đi trước.");
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `StartGame`, tức phần bắt đầu trận..

#### Dòng 244
```csharp
            Broadcast("TURN|1");
```
**Giải thích:** Dòng này liên quan đến giao thức `TURN|`: lệnh Server báo lượt hiện tại. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `StartGame`, tức phần bắt đầu trận..

#### Dòng 246
```csharp
            AddLog("Trận đấu bắt đầu. Người chơi 1 đi trước.");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `StartGame`, tức phần bắt đầu trận..


---

### Nhóm hàm `ProcessFire`

**Vai trò:** chuyển lệnh bắn sang đối thủ.

#### Dòng 249
```csharp
        private void ProcessFire(int playerNumber, string[] parts)
```
**Giải thích:** Khai báo hàm `ProcessFire(int playerNumber, string[] parts)`. Công dụng chính: chuyển lệnh bắn sang đối thủ.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 251
```csharp
            if (parts.Length < 3)
```
**Giải thích:** Kiểm tra điều kiện `(parts.Length < 3`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessFire`, tức phần chuyển lệnh bắn sang đối thủ..

#### Dòng 253
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ProcessFire`, tức phần chuyển lệnh bắn sang đối thủ..

#### Dòng 256
```csharp
            if (!gameRunning)
```
**Giải thích:** Kiểm tra điều kiện `(!gameRunning`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessFire`, tức phần chuyển lệnh bắn sang đối thủ..

#### Dòng 258
```csharp
                SendTo(playerNumber, "MESSAGE|Trận đấu chưa bắt đầu.");
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessFire`, tức phần chuyển lệnh bắn sang đối thủ..

#### Dòng 259
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ProcessFire`, tức phần chuyển lệnh bắn sang đối thủ..

#### Dòng 262
```csharp
            if (playerNumber != currentTurn)
```
**Giải thích:** Kiểm tra điều kiện `(playerNumber != currentTurn`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessFire`, tức phần chuyển lệnh bắn sang đối thủ..

#### Dòng 264
```csharp
                SendTo(playerNumber, "MESSAGE|Chưa đến lượt bạn.");
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessFire`, tức phần chuyển lệnh bắn sang đối thủ..

#### Dòng 265
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ProcessFire`, tức phần chuyển lệnh bắn sang đối thủ..

#### Dòng 268
```csharp
            int target = playerNumber == 1 ? 2 : 1;
```
**Giải thích:** Gán `playerNumber == 1 ? 2 : 1` cho `int target`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessFire`, tức phần chuyển lệnh bắn sang đối thủ..

#### Dòng 270
```csharp
            if (players[target] == null)
```
**Giải thích:** Kiểm tra điều kiện `(players[target] == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessFire`, tức phần chuyển lệnh bắn sang đối thủ..

#### Dòng 272
```csharp
                SendTo(playerNumber, "MESSAGE|Đối thủ chưa kết nối.");
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessFire`, tức phần chuyển lệnh bắn sang đối thủ..

#### Dòng 273
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ProcessFire`, tức phần chuyển lệnh bắn sang đối thủ..

#### Dòng 276
```csharp
            string row = parts[1];
```
**Giải thích:** Gán `parts[1]` cho `string row`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessFire`, tức phần chuyển lệnh bắn sang đối thủ..

#### Dòng 277
```csharp
            string col = parts[2];
```
**Giải thích:** Gán `parts[2]` cho `string col`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessFire`, tức phần chuyển lệnh bắn sang đối thủ..

#### Dòng 279
```csharp
            SendTo(target, "ENEMY_FIRE|" + row + "|" + col);
```
**Giải thích:** Dòng này liên quan đến giao thức `FIRE|`: lệnh người chơi bắn vào tọa độ. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessFire`, tức phần chuyển lệnh bắn sang đối thủ..

#### Dòng 280
```csharp
            AddLog("Người chơi " + playerNumber + " bắn người chơi " + target + " tại ô [" + row + "," + col + "]");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessFire`, tức phần chuyển lệnh bắn sang đối thủ..


---

### Nhóm hàm `ProcessFireResult`

**Vai trò:** chuyển kết quả hit/miss về người bắn và cập nhật thắng thua.

#### Dòng 283
```csharp
        private void ProcessFireResult(int playerNumber, string[] parts)
```
**Giải thích:** Khai báo hàm `ProcessFireResult(int playerNumber, string[] parts)`. Công dụng chính: chuyển kết quả hit/miss về người bắn và cập nhật thắng thua.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 285
```csharp
            if (parts.Length < 4)
```
**Giải thích:** Kiểm tra điều kiện `(parts.Length < 4`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 287
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 290
```csharp
            if (!gameRunning)
```
**Giải thích:** Kiểm tra điều kiện `(!gameRunning`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 292
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 295
```csharp
            string result = parts[1];
```
**Giải thích:** Gán `parts[1]` cho `string result`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 296
```csharp
            string row = parts[2];
```
**Giải thích:** Gán `parts[2]` cho `string row`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 297
```csharp
            string col = parts[3];
```
**Giải thích:** Gán `parts[3]` cho `string col`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 299
```csharp
            int defender = playerNumber;
```
**Giải thích:** Gán `playerNumber` cho `int defender`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 300
```csharp
            int shooter = defender == 1 ? 2 : 1;
```
**Giải thích:** Gán `defender == 1 ? 2 : 1` cho `int shooter`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 302
```csharp
            SendTo(shooter, "FIRE_RESULT|" + result + "|" + row + "|" + col);
```
**Giải thích:** Dòng này liên quan đến giao thức `FIRE_RESULT|`: lệnh trả kết quả bắn HIT/MISS. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 304
```csharp
            if (result == "HIT")
```
**Giải thích:** Kiểm tra điều kiện `(result == "HIT"`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 306
```csharp
                hitsOnPlayer[defender]++;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 307
```csharp
                AddLog("Người chơi " + defender + " bị bắn trúng. Tổng số ô trúng: " + hitsOnPlayer[defender] + "/" + totalShipCellsOnPlayer[defender]);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 309
```csharp
                if (hitsOnPlayer[defender] >= totalShipCellsOnPlayer[defender])
```
**Giải thích:** Kiểm tra điều kiện `(hitsOnPlayer[defender] >= totalShipCellsOnPlayer[defender]`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 311
```csharp
                    EndGame(shooter, defender);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 312
```csharp
                    return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 315
```csharp
                currentTurn = shooter;
```
**Giải thích:** Gán giá trị mới cho `currentTurn`. người chơi đang được quyền bắn. Giá trị `shooter` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 316
```csharp
                Broadcast("TURN|" + currentTurn);
```
**Giải thích:** Dòng này liên quan đến giao thức `TURN|`: lệnh Server báo lượt hiện tại. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 317
```csharp
                AddLog("Người chơi " + shooter + " bắn trúng nên được bắn tiếp.");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 319
```csharp
            else
```
**Giải thích:** Nhánh xử lý còn lại khi các điều kiện phía trước đều sai. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 321
```csharp
                currentTurn = defender;
```
**Giải thích:** Gán giá trị mới cho `currentTurn`. người chơi đang được quyền bắn. Giá trị `defender` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 322
```csharp
                Broadcast("TURN|" + currentTurn);
```
**Giải thích:** Dòng này liên quan đến giao thức `TURN|`: lệnh Server báo lượt hiện tại. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..

#### Dòng 323
```csharp
                AddLog("Người chơi " + shooter + " bắn hụt. Đổi lượt sang người chơi " + defender);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessFireResult`, tức phần chuyển kết quả hit/miss về người bắn và cập nhật thắng thua..


---

### Nhóm hàm `ProcessSunk`

**Vai trò:** chuyển thông báo tàu chìm.

#### Dòng 327
```csharp
        private void ProcessSunk(int playerNumber, string msg, string[] parts)
```
**Giải thích:** Khai báo hàm `ProcessSunk(int playerNumber, string msg, string[] parts)`. Công dụng chính: chuyển thông báo tàu chìm.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 329
```csharp
            if (parts.Length < 5)
```
**Giải thích:** Kiểm tra điều kiện `(parts.Length < 5`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessSunk`, tức phần chuyển thông báo tàu chìm..

#### Dòng 331
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ProcessSunk`, tức phần chuyển thông báo tàu chìm..

#### Dòng 334
```csharp
            int defender = playerNumber;
```
**Giải thích:** Gán `playerNumber` cho `int defender`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessSunk`, tức phần chuyển thông báo tàu chìm..

#### Dòng 335
```csharp
            int shooter = defender == 1 ? 2 : 1;
```
**Giải thích:** Gán `defender == 1 ? 2 : 1` cho `int shooter`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessSunk`, tức phần chuyển thông báo tàu chìm..

#### Dòng 337
```csharp
            SendTo(shooter, msg);
```
**Giải thích:** Gọi hàm `SendTo` để gửi một dòng lệnh TCP đến một Client. Ngữ cảnh: dòng này nằm trong `ProcessSunk`, tức phần chuyển thông báo tàu chìm..

#### Dòng 339
```csharp
            AddLog("Người chơi " + defender + " báo chìm tàu. Chuyển kết quả cho người chơi " + shooter);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessSunk`, tức phần chuyển thông báo tàu chìm..


---

### Nhóm hàm `EndGame`

**Vai trò:** gửi WIN/LOSE.

#### Dòng 342
```csharp
        private void EndGame(int winner, int loser)
```
**Giải thích:** Khai báo hàm `EndGame(int winner, int loser)`. Công dụng chính: gửi WIN/LOSE.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 344
```csharp
            gameRunning = false;
```
**Giải thích:** Gán giá trị mới cho `gameRunning`. trạng thái ván đấu đang diễn ra. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `EndGame`, tức phần gửi WIN/LOSE..

#### Dòng 345
```csharp
            currentTurn = 0;
```
**Giải thích:** Gán giá trị mới cho `currentTurn`. người chơi đang được quyền bắn. Giá trị `0` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `EndGame`, tức phần gửi WIN/LOSE..

#### Dòng 347
```csharp
            playAgain[1] = false;
```
**Giải thích:** Cập nhật phần tử mảng `playAgain[1]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `EndGame`, tức phần gửi WIN/LOSE..

#### Dòng 348
```csharp
            playAgain[2] = false;
```
**Giải thích:** Cập nhật phần tử mảng `playAgain[2]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `EndGame`, tức phần gửi WIN/LOSE..

#### Dòng 350
```csharp
            Broadcast("GAME_OVER");
```
**Giải thích:** Gọi hàm `Broadcast` để gửi một dòng lệnh TCP cho cả hai Client. Ngữ cảnh: dòng này nằm trong `EndGame`, tức phần gửi WIN/LOSE..

#### Dòng 351
```csharp
            SendTo(winner, "WIN");
```
**Giải thích:** Dòng này liên quan đến giao thức `WIN`: lệnh báo người chơi thắng. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `EndGame`, tức phần gửi WIN/LOSE..

#### Dòng 352
```csharp
            SendTo(loser, "LOSE");
```
**Giải thích:** Dòng này liên quan đến giao thức `LOSE`: lệnh báo người chơi thua. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `EndGame`, tức phần gửi WIN/LOSE..

#### Dòng 354
```csharp
            AddLog("Trận đấu kết thúc. Người chơi " + winner + " thắng.");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `EndGame`, tức phần gửi WIN/LOSE..


---

### Nhóm hàm `ProcessDroneScan`

**Vai trò:** chuyển yêu cầu Drone sang đối thủ.

#### Dòng 357
```csharp
        private void ProcessDroneScan(int playerNumber, string[] parts)
```
**Giải thích:** Khai báo hàm `ProcessDroneScan(int playerNumber, string[] parts)`. Công dụng chính: chuyển yêu cầu Drone sang đối thủ.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 359
```csharp
            if (parts.Length < 3)
```
**Giải thích:** Kiểm tra điều kiện `(parts.Length < 3`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessDroneScan`, tức phần chuyển yêu cầu Drone sang đối thủ..

#### Dòng 361
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ProcessDroneScan`, tức phần chuyển yêu cầu Drone sang đối thủ..

#### Dòng 364
```csharp
            if (!gameRunning)
```
**Giải thích:** Kiểm tra điều kiện `(!gameRunning`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessDroneScan`, tức phần chuyển yêu cầu Drone sang đối thủ..

#### Dòng 366
```csharp
                SendTo(playerNumber, "MESSAGE|Trận đấu chưa bắt đầu.");
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessDroneScan`, tức phần chuyển yêu cầu Drone sang đối thủ..

#### Dòng 367
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ProcessDroneScan`, tức phần chuyển yêu cầu Drone sang đối thủ..

#### Dòng 370
```csharp
            if (playerNumber != currentTurn)
```
**Giải thích:** Kiểm tra điều kiện `(playerNumber != currentTurn`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessDroneScan`, tức phần chuyển yêu cầu Drone sang đối thủ..

#### Dòng 372
```csharp
                SendTo(playerNumber, "MESSAGE|Chưa đến lượt bạn.");
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessDroneScan`, tức phần chuyển yêu cầu Drone sang đối thủ..

#### Dòng 373
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ProcessDroneScan`, tức phần chuyển yêu cầu Drone sang đối thủ..

#### Dòng 376
```csharp
            int target = playerNumber == 1 ? 2 : 1;
```
**Giải thích:** Gán `playerNumber == 1 ? 2 : 1` cho `int target`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessDroneScan`, tức phần chuyển yêu cầu Drone sang đối thủ..

#### Dòng 378
```csharp
            if (players[target] == null)
```
**Giải thích:** Kiểm tra điều kiện `(players[target] == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessDroneScan`, tức phần chuyển yêu cầu Drone sang đối thủ..

#### Dòng 380
```csharp
                SendTo(playerNumber, "MESSAGE|Đối thủ chưa kết nối.");
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessDroneScan`, tức phần chuyển yêu cầu Drone sang đối thủ..

#### Dòng 381
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ProcessDroneScan`, tức phần chuyển yêu cầu Drone sang đối thủ..

#### Dòng 384
```csharp
            string row = parts[1];
```
**Giải thích:** Gán `parts[1]` cho `string row`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessDroneScan`, tức phần chuyển yêu cầu Drone sang đối thủ..

#### Dòng 385
```csharp
            string col = parts[2];
```
**Giải thích:** Gán `parts[2]` cho `string col`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessDroneScan`, tức phần chuyển yêu cầu Drone sang đối thủ..

#### Dòng 386
```csharp
            SendTo(target, "DRONE_REQUEST|" + row + "|" + col);
```
**Giải thích:** Gọi hàm `SendTo` để gửi một dòng lệnh TCP đến một Client. Ngữ cảnh: dòng này nằm trong `ProcessDroneScan`, tức phần chuyển yêu cầu Drone sang đối thủ..

#### Dòng 387
```csharp
            AddLog("Người chơi " + playerNumber + " dùng Drone dò người chơi " + target + " tại hàng " + row + ", cột " + col + ".");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessDroneScan`, tức phần chuyển yêu cầu Drone sang đối thủ..


---

### Nhóm hàm `ProcessDroneResult`

**Vai trò:** chuyển kết quả Drone về người dùng.

#### Dòng 390
```csharp
        private void ProcessDroneResult(int playerNumber, string[] parts)
```
**Giải thích:** Khai báo hàm `ProcessDroneResult(int playerNumber, string[] parts)`. Công dụng chính: chuyển kết quả Drone về người dùng.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 392
```csharp
            if (parts.Length < 4)
```
**Giải thích:** Kiểm tra điều kiện `(parts.Length < 4`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessDroneResult`, tức phần chuyển kết quả Drone về người dùng..

#### Dòng 394
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ProcessDroneResult`, tức phần chuyển kết quả Drone về người dùng..

#### Dòng 397
```csharp
            int defender = playerNumber;
```
**Giải thích:** Gán `playerNumber` cho `int defender`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessDroneResult`, tức phần chuyển kết quả Drone về người dùng..

#### Dòng 398
```csharp
            int scanner = defender == 1 ? 2 : 1;
```
**Giải thích:** Gán `defender == 1 ? 2 : 1` cho `int scanner`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `ProcessDroneResult`, tức phần chuyển kết quả Drone về người dùng..

#### Dòng 400
```csharp
            SendTo(scanner, "DRONE_RESULT|" + parts[1] + "|" + parts[2] + "|" + parts[3]);
```
**Giải thích:** Dòng này liên quan đến giao thức `DRONE_RESULT|`: lệnh trả kết quả Drone. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessDroneResult`, tức phần chuyển kết quả Drone về người dùng..

#### Dòng 401
```csharp
            AddLog("Người chơi " + defender + " trả kết quả Drone cho người chơi " + scanner + ": " + (parts[3] == "1" ? "có tàu" : "không có tàu") + ".");
```
**Giải thích:** Cập nhật phần tử mảng `AddLog("Người chơi " + defender + " trả kết quả Drone cho người chơi " + scanner + ": " + (parts[3]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ProcessDroneResult`, tức phần chuyển kết quả Drone về người dùng..


---

### Nhóm hàm `ProcessSparePlaced`

**Vai trò:** cộng tổng ô tàu cho người dùng tàu dự phòng.

#### Dòng 404
```csharp
        private void ProcessSparePlaced(int playerNumber)
```
**Giải thích:** Khai báo hàm `ProcessSparePlaced(int playerNumber)`. Công dụng chính: cộng tổng ô tàu cho người dùng tàu dự phòng.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 406
```csharp
            if (!gameRunning)
```
**Giải thích:** Kiểm tra điều kiện `(!gameRunning`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessSparePlaced`, tức phần cộng tổng ô tàu cho người dùng tàu dự phòng..

#### Dòng 408
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ProcessSparePlaced`, tức phần cộng tổng ô tàu cho người dùng tàu dự phòng..

#### Dòng 411
```csharp
            if (playerNumber != currentTurn)
```
**Giải thích:** Kiểm tra điều kiện `(playerNumber != currentTurn`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessSparePlaced`, tức phần cộng tổng ô tàu cho người dùng tàu dự phòng..

#### Dòng 413
```csharp
                SendTo(playerNumber, "MESSAGE|Chưa đến lượt bạn.");
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessSparePlaced`, tức phần cộng tổng ô tàu cho người dùng tàu dự phòng..

#### Dòng 414
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ProcessSparePlaced`, tức phần cộng tổng ô tàu cho người dùng tàu dự phòng..

#### Dòng 417
```csharp
            if (sparePlaced[playerNumber])
```
**Giải thích:** Kiểm tra điều kiện `(sparePlaced[playerNumber]`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessSparePlaced`, tức phần cộng tổng ô tàu cho người dùng tàu dự phòng..

#### Dòng 419
```csharp
                SendTo(playerNumber, "MESSAGE|Bạn đã dùng tàu dự phòng rồi.");
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessSparePlaced`, tức phần cộng tổng ô tàu cho người dùng tàu dự phòng..

#### Dòng 420
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `ProcessSparePlaced`, tức phần cộng tổng ô tàu cho người dùng tàu dự phòng..

#### Dòng 423
```csharp
            sparePlaced[playerNumber] = true;
```
**Giải thích:** Cập nhật phần tử mảng `sparePlaced[playerNumber]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ProcessSparePlaced`, tức phần cộng tổng ô tàu cho người dùng tàu dự phòng..

#### Dòng 424
```csharp
            totalShipCellsOnPlayer[playerNumber] += 2;
```
**Giải thích:** Cập nhật phần tử mảng `totalShipCellsOnPlayer[playerNumber] +`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ProcessSparePlaced`, tức phần cộng tổng ô tàu cho người dùng tàu dự phòng..

#### Dòng 425
```csharp
            Broadcast("MESSAGE|Người chơi " + playerNumber + " đã đặt thêm tàu dự phòng.");
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessSparePlaced`, tức phần cộng tổng ô tàu cho người dùng tàu dự phòng..

#### Dòng 426
```csharp
            AddLog("Người chơi " + playerNumber + " đặt tàu dự phòng. Tổng ô cần bắn chìm: " + totalShipCellsOnPlayer[playerNumber]);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessSparePlaced`, tức phần cộng tổng ô tàu cho người dùng tàu dự phòng..


---

### Nhóm hàm `ProcessPlayAgain`

**Vai trò:** xử lý chơi lại.

#### Dòng 429
```csharp
        private void ProcessPlayAgain(int playerNumber)
```
**Giải thích:** Khai báo hàm `ProcessPlayAgain(int playerNumber)`. Công dụng chính: xử lý chơi lại.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 431
```csharp
            playAgain[playerNumber] = true;
```
**Giải thích:** Cập nhật phần tử mảng `playAgain[playerNumber]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ProcessPlayAgain`, tức phần xử lý chơi lại..

#### Dòng 433
```csharp
            SendTo(playerNumber, "MESSAGE|Bạn đã chọn chơi lại. Đang chờ người chơi còn lại...");
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessPlayAgain`, tức phần xử lý chơi lại..

#### Dòng 434
```csharp
            AddLog("Người chơi " + playerNumber + " muốn chơi lại.");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessPlayAgain`, tức phần xử lý chơi lại..

#### Dòng 436
```csharp
            if (players[1] != null && players[2] != null && playAgain[1] && playAgain[2])
```
**Giải thích:** Kiểm tra điều kiện `(players[1] != null && players[2] != null && playAgain[1] && playAgain[2]`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `ProcessPlayAgain`, tức phần xử lý chơi lại..

#### Dòng 438
```csharp
                ResetGameState();
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessPlayAgain`, tức phần xử lý chơi lại..

#### Dòng 440
```csharp
                Broadcast("RESTART");
```
**Giải thích:** Dòng này liên quan đến giao thức `RESTART`: lệnh reset ván mới. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessPlayAgain`, tức phần xử lý chơi lại..

#### Dòng 441
```csharp
                Broadcast("MESSAGE|Hai người chơi đã chọn chơi lại. Hãy đặt lại 3 tàu và nhấn sẵn sàng.");
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `ProcessPlayAgain`, tức phần xử lý chơi lại..

#### Dòng 443
```csharp
                AddLog("Hai người chơi đồng ý chơi lại. Khởi tạo lại trận đấu.");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `ProcessPlayAgain`, tức phần xử lý chơi lại..


---

### Nhóm hàm `ResetGameState`

**Vai trò:** reset dữ liệu Server.

#### Dòng 447
```csharp
        private void ResetGameState()
```
**Giải thích:** Khai báo hàm `ResetGameState()`. Công dụng chính: reset dữ liệu Server.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 449
```csharp
            ready[1] = false;
```
**Giải thích:** Cập nhật phần tử mảng `ready[1]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetGameState`, tức phần reset dữ liệu Server..

#### Dòng 450
```csharp
            ready[2] = false;
```
**Giải thích:** Cập nhật phần tử mảng `ready[2]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetGameState`, tức phần reset dữ liệu Server..

#### Dòng 452
```csharp
            playAgain[1] = false;
```
**Giải thích:** Cập nhật phần tử mảng `playAgain[1]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetGameState`, tức phần reset dữ liệu Server..

#### Dòng 453
```csharp
            playAgain[2] = false;
```
**Giải thích:** Cập nhật phần tử mảng `playAgain[2]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetGameState`, tức phần reset dữ liệu Server..

#### Dòng 455
```csharp
            hitsOnPlayer[1] = 0;
```
**Giải thích:** Cập nhật phần tử mảng `hitsOnPlayer[1]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetGameState`, tức phần reset dữ liệu Server..

#### Dòng 456
```csharp
            hitsOnPlayer[2] = 0;
```
**Giải thích:** Cập nhật phần tử mảng `hitsOnPlayer[2]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetGameState`, tức phần reset dữ liệu Server..

#### Dòng 458
```csharp
            totalShipCellsOnPlayer[1] = DEFAULT_SHIP_CELLS;
```
**Giải thích:** Cập nhật phần tử mảng `totalShipCellsOnPlayer[1]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetGameState`, tức phần reset dữ liệu Server..

#### Dòng 459
```csharp
            totalShipCellsOnPlayer[2] = DEFAULT_SHIP_CELLS;
```
**Giải thích:** Cập nhật phần tử mảng `totalShipCellsOnPlayer[2]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetGameState`, tức phần reset dữ liệu Server..

#### Dòng 460
```csharp
            sparePlaced[1] = false;
```
**Giải thích:** Cập nhật phần tử mảng `sparePlaced[1]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetGameState`, tức phần reset dữ liệu Server..

#### Dòng 461
```csharp
            sparePlaced[2] = false;
```
**Giải thích:** Cập nhật phần tử mảng `sparePlaced[2]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `ResetGameState`, tức phần reset dữ liệu Server..

#### Dòng 463
```csharp
            currentTurn = 0;
```
**Giải thích:** Gán giá trị mới cho `currentTurn`. người chơi đang được quyền bắn. Giá trị `0` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ResetGameState`, tức phần reset dữ liệu Server..

#### Dòng 464
```csharp
            gameRunning = false;
```
**Giải thích:** Gán giá trị mới cho `gameRunning`. trạng thái ván đấu đang diễn ra. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `ResetGameState`, tức phần reset dữ liệu Server..


---

### Nhóm hàm `SendTo`

**Vai trò:** gửi lệnh cho một player.

#### Dòng 467
```csharp
        private void SendTo(int playerNumber, string msg)
```
**Giải thích:** Khai báo hàm `SendTo(int playerNumber, string msg)`. Công dụng chính: gửi lệnh cho một player.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 469
```csharp
            try
```
**Giải thích:** Bắt đầu khối có thể phát sinh lỗi như kết nối mạng, parse dữ liệu, đọc ảnh/âm thanh. Ngữ cảnh: dòng này nằm trong `SendTo`, tức phần gửi lệnh cho một player..

#### Dòng 471
```csharp
                if (playerNumber < 1 || playerNumber > 2)
```
**Giải thích:** Kiểm tra điều kiện `(playerNumber < 1 || playerNumber > 2`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `SendTo`, tức phần gửi lệnh cho một player..

#### Dòng 473
```csharp
                    return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `SendTo`, tức phần gửi lệnh cho một player..

#### Dòng 476
```csharp
                if (writers[playerNumber] != null)
```
**Giải thích:** Kiểm tra điều kiện `(writers[playerNumber] != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `SendTo`, tức phần gửi lệnh cho một player..

#### Dòng 478
```csharp
                    lock (writers[playerNumber])
```
**Giải thích:** Khóa dữ liệu dùng chung để tránh nhiều thread Server sửa cùng lúc, đặc biệt khi nhiều Client gửi lệnh. Ngữ cảnh: dòng này nằm trong `SendTo`, tức phần gửi lệnh cho một player..

#### Dòng 480
```csharp
                        writers[playerNumber].WriteLine(msg);
```
**Giải thích:** Gửi một dòng lệnh text qua TCP. Đây là lệnh thật đi giữa Client và Server. Ngữ cảnh: dòng này nằm trong `SendTo`, tức phần gửi lệnh cho một player..

#### Dòng 484
```csharp
            catch
```
**Giải thích:** Bắt lỗi để chương trình không bị crash; thường hiển thị thông báo hoặc bỏ qua lỗi nhỏ. Ngữ cảnh: dòng này nằm trong `SendTo`, tức phần gửi lệnh cho một player..

#### Dòng 486
```csharp
                AddLog("Không gửi được dữ liệu tới người chơi " + playerNumber);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `SendTo`, tức phần gửi lệnh cho một player..


---

### Nhóm hàm `Broadcast`

**Vai trò:** gửi lệnh cho cả hai.

#### Dòng 490
```csharp
        private void Broadcast(string msg)
```
**Giải thích:** Khai báo hàm `Broadcast(string msg)`. Công dụng chính: gửi lệnh cho cả hai.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 492
```csharp
            SendTo(1, msg);
```
**Giải thích:** Gọi hàm `SendTo` để gửi một dòng lệnh TCP đến một Client. Ngữ cảnh: dòng này nằm trong `Broadcast`, tức phần gửi lệnh cho cả hai..

#### Dòng 493
```csharp
            SendTo(2, msg);
```
**Giải thích:** Gọi hàm `SendTo` để gửi một dòng lệnh TCP đến một Client. Ngữ cảnh: dòng này nằm trong `Broadcast`, tức phần gửi lệnh cho cả hai..


---

### Nhóm hàm `DisconnectPlayer`

**Vai trò:** xử lý mất kết nối.

#### Dòng 496
```csharp
        private void DisconnectPlayer(int playerNumber)
```
**Giải thích:** Khai báo hàm `DisconnectPlayer(int playerNumber)`. Công dụng chính: xử lý mất kết nối.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 498
```csharp
            lock (lockObj)
```
**Giải thích:** Khóa dữ liệu dùng chung để tránh nhiều thread Server sửa cùng lúc, đặc biệt khi nhiều Client gửi lệnh. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 500
```csharp
                if (playerNumber < 1 || playerNumber > 2)
```
**Giải thích:** Kiểm tra điều kiện `(playerNumber < 1 || playerNumber > 2`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 502
```csharp
                    return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 505
```csharp
                if (players[playerNumber] == null)
```
**Giải thích:** Kiểm tra điều kiện `(players[playerNumber] == null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 507
```csharp
                    return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 510
```csharp
                try
```
**Giải thích:** Bắt đầu khối có thể phát sinh lỗi như kết nối mạng, parse dữ liệu, đọc ảnh/âm thanh. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 512
```csharp
                    players[playerNumber].Close();
```
**Giải thích:** Đóng kết nối hoặc tài nguyên socket/file hiện tại. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 514
```csharp
                catch
```
**Giải thích:** Bắt lỗi để chương trình không bị crash; thường hiển thị thông báo hoặc bỏ qua lỗi nhỏ. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 519
```csharp
                players[playerNumber] = null;
```
**Giải thích:** Cập nhật phần tử mảng `players[playerNumber]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 520
```csharp
                readers[playerNumber] = null;
```
**Giải thích:** Cập nhật phần tử mảng `readers[playerNumber]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 521
```csharp
                writers[playerNumber] = null;
```
**Giải thích:** Cập nhật phần tử mảng `writers[playerNumber]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 523
```csharp
                ready[playerNumber] = false;
```
**Giải thích:** Cập nhật phần tử mảng `ready[playerNumber]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 524
```csharp
                playAgain[playerNumber] = false;
```
**Giải thích:** Cập nhật phần tử mảng `playAgain[playerNumber]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 525
```csharp
                hitsOnPlayer[playerNumber] = 0;
```
**Giải thích:** Cập nhật phần tử mảng `hitsOnPlayer[playerNumber]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 526
```csharp
                totalShipCellsOnPlayer[playerNumber] = DEFAULT_SHIP_CELLS;
```
**Giải thích:** Cập nhật phần tử mảng `totalShipCellsOnPlayer[playerNumber]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 527
```csharp
                sparePlaced[playerNumber] = false;
```
**Giải thích:** Cập nhật phần tử mảng `sparePlaced[playerNumber]`. Đây thường là cập nhật trạng thái một ô bàn cờ, một tàu hoặc một player. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 529
```csharp
                if (playerCount > 0)
```
**Giải thích:** Kiểm tra điều kiện `(playerCount > 0`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 531
```csharp
                    playerCount--;
```
**Giải thích:** Dòng code chính này tham gia vào gameplay, giao diện, hiệu ứng, âm thanh hoặc giao tiếp mạng theo ngữ cảnh hàm hiện tại. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 534
```csharp
                gameRunning = false;
```
**Giải thích:** Gán giá trị mới cho `gameRunning`. trạng thái ván đấu đang diễn ra. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 535
```csharp
                currentTurn = 0;
```
**Giải thích:** Gán giá trị mới cho `currentTurn`. người chơi đang được quyền bắn. Giá trị `0` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 536
```csharp
                AddLog("Người chơi " + playerNumber + " đã ngắt kết nối.");
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 538
```csharp
                int otherPlayer = playerNumber == 1 ? 2 : 1;
```
**Giải thích:** Gán `playerNumber == 1 ? 2 : 1` cho `int otherPlayer`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 540
```csharp
                if (players[otherPlayer] != null)
```
**Giải thích:** Kiểm tra điều kiện `(players[otherPlayer] != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 542
```csharp
                    SendTo(otherPlayer, "DISCONNECT");
```
**Giải thích:** Dòng này liên quan đến giao thức `DISCONNECT`: lệnh báo mất kết nối. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..

#### Dòng 543
```csharp
                    SendTo(otherPlayer, "MESSAGE|Người chơi " + playerNumber + " đã thoát. Trận đấu dừng lại.");
```
**Giải thích:** Dòng này liên quan đến giao thức `MESSAGE|`: lệnh thông báo text đơn giản. Nó giúp Client và Server hiểu cùng một hành động trong game. Ngữ cảnh: dòng này nằm trong `DisconnectPlayer`, tức phần xử lý mất kết nối..


---

### Nhóm hàm `UpdateStatus`

**Vai trò:** cập nhật trạng thái server.

#### Dòng 548
```csharp
        private void UpdateStatus(string text)
```
**Giải thích:** Khai báo hàm `UpdateStatus(string text)`. Công dụng chính: cập nhật trạng thái server.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 550
```csharp
            if (this.InvokeRequired)
```
**Giải thích:** Kiểm tra điều kiện `(this.InvokeRequired`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `UpdateStatus`, tức phần cập nhật trạng thái server..

#### Dòng 552
```csharp
                this.Invoke(new Action(() => UpdateStatus(text)));
```
**Giải thích:** Gán `> UpdateStatus(text)))` cho `this.Invoke(new Action(()`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `UpdateStatus`, tức phần cập nhật trạng thái server..

#### Dòng 553
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `UpdateStatus`, tức phần cập nhật trạng thái server..

#### Dòng 556
```csharp
            lblStatus.Text = text;
```
**Giải thích:** Đổi chữ hiển thị của `lblStatus`. Người chơi sẽ thấy trạng thái/nút/tiêu đề mới trên giao diện. Ngữ cảnh: dòng này nằm trong `UpdateStatus`, tức phần cập nhật trạng thái server..


---

### Nhóm hàm `AddLog`

**Vai trò:** ghi log server.

#### Dòng 559
```csharp
        private void AddLog(string text)
```
**Giải thích:** Khai báo hàm `AddLog(string text)`. Công dụng chính: ghi log server.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 561
```csharp
            if (this.InvokeRequired)
```
**Giải thích:** Kiểm tra điều kiện `(this.InvokeRequired`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `AddLog`, tức phần ghi log server..

#### Dòng 563
```csharp
                this.Invoke(new Action(() => AddLog(text)));
```
**Giải thích:** Gán `> AddLog(text)))` cho `this.Invoke(new Action(()`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AddLog`, tức phần ghi log server..

#### Dòng 564
```csharp
                return;
```
**Giải thích:** Thoát khỏi hàm ngay tại đây. Các dòng phía sau trong hàm không chạy ở nhánh này. Ngữ cảnh: dòng này nằm trong `AddLog`, tức phần ghi log server..

#### Dòng 567
```csharp
            listBoxLog.Items.Add(DateTime.Now.ToString("HH:mm:ss") + " - " + text);
```
**Giải thích:** Thêm control/dữ liệu vào collection. Với giao diện, control sẽ xuất hiện trên panel/form. Ngữ cảnh: dòng này nằm trong `AddLog`, tức phần ghi log server..

#### Dòng 569
```csharp
            if (listBoxLog.Items.Count > 200)
```
**Giải thích:** Kiểm tra điều kiện `(listBoxLog.Items.Count > 200`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `AddLog`, tức phần ghi log server..

#### Dòng 571
```csharp
                listBoxLog.Items.RemoveAt(0);
```
**Giải thích:** Gọi một hàm hoặc phương thức. Hành động cụ thể phụ thuộc tên hàm; trong ngữ cảnh này nó phục vụ xử lý game/giao diện/socket. Ngữ cảnh: dòng này nằm trong `AddLog`, tức phần ghi log server..

#### Dòng 574
```csharp
            listBoxLog.TopIndex = listBoxLog.Items.Count - 1;
```
**Giải thích:** Gán `listBoxLog.Items.Count - 1` cho `listBoxLog.TopIndex`. Dòng này thay đổi trạng thái dữ liệu hoặc giao diện theo bước xử lý hiện tại. Ngữ cảnh: dòng này nằm trong `AddLog`, tức phần ghi log server..


---

### Nhóm hàm `Form1_FormClosing`

**Vai trò:** dọn tài nguyên server khi đóng.

#### Dòng 577
```csharp
        private void Form1_FormClosing(object sender, FormClosingEventArgs e)
```
**Giải thích:** Khai báo hàm `Form1_FormClosing(object sender, FormClosingEventArgs e)`. Công dụng chính: dọn tài nguyên server khi đóng.. Các dòng phía dưới trong hàm sẽ chạy khi hàm này được gọi hoặc khi event tương ứng xảy ra.

#### Dòng 579
```csharp
            try
```
**Giải thích:** Bắt đầu khối có thể phát sinh lỗi như kết nối mạng, parse dữ liệu, đọc ảnh/âm thanh. Ngữ cảnh: dòng này nằm trong `Form1_FormClosing`, tức phần dọn tài nguyên server khi đóng..

#### Dòng 581
```csharp
                isRunning = false;
```
**Giải thích:** Gán giá trị mới cho `isRunning`. trạng thái Server đang chạy. Giá trị `false` sẽ ảnh hưởng các hàm xử lý sau đó. Ngữ cảnh: dòng này nằm trong `Form1_FormClosing`, tức phần dọn tài nguyên server khi đóng..

#### Dòng 583
```csharp
                if (listener != null)
```
**Giải thích:** Kiểm tra điều kiện `(listener != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `Form1_FormClosing`, tức phần dọn tài nguyên server khi đóng..

#### Dòng 585
```csharp
                    listener.Stop();
```
**Giải thích:** Dừng timer hoặc âm thanh đang chạy. Ngữ cảnh: dòng này nằm trong `Form1_FormClosing`, tức phần dọn tài nguyên server khi đóng..

#### Dòng 588
```csharp
                for (int i = 1; i <= 2; i++)
```
**Giải thích:** Vòng lặp `for` dùng để duyệt nhiều ô/tàu/control theo chỉ số, ví dụ qua từng dòng/cột bàn cờ. Ngữ cảnh: dòng này nằm trong `Form1_FormClosing`, tức phần dọn tài nguyên server khi đóng..

#### Dòng 590
```csharp
                    if (players[i] != null)
```
**Giải thích:** Kiểm tra điều kiện `(players[i] != null`. Nếu đúng thì chạy khối lệnh bên trong; nếu sai thì bỏ qua hoặc sang nhánh else. Ngữ cảnh: dòng này nằm trong `Form1_FormClosing`, tức phần dọn tài nguyên server khi đóng..

#### Dòng 592
```csharp
                        players[i].Close();
```
**Giải thích:** Đóng kết nối hoặc tài nguyên socket/file hiện tại. Ngữ cảnh: dòng này nằm trong `Form1_FormClosing`, tức phần dọn tài nguyên server khi đóng..

#### Dòng 596
```csharp
            catch
```
**Giải thích:** Bắt lỗi để chương trình không bị crash; thường hiển thị thông báo hoặc bỏ qua lỗi nhỏ. Ngữ cảnh: dòng này nằm trong `Form1_FormClosing`, tức phần dọn tài nguyên server khi đóng..


---
## Các file phụ

- `AssemblyInfo.cs`: chứa thông tin metadata của project như tên assembly, version. Không ảnh hưởng trực tiếp gameplay.

- `Resources.Designer.cs`: file tự sinh để truy cập resource nếu project dùng Resources. Bản này chủ yếu dùng ảnh/âm thanh qua thư mục `Images` và `Sounds`.

- `Settings.Designer.cs`: file tự sinh cho cấu hình ứng dụng. Không phải phần logic chính của đồ án.
