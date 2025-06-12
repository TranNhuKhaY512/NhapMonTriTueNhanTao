## Các công nghệ được sử dụng
####Ngôn ngữ: Python 3
####Môi trường: Jupyter Notebook 
####Thư viện:
+ collections.deque: Cung cấp cấu trúc dữ liệu hàng đợi (queue) hiệu suất cao, là "vũ khí" không thể thiếu để triển khai BFS.
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

### 2. DFS (Depth-First Search) 
- **Nguyên lý**: Khám phá một nhánh đến tận cùng trước khi quay lại thử nhánh khác.
- **Cấu trúc dữ liệu**: Ngăn xếp (Stack) hoặc đệ quy.
- **Đặc điểm**:
  - DFS không đảm bảo đường đi ngắn nhất trong cả đồ thị không trọng số và có trọng số.
  - Kết quả phụ thuộc vào thứ tự duyệt các nút kề.

