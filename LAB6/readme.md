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
<img width="2552" height="1370" alt="image" src="https://github.com/user-attachments/assets/25962793-13de-4fde-a113-5fa16c6766ea" />




