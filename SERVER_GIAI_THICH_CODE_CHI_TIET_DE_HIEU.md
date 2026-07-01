# GIẢI THÍCH CODE SERVER - BẢN SỬA LẠI CHI TIẾT

Nhóm: Lâm - Minh  
File chính: `Server/Form1.cs`

Server là phần trung gian điều phối trận đấu. Client không tự bắn trực tiếp vào nhau, mà gửi lệnh lên Server. Server kiểm tra lượt, chuyển lệnh sang đối thủ, nhận kết quả rồi gửi lại cho người bắn.

---

## 1. Biến chính của Server

```csharp
const int DEFAULT_SHIP_CELLS = 9;

TcpListener listener;
Thread listenThread;
bool isRunning = false;

TcpClient[] players = new TcpClient[3];
StreamReader[] readers = new StreamReader[3];
StreamWriter[] writers = new StreamWriter[3];

bool[] ready = new bool[3];
bool[] playAgain = new bool[3];
int[] hitsOnPlayer = new int[3];
int[] totalShipCellsOnPlayer = new int[3];
bool[] sparePlaced = new bool[3];

int playerCount = 0;
int currentTurn = 0;
bool gameRunning = false;
```

| Dòng code | Giải thích |
|---|---|
| `DEFAULT_SHIP_CELLS = 9` | Mỗi người ban đầu có tổng 9 ô tàu: 2 + 3 + 4. |
| `listener` | Đối tượng mở cổng TCP để Client kết nối vào. |
| `listenThread` | Luồng nền chuyên chờ Client mới, để Form Server không bị đứng. |
| `isRunning` | Cho biết Server đang chạy hay chưa. |
| `players` | Lưu kết nối của người chơi 1 và 2. Mảng có size 3 để dùng index 1, 2 cho dễ. |
| `readers` | Mỗi người chơi có một StreamReader để Server đọc lệnh Client gửi lên. |
| `writers` | Mỗi người chơi có một StreamWriter để Server gửi lệnh về Client. |
| `ready` | Đánh dấu người chơi đã bấm sẵn sàng chưa. |
| `playAgain` | Đánh dấu người chơi đã chọn chơi lại chưa. |
| `hitsOnPlayer` | Đếm số ô tàu của từng người đã bị bắn trúng. |
| `totalShipCellsOnPlayer` | Tổng số ô tàu cần bắn chìm của từng người. Khi đặt tàu dự phòng thì tăng thêm 2. |
| `sparePlaced` | Đánh dấu người chơi đã dùng tàu dự phòng chưa. |
| `playerCount` | Số Client đang kết nối. |
| `currentTurn` | Người chơi đang có lượt hiện tại. |
| `gameRunning` | Trận đấu đã bắt đầu và đang chạy hay chưa. |

---

## 2. Chạy Server

```csharp
private void btnStart_Click(object sender, EventArgs e)
{
    if (isRunning)
    {
        AddLog("Máy chủ đang chạy rồi.");
        return;
    }

    try
    {
        string ipText = txtIP.Text.Trim();
        int port = int.Parse(txtPort.Text.Trim());

        IPAddress ip = IPAddress.Parse(ipText);

        listener = new TcpListener(ip, port);
        listener.Start();

        isRunning = true;

        listenThread = new Thread(ListenClients);
        listenThread.IsBackground = true;
        listenThread.Start();

        UpdateStatus("Trạng thái: Máy chủ đang chạy...");
        AddLog("Máy chủ đã chạy tại " + ipText + ":" + port);
    }
    catch (Exception ex)
    {
        MessageBox.Show("Lỗi khi chạy máy chủ: " + ex.Message);
    }
}
```

| Dòng code | Giải thích |
|---|---|
| `if (isRunning)` | Nếu Server đã chạy rồi thì không chạy lần hai. |
| `AddLog(...)` | Ghi thông báo vào khung log để người demo biết Server đang chạy. |
| `txtIP.Text.Trim()` | Lấy IP từ ô nhập trên giao diện. |
| `int.Parse(txtPort.Text.Trim())` | Lấy Port từ giao diện và đổi sang số. |
| `IPAddress.Parse(ipText)` | Chuyển chuỗi IP thành kiểu `IPAddress` để dùng với TcpListener. |
| `new TcpListener(ip, port)` | Tạo Server TCP ở IP và Port đã nhập. |
| `listener.Start()` | Bắt đầu mở cổng chờ Client kết nối. |
| `isRunning = true` | Đánh dấu Server đang chạy. |
| `new Thread(ListenClients)` | Tạo luồng nền để chờ Client, tránh treo giao diện Server. |
| `IsBackground = true` | Luồng nền tự đóng khi chương trình tắt. |
| `listenThread.Start()` | Bắt đầu nhận Client. |
| `catch` | Nếu IP/Port sai hoặc cổng bị chiếm thì hiện lỗi. |

---

## 3. Nhận Client kết nối

```csharp
private void ListenClients()
{
    while (isRunning)
    {
        TcpClient tcpClient = listener.AcceptTcpClient();

        lock (lockObj)
        {
            if (playerCount >= 2)
            {
                StreamWriter tempWriter = new StreamWriter(tcpClient.GetStream(), encoding);
                tempWriter.AutoFlush = true;
                tempWriter.WriteLine("MESSAGE|Máy chủ đã đủ 2 người chơi.");
                tcpClient.Close();
                continue;
            }

            int playerNumber = players[1] == null ? 1 : 2;

            players[playerNumber] = tcpClient;
            readers[playerNumber] = new StreamReader(tcpClient.GetStream(), encoding);
            writers[playerNumber] = new StreamWriter(tcpClient.GetStream(), encoding);
            writers[playerNumber].AutoFlush = true;

            playerCount++;

            Thread clientThread = new Thread(() => HandleClient(playerNumber));
            clientThread.IsBackground = true;
            clientThread.Start();

            SendTo(playerNumber, "START|" + playerNumber);
        }
    }
}
```

| Dòng code | Giải thích |
|---|---|
| `while (isRunning)` | Server còn chạy thì tiếp tục chờ Client. |
| `AcceptTcpClient()` | Dừng tại đây cho đến khi có Client kết nối vào. |
| `lock (lockObj)` | Tránh nhiều luồng cùng sửa danh sách người chơi. |
| `if (playerCount >= 2)` | Game chỉ nhận 2 người chơi, Client thứ 3 bị từ chối. |
| `tempWriter.WriteLine("MESSAGE|...")` | Gửi thông báo cho Client thứ 3 biết phòng đã đủ. |
| `tcpClient.Close()` | Đóng kết nối Client thứ 3. |
| `players[1] == null ? 1 : 2` | Nếu slot 1 trống thì người mới là player 1, ngược lại là player 2. |
| `players[playerNumber] = tcpClient` | Lưu kết nối của người chơi vào mảng. |
| `new StreamReader(...)` | Tạo luồng đọc dữ liệu từ Client đó. |
| `new StreamWriter(...)` | Tạo luồng gửi dữ liệu về Client đó. |
| `AutoFlush = true` | Mỗi lần gửi `WriteLine` là dữ liệu đi ngay, không bị giữ trong buffer. |
| `playerCount++` | Tăng số người đang kết nối. |
| `new Thread(() => HandleClient(playerNumber))` | Tạo luồng riêng để đọc lệnh từ người chơi này. |
| `SendTo(playerNumber, "START|" + playerNumber)` | Gửi số người chơi cho Client, để Client biết mình là người chơi 1 hay 2. |

---

## 4. Đọc lệnh từ Client

```csharp
private void HandleClient(int playerNumber)
{
    try
    {
        while (isRunning && players[playerNumber] != null)
        {
            string msg = readers[playerNumber].ReadLine();

            if (msg == null)
            {
                break;
            }

            AddLog("Người chơi " + playerNumber + " gửi: " + msg);
            ProcessMessage(playerNumber, msg);
        }
    }
    catch
    {
        AddLog("Người chơi " + playerNumber + " mất kết nối.");
    }
    finally
    {
        DisconnectPlayer(playerNumber);
    }
}
```

| Dòng code | Giải thích |
|---|---|
| `while (isRunning && players[playerNumber] != null)` | Còn Server và người chơi chưa thoát thì tiếp tục đọc lệnh. |
| `ReadLine()` | Đọc một dòng lệnh Client gửi lên, ví dụ `READY`, `FIRE|2|3`. |
| `if (msg == null)` | Nếu null nghĩa là Client đã đóng kết nối. |
| `AddLog(...)` | Ghi lại lệnh Client gửi để dễ quan sát khi demo. |
| `ProcessMessage(playerNumber, msg)` | Phân tích lệnh và gọi hàm xử lý phù hợp. |
| `catch` | Nếu mạng lỗi thì ghi log mất kết nối. |
| `finally` | Dù lỗi hay thoát bình thường, Server vẫn dọn dữ liệu người chơi. |

---

## 5. Phân loại lệnh Client gửi lên

```csharp
private void ProcessMessage(int playerNumber, string msg)
{
    string[] parts = msg.Split('|');

    if (parts[0] == "READY")
    {
        ready[playerNumber] = true;
        Broadcast("MESSAGE|Người chơi " + playerNumber + " đã sẵn sàng.");

        if (players[1] != null && players[2] != null && ready[1] && ready[2] && !gameRunning)
        {
            StartGame();
        }
    }
    else if (parts[0] == "FIRE")
    {
        ProcessFire(playerNumber, parts);
    }
    else if (parts[0] == "FIRE_RESULT")
    {
        ProcessFireResult(playerNumber, parts);
    }
    else if (parts[0] == "DRONE_SCAN")
    {
        ProcessDroneScan(playerNumber, parts);
    }
    else if (parts[0] == "SPARE_PLACED")
    {
        ProcessSparePlaced(playerNumber);
    }
    else if (parts[0] == "SKIP_TURN")
    {
        ProcessSkipTurn(playerNumber);
    }
}
```

| Dòng code | Giải thích |
|---|---|
| `msg.Split('|')` | Tách lệnh theo dấu `|`. Ví dụ `FIRE|2|3` thành `FIRE`, `2`, `3`. |
| `READY` | Người chơi báo đã đặt tàu và sẵn sàng. |
| `ready[playerNumber] = true` | Ghi nhận người chơi này đã sẵn sàng. |
| `Broadcast("MESSAGE|...")` | Báo cho cả hai Client biết có người đã sẵn sàng. |
| `players[1] != null && players[2] != null` | Cần đủ 2 người chơi. |
| `ready[1] && ready[2]` | Cả hai đều phải sẵn sàng. |
| `!gameRunning` | Tránh gọi StartGame nhiều lần. |
| `ProcessFire` | Xử lý lệnh bắn. |
| `ProcessFireResult` | Xử lý kết quả hit/miss. |
| `ProcessDroneScan` | Xử lý Drone. |
| `ProcessSparePlaced` | Xử lý đặt tàu dự phòng. |
| `ProcessSkipTurn` | Xử lý hết giờ mất lượt. |

---

## 6. Bắt đầu trận

```csharp
private void StartGame()
{
    gameRunning = true;
    currentTurn = 1;

    hitsOnPlayer[1] = 0;
    hitsOnPlayer[2] = 0;

    totalShipCellsOnPlayer[1] = DEFAULT_SHIP_CELLS;
    totalShipCellsOnPlayer[2] = DEFAULT_SHIP_CELLS;

    Broadcast("BOTH_READY");
    Broadcast("MESSAGE|Cả hai người chơi đã sẵn sàng. Người chơi 1 đi trước.");
    Broadcast("TURN|1");
}
```

| Dòng code | Giải thích |
|---|---|
| `gameRunning = true` | Đánh dấu trận đã bắt đầu. |
| `currentTurn = 1` | Người chơi 1 đi trước. |
| `hitsOnPlayer[1] = 0` | Reset số ô bị bắn trúng của người chơi 1. |
| `hitsOnPlayer[2] = 0` | Reset số ô bị bắn trúng của người chơi 2. |
| `totalShipCellsOnPlayer = DEFAULT_SHIP_CELLS` | Mỗi người ban đầu có 9 ô tàu cần bị bắn chìm. |
| `Broadcast("BOTH_READY")` | Báo cả hai Client chuyển sang trạng thái bắt đầu trận. |
| `Broadcast("TURN|1")` | Báo lượt đầu tiên thuộc người chơi 1. |

---

## 7. Xử lý bắn

```csharp
private void ProcessFire(int playerNumber, string[] parts)
{
    if (parts.Length < 3)
    {
        return;
    }

    if (!gameRunning)
    {
        SendTo(playerNumber, "MESSAGE|Trận đấu chưa bắt đầu.");
        return;
    }

    if (playerNumber != currentTurn)
    {
        SendTo(playerNumber, "MESSAGE|Chưa đến lượt bạn.");
        return;
    }

    int target = playerNumber == 1 ? 2 : 1;

    string row = parts[1];
    string col = parts[2];

    SendTo(target, "ENEMY_FIRE|" + row + "|" + col);
}
```

| Dòng code | Giải thích |
|---|---|
| `parts.Length < 3` | Lệnh FIRE phải có dạng `FIRE|row|col`. Thiếu row/col thì bỏ. |
| `!gameRunning` | Nếu trận chưa bắt đầu thì không xử lý bắn. |
| `playerNumber != currentTurn` | Nếu chưa tới lượt thì không cho bắn. |
| `target = playerNumber == 1 ? 2 : 1` | Người bắn là 1 thì mục tiêu là 2, ngược lại. |
| `row = parts[1]`, `col = parts[2]` | Lấy tọa độ bắn từ lệnh Client gửi. |
| `SendTo(target, "ENEMY_FIRE|...")` | Chuyển tọa độ bắn sang người bị bắn. Server không tự tính hit/miss vì chỉ Client phòng thủ biết tàu của mình nằm đâu. |

---

## 8. Xử lý kết quả bắn

```csharp
private void ProcessFireResult(int playerNumber, string[] parts)
{
    string result = parts[1];
    string row = parts[2];
    string col = parts[3];

    int defender = playerNumber;
    int shooter = defender == 1 ? 2 : 1;

    SendTo(shooter, "FIRE_RESULT|" + result + "|" + row + "|" + col);

    if (result == "HIT")
    {
        hitsOnPlayer[defender]++;

        if (hitsOnPlayer[defender] >= totalShipCellsOnPlayer[defender])
        {
            EndGame(shooter, defender);
            return;
        }

        currentTurn = shooter;
        Broadcast("TURN|" + currentTurn);
    }
    else
    {
        currentTurn = defender;
        Broadcast("TURN|" + currentTurn);
    }
}
```

| Dòng code | Giải thích |
|---|---|
| `result = parts[1]` | Kết quả do Client phòng thủ gửi lên: `HIT` hoặc `MISS`. |
| `row`, `col` | Tọa độ phát bắn. |
| `defender = playerNumber` | Người gửi kết quả chính là người bị bắn. |
| `shooter = defender == 1 ? 2 : 1` | Người còn lại là người đã bắn. |
| `SendTo(shooter, "FIRE_RESULT|...")` | Gửi kết quả về cho người bắn để vẽ hit/miss trên bàn địch. |
| `if (result == "HIT")` | Nếu trúng thì xử lý tăng số ô bị trúng. |
| `hitsOnPlayer[defender]++` | Tăng số ô tàu bị phá của người phòng thủ. |
| `hitsOnPlayer >= totalShipCells` | Nếu số ô bị phá bằng tổng ô tàu thì người phòng thủ thua. |
| `EndGame(shooter, defender)` | Người bắn thắng, người phòng thủ thua. |
| `currentTurn = shooter` | Nếu bắn trúng thì được bắn tiếp. |
| `currentTurn = defender` | Nếu bắn hụt thì đổi lượt sang người phòng thủ. |
| `Broadcast("TURN|...")` | Báo lượt mới cho cả hai Client. |

---

## 9. Xử lý Drone trên Server

```csharp
private void ProcessDroneScan(int playerNumber, string[] parts)
{
    if (parts.Length < 3)
    {
        return;
    }

    if (!gameRunning)
    {
        SendTo(playerNumber, "MESSAGE|Trận đấu chưa bắt đầu.");
        return;
    }

    if (playerNumber != currentTurn)
    {
        SendTo(playerNumber, "MESSAGE|Chưa đến lượt bạn.");
        return;
    }

    int target = playerNumber == 1 ? 2 : 1;

    string row = parts[1];
    string col = parts[2];
    SendTo(target, "DRONE_REQUEST|" + row + "|" + col);
}
```

| Dòng code | Giải thích |
|---|---|
| `parts.Length < 3` | Drone cần có row và col. |
| `!gameRunning` | Chưa vào trận thì không cho dùng. |
| `playerNumber != currentTurn` | Chưa tới lượt thì không cho dùng Drone. |
| `target = ...` | Tìm đối thủ cần bị quét. |
| `row`, `col` | Lấy vị trí bắt đầu vùng Drone. |
| `SendTo(target, "DRONE_REQUEST|...")` | Chuyển yêu cầu Drone sang Client đối thủ. Client đối thủ mới là nơi biết có tàu hay không. |

```csharp
private void ProcessDroneResult(int playerNumber, string[] parts)
{
    int defender = playerNumber;
    int scanner = defender == 1 ? 2 : 1;

    SendTo(scanner, "DRONE_RESULT|" + parts[1] + "|" + parts[2] + "|" + parts[3]);
}
```

| Dòng code | Giải thích |
|---|---|
| `defender = playerNumber` | Người gửi kết quả là người bị Drone quét. |
| `scanner = defender == 1 ? 2 : 1` | Người còn lại là người dùng Drone. |
| `SendTo(scanner, "DRONE_RESULT|...")` | Gửi kết quả Drone về đúng Client đã dùng skill. |

---

## 10. Xử lý Tàu dự phòng

```csharp
private void ProcessSparePlaced(int playerNumber)
{
    if (!gameRunning)
    {
        return;
    }

    if (playerNumber != currentTurn)
    {
        SendTo(playerNumber, "MESSAGE|Chưa đến lượt bạn.");
        return;
    }

    if (sparePlaced[playerNumber])
    {
        SendTo(playerNumber, "MESSAGE|Bạn đã dùng tàu dự phòng rồi.");
        return;
    }

    sparePlaced[playerNumber] = true;
    totalShipCellsOnPlayer[playerNumber] += 2;
    Broadcast("MESSAGE|Người chơi " + playerNumber + " đã đặt thêm tàu dự phòng.");
}
```

| Dòng code | Giải thích |
|---|---|
| `!gameRunning` | Chưa vào trận thì không xử lý. |
| `playerNumber != currentTurn` | Chỉ cho đặt tàu dự phòng khi tới lượt. |
| `sparePlaced[playerNumber]` | Nếu đã dùng rồi thì không cho dùng lại. |
| `sparePlaced[playerNumber] = true` | Ghi nhận người này đã dùng tàu dự phòng. |
| `totalShipCellsOnPlayer[playerNumber] += 2` | Tăng tổng ô tàu cần bắn chìm thêm 2. Nếu không tăng, người chơi dùng tàu dự phòng sẽ vẫn thua khi bị bắn 9 ô cũ. |
| `Broadcast("MESSAGE|...")` | Thông báo cho cả hai biết có người đặt thêm tàu dự phòng. |

---

## 11. Xử lý hết giờ mất lượt

```csharp
private void ProcessSkipTurn(int playerNumber)
{
    if (!gameRunning)
    {
        return;
    }

    if (playerNumber != currentTurn)
    {
        SendTo(playerNumber, "MESSAGE|Chưa đến lượt bạn.");
        return;
    }

    int nextPlayer = playerNumber == 1 ? 2 : 1;
    currentTurn = nextPlayer;
    Broadcast("MESSAGE|Người chơi " + playerNumber + " hết thời gian bắn. Chuyển lượt cho người chơi " + nextPlayer + ".");
    Broadcast("TURN|" + currentTurn);
}
```

| Dòng code | Giải thích |
|---|---|
| `!gameRunning` | Không xử lý nếu trận chưa chạy hoặc đã kết thúc. |
| `playerNumber != currentTurn` | Chỉ người đang có lượt mới được gửi SKIP_TURN hợp lệ. |
| `nextPlayer = ...` | Tìm người còn lại để chuyển lượt. |
| `currentTurn = nextPlayer` | Cập nhật lượt mới trên Server. |
| `Broadcast("MESSAGE|...")` | Báo cả hai người rằng có người hết giờ. |
| `Broadcast("TURN|" + currentTurn)` | Gửi lượt mới cho cả hai Client. |

---

## 12. Kết thúc trận

```csharp
private void EndGame(int winner, int loser)
{
    gameRunning = false;
    currentTurn = 0;

    playAgain[1] = false;
    playAgain[2] = false;

    Broadcast("GAME_OVER");
    SendTo(winner, "WIN");
    SendTo(loser, "LOSE");
}
```

| Dòng code | Giải thích |
|---|---|
| `gameRunning = false` | Dừng trận đấu. |
| `currentTurn = 0` | Không còn ai có lượt. |
| `playAgain[1] = false`, `playAgain[2] = false` | Reset lựa chọn chơi lại. |
| `Broadcast("GAME_OVER")` | Báo cả hai Client khóa bàn cờ và dừng timer. |
| `SendTo(winner, "WIN")` | Gửi lệnh thắng cho người thắng. |
| `SendTo(loser, "LOSE")` | Gửi lệnh thua cho người thua. |

---

## 13. Gửi dữ liệu

```csharp
private void SendTo(int playerNumber, string msg)
{
    try
    {
        if (playerNumber < 1 || playerNumber > 2)
        {
            return;
        }

        if (writers[playerNumber] != null)
        {
            lock (writers[playerNumber])
            {
                writers[playerNumber].WriteLine(msg);
            }
        }
    }
    catch
    {
        AddLog("Không gửi được dữ liệu tới người chơi " + playerNumber);
    }
}
```

| Dòng code | Giải thích |
|---|---|
| `playerNumber < 1 || playerNumber > 2` | Chỉ có người chơi 1 và 2, số khác thì bỏ. |
| `writers[playerNumber] != null` | Chỉ gửi nếu Client đó còn luồng gửi dữ liệu. |
| `lock (writers[playerNumber])` | Khóa writer để tránh hai luồng cùng gửi một lúc làm lẫn dữ liệu. |
| `WriteLine(msg)` | Gửi một dòng lệnh qua TCP. |
| `catch` | Nếu gửi lỗi thì ghi log thay vì làm Server tắt. |

```csharp
private void Broadcast(string msg)
{
    SendTo(1, msg);
    SendTo(2, msg);
}
```

Dòng này gửi cùng một thông báo cho cả hai Client, ví dụ `TURN|1`, `GAME_OVER`, `MESSAGE|...`.

---

## 14. Tóm tắt luồng Server

1. Server mở cổng TCP.
2. Chờ tối đa 2 Client kết nối.
3. Gửi `START|1` hoặc `START|2` để Client biết số người chơi.
4. Nhận `READY` từ từng Client.
5. Khi cả hai READY thì gửi `BOTH_READY` và `TURN|1`.
6. Khi Client bắn, Server nhận `FIRE|row|col`.
7. Server chuyển thành `ENEMY_FIRE|row|col` cho đối thủ.
8. Đối thủ trả `FIRE_RESULT|HIT/MISS|row|col`.
9. Server gửi kết quả về người bắn.
10. Nếu HIT thì người bắn được bắn tiếp, nếu MISS thì đổi lượt.
11. Nếu đủ số ô bị bắn chìm thì gửi `WIN` và `LOSE`.
12. Nếu hết giờ, Client gửi `SKIP_TURN`, Server chuyển lượt.
13. Drone và Tàu dự phòng cũng đi qua Server để đồng bộ giữa 2 Client.
