
## Các công nghệ được sử dụng
#### Ngôn ngữ: Python 
#### Môi trường: Jupyter Notebook 
#### Thư viện:
+ collections.deque: Cung cấp cấu trúc dữ liệu hàng đợi (queue) hiệu suất cao, trong bài này, nó được sử dụng để triển khai thuật toán BFS một cách hiệu quả nhờ tốc độ thêm/xóa phần tử ở hai đầu nhanh.
+ time: Dùng để đo lường và so sánh thời gian chạy của các thuật toán.
+ streamlit: là một thư viện tạo giao diện web nhanh cho Python, thường dùng để trình bày kết quả của AI/Data Science được ứng dụng để tạo giao diện nhập đồ thị, chọn thuật toán, hiển thị kết quả đồ họa trên web.
+ NetworkX:  là thư viện mạnh để xử lý đồ thị (graph): biểu diễn, vẽ, tính toán, tìm đường,...ứng dụng trong biểu diễn đồ thị, vẽ các đỉnh và cạnh, thêm trọng số.
+ matplotlib.pyplot là thư viện vẽ biểu đồ/đồ thị 2D. Ứng dụng vẽ đồ thị từ networkx rồi chuyển thành ảnh để hiển thị bằng Streamlit
```python
from collections import deque
import time
import streamlit as st
import networkx as nx
import matplotlib.pyplot as plt
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
- BFS tối ưu hơn về số lượng cạnh
![image](https://github.com/user-attachments/assets/7cbc9504-3713-4c7f-9768-f478676bfe92)

- Biểu diễn đồ thị mẫu 2 theo BFS hiển thị trên web (dùng framework Streamlit)
![image](https://github.com/user-attachments/assets/748f5f36-0fa7-4331-8a95-6b9e405ced7d)

- DFS:
![image](https://github.com/user-attachments/assets/61c6203e-be57-457e-aaf2-3db8cff88db2)
- Biểu diễn đồ thị mẫu 2 theo DFS hiển thị trên web (dùng framework Streamlit)
![image](https://github.com/user-attachments/assets/a0946538-e4a4-46a6-a412-6d45825e5560)


2. Hạn chế trên Đồ thị có Trọng số (Đồ thị mẫu 6)
- BFS và DFS vốn không quan tâm về trọng số của các cạnh. Chúng chỉ quan tâm đến cấu trúc liên kết. BFS tối ưu về số lượng cạnh, còn DFS đi theo thứ tự duyệt.
- Ở đồ thị mẫu 6: Đường đi mà BFS và DFS tìm ra vô tình tối ưu về trọng số là S -> A -> B -> E -> H với tổng trọng số là 21 và BFS tìm được đường đi ngắn nhất về số lượng cạnh là 4.
![image](https://github.com/user-attachments/assets/2c0c9a1e-d65d-4c03-93e9-b1500627468d)

- Biểu diễn đồ thị mẫu 6 theo BFS hiển thị trên web (dùng framework Streamlit)
![image](https://github.com/user-attachments/assets/5cb73eba-f8be-4bad-9a5b-35609bbdeba9)

- Biểu diễn đồ thị mẫu 6 theo DFS hiển thị trên web (dùng framework Streamlit)
![image](https://github.com/user-attachments/assets/9d920c7e-1a01-43b5-8dda-8b37b81e5e48)


- Trong bài tập, khi yêu cầu thêm một cạnh mới B -> H với trọng số 20, BFS ngay lập tức tìm ra đường đi S -> A -> B -> H. Đường này chỉ có 3 cạnh (ngắn hơn đường cũ 4 cạnh) nhưng tổng trọng số lại là 25, cao hơn và không tối ưu.
![image](https://github.com/user-attachments/assets/b85f22ad-9e8b-4c27-ac24-a9a1f5a94f1c)
-Biểu diễn đồ thị mẫu 6 theo BFS hiển thị trên web (dùng framework Streamlit)
![image](https://github.com/user-attachments/assets/344fdc37-0e96-43c1-99f5-72aeb0573c96)

- Biểu diễn đồ thị mẫu 6 theo DFS hiển thị trên web (dùng framework Streamlit)

![image](https://github.com/user-attachments/assets/d8d4db3c-4cee-484a-9ac5-ea8559566e91)

- Biểu diễn đồ thị mẫu 7 theo BFS hiển thị trên web (dùng framework Streamlit)
  
![image](https://github.com/user-attachments/assets/e4b95afc-1362-45a4-b206-260ca43a4d1e)

- Biểu diễn đồ thị mẫu 7 theo DFS hiển thị trên web (dùng framework Streamlit)

![image](https://github.com/user-attachments/assets/13a2e032-9dc8-4089-9cbf-d90165bfa270)

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
- Dùng hàm time.perf_counter() - là một hàm trong module time dùng để đo thời gian chính xác cao (tính bằng giây).
```python
start_time_bfs = time.perf_counter()
path, weight = bfs(graph6, 'S', 'H')
end_time_bfs = time.perf_counter()
```
- Kết quả phân tích:
  - Không có thuật toán nào "luôn luôn" nhanh hơn. Hiệu suất phụ thuộc rất nhiều vào cấu trúc đồ thị và vị trí của nút đích.
  - DFS thường nhanh hơn nếu đích nằm sâu trong một nhánh và thuật toán "may mắn" đi đúng nhánh đó trước.
  - BFS lại có lợi nếu đích ở gần điểm bắt đầu (độ sâu thấp), vì BFS tìm theo từng lớp và có thể gặp đích sớm hơn.
  - Output Ở đồ thị mẫu 6 (có trọng số)
![image](https://github.com/user-attachments/assets/4961f1c6-b1a9-4332-8e0c-0d6477d3274d)
  - Output Ở đồ thị mẫu 7 (không có trọng số)
![image](https://github.com/user-attachments/assets/a395f18a-eb7c-4e9f-bf87-4b4793d5dfb4)
  - Về phần bộ nhớ, thì DFS thường sẽ chiếm bộ nhớ ít hơn do khi duyệt thì nó chỉ cần lưu đường đi hiện tại dựa trên chiều sâu, còn BFS thì cần nhiều bộ nhớ hơn do phải lưu tất cả các đỉnh ở cùng mức.


5. Đồ thị mẫu 8 ( tự thiết kế đồ thị có trọng số )
- Sử dụng cả hai thuật toán BFS và DFS để tìm đường đi.
- Đồ thị:
```graph TD
S ->|2| A
S ->|5| B
S ->|4| C
A ->|4| D
A ->|7| F
B ->|8| E
B ->|3| F
B ->|6| G
C ->|10| E
C ->|3| G
D ->|5| G
E ->|2| G
E ->|15| H
F ->|5| I
G ->|4| I
I ->|3| J
H ->|6| J
```
- Kết quả của BFS và DFS ở đồ thị mẫu 8 cho thấy được là BFS tìm đường đi ngắn theo số bước; đường đi tìm được: S → A → F → I → J; tổng trọng số: 17 . Còn DFS tìm theo chiều sau; đường đi tìm được: S → A → D → G → I → J; tổng trọng số: 18
![image](https://github.com/user-attachments/assets/842cd481-bab9-4ae8-bdad-1674f2e55498)

- Biểu diễn đồ thị mẫu 8 theo BFS hiển thị trên web (dùng framework Streamlit)
![image](https://github.com/user-attachments/assets/708483bd-f936-496d-8c11-263da9837c89)

- Biểu diễn đồ thị mẫu 8 theo DFS hiển thị trên web (dùng framework Streamlit)
![image](https://github.com/user-attachments/assets/ba14a50c-0d12-40ae-8b57-62b3608fa947)
