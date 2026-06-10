[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=24112991&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** mavinhloc2003@gmail.com
**Name:** Mã Vĩnh Lộc

---

## Mô tả

Bài lab Day 10 xây dựng một pipeline ETL (Extract – Validate – Transform – Load) tự động bằng Python. Pipeline đọc dữ liệu sản phẩm thô từ file JSON, kiểm tra chất lượng dữ liệu, áp dụng các phép biến đổi nghiệp vụ, rồi lưu kết quả ra file CSV. Ngoài ra, bài lab mô phỏng một AI Agent dùng dữ liệu đã xử lý để trả lời câu hỏi, và so sánh kết quả giữa dữ liệu sạch và dữ liệu rác (garbage data) để minh họa tầm quan trọng của Data Observability.

---

## Cách chạy

### Yêu cầu
```bash
pip install pandas
```

### Chạy ETL Pipeline
```bash
python solution.py
```
Pipeline sẽ đọc `raw_data.json`, xác thực, biến đổi dữ liệu và xuất ra `processed_data.csv`.

### Chạy Agent Simulation (Stress Test)
```bash
python agent_simulation.py
```
Script này chạy thử AI Agent với hai nguồn dữ liệu:
- **Clean data** (`processed_data.csv`): dữ liệu đã qua pipeline, kết quả trả về chính xác.
- **Garbage data** (`garbage_data.csv`): dữ liệu rác, kết quả trả về vô lý (ví dụ: "Nuclear Reactor at $999999").

---

## Cấu trúc thư mục

```
├── solution.py              # ETL Pipeline chính (Extract, Validate, Transform, Load)
├── agent_simulation.py      # Mô phỏng AI Agent đọc dữ liệu đã xử lý
├── raw_data.json            # Dữ liệu đầu vào thô (5 records)
├── processed_data.csv       # Dữ liệu đầu ra sau khi qua pipeline
├── garbage_data.csv         # Dữ liệu rác dùng để stress test Agent
└── README.md                # File này
```

---

## Kết quả

### ETL Pipeline (`python solution.py`)

```
==================================================
ETL Pipeline Started...
==================================================
Extracting data from raw_data.json...
  Dropped record id=3: Price <= 0
  Dropped record id=4: Missing Category
Validation complete. Valid: 3, Errors: 2
Data saved to processed_data.csv

Pipeline completed! 3 records saved.
```

| Chỉ số | Giá trị |
|---|---|
| Tổng số records đầu vào | 5 |
| Records hợp lệ (giữ lại) | 3 |
| Records bị loại | 2 |
| Lý do loại — Price ≤ 0 | 1 (Mystery Box, id=3, price = -10) |
| Lý do loại — Category rỗng | 1 (Phone, id=4, category = "") |
| Cột được thêm sau Transform | `discounted_price` (giảm 10%), `processed_at` (timestamp) |

---

### Agent Simulation (`python agent_simulation.py`)

```
Testing with CLEAN data:
Agent: Based on my data, the best choice is Laptop at $1200.

Testing with GARBAGE data:
Agent: Based on my data, the best choice is Nuclear Reactor at $999999.
```

**Kết luận:** Khi Agent dùng dữ liệu sạch đã qua pipeline, kết quả gợi ý hợp lý (Laptop $1200). Khi dùng dữ liệu rác, Agent đưa ra gợi ý vô lý (Nuclear Reactor $999999) — minh chứng rõ ràng cho tầm quan trọng của việc kiểm soát chất lượng dữ liệu đầu vào trong các hệ thống AI.
