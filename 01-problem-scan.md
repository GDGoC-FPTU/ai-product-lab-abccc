# 📄 01-problem-scan.md — Báo Cáo Phase 1 & 2 (Scan & 3 Quick Cards)

**Thông tin cá nhân:**
- Họ và tên: Đỗ Trung Kiên
- Mã số sinh viên (MSSV): 2A202601287

---

## 🔍 Phase 1 — SCAN

Bảng quét cơ hội và phát hiện bài toán/bottleneck thực tế tại các công ty thành viên Vingroup áp dụng **4 Lenses**:

| # | Subsidiary | Tên bài toán / Bottleneck | Lens | Mô tả ngắn bài toán |
|---|------------|---------------------------|------|---------------------|
| 1 | **Vinhomes** | Phân loại & Điều hướng phản ánh cư dân | Lặp lại | Phân loại tự động các khiếu nại (ví dụ: mất nước, hỏng đèn, ồn ào) gửi qua App Vinhomes Resident đến đúng ban quản lý từng tòa nhà. *(Lựa chọn làm bài nhóm)* |
| 2 | **Xanh SM** | Phân tích lý do hủy chuyến của khách hàng | Pain từ người khác | Tự động nghe ghi âm cuộc gọi hủy chuyến và ghi chú của tài xế để phân loại 10 lý do phổ biến nhất gây rò rỉ cuốc. |
| 3 | **VinFast** | Tự động soạn phản hồi đánh giá xe trên mạng xã hội | Tốn thời gian | Phân tích cảm xúc các bài đăng, bình luận về xe VinFast trên mạng xã hội và tự động dự thảo (draft) phản hồi phản ánh của khách hàng để nhân viên PR phê duyệt. |
| 4 | **Vinmec** | Nhận diện và số hóa đơn thuốc viết tay | Tốn thời gian | Sử dụng OCR kết hợp LLM để nhận diện và số hóa đơn thuốc viết tay của bác sĩ, tự động lưu thông tin thuốc vào bệnh án điện tử để dược sĩ kiểm tra. |
| 5 | **Vinpearl** | Đề xuất hoạt động giải trí cá nhân hóa cho du khách | AI có thể tốt hơn | Phân tích thói quen, độ tuổi, thành phần đoàn đi và thời tiết thực tế để tự động thiết kế gợi ý lịch trình vui chơi chi tiết tại VinWonders thông qua chatbot. |

---

## 🃏 Phase 2 — QUICK-ASSESS (3 Quick Problem Cards)

Dưới đây là phân tích chi tiết được thiết kế chặt chẽ cho 3 bài toán tiềm năng nhất (Bài số 1, 2, và 4 từ Phase 1), đảm bảo vượt qua các phản biện khắt khe về logic vận hành, hiệu quả chi phí và ranh giới an toàn:

### QUICK PROBLEM CARD #1

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Tự động phân loại và điều hướng phản ánh  │
│ cư dân Vinhomes đến đúng bộ phận và cảnh báo khẩn cấp.       │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Điều phối viên BQL & Cư dân Vinhomes.   │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cư dân gửi phản ánh bằng chữ/ảnh qua Resident App.     │
│   2. Điều phối viên đọc thủ công để phân loại bộ phận.     │
│   3. Tìm thông tin liên hệ và chuyển tiếp phản ánh đi.       │
│   4. Kỹ thuật viên tiếp nhận xử lý tại tòa nhà.             │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 15 phút/lượt)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3: Tự động    │
│ phân tích nội dung, gắn nhãn (điện, nước, an ninh) và gợi ý │
│ BQL tòa nhà phù hợp, ưu tiên cảnh báo khẩn cấp (rò rỉ gas). │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Giảm tỷ lệ chuyển sai bộ phận (re-routing) từ 12% ──> <3%│
│   - Thời gian điều phối phản ánh từ 15 phút ──> dưới 1 phút.  │
│   - Tránh 100% các lỗi bỏ sót phản ánh có tính khẩn cấp cao. │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
│ *Lưu ý logic: Cư dân thường chọn sai phân mục khi gửi hoặc   │
│ viết không rõ nghĩa, LLM đa phương thức (Multimodal) giúp    │
│ phân tích cả văn bản và hình ảnh để gán đúng bộ phận.       │
└─────────────────────────────────────────────────────────────┘
```

---

### QUICK PROBLEM CARD #2

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Tự động phân tích ghi âm cuộc gọi hủy     │
│ chuyến và ghi chú của tài xế để tìm nguyên nhân rò rỉ cuốc. │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Bộ phận Phân tích dữ liệu & QA Xanh SM.│
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cuốc xe bị hủy ──> 2. Tài xế hoặc tổng đài note lý do. │
│   ──> 3. QA nghe ngẫu nhiên ghi âm cuộc gọi để kiểm chứng.   │
│   ──> 4. Tổng hợp báo cáo thủ công để đề xuất điều chỉnh.    │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 (⏱ 8 phút/lượt)    │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3: Dùng Speech-   │
│ to-Text và LLM để quét toàn bộ ghi âm hội thoại nhằm phát  │
│ hiện lý do thực tế (ví dụ: tài xế ép khách hủy, kẹt đường). │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Tăng tỷ lệ rà soát từ 5% (chọn mẫu) ──> 100% cuốc hủy.   │
│   - Giảm tỷ lệ rò rỉ cuốc (do tài xế gian lận) xuống dưới 2%.│
│   - Rút ngắn chu kỳ phân tích từ 7 ngày ──> thời gian thực.  │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
│ *Lưu ý logic: Tài xế thường note vắn tắt/lách luật trên app. │
│ Phân tích hội thoại giọng nói thực tế giúp phát hiện gian lận│
│ mà các phương pháp phân tích log SQL thông thường bỏ sót.   │
└─────────────────────────────────────────────────────────────┘
```

---

### QUICK PROBLEM CARD #3

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Nhận diện chữ viết tay bác sĩ trên đơn    │
│ thuốc giấy từ viện khác để số hóa và kiểm tra an toàn thuốc.│
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Dược sĩ cấp phát thuốc tại quầy Vinmec. │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Bệnh nhân trình đơn thuốc giấy ngoài viện tại quầy.    │
│   2. Dược sĩ dịch chữ viết tay và gõ lại vào EMR để lưu.     │
│   3. Kiểm tra tính tương thích của thuốc trong kho.         │
│   4. Bàn giao thuốc và in nhãn hướng dẫn sử dụng.           │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 & 3 (⏱ 5 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3: Nhận dạng │
│ chữ viết tay bác sĩ bằng Multimodal LLM, đối chiếu EMR để   │
│ phát hiện liều lượng bất thường hoặc thuốc khắc nhau.        │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Giảm thời gian nhập liệu đơn ngoại viện từ 5p ──> dưới 30s│
│   - Tỷ lệ dược sĩ kiểm soát lỗi sai sót đơn thuốc đạt 100%  │
│   - Tỷ lệ dịch đúng tên thuốc chuyên biệt đạt >= 95%        │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
│ *Lưu ý logic: OCR thông thường thất bại hoàn toàn với chữ    │
│ viết tay bác sĩ. LLM kết hợp tri thức y khoa để suy luận    │
│ chính xác tên thuốc. Dược sĩ bắt buộc phải phê duyệt (HITL). │
└─────────────────────────────────────────────────────────────┘
```
