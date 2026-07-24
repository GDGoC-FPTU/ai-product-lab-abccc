| #   | Subsidiary   | Lens          | Mô tả ngắn bài toán                                                             |
| --- | ------------ | ------------- | ------------------------------------------------------------------------------- |
| 1   | **Vinhomes** | Lặp lại       | Nhân viên phân loại & Điều hướng phản ánh cư đến đúng bộ phận chịu trách nhiệm. |
| 2   | **Vinhomes** | Tốn thời gian | Kiểm tra hồ sơ đăng kí thi công nội thất.                                       |
| 3   | **Vihomes**  | Lặp lại       | So khớp hóa đơn sạc điện và đối chiếu số liệu trạm sạc đối tác hằng tuần.       |
| 4   | **Vinhomes** | AI-upgrade    | Chatbot hỗ trợ cư dân.                                                          |
| 5   | **Vinhomes** | lặp lại       | Thu thập báo cáo sự cố từ nhiều nguồn rồi tổng hợp vào Excel mỗi ngày/tuần.     |

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Phân loại & điều hướng phản ánh cư dân            │
│ đến đúng bộ phận xử lý.                                    │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau? Cư dân (chờ xử lý), Nhân viên CSKH,            │
│ Ban quản lý tòa nhà.                                       │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Cư dân gửi phản ánh qua App Resident                  │
│   → 2. Nhân viên đọc nội dung phản ánh                     │
│   → 3. Xác định loại sự cố và mức độ ưu tiên               │
│   → 4. Chuyển ticket đến đúng bộ phận                      │
│   → 5. Bộ phận kỹ thuật tiếp nhận và xử lý                 │
│                                                             │
│ Bước nào tốn nhất? Bước 2-4 (⏱ 3–5 phút/lượt)            │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4             │
│ (Phân loại phản ánh → Gán mức ưu tiên →                    │
│ Gợi ý bộ phận xử lý → Draft ticket)                        │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                      │
│ Giảm thời gian phân loại từ 5 phút xuống dưới 30 giây.     │
│ Độ chính xác phân loại >95%.                               │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Kiểm tra hồ sơ đăng ký thi công nội thất          │
│ của cư dân trước khi phê duyệt.                             │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau? Cư dân (chờ duyệt), Ban quản lý.               │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Cư dân nộp hồ sơ qua App/Email                         │
│   → 2. Nhân viên mở từng file PDF                           │
│   → 3. Kiểm tra đủ giấy tờ và thông tin                     │
│   → 4. Ghi chú hồ sơ thiếu hoặc sai                         │
│   → 5. Gửi phản hồi cho cư dân                              │
│                                                             │
│ Bước nào tốn nhất? Bước 2-4 (⏱ 10–20 phút/hồ sơ)           │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4             │
│ (OCR tài liệu → Kiểm tra checklist →                       │
│ Highlight giấy tờ thiếu → Draft phản hồi)                  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                      │
│ Giảm thời gian kiểm tra từ 15 phút xuống dưới 3 phút.      │
│ Giảm 80% hồ sơ phải kiểm tra thủ công.                      │
│                                                             │
│ Quick Architecture: [x] AI Workflow (OCR + LLM)             │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Trợ lý AI trả lời câu hỏi thường gặp              │
│ của cư dân về dịch vụ và thủ tục.                           │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau? Cư dân (chờ phản hồi), Nhân viên CSKH.         │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Cư dân gửi câu hỏi qua App/Hotline                     │
│   → 2. Nhân viên tiếp nhận yêu cầu                          │
│   → 3. Tra cứu quy định hoặc tài liệu                       │
│   → 4. Soạn câu trả lời                                     │
│   → 5. Gửi phản hồi cho cư dân                              │
│                                                             │
│ Bước nào tốn nhất? Bước 3-4 (⏱ 3–7 phút/yêu cầu)           │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4             │
│ (Tra cứu nội quy → Sinh câu trả lời →                      │
│ Nhân viên duyệt trước khi gửi nếu cần)                     │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                      │
│ Giảm thời gian phản hồi từ 5 phút xuống dưới 30 giây.      │
│ Tự động xử lý ≥70% câu hỏi FAQ.                             │
│                                                             │
│ Quick Architecture: [x] RAG + LLM Assistant                 │
└─────────────────────────────────────────────────────────────┘
```
