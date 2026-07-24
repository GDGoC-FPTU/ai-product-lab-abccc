# 🔍 Phase 1 — SCAN

Dưới đây là danh sách 5 bài toán vận hành thực tế tại các công ty thuộc Vingroup, được phân tích theo 4 lăng kính:

### 📝 List bài toán của tôi:
| # | Subsidiary (Công ty) | Lens (Lăng kính) | Mô tả ngắn bài toán |
|---|------------------------|------------------|---------------------|
| 1 | **VinFast** | Tốn thời gian (Time-consuming) | Phân loại & điều phối hàng ngàn ticket báo lỗi xe mỗi ngày. Nhân viên phải đọc thủ công từng mô tả lỗi để chuyển đến đúng xưởng/bộ phận chuyên môn (Pin, Phần mềm, Cơ khí). |
| 2 | **Xanh SM** | Stakeholder Pain (Nỗi đau của Tài xế) | Quá trình xác minh và giải quyết khiếu nại của tài xế (khách "bom" xe, sai cước phí) rất chậm do tổng đài quá tải, khiến tài xế bức xúc vì bị giam tiền. |
| 3 | **Vinhomes** | Lặp lại (Repetitive) | Ban quản lý mất nhiều giờ mỗi ngày để đọc, phân loại độ khẩn cấp và gõ câu trả lời cho hàng trăm tin nhắn phản ánh của cư dân (tiếng ồn, vệ sinh, sửa chữa) trên App Vinhomes. |
| 4 | **Vinmec** | Tốn thời gian (Time-consuming) | Y tá/Bác sĩ mất quá nhiều thời gian để gõ lại và trích xuất thông tin bệnh án, hồ sơ xét nghiệm từ giấy khám của tuyến dưới nhập vào hệ thống điện tử nội bộ (HIS). |
| 5 | **VinWonders** | AI-upgrade (Nâng cấp bằng AI) | Khách quốc tế và nội địa thường xuyên hỏi lễ tân các câu giống hệt nhau (giờ chạy xe buggie, giờ diễn show, vị trí nhà hàng). Trải nghiệm tra cứu thông tin hiện tại rập khuôn và thiếu tính cá nhân hóa. |

---

# 🃏 Phase 2 — QUICK-ASSESS

Lựa chọn 3 bài toán từ danh sách trên để phân tích nhanh:

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Phân loại và điều phối tự động các ticket │
│                   báo lỗi xe từ khách hàng đến đúng bộ phận.│
│                                                             │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên tổng đài & Điều phối xưởng   │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận ticket ──> 2. Đọc mô tả lỗi ──> 3. Quyết định bộ  │
│   phận (Pin/Cơ khí) ──> 4. Chuyển tiếp ticket               │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 5-10p/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Đọc hiểu mô tả văn    │
│ bản, tự động gán tag bộ phận và đánh giá mức độ nghiêm trọng│
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Tăng tỷ lệ điều phối  │
│ đúng ngay lần đầu lên 95%, giảm thời gian xử lý < 5 giây.   │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Tự động hóa việc tiếp nhận, xác minh thông│
│                   tin khiếu nại cước phí, cuốc xe ảo của TX.│
│                                                             │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Tài xế Xanh SM & Nhân viên CSKH        │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận khiếu nại ──> 2. Tra cứu mã cuốc xe ──> 3. Đối    │
│   chiếu GPS/giá tiền ──> 4. Ra quyết định bồi hoàn          │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 15p/lượt)   │
│ AI có thể nhảy vào hỗ trợ ở bước nào? AI trích xuất mã cuốc │
│ từ lời phàn nàn, gọi API tra cứu dữ liệu và đề xuất bồi hoàn│
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian giải   │
│ quyết khiếu nại cước phí từ 24 tiếng xuống dưới 5 phút.     │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘
```

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): AI phản hồi tự động và phân loại mức độ   │
│                   khẩn cấp cho tin nhắn phản ánh của cư dân.│
│                                                             │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Ban quản lý tòa nhà & Cư dân           │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Cư dân nhắn tin ──> 2. BQL đọc tin ──> 3. Phân loại độ │
│   khẩn cấp ──> 4. Gõ phản hồi ──> 5. Chuyển bộ phận kỹ thuật│
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2, 3, 4 (⏱ 5-10p/tin)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Dự đoán ý định, phân  │
│ loại mức khẩn (vd: rò nước) và tự động soạn nháp phản hồi.  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian phản   │
│ hồi cư dân lần đầu (First Response Time) xuống dưới 1 phút. │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```
