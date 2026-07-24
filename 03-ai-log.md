# 03 — AI Log & Reflection

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

Điểm tôi phải cảnh giác nhất là AI có xu hướng đề xuất giải pháp quá rộng. Khi tôi hỏi về cách xử lý ticket cư dân, AI ban đầu thường đẩy ý tưởng sang hướng “agentic workflow” hoặc tự động hóa gần như toàn bộ quy trình. Cách trả lời đó nghe rất hấp dẫn nhưng chưa chắc an toàn, vì trong bài toán Vinhomes có nhiều case nhạy cảm liên quan đến phí dịch vụ, tranh chấp giữa cư dân và ban quản lý, hoặc nội dung cần kiểm tra kỹ bằng con người.

Một điểm khác là AI đôi khi mô tả metric khá đẹp nhưng chưa đủ thực tế. Ví dụ, nếu không ép chặt yêu cầu, AI có thể nói chung chung rằng “giảm thời gian xử lý” mà không gắn ngưỡng đo cụ thể cho FRT, routing accuracy, hoặc tỷ lệ ticket cần sửa lại. Điều đó làm bài viết kém chặt chẽ và khó đánh giá.

---

## 3. Tôi đã sửa prompt hoặc đặt ranh giới như thế nào?

Để ép AI trả lời đúng hơn, tôi đã sửa prompt theo hướng rõ vai trò, rõ đầu vào và rõ giới hạn đầu ra. Tôi yêu cầu AI chỉ tập trung vào ba việc: phân loại ticket, gợi ý đội xử lý và sinh bản nháp phản hồi. Tôi cũng bổ sung ranh giới rằng AI không được tự chốt quyết định cuối cùng cho các case nhạy cảm và mọi phản hồi gửi ra cư dân phải có người duyệt.

Ngoài ra, tôi đổi cách hỏi từ “hãy đề xuất giải pháp AI” sang “hãy đánh giá xem phần nào có thể dùng LLM Feature, phần nào nên giữ rule-based, và phần nào bắt buộc Human-in-the-loop”. Cách hỏi này giúp AI trả lời bớt lan man và bám sát bài toán vận hành hơn.

Nếu gặp câu trả lời quá rộng, tôi phản biện lại bằng yêu cầu rất cụ thể như:

- Bài toán này có cần agent tự trị không, hay chỉ cần classification + draft?
- Bước nào là bottleneck đo được bằng thời gian?
- Case nào phải fallback về người thật?
- Metric nào có thể kiểm chứng bằng dữ liệu thực tế?

Nhờ đó, kết quả cuối cùng thực tế hơn và phù hợp với rubric của bài lab hơn.

---

## 4. Bài học rút ra

Tôi rút ra rằng AI hữu ích nhất khi được dùng như một công cụ phản biện và tăng tốc tư duy, không phải như một hệ thống tự quyết thay con người. Với bài toán Vinhomes, giá trị thật không nằm ở việc làm AI phức tạp, mà nằm ở việc xác định đúng phạm vi, đúng ranh giới và đúng chỗ cần người duyệt.

Nếu lần sau làm một bài scoping tương tự, tôi sẽ tiếp tục dùng AI để brainstorm nhưng sẽ ép chặt hơn ở ba điểm: metric phải đo được, boundary phải rõ, và fallback phải có ngay từ đầu.
