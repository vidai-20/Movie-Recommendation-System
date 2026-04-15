# Collaborative Filtering – Movie Recommendation System

Hệ thống gợi ý phim sử dụng **User-based Collaborative Filtering** với KNN, đánh giá bằng RMSE, MAE, Precision@K và Recall@K. Hỗ trợ xử lý batch, resume checkpoint và lưu toàn bộ ma trận trung gian.

---

## Tổng quan

Dự án xây dựng hệ thống gợi ý phim dựa trên hành vi đánh giá của người dùng. Hệ thống phân tích độ tương đồng giữa các người dùng (User-based CF) để dự đoán rating cho những bộ phim họ chưa xem, từ đó đưa ra gợi ý phù hợp.

---

## Tính năng

- Xây dựng ma trận utility từ dữ liệu rating thực tế
- Tính độ tương đồng cosine giữa người dùng (mean-centered)
- Dự đoán rating bằng KNN weighted average
- Đánh giá mô hình với RMSE, MAE, Precision@K, Recall@K
- Hỗ trợ xử lý batch theo từng nhóm người dùng
- Resume từ checkpoint, tránh tính toán lại từ đầu
- Lưu toàn bộ ma trận trung gian ra file CSV

---

## Dữ liệu đầu vào

| File | Các cột |
|---|---|
| `movies.csv` | `MovieID`, `Title`, `Genres` |
| `ratings.csv` | `UserID`, `MovieID`, `Rating` |

---

## Luồng xử lý

1. Tải và chuẩn hóa dữ liệu từ `movies.csv` và `ratings.csv`
2. Với mỗi người dùng: chia tập train/test theo tỉ lệ 80/20
3. Xây dựng ma trận utility, chuẩn hóa và tính độ tương đồng
4. Điền các ô còn thiếu bằng KNN weighted average
5. Dự đoán rating cho tập test
6. Tính Precision@K, Recall@K và lưu kết quả

---

## Tham số chính

| Tham số | Mặc định | Mô tả |
|---|---|---|
| `--k_neighbors` | `2` | Số láng giềng gần nhất |
| `--like_threshold` | `3.5` | Ngưỡng đánh giá "thích" |
| `--k_at` | `10` | K trong Precision@K / Recall@K |
| `--limit` | `300` | Số người dùng xử lý mỗi lần chạy |
| `--resume` | `False` | Bỏ qua user đã xử lý |

---

## Cấu trúc thư mục đầu ra

```
Ma_tran_ban_dau/        ← Các ma trận utility theo từng user
extend_train_test/      ← Tập train/test đã mở rộng
Du_doan_trong_test/     ← Kết quả dự đoán rating
evaluate/               ← Precision, Recall và trung bình toàn bộ
```
