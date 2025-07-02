# Thuật Toán Di Truyền (Genetic Algorithm)
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
---
## Mục tiêu
- Hiểu rõ nguyên lý hoạt động của thuật toán di truyền.
- Áp dụng thuật toán để giải các bài toán tối ưu hóa một biến.
- Phân tích và trực quan hóa kết quả.
- Thực hành mở rộng để nâng cao kỹ năng lập trình thuật toán tiến hóa.
---
## Tổng quan lý thuyết
Thuật toán di truyền (Genetic Algorithm - GA) là một phương pháp tìm kiếm dựa trên cơ chế tiến hóa tự nhiên, lấy cảm hứng từ thuyết tiến hóa của Darwin. GA được sử dụng để tìm lời giải gần tối ưu cho các bài toán tối ưu hóa phức tạp.
---
### Các bước cơ bản:
1. **Khởi tạo quần thể**: Tạo ngẫu nhiên các cá thể (giải pháp).
2. **Đánh giá độ thích nghi (fitness)**: Tính giá trị hàm mục tiêu.
3. **Lựa chọn (Selection)**: Chọn cá thể tốt hơn cho lai ghép.
4. **Lai ghép (Crossover)**: Kết hợp tạo cá thể con.
5. **Đột biến (Mutation)**: Thay đổi cá thể để tăng đa dạng.
6. **Lặp lại** các bước trên qua nhiều thế hệ để tìm lời giải tối ưu.

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



