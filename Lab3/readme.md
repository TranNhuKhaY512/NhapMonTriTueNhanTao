## Các công nghệ được sử dụng
```python
import numpy as np
import random # thêm vào ở TH7
```
### Trong bài này sử dụng các thư viện như là numpy hỗ trợ tạo ma trận bàn cờ và thao tác dữ liệu dạng mảng nhanh chóng, thư viện random dùng để hiển thị ngẫu nhiên.
## Giải thích cách hoạt động
### THỰC HÀNH 06: Bài toán 4-Queens
1. Bài toán 4-Queens là phiên bản đơn giản của bài toán N-Queens trong trí tuệ nhân tạo.
2. Mục tiêu là đặt **4 quân hậu (Queens)** lên **bàn cờ 4x4** sao cho **không có 2 quân hậu nào tấn công lẫn nhau**.
3. Các quân hậu có thể tấn công theo:
- Cùng hàng (ngang)
- Cùng cột (dọc)
- Đường chéo
4. Quy trình giải :
- Nhập N – số quân hậu và kích thước bàn cờ N x N
- Duyệt từng hàng, tìm cột hợp lệ để đặt quân hậu:
    - Không trùng cột
    - Không trùng đường chéo
- Thử đặt quân hậu, sau đó:
    - Gọi đệ quy để đặt quân hậu tiếp theo
    - Nếu đủ N quân hậu → lưu lời giải
- Backtrack – bỏ vị trí vừa đặt để thử vị trí khác.
- In kết quả – số lời giải, bàn cờ, tọa độ các quân hậu.
  
- Trong bài này, sử dụng thư viện numpy và thuật toán ***backtrackinng*** dùng để quay lùi hay gọi là đệ quy tìm kiếm giải pháp khác.
- Định nghĩa hàm get_candidates() để lấy các cột có thể đặt quân hậu tiếp theo trên hàng hiện tại và dùng  discard() để loại bỏ các vị trí đã bị các quân hậu tấn công.
```python
# Hàm lấy các cột (candidates) có thể đặt quân hậu tiếp theo trên hàng hiện tại
def get_candidates(state, num_queens):
    if not state:
        return range(num_queens)
    position = len(state) # Vị trí hàng hiện tại (độ dài của state)
    candidates = set(range(num_queens)) # Tập hợp tất cả các cột có thể (0 đến N-1)
    # Duyệt qua các quân hậu đã được đặt trong state
    for row, col in enumerate(state):
        # Tính khoảng cách hàng giữa quân hậu hiện tại và quân hậu đã đặt
        dist = position - row
        # Loại bỏ các cột bị tấn công:
        candidates.discard(col)           # Cùng cột
        candidates.discard(col + dist)    # Cùng đường chéo chính (/)
        candidates.discard(col - dist)    # Cùng đường chéo phụ (\)
    # Trả về tập các cột hợp lệ còn lại
    return candidates
```
- Định nghĩa hàm search(), thuật toán backtracking.
```python
# Quay lùi hay gọi là đệ quy tìm kiếm giải pháp khác lúc này chúng ta có thuật toán backtrackinng 
def search(state, solutions, num_queens):
 # Nếu state hiện tại là một lời giải hợp lệ (đã đủ số quân hậu)
    if is_valid_state(state, num_queens):
        solutions.append(state.copy())  # Thêm lời giải vào danh sách kết quả
        return
    # Lặp qua các vị trí có thể đặt tiếp theo
    for candidate in get_candidates(state, num_queens):
        state.append(candidate)
        search(state, solutions, num_queens) # Gọi đệ quy để thử các bước tiếp theo
        state.pop() # Bỏ vị trí vừa thử để thử vị trí khác (backtrack)
```
- Hàm giải bài toán, định nghĩa hàm solve()
```python
def solve(num_queens):
    solutions = []
    state = []
    search(state, solutions, num_queens)
    return solutions
```
- Hàm main cho phép người dùng nhập số lượng quân hậu, sau đó gọi hàm giải solve và đưa ra lời giải, tọa độ quân hậu và biểu diễn bảng chứa vị trí các quân hậu đứng.
```python
# Hàm main chạy chương trình
if __name__ == "__main__":
  # nhập số lượng và in bàng cớ trắng
    num_queens = int(input("Nhap vao so quan hau N = "))    
    empty_board = np.full((num_queens, num_queens), "-")
    coords = [] # Danh sách lưu tọa độ của quân hậu
    print(empty_board)
    solutions = solve(num_queens)
    print(f"\n tong loi giai tim duoc: {len(solutions)}")
    for index, solution in enumerate(solutions, start=1):
        board = np.full((num_queens, num_queens), "-") # Tạo lại bàn cờ trống cho mỗi lời giải
        for row, col in enumerate(solution):
            board[row][col] = 'Q'
            coords.append((row, col))  # Lưu tọa độ

    print(f"\nLời giải {index}: {solution}")
    print("Tọa độ của các quân hậu:", coords)  # In ra tọa độ (row, col)

    for row in board:
        print(" ".join(row))
```
- Output của bài:
![image](https://github.com/user-attachments/assets/74287078-e677-4576-b5b8-70d90983085f)

### Thực hành 7: 
1. Tương tự bài toán 4-Queens, nhưng bàn cờ được mở rộng lên **8x8** và cần đặt **8 quân hậu** sao cho **không quân nào tấn công quân khác**.
2. Các quân hậu có thể tấn công theo:
- Cùng hàng (ngang)
- Cùng cột (dọc)
- Đường chéo
3. Quy trình giải :
- Nhập N – số quân hậu và kích thước bàn cờ N x N
- Duyệt từng hàng, tìm cột hợp lệ để đặt quân hậu:
    - Không trùng cột
    - Không trùng đường chéo
- Thử đặt quân hậu, sau đó:
    - Gọi đệ quy để đặt quân hậu tiếp theo
    - Nếu đủ N quân hậu → lưu lời giải
- Backtrack – bỏ vị trí vừa đặt để thử vị trí khác.
- In kết quả – số lời giải, bàn cờ, tọa độ các quân hậu.

- Trong bài nay sử dụng thư viện numpy, random  và thuật toán ***backtraccking*** dùng để quay lùi hay gọi là đệ quy tìm kiếm giải pháp khác.
- Định nghĩa hàm get_candidates() để lấy các cột có thể đặt quân hậu tiếp theo trên hàng hiện tại và dùng  discard() để loại bỏ các vị trí đã bị các quân hậu tấn công.
```python
# Hàm lấy các cột (candidates) có thể đặt quân hậu tiếp theo trên hàng hiện tại
def get_candidates(state, num_queens):
    if not state:
        return range(num_queens) 
    position = len(state) # Vị trí hàng hiện tại (độ dài của state)
    candidates = set(range(num_queens))

     # Duyệt qua các quân hậu đã được đặt trong state
    for row, col in enumerate(state):
        # Tính khoảng cách hàng giữa quân hậu hiện tại và quân hậu đã đặt
        dist = position - row
        # Loại bỏ các cột bị tấn công:
        candidates.discard(col)           # Cùng cột
        candidates.discard(col + dist)    # Cùng đường chéo chính (/)
        candidates.discard(col - dist)    # Cùng đường chéo phụ (\)
    # Trả về tập các cột hợp lệ còn lại
    return candidates
```
- Định nghĩa hàm search(), thuật toán backtracking.
```python
# Quay lùi hay gọi là đệ quy tìm kiếm giải pháp khác lúc này chúng ta có thuật toán backtrackinng 
def search(state, solutions, num_queens):
    if is_valid_state(state, num_queens):
        solutions.append(state.copy())  # Thêm lời giải vào danh sách kết quả
        return
   # Lặp qua các vị trí có thể đặt tiếp theo
    for candidate in get_candidates(state, num_queens):
        state.append(candidate)
        search(state, solutions, num_queens)  # Gọi đệ quy để thử các bước tiếp theo
        state.pop() # Bỏ vị trí vừa thử để thử vị trí khác (backtrack)
```
- Hàm giải bài toán, định nghĩa hàm solve()
```python
def solve(num_queens):
    solutions = []
    state = []
    search(state, solutions, num_queens)
    return solutions
```
- Hàm main cho phép người dùng nhập số lượng quân hậu, sau đó gọi hàm giải solve, in ra tổng số lời giải tìm được, in ra ngẫu nhiên 2 lời giải, tọa độ quân hậu của 2 lời giải ngẫu nhiên đó và kèm theo biểu diễn bảng chứa vị trí các quân hậu đứng.
```python
if __name__ == "__main__":
    num_queens = int(input("Nhap vao so quan hau N = "))
    solutions = solve(num_queens)
    print(f"\n Tổng số lời giải tìm được: {len(solutions)}")

    # Lấy ngẫu nhiên 2 lời giải
    if len(solutions) >= 2:
        # Chọn ngẫu nhiên 2 lời giải khác nhau
        random_solutions = random.sample(solutions, 2)
    else:
        random_solutions = solutions  # Nếu ít hơn 2 thì lấy tất cả

    for idx, sol in enumerate(random_solutions, start=1):
        coords = [(row, col) for row, col in enumerate(sol)]
        print(f"\nLời giải ngẫu nhiên {idx}: {sol}")
        print("Tọa độ các quân hậu:", coords)
        print_board(sol)
```
- Output của bài:
![image](https://github.com/user-attachments/assets/fa9a6635-9f6a-4d10-a056-ab816bdbfaf3)


  

