# Hỏi đáp thủ tục đất đai (Tây Ninh)

Khung tĩnh: `index.html` + `data/answers.json`. Không gọi AI, không backend, không lưu câu hỏi người dùng.

## Nguyên tắc
- App chỉ trả lời bằng mục có trong `data/answers.json`. Không có mục khớp thì từ chối, không đoán.
- Mục chỉ được coi là đáng tin khi `status = "da_kiem_chung"`, có nguồn văn bản chính thức, người kiểm chứng và ngày hiệu lực. Mục khác hiện cảnh báo đỏ.
- Đây là hướng dẫn thủ tục, không phải tư vấn pháp lý.

## Chạy thử
Mở bằng một server tĩnh (ví dụ `python -m http.server` trong thư mục này), vì trình duyệt chặn `fetch` khi mở file trực tiếp.

## Chưa làm (cố ý)
Gọi AI, thanh toán, đăng nhập. Quyết định sau khi có tín hiệu nhu cầu thật.
