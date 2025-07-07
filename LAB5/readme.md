# Thực hành:  Convolutional Neural Network (CNN)
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
  - `random`: Sinh số ngẫu nhiên cho lai ghép và đột biến.
  - `math`: Hỗ trợ thực hiện các phép toán học.
  - `torch`: Hỗ trợ xây dựng và huấn luyện mô hình học sâu bằng tensor và GPU.
  - `torch.optim`: Cung cấp các thuật toán tối ưu hóa trọng số mô hình như SGD, Adam.
  - `torchvision`: Cung cấp dữ liệu ảnh, mô hình sẵn có và công cụ xử lý ảnh cho PyTorch.
  - `torchvision.transforms`: Thực hiện các phép biến đổi ảnh như chuyển tensor, chuẩn hóa.
---
## Mục tiêu
- Hiểu rõ vai trò và cơ chế hoạt động của từng thành phần trong CNN.
- Biết cách sử dụng tích chập, ReLU, pooling để xử lý ảnh.
- Biết áp dụng CNN để trích xuất đặc trưng và phân loại ảnh.
- Có thể giải thích quy trình nhận diện ảnh số đơn giản như “7” và các số khác (chữ viết tay) bằng CNN.
---
## Tổng quan lý thuyết
## 1. CNN là gì?
CNN (Convolutional Neural Network) là một loại mạng nơ-ron nhân tạo giúp máy tính "nhìn" và hiểu ảnh, tương tự cách con người nhận diện vật thể trong đời thực. Thay vì xem toàn bộ ảnh một lúc như mạng nơ-ron thông thường (fully connected), CNN chia nhỏ ảnh ra, tìm các đặc trưng như đường thẳng, góc, vòng tròn, rồi ghép lại để đoán xem ảnh đó là gì.
**Ví dụ**: Khi ta nhìn một con mèo, không cần xem hết cả ảnh ngay lập tức. Ta nhận ra tai mèo (hình tam giác), mắt mèo (hình tròn), ria mèo (đường thẳng), rồi kết luận "Đây là mèo". CNN cũng làm như vậy bằng cách dùng các "kính lúp" nhỏ quét qua ảnh từng phần một.
---
## 2. Các thành phần chính của CNN
### 2.1. Tầng tích chập (Convolution Layer)
Đây là bước quan trọng nhất, giống như "đôi mắt" của CNN, giúp tìm các đặc trưng nhỏ trong ảnh như cạnh, góc, hoặc đường cong.
#### Ý tưởng cơ bản
- Chúng ta có một ảnh, giả sử kích thước là $6 \times 6$ pixel.
- Dùng một **bộ lọc** (filter/kernel), ví dụ $3 \times 3$, như một "kính lúp" nhỏ để quét qua ảnh.
- Kết quả là một **feature map** (bản đồ đặc trưng), cho biết chỗ nào trong ảnh có đặc trưng mà bộ lọc tìm được.
#### Công thức tích chập
Công thức toán học của tích chập là:
```math
S(i, j) = \sum_{m=0}^{F-1} \sum_{n=0}^{F-1} I(i+m, j+n) \cdot K(m, n)
```
- `I`: Ảnh đầu vào (input image).
- `K`: Bộ lọc (kernel/filter).
- `F`: Kích thước bộ lọc (ví dụ $F=3$ nếu là $3 \times 3$).
- `S(i, j)`: Giá trị tại vị trí $(i, j)$ trong feature map.

### 2.2. Hàm kích hoạt (ReLU)
- Sau khi có feature map từ tầng tích chập, ta dùng hàm ReLU để "lọc" nó, giữ lại các đặc trưng rõ ràng và loại bỏ những phần không quan trọng.
#### Công thức
$$ \text{ReLU}(x) = \max(0, x) $$

**Giải thích đơn giản**:
- Nếu số lớn hơn 0, giữ nguyên.
- Nếu số nhỏ hơn hoặc bằng 0, biến thành 0.
### 2.Các bước cơ bản:
1. **Tích chập**: Tìm đặc trưng như đường ngang → Feature map $S$.
2. **ReLU**: Lọc bỏ các nét mờ (giá trị âm) → Feature map $S_relu$.
3. **Pooling**: Tóm tắt, giảm kích thước → Feature map $S_pooled$.
4. **Fully Connected**: Ghép các đặc trưng, dự đoán.

### 3. Ứng dụng thực tế
CNN được dùng trong:
- **Nhận diện khuôn mặt**: Facebook dùng CNN để gắn thẻ bạn bè trong ảnh.
- **Xe tự lái**: Phát hiện biển báo, người đi bộ qua camera.
- **Y khoa**: Phân tích ảnh X-quang để tìm bệnh.
- ...
---
### Có 2 phương pháp để lựa chọn cá thể tốt cho lai ghép là Tournament Selection hoặc Roulette Wheel.
#### Tournament Selection : Chọn ngẫu nhiên một nhóm nhỏ cá thể, sau đó chọn cá thể tốt nhất trong nhóm
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
    
#### Roulette Wheel: Xác suất chọn cá thể tỉ lệ với độ thích nghi (fitness)
- Tổng hợp fitness toàn quần thể
- Mỗi cá thể chiếm một phần tương ứng trên "vòng quay"
- Quay ngẫu nhiên để chọn
- Cá thể có fitness càng cao thì chiếm diện tích càng lớn trên vòng quay → xác suất được chọn càng cao.
- Ưu điểm:
  - Giữ được tính ngẫu nhiên cao
  - Cá thể yếu vẫn có cơ hội được chọn (có lợi cho duy trì đa dạng di truyền)
- Nhược điểm:
  - Nếu có một cá thể fitness vượt trội, nó sẽ chi phối toàn bộ quá trình chọn, dẫn đến mất đa dạng
  - Tính toán phức tạp hơn một chút vì phải chuẩn hóa và tính xác suất
---
## Nội dung thực hành
Trong lab này triển khai một thuật toán GA để tối ưu hàm bậc hai đơn biến,đa biến và hàm lượng giác :

```python
fitness_function(x) = -(x**2) + 10*x + 50
---
fitness_function(x) = -(x**2 + y**2)
---
fitness_function(x) = math.sin(x) + math.cos(x)
```

### Một số hàm quan trọng:
- `initialize_population()`: Khởi tạo quần thể ban đầu ngẫu nhiên.
- `select_parents()`: Lựa chọn cá thể theo Tournament Selection hoặc Roulette Wheel.
- `crossover()`: Lai ghép 2 cá thể.
- `mutate()`: Đột biến cá thể.
- `genetic_algorithm_example1()`: Hàm chính thực hiện GA, lặp qua nhiều thế hệ.

### Trực quan hóa:
- Dùng thư viện `matplotlib` để vẽ biểu đồ quá trình tối ưu.
- Theo dõi sự thay đổi giá trị tốt nhất qua các thế hệ.
---

## Hướng dẫn chạy
1. Mở file Jupyter Notebook:  
   `2374802010582_TranNhuKhaY.ipynb`
2. Chạy lần lượt từng cell để xem kết quả.
3. Kết quả cuối cùng sẽ hiển thị:
   - Giá trị x tối ưu.
   - Hàm mục tiêu đạt giá trị cực đại.
   - Đồ thị quá trình tiến hóa.

---
## Ghi chú thêm
- Có thể điều chỉnh các thông số như:
  - `population_size`
  - `mutation_rate`
  - `crossover_rate`
  - `generation`  
  Để kiểm tra hiệu quả của GA trong các điều kiện khác nhau.
---



