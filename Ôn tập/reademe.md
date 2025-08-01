
# Thực hành:  Thi thử
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
  - `pandas`: Dùng để thao tác và phân tích dữ liệu dạng bảng (DataFrame).
  - `torch`: Hỗ trợ xây dựng và huấn luyện mô hình học sâu bằng tensor và GPU.
  - `torch.optim`: Cung cấp các thuật toán tối ưu hóa trọng số mô hình như SGD, Adam.
  - `torchvision`: Cung cấp dữ liệu ảnh, mô hình sẵn có và công cụ xử lý ảnh cho PyTorch.
  - `torchvision.transforms`: Thực hiện các phép biến đổi ảnh như chuyển tensor, chuẩn hóa.
  - `collections`: Sử dụng deque để quản lý hàng đợi hai đầu trong các thao tác lưu trữ trạng thái
  - `train_test_split`: Tách dữ liệu thành tập huấn luyện và kiểm tra.
  - `CategoricalNB`: Thuật toán Naive Bayes cho dữ liệu rời rạc (categorical).
  - `classification_report`: Đánh giá mô hình bằng các chỉ số precision, recall, F1.
  - `accuracy_score`: Tính độ chính xác của mô hình.
  - `LabelEncoder`: Mã hóa dữ liệu nhãn từ dạng chữ sang số.
  ---
## Các thuật toán sử dụng:
### 1. Tìm kiếm theo chiều sâu (DFS) : tìm kiếm theo chiều sâu được ứng dụng trên đồ thị có trọng số và đồ thị không trọng số.
- **Nguyên lý**: Khám phá một nhánh đến tận cùng trước khi quay lại thử nhánh khác.
- **Cấu trúc dữ liệu**: Ngăn xếp (Stack) hoặc đệ quy.
- **Đặc điểm**:
  - DFS không đảm bảo đường đi ngắn nhất trong cả đồ thị không trọng số và có trọng số. Bên cạnh đó, nó có thể bị kẹt trong vòng lặp vô hạn trên đồ thị có chu trình nếu không sử dụng tập visited.
  - Kết quả phụ thuộc vào thứ tự duyệt các nút kề.
- Code chính:
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
---
### 2. Tối ưu hóa hàm 1 biến (thuật toán di truyền) 
 - Thuật toán di truyền (Genetic Algorithm - GA) là một phương pháp tìm kiếm dựa trên cơ chế tiến hóa tự nhiên, lấy cảm hứng từ thuyết tiến hóa của Darwin. GA được sử dụng để tìm lời giải gần tối ưu cho các bài toán tối ưu hóa phức tạp.

### Các bước cơ bản:
1. **Khởi tạo quần thể**: Tạo ngẫu nhiên các cá thể (giải pháp).
2. **Đánh giá độ thích nghi (fitness)**: Tính giá trị hàm mục tiêu.
3. **Lựa chọn (Selection)**: Chọn cá thể tốt hơn cho lai ghép.
4. **Lai ghép (Crossover)**: Kết hợp tạo cá thể con.
5. **Đột biến (Mutation)**: Thay đổi cá thể để tăng đa dạng.
6. **Lặp lại** các bước trên qua nhiều thế hệ để tìm lời giải tối ưu.
---
#### Sử dụng Tournament Selection : Chọn ngẫu nhiên một nhóm nhỏ cá thể, sau đó chọn cá thể tốt nhất trong nhóm
- Lấy ngẫu nhiên **k** cá thể
- So sánh độ thích nghi của chúng
- Chọn cá thể có fitness cao nhất
- Có thể điều chỉnh mức độ cạnh tranh bằng cách thay đổi **k** (kích thước giải đấu).  Nếu **k** lớn thì mức chọn lọc sẽ cao hơn (ít cơ hội cho cá thể yếu), còn nếu **k** nhỏ thì mức độ ngẫu nhiên sẽ cao hơn (giữ được đa dạng).
- Ưu điểm:
  - Dễ cài đặt
  - Kiểm soát tốt mức độ chọn lọc
  - Không bị ảnh hưởng nhiều bởi cá thể có fitness cực cao
- Nhược điểm:
  - Nếu **k** quá lớn, sẽ làm giảm sự đa dạng, dễ rơi vào tối ưu cục bộ

### Một số hàm quan trọng:
- `initialize_population()`: Khởi tạo quần thể ban đầu ngẫu nhiên.
- `select_parents()`: Lựa chọn cá thể theo Tournament Selection hoặc Roulette Wheel.
- `crossover()`: Lai ghép 2 cá thể.
- `mutate()`: Đột biến cá thể.
- `genetic_algorithm_example1()`: Hàm chính thực hiện GA, lặp qua nhiều thế hệ.
---
### 3. Sử dụng CNN để phân loại dữ liệu chó và mèo :
### Một số hàm quan trọng:
 1. __init__ (trong class DogsCatsCNN)
- Vai trò: Khởi tạo các tầng của mô hình CNN:
- conv1: tầng tích chập đầu tiên (1→16 kênh)
- conv2: tầng tích chập thứ hai (16→32 kênh)
- pool: tầng giảm kích thước (max pooling)
- fc1: tầng fully connected (ẩn → 10 lớp)
2. forward(x): Định nghĩa luồng dữ liệu từ đầu vào → ra dự đoán
  ```python
    x = self.pool(torch.relu(self.conv1(x)))  # Conv1 -> ReLU (loại giá trị âm) -> Pool (giảm kích thước)
        x = self.pool(torch.relu(self.conv2(x)))  # Conv2 -> ReLU -> Pool, cuối cùng ra 32x5x5
        x = x.view(-1, 32 * 14 * 14) # Duỗi tensor thành vector, -1 tự động tính batch size
        x = self.fc1(x) # Qua tầng fully connected, ra 10 giá trị (logits cho 0-9)
        return x   # Trả về kết quả dự đoán

  ```
3. visualize_prediction(): Vẽ 5 ảnh đầu tiên từ tập kiểm tra và hiển thị
4. visualize_feature_map()
Vai trò: Trực quan hóa:
- Ảnh gốc (ví dụ ảnh số 4)
- Feature map của tầng conv1 (2 kênh đầu)
- Dùng để hiểu CNN học được gì sau lớp tích chập đầu tiên.
5. Vòng lặp huấn luyện:
- Vai trò: Huấn luyện mô hình (trong 10 lần,... - epoch).
- Trong đó:
  - loss.backward() và optimizer.step() là bước cập nhật trọng số.
  - loss_values, accuracy_values lưu lại quá trình huấn luyện để vẽ biểu đồ.
6. Đánh giá mô hình:
```python
with torch.no_grad():
```
- Vai trò: Đánh giá độ chính xác mô hình trên tập test.
- Không tính gradient để tiết kiệm tài nguyên.
 7. Vẽ biểu đồ Loss & Accuracy
Vai trò: Hiển thị quá trình học theo thời gian (qua các epoch).
---
### 4. Sử dụng Navie Bayes:
- Dữ liệu từ bảng đã cho:
```python
# Dữ liệu từ bảng đề bài
data = pd.DataFrame({
    'Outlook': ['Sunny', 'Sunny', 'Overcast', 'Rainy', 'Rainy', 'Rainy', 'Overcast',
                'Sunny', 'Sunny', 'Rainy', 'Sunny', 'Overcast', 'Overcast', 'Rainy'],
    'Temperature': ['Hot', 'Hot', 'Hot', 'Mild', 'Cool', 'Cool', 'Cool',
                    'Mild', 'Cool', 'Mild', 'Mild', 'Mild', 'Hot', 'Mild'],
    'Humidity': ['High', 'High', 'High', 'High', 'Normal', 'Normal', 'Normal',
                 'High', 'Normal', 'Normal', 'Normal', 'High', 'Normal', 'High'],
    'Wind': [False, True, False, False, False, True, True,
             False, False, False, True, True, False, True],
    'Play': ['No', 'No', 'Yes', 'Yes', 'Yes', 'No', 'Yes',
             'No', 'Yes', 'Yes', 'Yes', 'Yes', 'Yes', 'No']
})
data.tail(14)
```
- Xử lý dữ liệu:
  - One-hot encoding các cột đầu vào (X) chứa dữ liệu phân loại.
  - Label encoding cột đầu ra (y) với giá trị Yes/No.
```python
X = data.drop('Play', axis=1)
y = data['Play']

# Mã hóa các cột dạng chữ (categorical) -> số nguyên
X_encoded = pd.get_dummies(X, dtype=int)  # One-hot encoding
le = LabelEncoder()
y_encoded = le.fit_transform(y)  # "No" -> 0, "Yes" -> 1
```
- Tách tập dữ liệu thành train/test (80% / 20%).
```python
# Chia dữ liệu 
X_train, X_test, y_train, y_test = train_test_split(X_encoded, y_encoded, test_size=0.2, random_state=42)
```
- Huấn luyện mô hình Categorical Naive Bayes.
```python
# Huấn luyện mô hình
model = CategoricalNB()
model.fit(X_train, y_train)
```
- Dự đoán trên tập test.
```python
# Dự đoán
y_pred = model.predict(X_test)
```
- Đánh giá mô hình với độ chính xác và báo cáo phân loại.
```python
# Đánh giá mô hình
print("Độ chính xác:", accuracy_score(y_test, y_pred))
print("\nClassification Report:\n", classification_report(y_test, y_pred, target_names=le.classes_))
```
- Hiển thị kết quả so sánh giữa giá trị thực tế và dự đoán.
```python
print("X_test:\n", X_test)
print("Dự đoán:", le.inverse_transform(y_pred))
print("Thực tế: ", le.inverse_transform(y_test))
```
- Kết quả đầu ra: 
  - Độ chính xác của mô hình trên tập kiểm tra.
  - Báo cáo phân loại (Precision, Recall, F1-score).
  - Giá trị đầu vào X_test, dự đoán của mô hình, và giá trị thực tế tương ứng.

## Hướng dẫn chạy
1. Mở file Jupyter Notebook:  
   `2374802010582_TranNhuKhaY.ipynb`
2. Chạy lần lượt từng cell để xem kết quả.


