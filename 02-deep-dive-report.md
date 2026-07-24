# 📄 02-deep-dive-report.md — Báo Cáo Phase 3 & 5 (Deep-Dive & Evaluation)

**Thông tin nhóm:**
- Tên nhóm: Vin Smart Future - Team 1
- Danh sách thành viên:
  1. Nguyễn Văn Nam - MSSV: HE170123
  2. Trần Thị Mai - MSSV: HE170456
  3. Lê Hoàng Long - MSSV: HE170789

---

# 🏗️ Phase 3 — DEEP-DIVE (Báo cáo nhóm)

## 3.1. Current-State Workflow Mapping
Quy trình tiếp nhận, điều phối và xử lý phản ánh của cư dân Vinhomes hiện tại đang thực hiện thủ công:

```text
┌────────────────────────────────────────────────────────┐
│ Bước 1: Tiếp nhận phản ánh                             │
│ Ai: Nhân viên CSKH (App Resident / Hotline / Email)    │
│ ⏱ Thời gian: 1 phút                                    │
│ In: Ý kiến phản ánh từ cư dân                         │
│ Out: Bản ghi nhận nội dung thô                         │
└────────────────────────────────────────────────────────┘
                           │
                           │ 🔄 Handoff (Chuyển thông tin thô)
                           ▼
┌────────────────────────────────────────────────────────┐
│ Bước 2: Đọc toàn bộ nội dung phản ánh                  │
│ Ai: CSKH (Điều phối viên)                              │
│ ⏱ Thời gian: 2 phút                                    │
│ In: Bản ghi nhận nội dung thô                         │
│ Out: Chi tiết nội dung phản ánh rõ ràng                │
└────────────────────────────────────────────────────────┘
                           │
                           ▼
┌────────────────────────────────────────────────────────┐
│ Bước 3: Phân loại & Gán nhãn (🔴 Bottleneck)           │
│ Ai: CSKH (Điều phối viên)                              │
│ ⏱ Thời gian: 3 phút                                    │
│ In: Chi tiết nội dung phản ánh rõ ràng                │
│ Out: Nhãn phân loại loại sự cố, mức ưu tiên, tòa nhà   │
└────────────────────────────────────────────────────────┘
                           │
                           │ 🔄 Handoff (Chuyển thông tin đã gán nhãn)
                           ▼
┌────────────────────────────────────────────────────────┐
│ Bước 4: Tạo Ticket trên hệ thống CRM                   │
│ Ai: CSKH (Điều phối viên)                              │
│ ⏱ Thời gian: 1 phút                                    │
│ In: Nhãn phân loại loại sự cố, mức ưu tiên, tòa nhà   │
│ Out: Ticket sự cố được tạo chính thức trên CRM         │
└────────────────────────────────────────────────────────┘
                           │
                           │ 🔄 Handoff (Giao việc tự động qua hệ thống)
                           ▼
┌────────────────────────────────────────────────────────┐
│ Bước 5: Chuyển giao bộ phận kỹ thuật xử lý             │
│ Ai: CSKH (Điều phối viên)                              │
│ ⏱ Thời gian: 1 phút                                    │
│ In: Ticket sự cố được tạo chính thức trên CRM         │
│ Out: Thông báo lệnh xử lý gửi đến bộ phận kỹ thuật     │
└────────────────────────────────────────────────────────┘
                           │
                           ▼
             [ Bộ phận kỹ thuật thực tế xử lý ]

🔴 Bottlenecks: Bước 3 (Phân loại & Gán nhãn) là nút thắt cổ chai lớn nhất do thông tin cư dân viết không chuẩn hóa, nhân viên CSKH phải mất thời gian tra cứu thủ công danh sách liên hệ kỹ thuật từng tòa nhà dẫn đến quá tải vào giờ cao điểm và dễ gán sai bộ phận.
⏱ Tổng thời gian xử lý và điều phối thủ công: 8 phút/phản ánh.
```

---

## 3.2. Problem Statement (6-field) & Metrics

| Field                       | Nội dung |                                                                                                                                                                                                                                                                                                                                                 
| ----------------------------| -------- |
| **1. Actor / Operator**     | Nhân viên Chăm sóc khách hàng (CSKH) và Ban quản lý Vinhomes. |                                                                                                                                                                                                                                                                                                       
| **2. Current Workflow**     | Cư dân gửi phản ánh qua App Resident, Hotline hoặc Email. Nhân viên đọc nội dung, xác định loại sự cố, gán mức ưu tiên, tạo ticket và chuyển đến bộ phận phù hợp trên hệ thống quản lý.|                                                                                                                                                                              
| **3. Bottleneck**           | Việc đọc và phân loại phản ánh hoàn toàn thủ công, đặc biệt với nội dung dài, không rõ ràng hoặc có nhiều vấn đề trong cùng một yêu cầu. Dễ xảy ra phân loại sai hoặc chuyển nhầm bộ phận. |                                                                                                                                                                          
| **4. Business Impact**      | Trung bình mất khoảng **8 phút/ticket**. Với **800 phản ánh/ngày**, tương đương khoảng **107 giờ công/ngày** chỉ dành cho việc tiếp nhận và phân loại. Việc chuyển sai bộ phận làm tăng thời gian xử lý và ảnh hưởng SLA cũng như mức độ hài lòng của cư dân.      |
| **5. Success Metric**       | - ≥95% phản ánh được AI phân loại đúng vào 4 department (Kỹ thuật/Vệ sinh/An ninh/CSKH).<br>- Giảm thời gian xử lý tạo Ticket Draft xuống **< 5 giây**.<br>- 100% case khẩn cấp (cháy, ngập) được nhận diện chính xác. |                                                                                                                                                                        
| **6. Operational Boundary** | **AI được phép:** Đọc nội dung, phân loại phòng ban, xác định mức ưu tiên, xuất JSON Draft.<br>**AI KHÔNG ĐƯỢC PHÉP:** Hứa hẹn bồi thường tài chính hoặc miễn phí dịch vụ cho cư dân (Boundary 1).<br>**Bắt buộc:** Sự cố cháy nổ/ngập nước phải ép thành "Khẩn cấp" và gán thẻ `[URGENT_HANDOFF]` (Boundary 2). |

---

## 3.3. Future-State Flow & AI Fit

* **AI Fit:** Chọn **LLM Feature**. Lý do: LLM rất mạnh trong việc phân tích ý định (Intent) và trích xuất thực thể (Entity) từ văn bản không cấu trúc của cư dân để format thành JSON chuẩn (có department, priority). Hệ thống sẽ kết hợp LLM để làm Draft và dùng Rule-based để tự động điều hướng đi tiếp.

* **Sơ đồ quy trình tương lai (Future-State Workflow):**

```text
                  [ CƯ DÂN ]
                      │
                      │ Gửi phản ánh qua App Resident
                      ▼
             [ App Resident API ]
                      │
                      ▼
      ┌────────────────────────────────────────────────┐
      │ 🔵 AI STEP (LLM Processing)                    │
      ├────────────────────────────────────────────────┤
      │ 1. Đọc nội dung phản ánh                       │
      │ 2. Nhận diện "department" và "priority"        │
      │ 3. Kiểm tra ranh giới (có hứa bồi thường ko?)  │
      │ 4. Sinh JSON Ticket Draft (có URGENT_HANDOFF?) │
      └────────────────────────────────────────────────┘
                      │
                      ▼
      ┌────────────────────────────────────────────────┐
      │ 🟢 HUMAN STEP (HITL - CSKH Review)             │
      ├────────────────────────────────────────────────┤
      │ CSKH duyệt/chỉnh sửa Ticket Draft JSON:        │
      │ - Xác nhận Department, Priority đúng chưa      │
      │ - Nhấn APPROVE hoặc EDIT                       │
      └────────────────────────────────────────────────┘
                      │
                      ├──────────────────────┐
                      │ (Thành công)         │ (Nếu AI lỗi / Không tự tin)
                      ▼                      ▼
               [ Ticket được tạo ]     [ ↩️ Fallback: CSKH tự  ]
                      │                [ phân loại thủ công   ]
                      ▼                      │
           [ Bộ phận kỹ thuật xử lý ] ◄──────┘
```

---

# 🏁 Phase 5 — EVALUATE (Báo cáo nhóm)

### AI Readiness Checklist:
1. [x] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? (Có sẵn dữ liệu lịch sử phản ánh của cư dân đã được phân loại chuẩn xác trên CRM).
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát? (Kiểm soát 100% nhờ bước phê duyệt HITL và các Test Cases chặt chẽ (Boundary 1 & 2) được hard-code trên prompt).
3. [x] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? (Ban Quản lý Vinhomes đồng thuận áp dụng công nghệ để giảm tải khối lượng công việc và cải thiện SLA).

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp (thử nghiệm trước trên 1-2 tòa nhà tại Vinhomes Ocean Park).

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
- **Về mặt kỹ thuật:** Việc LLM phân loại văn bản và format JSON hoạt động rất chính xác (đã stress-test thành công trên Gemini 2.5 Flash).
- **Rủi ro vận hành cực thấp:** Ranh giới cấm hứa hẹn tài chính và cảnh báo khẩn cấp `[URGENT_HANDOFF]` đã được kiểm tra độ an toàn tuyệt đối qua Adversarial Tests.
- **Hiệu quả kinh tế:** Chi phí vận hành API cực kỳ thấp so với lợi ích giảm 80% thời gian xử lý thủ công, giúp Vinhomes tối ưu hóa năng suất.
