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
| **5. Success Metric**       | - ≥95% phản ánh được AI phân loại đúng.<br>- ≥90% ticket được tạo trong **<30 giây**.<br>- Giảm thời gian xử lý từ **8 phút xuống dưới 1 phút**.<br>- Giảm ≥70% số ticket chuyển sai bộ phận.|                                                                                                                                                                        
| **6. Operational Boundary** | **AI được phép:** đọc nội dung phản ánh, phân loại loại sự cố, xác định mức ưu tiên, đề xuất bộ phận xử lý, tạo ticket nháp.<br>**AI không được phép:** tự đóng ticket, tự trả lời cư dân về kết quả xử lý, tự quyết định các trường hợp khẩn cấp hoặc ảnh hưởng đến an toàn.<br>**Human-in-the-loop:** nhân viên CSKH xác nhận đề xuất của AI trước khi gửi ticket. |


---

## 3.3. Future-State Flow & AI Fit

* **AI Fit:** Chọn **LLM Feature**. Lý do: Bài toán chủ yếu gồm phân loại phản ánh (Classification), trích xuất thông tin (Information Extraction) và xác định mức độ ưu tiên (Priority Detection) từ nội dung cư dân gửi. AI chỉ hỗ trợ phân tích và đề xuất, còn nhân viên CSKH phê duyệt kết quả, nên LLM Feature là giải pháp phù hợp nhất.

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
      │ 2. Trích xuất: Loại sự cố, vị trí, mức ưu tiên │
      │ 3. Đề xuất: Bộ phận xử lý thích hợp            │
      │ 4. Sinh Ticket Draft trên CRM                  │
      └────────────────────────────────────────────────┘
                      │
                      ▼
      ┌────────────────────────────────────────────────┐
      │ 🟢 HUMAN STEP (HITL - CSKH Review)             │
      ├────────────────────────────────────────────────┤
      │ CSKH duyệt/chỉnh sửa Ticket Draft:             │
      │ - Xác nhận Category, Priority, Department      │
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
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát? (Kiểm soát 100% nhờ bước phê duyệt bắt buộc của CSKH - HITL và luồng dự phòng Fallback khi AI có độ tin cậy thấp).
3. [x] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? (Ban Quản lý Vinhomes đồng thuận áp dụng công nghệ để giảm tải khối lượng công việc và cải thiện SLA).

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp (thử nghiệm trước trên 1-2 tòa nhà tại Vinhomes Ocean Park).

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
- **Về mặt kỹ thuật:** Việc phân loại văn bản ngắn và trích xuất thực thể (như loại sự cố, vị trí) là thế mạnh cốt lõi của mô hình LLM hiện nay, độ chính xác có thể dễ dàng tối ưu hóa bằng phương pháp Few-shot Prompting.
- **Rủi ro vận hành cực thấp:** Nhờ có luồng Human-in-the-loop, bất kỳ lỗi phân loại nào từ AI cũng được phát hiện và sửa chữa ngay tại quầy CSKH trước khi chuyển xuống bộ phận kỹ thuật.
- **Hiệu quả kinh tế:** Chi phí vận hành API của Gemini 2.5 Flash cực kỳ thấp so với lợi ích giảm 80% thời gian xử lý thủ công, giúp Vinhomes tối ưu hóa năng suất của nhân sự hiện tại mà không cần tuyển thêm người vào giờ cao điểm.
