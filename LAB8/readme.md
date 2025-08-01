
# Thực hành: AI Game – Giải thuật Minimax cho Game XO 4x4, 5x5 và 6x6
## Sinh viên thực hiện: 
- Trần Như Khả Ý  
- Mã SV: 2374802010582  
- Bài tập thực hành môn: **Nhập môn trí tuệ nhân tạo**
- Giảng viên: Nguyễn Thái Anh
---
## Công nghệ sử dụng
- **Ngôn ngữ**: Python 
- **Notebook**: Jupyter Notebook
- **Thư viện**:
  - `tkinter`: Thiết kế giao diện game.
  - `messagebox`: Hiển thị thông báo người thắng/thua.
---
## Các thuật toán sử dụng:
### Giải thuật Minimax
- Hai người thay phiên nhau đưa ra các nước đi tuân theo các luật của trò chơi.
- Ví dụ : cờ vua, cờ tướng, cờ ca rô (tic-tac-toe)
- Biểu diễn trong không gian trạng thái, mỗi trạng thái là một tình thế của cuộc chơi
- Trạng thái xuất phát là sự sắp xếp các quân cờ của hai bên khi bắt đầu cuộc chơi
- Các toán tử biến đổi trạng thái là các nước đi hợp lệ
- Các trạng thái kết thúc là các tình thế mà cuộc chơi dừng, được xác định bởi một số điều kiện dừng.
- Hàm kết cuộc: mang giá trị tương ứng với mỗi trạng thái kết thúc.
- **Đặc điểm:**
  - Dạng đệ quy, duyệt cây trạng thái theo chiều sâu.
  - Mỗi nút trong cây đại diện cho một trạng thái của bàn cờ.
  - Một người chơi là Max (cố gắng tối đa hóa điểm), người kia là Min (tối thiểu hóa điểm).
- Kiểm tra thắng:
  - Duyệt tất cả các hàng.
  - Duyệt tất cả các cột.
  - Duyệt chéo chính (trái trên → phải dưới).
  - Duyệt chéo phụ (phải trên → trái dưới).
  - Kiểm tra hòa: nếu tất cả ô đã điền mà không ai thắng → hòa.
### Chi tiết hoạt động:
#### 1. Khởi tạo giao diện cho trò chơi: tạo cửa sổ chính của game.
```python
root = Tk()
root.title('Game XO NxN')
```
#### 2. Khai báo biến toàn cục:
```python
clicked = True
count = 0
win = False
buttons = []
size = 5  # Mặc định 5x5
```
#### 3. Khởi tạo hàm disableButtons(): vô hiệu hóa tất cả các ô sau khi có người thắng hoặc game kết thúc.
```python
def disableButtons():
    for row in buttons:
        for button in row:
            button.config(state=DISABLED)
```
#### 4. Khởi tạo hàm checkWinner(): kiểm tra điều kiện thắng:
- Theo hàng:
```python
 if buttons[i][0]["text"] != "" and all(buttons[i][j]["text"] == buttons[i][0]["text"] for j in range(size))
```
- Theo cột:
```python
if buttons[0][i]["text"] != "" and all(buttons[j][i]["text"] == buttons[0][i]["text"] for j in range(size))
```
- Theo đường chéo chính:
```python
if buttons[0][0]["text"] != "" and all(buttons[i][i]["text"] == buttons[0][0]["text"] for i in range(size))
```
- Theo đường chéo phụ:
```python
if buttons[0][size - 1]["text"] != "" and all(buttons[i][size - 1 - i]["text"] == buttons[0][size - 1]["text"] for i in range(size))
```
#### 5. Khởi tạo hàm checkDraw() : Nếu đã đi hết tất cả các ô mà không ai thắng → game hòa.
```python
def checkDraw():
    global count, win
    if count == size * size and not win:
        messagebox.showerror("OX Game", "DRAW!!")
        start()
```
#### 6. Khởi tạo hàm buttonClicked(button) : Gán lượt chơi X hoặc O vào ô được nhấn, đổi lượt chơi và gọi kiểm tra thắng và hòa.
```python
def buttonClicked(button):
    global clicked, count
    if button["text"] == "" and clicked:
        button["text"] = "X"
        clicked = False
        count += 1
        checkWinner()
        checkDraw()
    elif button["text"] == "" and not clicked:
        button["text"] = "O"
        clicked = True
        count += 1
        checkWinner()
        checkDraw()
```
#### 7. Khởi tạo hàm start(): 
- Reset trạng thái game.
- Xóa các nút cũ (nếu có).
- Tạo bàn cờ mới với kích thước size x size.
```python
def start():
    global buttons, clicked, count
    clicked = True
    count = 0
  # Xóa hết các button cũ
    for widget in root.winfo_children():
        if isinstance(widget, Button) and widget != startButton:
            widget.destroy()
....
```
#### 8. Khởi tạo hàm setSize() :
- Lấy số người chơi nhập vào.
- Kiểm tra kích thước hợp lệ trong khoảng 3–7.
- Cập nhật size và khởi động game.
```python
def setSize():
    global size
    try:
        s = int(sizeEntry.get())
        if s < 3 or s > 7:
            messagebox.showerror("Lỗi", "Vui lòng nhập số từ 3 đến 10")
            return
```
#### 9. Thiết lập giao diện: tạo thanh menu với chức năng restart.
```python
gameMenu = Menu(root)
root.config(menu=gameMenu)
optionMenu = Menu(gameMenu, tearoff=False)
gameMenu.add_cascade(label="Options", menu=optionMenu)
optionMenu.add_command(label="Restart Game", command=start)
```
---
## Hướng dẫn chạy
1. Mở các file Jupyter Notebook:  
   - `TranNhuKhaY_2374802010582_codeSlide.ipynb` (code theo slide)
   - `TranNhuKhaY_2374802010582.ipynb` 
2. Chạy lần lượt từng ô lệnh để khởi động giao diện.
3. Chơi luân phiên giữa 2 người

---
Kết quả cuối cùng:
- Giao diện game XO với kích thước 4x4 hoặc 5x5 hoặc 6x6.
- Thông báo người thắng/thua qua `messagebox`.
