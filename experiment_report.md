# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** 2A202600662  
**Name:** Đặng Trần Đạt  
**Date:** 2026-06-10

---

## 1. Kết quả thí nghiệm

Mình chạy `agent_simulation.py` với 2 bộ dữ liệu: clean data từ `processed_data.csv` và garbage data từ
`garbage_data.csv`.

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | Agent: Based on my data, the best choice is Laptop at $1200.0. | 9 | Dữ liệu đã được validate, category được chuẩn hóa và các bản ghi lỗi đã bị loại bỏ. Agent tìm đúng nhóm Electronics và chọn sản phẩm có giá cao nhất một cách hợp lý. |
| Garbage Data (`garbage_data.csv`) | Agent: Based on my data, the best choice is Nuclear Reactor at $999999. | 2 | Dữ liệu có duplicate ID, sai kiểu dữ liệu, null value và outlier rất lớn. Agent bị outlier làm lệch kết quả nên đưa ra gợi ý không phù hợp. |

---

## 2. Phân tích & nhận xét (Phan tich)

### Tại sao Agent trả lời sai khi dùng Garbage Data?

Agent trả lời sai khi dùng Garbage Data vì logic của agent tin trực tiếp vào bảng dữ liệu đầu vào. File
garbage có nhiều vấn đề chất lượng dữ liệu như duplicate ID, giá trị `price` sai kiểu `"ten dollars"`,
giá trị rỗng, category bị thiếu và một outlier rất lớn là `Nuclear Reactor` có giá 999999 trong nhóm
electronics. Khi agent lọc các sản phẩm electronics rồi chọn item có `price` cao nhất, nó không tự biết
rằng đây là một bản ghi bất thường.

Nếu không có bước validate, clean và giám sát chất lượng dữ liệu trước khi truy vấn, agent có thể bị data
poisoning làm sai lệch kết quả. Điều này cho thấy prompt tốt không đủ để sửa dữ liệu xấu. Pipeline cần
kiểm tra schema, kiểu dữ liệu, giá trị bất thường, duplicate ID và missing values trước khi đưa dữ liệu
cho AI sử dụng. Trong bài lab này, cùng một câu hỏi nhưng clean data tạo ra kết quả hợp lý, còn garbage
data làm agent chọn một sản phẩm phi thực tế.

---

## 3. Kết luận

**Quality Data > Quality Prompt?** Mình đồng ý. Prompt rõ ràng giúp agent hiểu yêu cầu, nhưng nếu dữ liệu
đầu vào sai thì agent vẫn có thể trả lời sai một cách tự tin. Chất lượng dữ liệu và observability là nền
tảng quan trọng để xây dựng ứng dụng AI đáng tin cậy.
