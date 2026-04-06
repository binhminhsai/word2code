# 12. Nghiên cứu cách setup model Open-Source trên Mac Mini

## 12.1 Mục tiêu

Ngoài phương án triển khai model trên cloud GPU, nhóm cũng nghiên cứu khả năng **chạy model AI open-source trực tiếp trên máy cá nhân**, cụ thể là **Mac Mini sử dụng Apple Silicon (M1/M2/M3)**.

Việc chạy model trên Mac Mini phù hợp cho:

- nghiên cứu và thử nghiệm model
- phát triển AI pipeline
- test prompt engineering
- kiểm thử code generation
- demo hệ thống

Mac Mini sử dụng **Apple Silicon GPU** với công nghệ **Metal**, cho phép tăng tốc tính toán AI mà không cần GPU rời như NVIDIA.

---

# 12.2 Các framework hỗ trợ chạy LLM trên Mac

Một số framework phổ biến có thể chạy Large Language Model trên Mac:

| Framework | Hỗ trợ Mac | GPU Acceleration | Mức độ cài đặt |
|-----------|------------|------------------|---------------|
| Ollama | Có | Metal | Rất dễ |
| LM Studio | Có | Metal | Dễ |
| llama.cpp | Có | Metal | Trung bình |
| vLLM | Hạn chế | Không tối ưu | Khó |

Sau khi nghiên cứu, nhóm lựa chọn **Ollama** để chạy model trên Mac Mini.

---

# 12.3 Giới thiệu Ollama

Ollama là framework cho phép **chạy Large Language Model locally** trên máy cá nhân.

Ưu điểm của Ollama:

- cài đặt nhanh
- hỗ trợ nhiều model open-source
- tối ưu cho Apple Silicon
- có REST API
- dễ tích hợp vào backend service

Một số model được Ollama hỗ trợ:

- Qwen2.5-Coder
- LLaMA
- CodeLlama
- DeepSeek-Coder
- Mistral

---

# 12.4 Cài đặt Ollama trên Mac Mini

### Bước 1: Cài đặt Ollama

Có thể cài đặt bằng Homebrew:

```bash
brew install ollama
```

Hoặc tải trực tiếp từ website:

https://ollama.com

Sau khi cài đặt xong, kiểm tra phiên bản:

```bash
ollama --version
```

---

# 12.5 Chạy coding model

Ví dụ chạy model **Qwen2.5-Coder**

```bash
ollama run qwen2.5-coder
```

Khi chạy lệnh trên, Ollama sẽ:

1. tải model từ registry
2. lưu model vào máy local
3. chạy model bằng GPU Apple Silicon

Sau khi tải xong, có thể tương tác trực tiếp với model bằng command line.

Ví dụ:

```
>>> Write a React component for login form
```

---

# 12.6 Sử dụng API của Ollama

Ollama cung cấp **REST API** để tích hợp vào hệ thống backend.

Ví dụ gọi API:

```bash
curl http://localhost:11434/api/generate \
-d '{
  "model": "qwen2.5-coder",
  "prompt": "Convert PHP code to React component"
}'
```

API này có thể được tích hợp vào:

- NodeJS backend
- Python service
- microservices architecture

---

# 12.7 Cấu hình phần cứng đề xuất

Để chạy coding model hiệu quả trên Mac Mini:

| Thiết bị | RAM đề xuất |
|--------|-------------|
| Mac Mini M1 | 16GB |
| Mac Mini M2 | 16GB |
| Mac Mini M3 | 24GB |

Các model có thể chạy tốt trên Mac:

- Qwen2.5-Coder 7B (quantized)
- CodeLlama 7B
- DeepSeek-Coder 6.7B

Các model lớn hơn như **32B hoặc 70B** sẽ cần GPU server.

---

# 12.8 Vai trò của Mac Mini trong hệ thống

Mac Mini chủ yếu được sử dụng cho:

- phát triển AI pipeline
- thử nghiệm prompt
- test code generation
- chuẩn bị dataset fine-tune

Sau khi hệ thống hoàn thiện, model sẽ được deploy lên **cloud GPU server** để chạy production.

---

# 12.9 Kết luận

Mac Mini là môi trường phù hợp để **nghiên cứu và thử nghiệm các model AI open-source**.

Framework được sử dụng:

Ollama

Model chạy local:

Qwen2.5-Coder

Giải pháp này giúp:

- giảm chi phí thử nghiệm
- phát triển AI pipeline nhanh
- kiểm thử model trước khi deploy lên GPU server.