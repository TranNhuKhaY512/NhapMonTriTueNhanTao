# Thực hành:  Reinforcement Learning – đường đi Mê cung & dạng 6x6
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
  - `numpy`: Xử lý số học, mảng.
  - `matplotlib`: Trực quan hóa dữ liệu.
  - `collections`: Sử dụng deque để quản lý hàng đợi hai đầu trong các thao tác lưu trữ trạng thái 
---
## Các thuật toán sử dụng:
### Q-Learning
- Đây là thuật toán học tăng cường (Reinforcement Learning) không mô hình (model-free).
- Mục tiêu: Học một chính sách tối ưu để đưa ra hành động tại mỗi trạng thái sao cho phần thưởng tích lũy là lớn nhất.
- Công thức toán học:
  <img width="1479" height="176" alt="image" src="https://github.com/user-attachments/assets/b9ab090b-399a-4603-9816-2a95047336e6" />
- Trong đó:
  - Q(s, a) là giá trị Q cho trạng thái s và hành động a.
  - alpha là tốc độ học, kiểm soát lượng thông tin mới ghi đè lên thông tin cũ.
  - R là phần thưởng ngay lập tức khi thực hiện hành động a trong trạng thái s.
  - gamma là hệ số chiết khấu, biểu thị tầm quan trọng của phần thưởng trong tương lai.
  - maxQ(s', a') là giá trị Q tối đa cho trạng thái tiếp theo s', biểu thị phần thưởng tốt nhất có thể đạt được từ trạng thái đó.
- Các bước thuật toán tăng cường:
- Bước 1: Xác định kích thước và chướng ngại vật của Mê cung
Xác định một maze_size biểu thị kích thước của mê cung. Trường hợp kích thước lưới 6x6. Một danh sách tọa độ biểu thị vị trí của chướng ngại vật trong mê cung cũng được chỉ định. Robot được định vị ở trạng thái ban đầu tại ô (0, 0) và hướng đến trạng thái mục tiêu tại ô (0, 5).
  - Ví dụ kích thước, vị trí của các vật cản, trạng thái ban đầu và mục tiêu:
```python
maze_size = 6
obstacles = [(0,1), (2,2), (3,2), (4,2), (5,2), (0,3),(2,4),(5,4)]
start = (0,0)
goal = (0,5)
```
    
- Bước 2: Xác định hàm is_valid
Hàm is_valid kiểm tra xem vị trí nhất định của (x,y) có hợp lệ hay không để kiểm tra xem vị trí đó có nằm trong ranh giới của mê cung và không bị bất kỳ chướng ngại vật nào cản trở hay không.
```python
    def is_valid(self, position):
        r, c = position
        return 0 <= r < self.rows and 0 <= c < self.cols and self.grid[r][c] == 0
```
- Bước 3: Hàm DFS - tìm kiếm chiều sâu
  - Định nghĩa hàm DFS để triển khai thuật toán tìm kiếm chiều sâu. Thuật toán sử dụng một ngăn xếp để lưu trữ hồ sơ các nút mà nó truy cập và lặp lại qua từng nút giúp khám phá các nút lân cận theo cách đệ quy cho đến khi tìm thấy nút mục tiêu hoặc hết mọi khả năng.
  - Ngăn xếp rỗng lưu trữ các nút cần khám phá và tập rỗng được sử dụng để theo dõi các nút đã được truy cập để tránh truy cập lại chính nó.
  - Nếu nút hiện tại được tìm thấy là nút mục tiêu, nó sẽ đảo ngược đường dẫn để robot có thể đi theo và cuối cùng trả về "Đã tìm thấy đường dẫn".
  - Nếu không tìm thấy đường dẫn nào, nó sẽ trả về "Không tìm thấy đường dẫn". Đây là phương pháp tìm kiếm vét cạn. Đảm bảo rằng tất cả các đường dẫn tiềm năng đều được khám phá và cung cấp giải pháp toàn diện cho vấn đề hướng đến mê cung.
  - Code chính:
```python
def dfs (current, visited, path):
    x,y = current
    if current == goal:
        path.append(current)
        return True
    visited.add(current)
    moves = [(x-1, y), (x+1, y), (x,y-1), (x, y+1)]
    for move in moves:
        if is_valid(*move) and move not in visited:
            if dfs(move, visited, path):
                path.append(current)
                return True
    return False

  ```

- Bước 4: Tìm đường bằng BFS (Breadth-First Search) (nếu có gọi)
  - Sử dụng hàng đợi (queue) để thực hiện tìm kiếm theo chiều rộng.
  - code chính:
```python
def bfs(self):
        queue = deque([(self.start, [self.start])])
        visited = set([self.start])
        while queue:
            current, path = queue.popleft()
            if current == self.goal:
                return path
            # Khám phá tất cả hướng đi
            for move in MOVES:
                next_r, next_c = current[0] + move[0], current[1] + move[1]
                next_position = (next_r, next_c)
                if self.is_valid(next_position) and next_position not in visited:
                    visited.add(next_position)
                    queue.append((next_position, path + [next_position]))
        # Không tìm thấy đường đi
        return None
```
- Bước 5: Gọi hàm DFS hoặc BFS để tìm đường dẫn:
  - code chính:
  - BFS:
```python
planner = FSSP_BFS(grid, start, goal)
path = planner.bfs()

if path:
    print(f"Path found: {path}")
    planner.visualize(path)
else:
    print("No path found")
```
- DFS:
```python
if dfs(start, visited, path):
    path.reverse()
    print("Path found")
    for position in path:
        print(position)
else:
    print("No path found!")
```
  
## Hướng dẫn chạy
1. Mở file Jupyter Notebook:  
   `2374802010582_TranNhuKhaY.ipynb`
2. Chạy lần lượt từng cell để xem kết quả.
3. Kết quả cuối cùng sẽ hiển thị:
- **Q-table cuối cùng** sau quá trình huấn luyện.
- **Lộ trình (path) tối ưu** mà agent tìm được từ điểm bắt đầu đến đích
- Hình vẽ mô phỏng đường đi tới goal.


