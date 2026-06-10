[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=24112837&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** dtrdat.work@gmail.com  
**Student ID:** 2A202600662  
**Name:** Đặng Trần Đạt

---

## Mô tả

Bài lab này xây dựng một ETL pipeline đơn giản để xử lý dữ liệu sản phẩm từ file `raw_data.json`.
Pipeline thực hiện bốn bước chính: Extract, Validate, Transform và Load ra file `processed_data.csv`.
Phần observability được thể hiện qua log số bản ghi hợp lệ, số bản ghi bị loại bỏ và cột `processed_at`
trong dữ liệu đầu ra.

Repo cũng có phần stress test để so sánh phản hồi của agent khi sử dụng dữ liệu sạch
(`processed_data.csv`) và dữ liệu rác (`garbage_data.csv`). Kết quả cho thấy chất lượng dữ liệu ảnh
hưởng trực tiếp đến độ tin cậy của câu trả lời từ AI agent.

---

## Cách chạy

### 1. Tạo và kích hoạt môi trường ảo trên Windows

```powershell
python -m venv venv
.\venv\Scripts\activate
```

Nếu PowerShell chặn script kích hoạt môi trường, chạy thêm:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\venv\Scripts\activate
```

### 2. Cài thư viện cần thiết

```powershell
pip install pandas pytest
```

### 3. Chạy ETL pipeline

```powershell
python solution.py
```

Sau khi chạy, chương trình tạo file `processed_data.csv`. Với dữ liệu mẫu hiện tại, pipeline đọc 5 bản
ghi, giữ lại 3 bản ghi hợp lệ và loại 2 bản ghi lỗi: một bản ghi có `price <= 0`, một bản ghi thiếu
`category`.

### 4. Chạy stress test Clean vs Garbage

```powershell
python generate_garbage.py
python agent_simulation.py
```

Với dữ liệu sạch, agent chọn `Laptop` trong nhóm Electronics. Với dữ liệu rác, agent chọn
`Nuclear Reactor` giá 999999 vì bị ảnh hưởng bởi outlier và dữ liệu bị nhiễu. Điều này minh họa rằng
một prompt tốt vẫn có thể tạo ra kết quả sai nếu dữ liệu đầu vào không được kiểm soát chất lượng.

### 5. Chạy test tự động

```powershell
python -m pytest -q
```

Kết quả kiểm tra local hiện tại: `9 passed`.

---

## Cấu trúc thư mục

```text
solution.py              # ETL pipeline script
raw_data.json            # Dữ liệu gốc
processed_data.csv       # Output sau khi validate và transform
generate_garbage.py      # Tạo garbage_data.csv
garbage_data.csv         # Dữ liệu rác dùng cho stress test
agent_simulation.py      # Mô phỏng agent với clean/garbage data
experiment_report.md     # Báo cáo kết quả stress test
tests/test_autograder.py # Test chấm điểm tự động
```

---

## Kết quả

- Extracted: 5 records
- Valid records kept: 3
- Dropped records: 2
- Output columns: `id`, `product`, `price`, `category`, `discounted_price`, `processed_at`
- `discounted_price` được tính bằng `price * 0.9`
- `category` được chuẩn hóa về Title Case
- `processed_at` ghi lại thời điểm xử lý dữ liệu để hỗ trợ observability
