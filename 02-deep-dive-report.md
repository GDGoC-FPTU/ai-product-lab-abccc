# 02 - Deep-Dive Report (Bài Nhóm)

**Tên nhóm:** abccc
**Thành viên 1:** Vũ Xuân Hiếu (MSSV: 2A202601447)
**Thành viên 2:** Nguyễn Hoài Nam (MSSV: 2A202601399)
**Thành viên 3:** Đỗ Trung Kiên (MSSV: 2A202601287)
**Thành viên 4:** Trịnh Quốc Trọng (MSSV: 2A202601779)
**Thành viên 5:** Lê Kim Nam (MSSV: 2A202601803)

---

## Phase 3 — DEEP-DIVE (Phân tích sâu bài toán)

### Quyết định lựa chọn:
Nhóm chúng tôi quyết định chọn phân tích sâu bài toán: **Vinhomes - Phân loại tự động và điều hướng phản ánh của cư dân trên App Vinhomes Resident**. 
Lý do: Đây là một quy trình lặp đi lặp lại rất tốn thời gian, nhưng đầu vào (text) rất phù hợp với thế mạnh xử lý ngôn ngữ tự nhiên của LLM.

### 3.2. Problem Statement (6-field) & Metrics

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Ban quản lý tòa nhà, Lễ tân trực ban, và Nhân viên CSKH trực tổng đài nội bộ tại các khu đô thị Vinhomes. |
| **2. Current Workflow** | 1. Cư dân gõ phản ánh qua App. <br>2. Tin nhắn đổ về hệ thống CRM của BQL. <br>3. Trực ban mở CRM, đọc từng tin nhắn để hiểu ngữ cảnh. <br>4. Dựa vào kinh nghiệm, trực ban quyết định sự cố thuộc tổ kỹ thuật điện, nước, vệ sinh hay an ninh. <br>5. Trực ban tạo Ticket thủ công và gán (assign) cho đội tương ứng. |
| **3. Bottleneck** | **Bước 3 và 4 (Đọc hiểu & Phân loại):** Đòi hỏi con người đọc hiểu văn bản không có cấu trúc. Rất dễ bị quá tải (overload) vào các khung giờ cao điểm (sáng sớm/chiều tối) hoặc khi có sự kiện (cúp điện, mất nước diện rộng). Thời gian xử lý thủ công từ 3-5 phút/ticket. |
| **4. Business Impact** | - **SLA (Service Level Agreement) bị vi phạm:** Cư dân phải chờ lâu để được hỗ trợ, gây bức xúc và đánh giá 1 sao cho dịch vụ quản lý.<br>- **Lãng phí nhân sự:** Cần duy trì lượng lớn nhân viên trực ban chỉ để làm công việc copy/paste và phân loại lặp lại. |
| **5. Success Metric** | - **Tốc độ:** Rút ngắn thời gian từ lúc nhận phản ánh đến lúc tạo thành công Ticket điều phối xuống **dưới 5 giây**.<br>- **Độ chính xác (Accuracy):** Tỷ lệ phân loại đúng phòng ban xử lý đạt **> 95%**.<br>- **Tỷ lệ tự động hóa (Straight-through processing):** Tự động xử lý hoàn toàn 80% lượng Ticket mà không cần con người nhúng tay. |
| **6. Operational Boundary** | **Được phép:** Đọc tin nhắn, trích xuất loại sự cố, độ khẩn cấp, số phòng và tự tạo Ticket.<br>**TUYỆT ĐỐI KHÔNG ĐƯỢC PHÉP:** Trả lời tự động các vấn đề liên quan đến pháp lý nhà đất, khiếu nại tài chính. Không được tự ý đưa ra lời khuyên sửa chữa nguy hiểm cho cư dân (vd: khuyên cư dân tự sửa điện). Các trường hợp này phải điều hướng về Lễ tân. |

### 3.3. Future-State Flow & AI Fit

* **Xác định mức AI Fit (AI-Fit Matrix):** 
  - [ ] Rule / State-Machine
  - [x] **LLM Feature** (Áp dụng API gọi Gemini để Text Classification & Extraction).
  - [ ] Agentic Loop

* **Vẽ Future-State Flow:**
  1. Cư dân gửi tin nhắn phản ánh qua App Vinhomes Resident.
  2. **AI Step:** Backend hệ thống gọi API Gemini 2.5 Flash, đẩy nội dung tin nhắn và yêu cầu phân tích.
  3. **AI Step:** Gemini trả về cấu trúc JSON gồm: `category` (Điện/Nước/Vệ sinh/An ninh/Khác), `urgency` (Bình thường/Khẩn cấp), và `room_number`.
  4. **Human Step (HITL - Human in the loop):** Nếu `urgency` là "Khẩn cấp" HOẶC `category` là "Khác" (Không xác định), hệ thống sẽ không tự động gửi cho kỹ thuật mà sẽ Pop-up cảnh báo để Trực ban vào kiểm tra và duyệt tay.
  5. Đối với các ca "Bình thường", hệ thống tự động bắn Notification công việc đến điện thoại của tổ kỹ thuật tương ứng.

* **Kế hoạch dự phòng (Fallback):**
  - Nếu API của LLM bị sập, timeout quá 5 giây, hoặc trả về lỗi JSON: Hệ thống tự động Bypass AI, đẩy toàn bộ tin nhắn mới vào hàng đợi (Queue) truyền thống để Trực ban xử lý thủ công như cũ.

---

## Phase 5 — EVALUATE (Quyết định triển khai)

### 5.1. AI Readiness Checklist (Đánh giá mức độ sẵn sàng):
1. [x] **Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?** 
   - **Tình trạng:** Vinhomes đang lưu trữ trên hệ thống CRM hơn 500,000 log tin nhắn phản ánh của cư dân trong 3 năm qua.
   - **Chất lượng:** Các log này đều đã có kết quả xử lý thực tế (được thợ nào sửa, thuộc hạng mục nào). Nhóm có thể trích xuất khoảng 10,000 dòng dữ liệu sạch để làm bộ Test-set (Baseline) đánh giá độ chính xác của AI.

2. [x] **Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?**
   - **Rủi ro:** AI phân loại sai sự cố (ví dụ: mất điện nhưng chuyển cho thợ nước) hoặc bỏ qua báo động cháy khẩn cấp.
   - **Kiểm soát:** Thiết kế luồng HITL bắt buộc Trực ban phải xác nhận tay các ca "Khẩn cấp". Ngoài ra, nếu có sai sót ở các ca "Bình thường", thợ kỹ thuật khi nhận được thông báo sai trên App sẽ bấm nút "Từ chối/Chuyển tuyến" để đẩy lại về tổng đài trong chưa tới 1 phút. Hậu quả là rất thấp và không ảnh hưởng đến an toàn cư dân.

3. [x] **Stakeholders (Các bên liên quan) sẵn sàng thay đổi quy trình làm việc cũ?**
   - **Động lực:** Ban quản lý các tòa nhà và đội ngũ Lễ tân/CSKH hiện đang chịu áp lực rất lớn vì thường xuyên bị cư dân phàn nàn do phản hồi chậm. Họ cực kỳ cởi mở với một công cụ giúp tự động hóa khâu đọc tin nhắn rập khuôn này.

### 5.2. Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp (Pilot chạy thử nghiệm tại 2 tòa nhà thuộc khu đô thị Vinhomes Times City trong 1 tháng trước khi roll-out toàn hệ thống).
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline)**
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn)**

### 5.3. Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật, chi phí và ROI):

**A. Đánh giá tính khả thi về mặt Kỹ thuật (Technical Feasibility):**
- **Không yêu cầu R&D đắt đỏ:** Thay vì phải tự train một mô hình NLP riêng tốn kém hàng tỷ đồng, chúng ta chỉ cần tích hợp API của các LLM thương mại mạnh mẽ (như Gemini 2.5 Flash). Mô hình này vốn đã có khả năng đọc hiểu tiếng Việt, phân tích ngữ cảnh (Intent Classification) và trích xuất thông tin (NER) cực kỳ xuất sắc.
- **Dễ dàng bảo trì:** Logic phân loại nằm toàn bộ trong System Prompt. Khi có thêm phòng ban mới hoặc loại sự cố mới, kỹ sư chỉ cần cập nhật vài dòng văn bản trong Prompt mà không cần re-train hệ thống hay đụng chạm vào code Core backend.

**B. Đánh giá tỷ suất hoàn vốn và Chi phí (Business ROI):**
- **Chi phí API:** Phân tích một tin nhắn text siêu ngắn (dưới 100 chữ) tốn cực kỳ ít token. Ước tính chi phí gọi API chỉ rơi vào khoảng vài VNĐ cho mỗi phản ánh. Với 10,000 phản ánh/tháng, chi phí duy trì AI chưa tới 100,000 VNĐ.
- **Tiết kiệm nguồn lực (Cost-saving):** Nếu không có AI, một nhân viên CSKH mất 3 phút/phản ánh. 10,000 phản ánh tương đương 500 giờ làm việc thủ công mỗi tháng. AI giúp cắt giảm 80% khối lượng thời gian vô ích này.
- **Trải nghiệm khách hàng (CSAT):** Thời gian trung bình từ lúc cư dân nhắn tin đến khi thợ tiếp nhận giảm từ 15 phút xuống còn dưới 5 giây. Trải nghiệm sống "Smart Home" tăng cao, trực tiếp tác động tốt đến uy tín thương hiệu Vinhomes.

**Kết luận:** Với chi phí đầu tư ban đầu cực thấp (chỉ tốn công viết Prompt và tích hợp API), rủi ro được cô lập tốt qua HITL, và lợi ích mang lại khổng lồ về mặt vận hành, dự án **"AI Phân loại phản ánh Vinhomes"** đạt điểm đánh giá tuyệt đối và xứng đáng được cấp vốn triển khai ngay lập tức (GO).
