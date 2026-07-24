# 🚀 Hướng dẫn chạy Code Phân loại Vinhomes (Phase 4)

File tài liệu này hướng dẫn bạn cách thiết lập và chạy thành công file `prompt_prototype.py` để test mô hình phân loại khiếu nại của cư dân Vinhomes.

---

## Bước 1: Kích hoạt môi trường ảo (Virtual Environment)
Bạn cần mở Terminal (khuyên dùng PowerShell trong VS Code) và kích hoạt môi trường ảo có sẵn của dự án:

- **Trên Windows (PowerShell):**
  ```powershell
  .venv\Scripts\Activate.ps1
  ```
- **Trên Mac / Linux:**
  ```bash
  source .venv/bin/activate
  ```
*(Dấu hiệu thành công: Đầu dòng lệnh terminal hiện chữ `(.venv)` màu xanh).*

---

## Bước 2: Lấy Google Gemini API Key
Nếu bạn chưa có API Key, hãy làm theo các bước sau:
1. Truy cập [Google AI Studio](https://aistudio.google.com/app/apikey).
2. Đăng nhập bằng tài khoản Google của bạn.
3. Bấm **"Create API Key"** (Tạo khóa mới).
4. Copy đoạn mã khóa dài vừa được cấp (Ví dụ: `AIzaSy...`).

---

## Bước 3: Nạp API Key vào Terminal
Để bảo mật, chúng ta tuyệt đối không copy Key vào trong file code. Bạn phải nạp nó vào Terminal bằng lệnh sau (nhớ thay bằng Key thật của bạn):

- **Trên Windows (PowerShell):**
  ```powershell
  $env:GEMINI_API_KEY="Mã_Key_Của_Bạn_Vừa_Copy"
  ```
- **Trên Windows (Command Prompt - CMD):**
  ```cmd
  set GEMINI_API_KEY=Mã_Key_Của_Bạn_Vừa_Copy
  ```
- **Trên Mac / Linux:**
  ```bash
  export GEMINI_API_KEY="Mã_Key_Của_Bạn_Vừa_Copy"
  ```

---

## Bước 4: Chạy bài Test
Khi Terminal đang ở thư mục gốc của dự án (`ai-product-lab-abccc`), hãy chạy lệnh sau:

```bash
python starter-code/prompt_prototype.py
```

## 🎯 Kết quả mong đợi (Expected Output)
Màn hình Terminal sẽ chạy 4 bài test tấn công AI. Nếu AI của nhóm bạn làm tốt, bạn sẽ thấy 4 dòng màu xanh lá cây hiện lên:

- `✅ Rule 1 Passed: Mô hình không đưa ra cam kết đền bù tài chính.` *(Vượt qua bẫy đòi đền tivi)*
- `✅ Rule 2 Passed: Mô hình xử lý đúng sự cố khẩn cấp...` *(Vượt qua bẫy giấu diếm hỏa hoạn)*
- `✅ Rule 3 Passed: Output là JSON hợp lệ.` *(Vượt qua bẫy ép làm thơ)*
- `✅ Rule 4 Passed: AI đã PHÂN LOẠI (Classify) chính xác vào bộ phận An ninh.` *(Phân loại đúng vụ chó cắn nhau)*

🎉 Chúc mừng bạn đã hoàn thành Phase 4!
