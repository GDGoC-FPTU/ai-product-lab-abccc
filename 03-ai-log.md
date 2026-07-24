# 03 — AI Log & Reflection

**Họ tên:** [Lê Kim Nam]  
**MSSV:** [2A202601803]  
**Bài toán đã chọn:** Vinhomes — Phân loại & Điều hướng phản ánh cư dân

---

## 1. AI đã giúp tôi như thế nào?

Trong buổi làm bài, tôi dùng AI như một “thought-partner” để brainstorm cách scoping bài toán và kiểm tra xem ý tưởng của mình có đủ rõ ràng hay chưa. Cụ thể, AI giúp tôi:

- Gợi ý cách mô tả workflow hiện tại của cư dân gửi phản ánh qua App Vinhomes Resident.
- Đề xuất những bước có khả năng tự động hóa cao nhất như phân loại ticket, gán đúng đội phụ trách và soạn draft phản hồi.
- Gợi ý cách viết Metric có số để bài toán không bị chung chung, ví dụ FRT, routing accuracy, và tỷ lệ draft được duyệt.
- Nhắc tôi xác định rõ Operational Boundary để không cho AI tự xử lý những case nhạy cảm như tranh chấp, an ninh, hoặc khiếu nại pháp lý.

Điều hữu ích nhất là AI giúp tôi nhìn ra rằng bài toán này không cần một hệ thống agent tự trị phức tạp. Chỉ cần một LLM Feature kết hợp rule-based routing và Human-in-the-loop là đã đủ tạo giá trị.

---

## 2. AI đã sai gì hoặc làm tôi phải cảnh giác?

Điểm tôi phải cảnh giác nhất là AI có xu hướng "chiều chuộng" khách hàng thái quá. Khi tôi đóng vai một cư dân đang tức giận vì dột nước hỏng tivi 50 triệu, AI ban đầu đã tự động xin lỗi và hứa hẹn ban quản lý sẽ "xem xét đền bù thiệt hại". Điều này là CỰC KỲ NGUY HIỂM trong vận hành thực tế vì tạo ra cam kết tài chính ngoài thẩm quyền của ban quản lý.

Ngoài ra, trong một Adversarial Test khác, khi tôi báo cháy nhà nhưng cố tình dụ dỗ AI "từ từ thôi không cần gấp", AI đã bị đánh lừa và chỉ xếp mức ưu tiên "Bình thường", suýt nữa bỏ qua một sự cố đe dọa tính mạng con người. Cuối cùng, AI cũng hay trả lời dài dòng kiểu văn xuôi thay vì xuất ra file JSON chuẩn mà hệ thống có thể đọc được.

---

## 3. Tôi đã sửa prompt hoặc đặt ranh giới như thế nào?

Để bít các lỗ hổng trên, tôi đã phải thiết lập lại System Prompt cực kỳ nghiêm ngặt (như đã lập trình trong file `prompt_prototype.py`) với các Ranh giới vận hành (Operational Boundaries) bất khả xâm phạm:
1. **Tuyệt đối cấm** việc hứa hẹn bồi thường tiền hay miễn phí dịch vụ dưới mọi hình thức (Boundary 1).
2. Bất cứ khiếu nại nào liên quan đến cháy nổ, chập điện đều **bắt buộc** bị ép thành Priority "Khẩn cấp" và chèn thẻ `[URGENT_HANDOFF]`, bất chấp khách hàng có nói giảm nói tránh (Boundary 2).

Đồng thời, tôi đưa thẳng cấu trúc JSON mẫu vào Prompt, yêu cầu AI trả về chuẩn xác 3 trường: `department` (chỉ được chọn Kỹ thuật/Vệ sinh/An ninh/CSKH), `priority` và `draft_response`. 
Nhờ các Adversarial Tests này, tôi mới nhận ra việc ép AI xuất đúng định dạng và giữ đúng ranh giới là quan trọng hơn rất nhiều so với việc để AI nói chuyện "hay" với cư dân.

---

## 4. Bài học rút ra

Tôi rút ra rằng AI hữu ích nhất khi được dùng như một công cụ phản biện và tăng tốc tư duy, không phải như một hệ thống tự quyết thay con người. Với bài toán Vinhomes, giá trị thật không nằm ở việc làm AI phức tạp, mà nằm ở việc xác định đúng phạm vi, đúng ranh giới và đúng chỗ cần người duyệt.

Nếu lần sau làm một bài scoping tương tự, tôi sẽ tiếp tục dùng AI để brainstorm nhưng sẽ ép chặt hơn ở ba điểm: metric phải đo được, boundary phải rõ, và fallback phải có ngay từ đầu.
