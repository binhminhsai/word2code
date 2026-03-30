# Tips chạy LLM open source gen code trên Mac mini M4 32GB

## 1. Chọn công cụ chạy LLM (runtime)

- Ưu tiên mấy tool đã tối ưu cho Apple Silicon (Metal / Neural Engine):  
  - **Ollama**: cài qua Homebrew `brew install ollama`, hỗ trợ nhiều model, có API local.
  - **LM Studio**: giao diện GUI dễ xài, có sẵn OpenAI-compatible server để gọi từ editor/IDE. 
  - **mlx-lm** (Apple MLX): dành cho dev muốn tận dụng GPU Apple Silicon tối đa, dùng tốt cho code assistant trong terminal hoặc tool như OpenCode. 

- Nguyên tắc:  
  - Dùng Ollama/LM Studio cho “xài là chạy”.  
  - Dùng mlx-lm khi cần vọc sâu / custom pipeline / tự host API.

---

## 2. Chọn model chuyên code, hợp cấu hình 32GB

- Với 32GB unified memory trên Mac mini M4, có thể chạy tốt các model code 7B–14B (4bit/8bit) và một số 30B đã nén (8bit + offload). 
- Nên ưu tiên các model chuyên code (đã fine‑tune):  
  - Qwen / Qwen-Coder / Qwen3-Coder (7B–14B, nếu 30B thì dùng bản 8bit/AWQ). 
  - StarCoder2, DeepSeek-Coder, CodeLlama nếu runtime hỗ trợ. 

**Rule of thumb:**

- Ưu tiên model:  
  - `*-Coder*`, `*-Code*`, `*-Instruct*`.  
- Độ lớn đề xuất:  
  - 7B: nhanh, đủ dùng cho project nhỏ.  
  - 14B: cân bằng chất lượng/tốc độ cho codebase vừa.  
  - 30B 8bit: dùng khi cần chất lượng cao, chấp nhận chậm hơn và tốn RAM. 

---

## 3. Thiết lập tham số để chạy mượt

### 3.1. Context / sequence length

- Optimize context 32k–128k khi gen code; khởi điểm:  
  - `context_length`: 8k–16k là đủ cho hầu hết session coding. 
- Khi IDE cần đọc nhiều file:  
  - Tăng context dần, chỉ khi thấy model “quên” file quan trọng.  

### 3.2. Temperature và decoding

- Gen code ưu tiên tính **ổn định** hơn sáng tạo:  
  - `temperature`: 0.1–0.3.  
  - `top_p`: 0.8–0.95.  
  - `top_k`: 20–50.   
- Bật “stop tokens” đúng (vd: `</s>`, `</assistant>`, hoặc cặp ``` ``` nếu gen code block) để tránh model chạy vô hạn.

### 3.3. Batch size, GPU offload

- Để mượt trên M4 32GB:  
  - Tắt multi‑client nặng (đừng để 3–4 app cùng hỏi model).  
  - Nếu runtime hỗ trợ:  
    - Offload nhiều layer model lên GPU/Neural Engine.
    - Giảm batch size nếu thấy máy nóng + tụt tốc độ.  

---

## 4. Tối ưu macOS cho workload LLM

- Cập nhật macOS bản mới để dùng driver/Metal tối ưu cho M4. 
- Tắt bớt app ngầm khi chạy model:  
  - Chrome tab nặng, app Electron, máy ảo, Docker.  
- Cắm sạc, đặt chế độ performance (không dùng khi pin yếu với MacBook; Mac mini thì ổn định nguồn sẵn).  
- Dùng terminal profile nhẹ (iTerm2, zsh + ít plugin) để tránh ăn CPU không cần.

---

## 5. Gắn LLM vào editor/IDE

- VS Code / Cursor / Zed / JetBrains:  
  - Trỏ API endpoint về local runtime (Ollama, LM Studio server, mlx-lm server). 
  - Giữ prompt ngắn, chỉ gửi:  
    - File hiện tại.  
    - Một ít context quanh đoạn code, đừng gửi cả repo mỗi lần.  

- Pattern prompt gợi ý cho gen code:

  - “You are a coding assistant running on my local Mac. Only respond with valid code in ```language``` blocks, minimal explanation.”  
  - Khi refactor/test: gửi trước test + đoạn code liên quan.

---

## 6. Quy trình sử dụng để máy luôn mát và mượt

1. Chọn 1 runtime chính (vd: Ollama).  
2. Chọn 1–2 model code chính (7B nhanh, 14B/30B chất lượng).  
3. Giới hạn context ~8k–16k, temperature thấp.  
4. Đóng bớt app nặng, không mở nhiều IDE cùng chat với model.  
5. Chỉ bật thêm model lớn hơn khi thực sự cần (review kiến trúc, sinh nhiều file).
6. Dung mac mini nhu mot host AI, tao API gateway toi may va khong code truc tiep tren do

---

## 7. Ghi chú bảo trì / update

- Thường xuyên pull/cập nhật model mới: nhiều model code 2025–2026 (Qwen3, DeepSeek, v.v.) tối ưu hơn rất nhiều so với đời CodeLlama cũ. 
- Đọc release note của runtime (Ollama, LM Studio, mlx-lm) vì thường có cải thiện lớn về tốc độ trên Apple Silicon sau mỗi bản cập nhật. 

---
