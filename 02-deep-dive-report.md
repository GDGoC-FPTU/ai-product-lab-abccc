# 📄 02-deep-dive-report.md — Báo Cáo Phase 3 & 5 (Deep-Dive & Evaluation)

**Thông tin nhóm:**

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

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên CSKH tại Ban Quản lý các tòa nhà Vinhomes. |
| **2. Current Workflow** | Cư dân gửi phản ánh qua App Resident/Hotline/Email. CSKH tiếp nhận, đọc toàn bộ nội dung, phân loại sự cố (điện, nước, an ninh...), gán mức độ ưu tiên, tra cứu bộ phận phụ trách của tòa nhà đó, tạo ticket trên CRM và chuyển giao đến bộ phận kỹ thuật tương ứng. Quy trình 6 bước thủ công mất 7 phút/lượt. |
| **3. Bottleneck** | Bước đọc, phân loại sự cố và xác định bộ phận phụ trách. Nội dung phản ánh bằng ngôn ngữ tự nhiên không đồng nhất, nhân viên dễ gán nhãn nhầm bộ phận hoặc chậm trễ khi lượng phản ánh đổ về lớn trong giờ cao điểm. |
| **4. Business Impact** | Vinhomes nhận hàng ngàn phản ánh mỗi ngày. Việc xử lý thủ công gây trễ nải SLA tiếp nhận. Việc gán nhầm bộ phận làm kỹ thuật viên di chuyển sai vị trí, lãng phí nguồn lực vận hành và làm giảm mức độ hài lòng của cư dân. |
| **5. Success Metric** | - Giảm tổng thời gian xử lý và điều phối phản ánh từ 7 phút xuống dưới 1.5 phút/phản ánh.<br>- Độ chính xác phân loại tự động của hệ thống đạt >= 92%.<br>- Tỷ lệ chuyển nhầm bộ phận (re-routing) giảm xuống dưới 3%. |
| **6. Operational Boundary** | AI được phép đọc nội dung phản ánh, tự động phân loại, gán nhãn mức độ ưu tiên, đề xuất bộ phận và sinh Ticket nháp (Draft).<br>**CẤM:** AI không được phép tự động phê duyệt và chuyển trực tiếp ticket đến bộ phận xử lý mà không qua bước duyệt của điều phối viên (HITL). AI tuyệt đối không được tự ý chỉnh sửa nội dung phản ánh gốc của cư dân. |

---

## 3.3. Future-State Flow & AI Fit

* **Phân loại AI Fit:** Giải pháp thuộc nhóm **LLM Feature** (Hệ thống có luồng xử lý cấu trúc cố định, không cần Agent tự trị thực hiện các chuỗi hành động phức tạp nhằm tránh rủi ro bảo mật và kiểm soát tốt chi phí API).
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
