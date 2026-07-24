# 01 - Problem Scan (Bài Cá Nhân)

**Họ và tên:** [Vũ Xuân Hiếu]
**MSSV:** [2A202601447]

---

## Phase 1 — SCAN: Tìm kiếm cơ hội (Quét các nút thắt vận hành)

Dựa trên việc phân tích 4 Thấu kính (Lenses), tôi đã rà soát hoạt động của các công ty thành viên Vingroup và xác định được 5 điểm nghẽn (bottleneck) có tiềm năng giải quyết bằng AI:

| # | Công ty thành viên | Thấu kính (Lens) | Mô tả chi tiết bài toán |
|---|---|---|---|
| 1 | **Vinhomes** | Lặp đi lặp lại | **Phân loại phản ánh cư dân:** Ban quản lý phải đọc hàng ngàn tin nhắn báo sự cố trên App Vinhomes Resident mỗi tháng để tạo ticket thủ công và phân cho thợ sửa chữa. Công việc rập khuôn, dễ gây chậm trễ. |
| 2 | **VinFast** | AI có thể tốt hơn | **Chẩn đoán bệnh của xe qua giọng nói/văn bản:** Khách hàng thường mô tả lỗi bằng ngôn ngữ rất bình dân (vd: "xe kêu cọc cọc ở gầm"). Tổng đài viên phải tốn nhiều thời gian dịch lại sang mã lỗi kỹ thuật. |
| 3 | **Vinpearl** | Nỗi đau của người khác | **Khai thác dữ liệu Review khẩn cấp:** Hàng ngày có hàng nghìn đánh giá trên OTA (Agoda/Booking). Quản lý khách sạn không thể đọc hết, dẫn đến việc bỏ sót các phàn nàn nghiêm trọng (như ngộ độc, phòng bẩn) khiến khách bức xúc. |
| 4 | **Xanh SM** | Nỗi đau của người khác | **Phân tích nguyên nhân hủy chuyến:** Đội ngũ QA phải nghe lại thủ công hàng trăm đoạn ghi âm cuộc gọi hủy chuyến để tìm nguyên nhân (do tài xế hay do app lỗi), rất mất thời gian và tốn nhân lực. |
| 5 | **Vinmec** | Tốn thời gian | **Tóm tắt hồ sơ xuất viện (Discharge Summary):** Bác sĩ tốn trung bình 20-30 phút chỉ để gom nhặt dữ liệu từ bệnh án điện tử, kết quả xét nghiệm để viết một bản tóm tắt xuất viện dễ hiểu cho bệnh nhân. |

---

## Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Dựa trên bảng trên, tôi đã chọn ra 3 bài toán có tính khả thi cao nhất để đánh giá nhanh (Quick Assess).

### QUICK PROBLEM CARD #1 (Lựa chọn ưu tiên của nhóm)

**Bài toán:** Tự động phân loại văn bản và điều hướng phản ánh của cư dân trên App.
**Công ty thành viên:** [x] Vinhomes 

**Ai đang đau (Actor)?** Nhân viên hành chính / Trực ban Lễ tân tại các khu đô thị.

**Workflow thủ công hiện tại (4 bước):**
1. Cư dân nhắn tin báo cáo sự cố qua App. ──> 2. Trực ban đọc nội dung tin nhắn trên hệ thống CRM. ──> 3. Trực ban suy nghĩ và quyết định sự cố này thuộc phòng ban nào (Điện / Nước / Vệ sinh / An ninh). ──> 4. Nhập liệu để tạo Ticket và chuyển cho nhân viên kỹ thuật.

**Bước nào tốn thời gian/lỗi nhất?** Bước 2 và 3. Việc đọc hiểu văn bản không có cấu trúc và phân loại thủ công mất khoảng 3-5 phút/lượt xử lý. Gây ra tình trạng thắt cổ chai cực lớn vào giờ cao điểm.
**AI có thể nhảy vào hỗ trợ ở bước nào?** AI sẽ thay thế con người ở bước 2 và 3. Đọc ngay lập tức đoạn text phản ánh và tự động trích xuất các trường thông tin (thực thể) để gắn Tag/Phòng ban xử lý tự động.

**Đo thành công bằng gì (Metric có số)?**
- **Về tốc độ:** Giảm thời gian xử lý ban đầu từ 5 phút/Ticket ──> dưới 5 giây/Ticket.
- **Về chất lượng:** Tỷ lệ phân loại chính xác (Accuracy) > 95%.

**Quick Architecture:** [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent

---

### QUICK PROBLEM CARD #2

**Bài toán:** Trợ lý AI chẩn đoán sơ bộ lỗi xe điện từ mô tả ngôn ngữ tự nhiên.
**Công ty thành viên:** [x] VinFast

**Ai đang đau (Actor)?** Nhân viên trực tổng đài CSKH (Call Center).

**Workflow thủ công hiện tại (4 bước):**
1. Khách gọi điện phàn nàn và mô tả tình trạng xe. ──> 2. CSKH ghi chú lại bằng văn bản theo lời khách. ──> 3. CSKH tra cứu sổ tay kỹ thuật hoặc liên hệ hỏi chéo bộ phận Kỹ thuật. ──> 4. CSKH chốt phương án, hẹn lịch bảo dưỡng.

**Bước nào tốn thời gian/lỗi nhất?** Bước 3. Việc tra cứu chéo giữa CSKH (người không chuyên kỹ thuật) và Kỹ thuật viên (người bận rộn) mất từ 10-15 phút/cuộc gọi.
**AI có thể nhảy vào hỗ trợ ở bước nào?** Bước 3. Đọc đoạn ghi chú của CSKH và đối chiếu ngay lập tức với cơ sở dữ liệu kỹ thuật để gợi ý 3 mã lỗi có xác suất cao nhất.

**Đo thành công bằng gì (Metric có số)?** Rút ngắn thời gian tra cứu lỗi từ 15 phút ──> dưới 30 giây, giúp giảm thời gian chờ máy của khách hàng xuống 80%.

**Quick Architecture:** [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent

---

### QUICK PROBLEM CARD #3

**Bài toán:** Tự động giám sát, cảnh báo sớm các review tiêu cực khẩn cấp của khách lưu trú.
**Công ty thành viên:** [x] Vinpearl

**Ai đang đau (Actor)?** Quản lý chất lượng dịch vụ khách sạn (Quality Assurance Manager).

**Workflow thủ công hiện tại (4 bước):**
1. Khách viết đánh giá trên các kênh OTA (Agoda/Booking/Google). ──> 2. Nhân viên mở từng trang web để lướt đọc và copy review. ──> 3. Đọc thủ công và lọc ra các review 1-2 sao mang tính chất khẩn cấp (như ngộ độc thực phẩm, mất đồ). ──> 4. Soạn email báo cáo cho Manager.

**Bước nào tốn thời gian/lỗi nhất?** Bước 2 và 3. Việc gom và đọc thủ công hàng trăm review mỗi ngày ngốn khoảng 2-3 tiếng đồng hồ, rất dễ đọc sót chữ.
**AI có thể nhảy vào hỗ trợ ở bước nào?** Bước 3. AI tự động quét (scan) tất cả review đổ về, phân tích cảm xúc (Sentiment Analysis) để lọc ra các phàn nàn nghiêm trọng và tự động bắn cảnh báo (Alert).

**Đo thành công bằng gì (Metric có số)?**
- **Về tốc độ:** Tiết kiệm 100% thời gian đọc review thủ công (khoảng 15-20 giờ công/tuần).
- **Về SLAs:** Gửi cảnh báo SMS/Email cho Manager trong vòng tối đa 5 phút kể từ khi review xấu được đăng tải, tránh khủng hoảng truyền thông.

**Quick Architecture:** [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent
    