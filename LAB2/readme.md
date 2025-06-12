## Các công nghệ được sử dụng
#### Ngôn ngữ: Python 3
#### Môi trường: Jupyter Notebook 
#### Thư viện:
+ collections.deque: Cung cấp cấu trúc dữ liệu hàng đợi (queue) hiệu suất cao, trong bài này, nó được sử dụng để triển khai thuật toán BFS một cách hiệu quả nhờ tốc độ thêm/xóa phần tử ở hai đầu nhanh.
+ time: Dùng để đo lường và so sánh thời gian chạy của các thuật toán.
```python
from collections import deque
----
import time
```
## Các thuật toán sử dụng:
#### Trong bài này sử dụng 2 thuật toán chính :
- BFS (Breadth-First Search) : tìm kiếm theo chiều rộng được ứng dụng trên đồ thị có trọng số và đồ thị không trọng số.
- DFS (Depth-First Search) : tìm kiếm theo chiều sâu được ứng dụng trên đồ thị có trọng số và đồ thị không trọng số.
## Giải thích cách hoạt động:
### 1. BFS (Breadth-First Search): 
- **Nguyên lý**: Khám phá tất cả các nút ở mức hiện tại trước khi chuyển sang mức tiếp theo.
- **Cấu trúc dữ liệu**: Hàng đợi (Queue).
- **Đặc điểm**:
  - Trong đồ thị không trọng số, BFS đảm bảo tìm đường đi ngắn nhất (theo số cạnh).
  - Trong đồ thị có trọng số, BFS không đảm bảo đường đi có tổng trọng số nhỏ nhất, vì nó không xem xét trọng số của các cạnh.
- code minh họa BFS trên đồ thị không trọng số
```python
def bfs(graph, start, goal):
    # Khởi tạo hàng đợi với nút bắt đầu và đường đi ban đầu
    queue = deque([(start, [start])])
    # Khởi tạo tập hợp các nút đã thăm để tránh lặp
    visited = set([start])
    # Lặp cho đến khi hàng đợi rỗng
    while queue:
        # Lấy nút đầu tiên và đường đi tương ứng từ hàng đợi
        (node, path) = queue.popleft()
        # Kiểm tra nếu nút hiện tại là đích
        if node == goal:
            return path
        # Duyệt qua các nút kề của nút hiện tại
        for neighbor in graph[node]:
            # Nếu nút kề chưa được thăm
            if neighbor not in visited:
                # Thêm nút kề vào tập đã thăm
                visited.add(neighbor)
                # Thêm nút kề và đường đi mới vào hàng đợi
                queue.append((neighbor, path + [neighbor]))
    # Trả về None nếu không tìm thấy đường đi
    return None
```
- Code minh họa BFS trên đồ thị có trọng số
```python
def bfs_weighted(graph, start, goal):
    # Khởi tạo hàng đợi với nút bắt đầu, đường đi, và tổng trọng số
    queue = deque([(start, [start], 0)])
    # Khởi tạo tập hợp các nút đã thăm
    visited = set([start])
    
    # Lặp cho đến khi hàng đợi rỗng
    while queue:
        # Lấy nút, đường đi, và tổng trọng số từ đầu hàng đợi
        (node, path, total_weight) = queue.popleft()
        # Kiểm tra nếu nút hiện tại là đích
        if node == goal:
            return path, total_weight
        # Duyệt qua các nút kề và trọng số tương ứng
        for neighbor, weight in graph[node]:
            # Nếu nút kề chưa được thăm
            if neighbor not in visited:
                # Thêm nút kề vào tập đã thăm
                visited.add(neighbor)
                # Thêm nút kề, đường đi mới, và tổng trọng số cập nhật vào hàng đợi
                queue.append((neighbor, path + [neighbor], total_weight + weight))
    # Trả về None và 0 nếu không tìm thấy đường đi
    return None, 0
```

### 2. DFS (Depth-First Search) 
- **Nguyên lý**: Khám phá một nhánh đến tận cùng trước khi quay lại thử nhánh khác.
- **Cấu trúc dữ liệu**: Ngăn xếp (Stack) hoặc đệ quy.
- **Đặc điểm**:
  - DFS không đảm bảo đường đi ngắn nhất trong cả đồ thị không trọng số và có trọng số. Bên cạnh đó, nó có thể bị kẹt trong vòng lặp vô hạn trên đồ thị có chu trình nếu không sử dụng tập visited.
  - Kết quả phụ thuộc vào thứ tự duyệt các nút kề.
- Code minh họa DFS trên đồ thị không trọng số:
```python
def dfs(graph, start, goal, visited=None, path=None):
    # Khởi tạo tập đã thăm và đường đi nếu chưa có
    if visited is None:
        visited = set()
    if path is None:
        path = [start]
    
    # Thêm nút hiện tại vào tập đã thăm
    visited.add(start)
    # Kiểm tra nếu nút hiện tại là đích
    if start == goal:
        return path
    # Duyệt qua các nút kề của nút hiện tại
    for neighbor in graph[start]:
        # Nếu nút kề chưa được thăm
        if neighbor not in visited:
            # Gọi đệ quy DFS trên nút kề với đường đi mới
            new_path = dfs(graph, neighbor, goal, visited, path + [neighbor])
            # Nếu tìm thấy đường đi, trả về
            if new_path:
                return new_path
    # Trả về None nếu không tìm thấy đường đi
    return None
```
- Code minh họa DFS trên đồ thị có trọng số
```python
def dfs_weighted(graph, start, goal, visited=None, path=None, total_weight=0):
    # Khởi tạo tập đã thăm và đường đi nếu chưa có
    if visited is None:
        visited = set()
    if path is None:
        path = [start]
    
    # Thêm nút hiện tại vào tập đã thăm
    visited.add(start)
    # Kiểm tra nếu nút hiện tại là đích
    if start == goal:
        return path, total_weight
    
    # Duyệt qua các nút kề và trọng số tương ứng
    for neighbor, weight in graph[start]:
        # Nếu nút kề chưa được thăm
        if neighbor not in visited:
            # Gọi đệ quy DFS trên nút kề với đường đi và tổng trọng số mới
            new_path, new_weight = dfs_weighted(graph, neighbor, goal, visited, path + [neighbor], total_weight + weight)
            # Nếu tìm thấy đường đi, trả về
            if new_path:
                return new_path, new_weight
    # Trả về None và 0 nếu không tìm thấy đường đi
    return None, 0
```
#### Phân tích bài tập 
1. Xử lý chu trình (Đồ thị mẫu 2)
- Các đồ thị trong thực tế thường có chu trình (ví dụ A -> B và B -> A). Nếu không có cơ chế kiểm soát, cả BFS và DFS sẽ bị kẹt trong một vòng lặp vô hạn, duyệt qua lại giữa các nút này và không bao giờ kết thúc.
- Giải pháp: Sử dụng một tập hợp visited để lưu trữ các nút đã được khám phá. Trước khi thêm một nút kề vào hàng đợi/ngăn xếp, kiểm tra xem nó đã có trong visited chưa. Nếu rồi, thì bỏ qua.
- Kết quả: Trong bài tập với Đồ thị mẫu 2, việc áp dụng visited đã giúp cả hai thuật toán hoạt động chính xác và tìm ra đường đi đến đích mà không bị lặp. 
2. Hạn chế trên Đồ thị có Trọng số (Đồ thị mẫu 5 & 6)
- BFS và DFS vốn không quan tâm về trọng số của các cạnh. Chúng chỉ quan tâm đến cấu trúc liên kết. BFS tối ưu về số lượng cạnh, còn DFS đi theo thứ tự duyệt.
- Ở đồ thị mẫu 6: Đường đi mà BFS và DFS tìm ra vô tình tối ưu về trọng số là S -> A -> B -> E -> H với tổng trọng số là 21 và BFS tìm được đường đi ngắn nhất về số lượng cạnh là 4.
![image](https://github.com/user-attachments/assets/2c0c9a1e-d65d-4c03-93e9-b1500627468d)
- Trong bài tập, khi yêu cầu thêm một cạnh mới B -> H với trọng số 20, BFS ngay lập tức tìm ra đường đi S -> A -> B -> H. Đường này chỉ có 3 cạnh (ngắn hơn đường cũ 4 cạnh) nhưng tổng trọng số lại là 25, cao hơn và không tối ưu.
![image](https://github.com/user-attachments/assets/b85f22ad-9e8b-4c27-ac24-a9a1f5a94f1c)

  
3. Tìm tất cả các đường đi (Đồ thị mẫu 7)
- Yêu cầu: Sửa đổi thuật toán để tìm tất cả các đường đi có thể từ S đến H.
- Giải pháp: cần phải thay đổi thuật toán duyệt. Thay vì dừng lại ngay khi tìm thấy đỉnh H, thuật toán sẽ lưu lại đường đi hiện tại và tiếp tục tìm các nhánh khác. Đồng thời, điều kiện để tránh lặp cũng phải thay đổi: thay vì dùng visited toàn cục, ta chỉ cần đảm bảo một nút không xuất hiện lặp lại trên cùng một đường đi (if neighbor not in path).
```python
 # Duyệt qua các nút kề của nút hiện tại.
        for neighbor in graph.get(node, []):
            if neighbor not in path:
                # Tạo một đường đi mới bằng cách lấy đường đi cũ...
                new_path = list(path)
                # ...và thêm nút kề vào.
                new_path.append(neighbor)
                
                # Thêm đường đi mới này vào cuối hàng đợi 
                queue.append(new_path)
```
Kết quả: Thuật toán sau khi sửa chạy thành công và liệt kê ra rất nhiều đường đi khác nhau từ S đến H trong Đồ thị mẫu 7. 

4. So sánh hiệu suất (Bài tập nâng cao)
- Phương pháp: Sử dụng thư viện time để đo thời gian thực thi của BFS và DFS trên các đồ thị phức tạp (mẫu 6 và 7).
- Kết quả phân tích:
  - Không có thuật toán nào "luôn luôn" nhanh hơn. Hiệu suất phụ thuộc rất nhiều vào cấu trúc đồ thị và vị trí của nút đích.
  - DFS thường nhanh hơn nếu đích nằm sâu trong một nhánh và thuật toán "may mắn" đi đúng nhánh đó trước.
  - BFS lại có lợi nếu đích ở gần điểm bắt đầu (độ sâu thấp), vì BFS tìm theo từng lớp và có thể gặp đích sớm hơn.
  - Về phần bộ nhớ, thì DFS thường sẽ chiếm bộ nhớ ít hơn do khi duyệt thì nó chỉ cần lưu đường đi hiện tại dựa trên chiều sâu, còn BFS thì cần nhiều bộ nhớ hơn do phải lưu tất cả các đỉnh ở cùng mức.


