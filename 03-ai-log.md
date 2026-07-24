# 03 — AI Log & Reflection

---

## 1. AI đã giúp tôi như thế nào?

Trong buổi làm bài, tôi sử dụng AI để hỗ trợ xây dựng bài toán Phân loại & Điều hướng phản ánh cư dân cho Vinhomes. AI giúp tôi brainstorm các pain point trong quy trình hiện tại, xây dựng Current-State Workflow, Problem Statement (6 fields), Future-State Flow và xác định Operational Boundary. Ngoài ra, AI còn gợi ý các chỉ số đánh giá (Success Metrics) như thời gian xử lý, độ chính xác phân loại và tỷ lệ ticket được xử lý tự động, giúp bài toán có tính định lượng và bám sát yêu cầu của bài lab.

---

## 2. AI đã sai gì hoặc làm tôi phải cảnh giác?

Trong quá trình trao đổi, AI đôi khi đưa ra các số liệu như số lượng phản ánh mỗi ngày hoặc số giờ công tiết kiệm mà không nêu rõ đây chỉ là ước tính minh họa, dễ khiến người đọc hiểu nhầm là dữ liệu thực tế của Vinhomes. Bên cạnh đó, AI cũng có xu hướng đề xuất phạm vi giải pháp quá rộng, chẳng hạn gợi ý AI tự động xử lý toàn bộ ticket thay vì chỉ hỗ trợ phân loại và đề xuất. Nếu áp dụng trực tiếp, giải pháp sẽ không phù hợp với yêu cầu về an toàn và Human-in-the-loop của bài toán.

---

## 3. Tôi đã sửa prompt hoặc đặt ranh giới như thế nào?

Để AI trả lời đúng hơn, tôi điều chỉnh prompt theo hướng cụ thể hơn: yêu cầu AI chỉ sử dụng số liệu ước tính và ghi rõ là giả định, đồng thời giới hạn vai trò của AI ở các tác vụ như phân loại phản ánh, xác định mức độ ưu tiên, đề xuất bộ phận xử lý và tạo ticket nháp. Tôi cũng bổ sung yêu cầu rằng mọi quyết định cuối cùng phải do nhân viên CSKH phê duyệt, AI không được tự trả lời cư dân hay tự xử lý các trường hợp khẩn cấp hoặc tranh chấp.

---

## 4. Bài học rút ra

Qua bài lab, tôi nhận thấy AI là công cụ hỗ trợ rất hiệu quả trong việc phân tích bài toán, xây dựng quy trình và soạn thảo tài liệu. Tuy nhiên, AI không thể thay thế hoàn toàn tư duy của người thực hiện. Người dùng cần kiểm tra lại tính chính xác của thông tin, xác định rõ phạm vi hoạt động của AI và thiết kế các ràng buộc phù hợp. Một prompt càng cụ thể về vai trò, mục tiêu và giới hạn thì kết quả AI trả về càng chính xác, thực tế và phù hợp với yêu cầu của bài toán.
