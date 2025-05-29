Dưới đây là phiên bản đã chỉnh sửa, trình bày đẹp, dễ đọc và chuyên nghiệp hơn cho file `README.md` của project **Legal Text Retrieval**:

---

````markdown
# 📚 Legal-Text-Retrieval

Dự án này nhằm mục tiêu xây dựng một hệ thống truy xuất văn bản pháp luật.  
**Đầu vào** là một đoạn văn bản liên quan đến luật pháp, **đầu ra** là nội dung đầy đủ của điều luật phù hợp nhất với đoạn đầu vào đó.

---

## 🔍 Mô tả dữ liệu

- Dataset: [Zalo AI Challenge 2021 - Legal Text Retrieval](https://www.kaggle.com/datasets/hariwh0/zaloai2021-legal-text-retrieval)
- Vì toàn bộ tập dữ liệu có kích thước lớn và việc huấn luyện mất thời gian, nên tui chỉ lấy **1000 đoạn text đầu tiên** để làm mẫu và đưa vào một list để truy vấn.

---

## 🚀 Pipeline hệ thống

### 📌 Bước 1: Tiền xử lý dữ liệu
- Mỗi mẫu dữ liệu gồm:
  - `title`: tiêu đề văn bản luật.
  - `text`: nội dung chính (đây là phần được sử dụng cho truy xuất).
- Tạo một danh sách các văn bản (`corpus`) để làm tập tài liệu.

---

### 📌 Bước 2: Hàm xử lý văn bản

Các bước xử lý văn bản bao gồm:

- ✅ Chuyển toàn bộ văn bản thành chữ thường (`lowercase`)
- ✅ Xoá khoảng trắng dư thừa (`spacing removal`)
- ✅ Xoá các từ dừng (`stopword removal`), ví dụ: "là", "thì", "và",...
- ✅ Xoá dấu câu, ký tự đặc biệt (`punctuation removal`)
- ✅ (Tuỳ chọn) Dùng **stemming** nếu làm với tiếng Anh

---

### 📌 Bước 3: Tạo từ điển (dictionary)

- Duyệt qua toàn bộ `corpus` và xây dựng một từ điển chứa **các từ duy nhất (unique)** xuất hiện trong tập dữ liệu.

---

### 📌 Bước 4: Vector hoá văn bản

Giả sử `dictionary = [a, b, c, d, e, f, g, h, i, k, l, m]`

- Text 1: `a b a c d l`  
  ➡️ Vector: `[2 1 1 1 0 0 0 0 0 0 1 0]`

- Text 2: `f g a b l m`  
  ➡️ Vector: `[1 1 0 0 0 1 1 0 0 0 1 1]`

---

### 📌 Bước 5: Xây dựng Doc-Term Matrix

Ví dụ với corpus:

```python
corpus = ['reading math books',
          'linear algebra book',
          'learn how ai learns',
          'reading books']
````

Từ điển tương ứng:

```python
dictionary = ['reading', 'math', 'books', 'linear', 'algebra', 'book', 'learn', 'how', 'learns']
```

Doc-term matrix:

|       | reading | math | books | linear | algebra | book | learn | how | learns |
| ----- | ------- | ---- | ----- | ------ | ------- | ---- | ----- | --- | ------ |
| Doc 1 | 1       | 1    | 1     | 0      | 0       | 0    | 0     | 0   | 0      |
| Doc 2 | 0       | 0    | 0     | 1      | 1       | 1    | 0     | 0   | 0      |
| Doc 3 | 0       | 0    | 0     | 0      | 0       | 0    | 1     | 1   | 1      |
| Doc 4 | 1       | 0    | 1     | 0      | 0       | 0    | 0     | 0   | 0      |

---

### 📌 Bước 6: Tính Cosine Similarity

Dùng công thức:

```latex
$$
\text{cosine\_similarity}(\mathbf{A}, \mathbf{B}) = \frac{\mathbf{A} \cdot \mathbf{B}}{\|\mathbf{A}\| \cdot \|\mathbf{B}\|} = \frac{\sum_{i=1}^n A_i B_i}{\sqrt{\sum_{i=1}^n A_i^2} \cdot \sqrt{\sum_{i=1}^n B_i^2}}
$$
```

> Trong đó:
>
> * $\mathbf{A} \cdot \mathbf{B}$: là tích vô hướng.
> * $\|\mathbf{A}\|$: là chuẩn Euclid của vector $A$
> * $n$: là số chiều của vector

---

### 📌 Bước 7: Truy vấn và đánh giá

* Tạo một hàm truy vấn để so sánh vector của input query với các văn bản trong corpus.
* Tính cosine similarity giữa query và từng văn bản.
* Trả về **Top-K** văn bản có độ tương đồng cao nhất.

---

## ✅ Kết luận

Dự án này là một baseline đơn giản cho hệ thống **Legal Text Retrieval**.
Nó có thể được mở rộng bằng các kỹ thuật nâng cao như:

* TF-IDF Vectorizer
* Embedding (Word2Vec, BERT)
* FAISS for fast nearest neighbor search
* Reranking bằng mô hình ngôn ngữ

---

## 📂 Dữ liệu

Dataset gốc:
👉 [https://www.kaggle.com/datasets/hariwh0/zaloai2021-legal-text-retrieval](https://www.kaggle.com/datasets/hariwh0/zaloai2021-legal-text-retrieval)

```

---
