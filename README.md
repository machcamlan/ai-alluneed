# Sổ tay thủ tục và đời sống (Tây Ninh)

Trang web tĩnh: xem từng bước làm một thủ tục (làm gì, ở đâu, giấy tờ, chi phí, thời gian), kèm thời tiết, giá cả, lịch điện nước và nhắc hạn giấy tờ. Không backend, không gọi AI, không lưu câu hỏi của người dùng.

## Tệp
- `index.html`: giao diện và logic.
- `data/answers.json`: các chủ đề thủ tục.
- `data/prices.json`: bảng giá nhập tay.
- `data/local.json`: khu vực thời tiết, liên kết điện nước, loại giấy tờ nhắc hạn.
- `HANDOFF.md`: trạng thái, quyết định, việc còn dở. **Đọc trước khi làm tiếp.**

## Nguyên tắc
- Không có dữ liệu thì ghi «Chưa có dữ liệu» hoặc từ chối, không đoán.
- Mục chỉ đáng tin khi `status = "da_kiem_chung"` kèm người kiểm chứng và ngày. Hiện **tất cả mục là bản nháp chưa kiểm chứng**.
- Đây là hướng dẫn tham khảo, không phải tư vấn pháp lý, y tế hay tài chính.

## Chạy thử
Mở terminal trong thư mục này và chạy `python -m http.server 8000`, rồi vào `http://localhost:8000`. Mở file trực tiếp sẽ không tải được dữ liệu.

## Nguồn dữ liệu thời tiết
[Open-Meteo.com](https://open-meteo.com/), giấy phép CC BY 4.0, chỉ dùng cho mục đích phi thương mại.
