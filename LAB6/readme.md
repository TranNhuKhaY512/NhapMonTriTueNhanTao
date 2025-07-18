# THỰC HÀNH MÔN NHẬP MÔN TRÍ TUỆ NHÂN TẠO
## **NAIVE BAYES**
### Sinh viên thực hiện: Trần Như Khả Ý
### MSSV : 2374802010582
### GVHD : Nguyễn Thái Anh
---
### Các công nghệ sử dụng
```python
import numpy as np
import pandas as pd
import matplotlib as plt
from sklearn.naive_bayes import BernoulliNB
from sklearn.naive_bayes import MultinomialNB
from sklearn.feature_extraction.text import CountVectorizer
```
- `numpy`: Hỗ trợ tính toán mảng số học hiệu suất cao.
- `pandas`: Dùng để thao tác và phân tích dữ liệu dạng bảng (DataFrame).
- `matplotlib.pyplot`: Tạo biểu đồ và trực quan hóa dữ liệu.
- `sklearn.naive_bayes.BernoulliNB`: Mô hình Naive Bayes dùng cho dữ liệu nhị phân.
- `sklearn.naive_bayes.MultinomialNB`: Mô hình Naive Bayes dùng cho dữ liệu rời rạc như đếm từ.
- `sklearn.feature_extraction.text.CountVectorizer`: Chuyển văn bản thành vector đặc trưng dựa trên tần suất từ.

### Exercise 1: Phân phối Bernoulli và Multinomial
- Dự đoán cảm xúc của văn bản là tích cực hay tiêu cực.
- Code chính:
  - Khởi tạo hàm load_data() để đọc dữ liệu từ file Education.csv
```python
def load_data():
    return pd.read_csv('Education.csv')
```
  - Khởi tạo hàm split_train_test(): chia dữ liệu thành tập train và test
```python
def split_train_test(data, ratio_test=0.2): #0.2 là tỷ lệ dữ liệu cho tập test
    shuffled = data.sample(frac=1, random_state=42).reset_index(drop=True)
    test_size = int(len(shuffled) * ratio_test) #Tính số mẫu sẽ dùng cho tập test
    return shuffled[:-test_size], shuffled[-test_size:]
```
  - Khởi tạo hàm train_model(): huấn luyện mô hình phân loại văn bản
```python
def train_model():
    data = load_data()
    train_set, test_set = split_train_test(data) #Chia dữ liệu thành hai phần: huấn luyện (train) và kiểm tra (test).
    X_train = train_set['Text']
    y_train = train_set['Label'].map({"positive": 1, "negative": 0})
    vectorizer = CountVectorizer(binary=True, stop_words='english')
    X_train_vec = vectorizer.fit_transform(X_train)
    model = BernoulliNB().fit(X_train_vec, y_train) # huấn luyện mô hình sử dụng Bernoulli (phù hợp với dữ liệu nhị phân) trên tập train.
    return model, vectorizer, test_set
```
- Giao diện streamlit :
- Câu lệnh run : streamlit run "filenam".py
- Giao diện người dùng nhập liệu :
  ```python
  user_input = st.text_area("Nhập văn bản", height=150, placeholder="Ví dụ: The environment here is very supportive and encouraging.")
  ```
  <img width="1355" height="401" alt="image" src="https://github.com/user-attachments/assets/90e09d93-f340-421b-b3b7-bac82a5bc43a" />

- Ví dụ: cho nhập vào dữ liệu "Teacher tenure policies aim to protect educators, but they also hinder accountability" từ file Education.csv -> submit thì sẽ hiện kết quả như hình bên dưới gồm kết quả (positive / negative), độ tin cậy, đường cong ROC và biểu đồ phân hóa xác suất.
- Phân bố xác suất: xác suất của positve và negative của văn bản
- Đường cong ROC: 
  - Trục y là TPR (True Positive Rate/ Recall), trục x là FPR (False Positive Rate).
  - FPR là tỉ lệ các mẫu Negative bị phân loại sai thành Positive,
<img width="2560" height="1440" alt="image" src="https://github.com/user-attachments/assets/1fbadbf4-27a5-4f52-a99c-8750b15ed615" />

### Exercise 2: Áp dụng thuật toán Naive Bayes (phân phối Gaussian) để dự đoán kết quả loại thuốc phù hợp với bệnh nhân.
- Code chính:
  - Khởi tạo hàm load_data() để đọc dữ liệu từ file Education.csv
```python
def load_data():
    return pd.read_csv("drug200.csv")
```
  - Huấn luyện mô hình:
```python
X = encoded_data.drop('Drug', axis=1)
y = encoded_data['Drug']

model = GaussianNB()
model.fit(X, y)
```
- Giao diện streamlit :
- Câu lệnh run : streamlit run "filenam".py
- Giao diện người dùng nhập liệu:
```python
     st.markdown("<div class='info-label'>TUỔI</div>", unsafe_allow_html=True)
            age_input = st.text_input("", value="30")
            st.markdown("<div class='info-label'>GIỚI TÍNH</div>", unsafe_allow_html=True)
            sex = st.radio("", ['M', 'F'])
            st.markdown("<div class='info-label'>HUYẾT ÁP</div>", unsafe_allow_html=True)
            bp = st.radio("", ['LOW', 'NORMAL', 'HIGH'])
            st.markdown("<div class='info-label'>CHOLESTEROL</div>", unsafe_allow_html=True)
            cholesterol = st.radio("", ['NORMAL', 'HIGH']
            st.markdown("<div class='info-label'>TỶ LỆ Na/K</div>", unsafe_allow_html=True)
            na_to_k_input = st.text_input("", value="15.0")
```
<img width="520" height="640" alt="image" src="https://github.com/user-attachments/assets/236b5a26-6ffb-46aa-8288-3cb4c7aa9caa" />

- Ví dụ: cho người dùng điền đầy đủ các mục lấy dữ liệu từ file drug200.csv -> submit thì sẽ hiện kết quả như hình bên dưới gồm kết quả loại thuốc dự đoán và biểu đồ xác suất dự đoán xác suất cho từng loại thuốc. 
<img width="2470" height="1250" alt="image" src="https://github.com/user-attachments/assets/c213808c-2863-4b28-b287-7c78736f8f49" />










