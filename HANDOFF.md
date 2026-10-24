# Handoff: Sổ tay thủ tục và đời sống

Cập nhật: 2026-10-24. Dành cho phiên làm việc sau (người hoặc AI) đọc để làm tiếp mà không phải hỏi lại từ đầu.

## 1. Dự án là gì
Một trang web **tĩnh** (HTML/JS, không backend, không gọi AI) giúp người dân ở Tây Ninh (gồm Long An cũ sau sáp nhập) xem **từng bước** làm một việc: làm gì, ở đâu/phòng ban nào, giấy tờ, chi phí, thời gian. Kèm vài tiện ích hằng ngày: thời tiết, giá cả, lịch điện nước, nhắc hạn giấy tờ.

Repo: https://github.com/machcamlan/ai-alluneed (công khai, nhánh `main`).

## 2. Cấu trúc
| Tệp | Vai trò |
|---|---|
| `index.html` | Toàn bộ giao diện và logic (một tệp, vanilla JS, router theo `#/...`). |
| `data/answers.json` | Các chủ đề thủ tục (11 mục). Mỗi mục có các bước, chi phí, nguồn, trạng thái kiểm chứng. |
| `data/prices.json` | Bảng giá nhập tay (xăng dầu, vàng, nông sản), mỗi dòng có ngày số liệu và nguồn. |
| `data/local.json` | Danh sách khu vực thời tiết, liên kết điện/nước, loại giấy tờ nhắc hạn. |
| `README.md` | Giới thiệu ngắn và cách chạy. |

Chạy thử: mở terminal trong thư mục dự án, chạy `python -m http.server 8000`, vào `http://localhost:8000`. Mở file trực tiếp sẽ không tải được dữ liệu (trình duyệt chặn `fetch`).

## 3. Nguyên tắc thiết kế (đừng phá)
- **Fail-closed:** không có dữ liệu thì ghi «Chưa có dữ liệu» hoặc từ chối trả lời. Không bịa, không đoán.
- Mọi mục thủ tục có `status`. Chỉ `da_kiem_chung` mới được coi là đáng tin; mục khác hiện cảnh báo đỏ. **Hiện chưa mục nào được kiểm chứng.**
- Mỗi bước có đủ các trường `what, where, documents, cost, time, details, caveat`. Trường trống = chưa có nguồn.
- Phân biệt `kind: thu-tuc` (có quy định) và `kind: kinh-nghiem` (gợi ý chung, không có nguồn: mua nệm, giường, đồ gia dụng).
- Không lưu câu hỏi hay dữ liệu người dùng. Mục Nhắc hạn chỉ dùng `localStorage`.
- Mọi dữ liệu pháp lý ghi rõ nguồn là thứ cấp hay gốc, và ngày AI tra cứu (`checkedOn`).

## 4. Trạng thái nội dung
**Thủ tục (bản nháp, chưa kiểm chứng):** sang tên sổ đỏ; tách thửa Tây Ninh (Quyết định 139/2026/QĐ-UBND, hiệu lực 29/09/2026); thừa kế đất; trợ cấp thất nghiệp; rút BHXH một lần; khám bệnh bằng thẻ BHYT; mua xe máy mới (đăng ký biển số); mua vàng; mua nệm, giường, đồ gia dụng (gợi ý chung).

**Chỗ yếu nhất, cần xử lý trước khi chia sẻ rộng:**
- Tách thửa: nội dung lấy từ **bản chép trên LuatVietnam**, không phải Công báo. Có đoạn thiếu, trong đó Điều 17 (điều khoản chuyển tiếp) chưa đọc được. Cần đối chiếu bản gốc.
- Thất nghiệp: phần danh mục hồ sơ và thông báo hằng tháng lấy từ trang dùng quy định **cũ** (Luật Việc làm 2013, Nghị định 28/2015). Mức hưởng và thời điểm hưởng theo Luật Việc làm 2025.
- Khám BHYT: có báo quy định đổi từ 07/2026 nhưng không tìm được nguồn tin cậy; mọi mức hưởng cần xác minh.
- Xe máy: chưa biết Tây Ninh thuộc khu vực lệ phí nào; chưa xác minh lệ phí trước bạ và việc miễn từ 15/08/2026.
- Phần lớn ô «Chi phí», «Giấy tờ», «Thời gian» đang trống vì chưa có nguồn. Cách lấp tốt nhất: hỏi trực tiếp Văn phòng đăng ký đất đai, văn phòng công chứng, cơ quan BHXH ở Tây Ninh.

**Tiện ích:**
- Thời tiết: gọi trực tiếp Open-Meteo (không cần khóa, có CORS). Giấy phép miễn phí chỉ cho **phi thương mại**; có quảng cáo hoặc thu phí thì phải đổi gói trả phí. Đã ghi nguồn CC BY 4.0 trên trang.
- Giá cả: nhập tay. Giá heo hơi bị bỏ vì bài tìm được đăng năm 2025 dù đề ngày 24/10. Giá xăng các nguồn lệch nhau, đã ghi cảnh báo. Gạo bán lẻ, trứng, rau: chưa có số liệu.
- Điện nước: chỉ có liên kết tới trang chính thức, không chép lịch. Chưa xác minh khu vực Long An cũ.
- Nhắc hạn: chỉ bằng lái có quy tắc (A1/A/B1 không thời hạn; B và C1 10 năm theo bài Chính phủ 08/2024). Đăng kiểm và BHXH tự nguyện không đưa số vì chưa có nguồn.

## 5. Quyết định đã chốt
- Không làm mảng nhận định bóng đá (rủi ro liên quan cá cược, và không phù hợp với phần còn lại).
- Không dùng AI hay backend trong app ở giai đoạn này (ngân sách 0 đồng, và câu trả lời pháp lý sai gây hại).
- Giao diện sẽ được xây lại sau; dữ liệu tách riêng trong `data/` để dùng lại được.

## 6. Câu hỏi còn mở (quan trọng nhất)
1. **Kiếm tiền bằng cách nào?** Quảng cáo, gói đăng ký, hay bán dịch vụ làm giúp thủ tục. Câu trả lời quyết định có dùng được Open-Meteo miễn phí không, và ảnh hưởng trách nhiệm pháp lý khi bán nội dung pháp lý.
2. **Chưa kiểm chứng nhu cầu.** Chưa có ai trả tiền hay cam kết dùng. Bước đã đề xuất: tìm 20 câu hỏi thật về thủ tục ở Tây Ninh, hoặc hỏi 3 doanh nghiệp/người dân nhỏ xem họ có chịu trả tiền không, trước khi mở rộng thêm tính năng.
3. **Pháp lý:** cần hỏi người am hiểu về việc cung cấp hướng dẫn thủ tục có thu phí ở Việt Nam trước khi thu tiền.

## 7. Việc nên làm tiếp (một bước mỗi lần)
1. Đưa mục tách thửa (và sang tên) cho một cán bộ địa chính hoặc công chứng viên ở Tây Ninh kiểm tra; đếm đúng/sai. Nếu đúng, đặt `status = "da_kiem_chung"`, điền `verifiedBy` và `verifiedDate`.
2. Điền các ô chi phí/giấy tờ/thời gian còn trống từ thực tế khi hỏi trực tiếp.
3. Tìm nguồn cho giá heo hơi miền Nam 2026, gạo, trứng, rau, và cập nhật `data/prices.json` hằng tuần.
4. Đọc bản gốc Quyết định 139/2026/QĐ-UBND (Điều 17 chuyển tiếp) và Luật Việc làm 2025 (Điều 38) để hoàn thiện hai mục tương ứng.
5. Quyết định cách kiếm tiền (mục 6.1) trước khi xây tính năng mới.

## 8. Lưu ý vận hành cho AI làm tiếp
- Chỉ làm việc trong thư mục dự án này. Không đọc thư mục khác trên máy.
- Không đoán thông tin cá nhân của chủ dự án. Hỏi nếu cần.
- Mỗi lần đề xuất một bước, không liệt kê hàng loạt ý tưởng.
- Khi tìm giá hay luật, **kiểm tra năm đăng của bài** (đã có trường hợp đề «24/10» nhưng đăng năm 2025).
- Đẩy lên GitHub chỉ khi chủ dự án đồng ý rõ ràng. Repo công khai, nên không đưa thông tin cá nhân vào tệp.
- Môi trường: Windows, `gh` đăng nhập tài khoản `machcamlan`. Nếu push lỗi 403 thì kiểm tra lại `gh auth status` và credential của Git.
