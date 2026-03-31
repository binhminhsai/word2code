# Nghiên cứu Model AI hỗ trợ Coding và Framework Open-Source cho bài toán migrate WordPress → React

## 1. Giới thiệu

Trong quá trình phát triển hệ thống **AI hỗ trợ migrate code từ WordPress sang React**, nhóm cần lựa chọn một **Large Language Model (LLM)** có khả năng:

- Sinh mã nguồn (Code Generation)
- Phân tích và giải thích code
- Refactor và debug code
- Chuyển đổi code giữa các ngôn ngữ hoặc framework
- Có khả năng **fine-tune theo domain cụ thể**

Ngoài ra hệ thống cần:

- Chạy được bằng **framework open-source**
- Có thể **self-host**
- Chi phí vận hành thấp (khoảng **20–30 USD/tháng**)

Do đó nhóm tiến hành nghiên cứu các **AI Coding Models phổ biến**, bao gồm cả **proprietary models (GPT, Gemini)** và **open-source models**.

---

# 2. Các mô hình AI hỗ trợ Coding

## 2.1 GPT (OpenAI)

GPT là dòng **Large Language Model** do OpenAI phát triển.

Các model phổ biến:

- GPT-4
- GPT-4 Turbo
- GPT-4o

### Ưu điểm

- Khả năng hiểu code rất tốt  
- Code generation chất lượng cao  
- Hỗ trợ nhiều ngôn ngữ lập trình  
- Reasoning mạnh  

### Nhược điểm

- Không phải open-source  
- Không thể self-host  
- Chi phí API cao  

Do đó GPT **không phù hợp cho hệ thống cần self-host và fine-tune**.

---

## 2.2 Gemini (Google)

Gemini là mô hình AI do Google phát triển.

Các model:

- Gemini 1.5 Pro
- Gemini 1.5 Flash

### Ưu điểm

- Context window lớn  
- Reasoning tốt  
- Khả năng hiểu code tốt  

### Nhược điểm

- Không phải open-source  
- Phải sử dụng qua API  
- Không thể fine-tune trực tiếp  

Do đó Gemini **không phù hợp với yêu cầu self-host của hệ thống**.

---

# 3. Các Model Coding Open-Source

## 3.1 DeepSeek-Coder

DeepSeek-Coder là coding model open-source được phát triển bởi DeepSeek AI.

### Đặc điểm

- Training trên lượng lớn code từ GitHub  
- Tối ưu cho code completion và generation  
- Context window lớn  

### Ưu điểm

- Open-source  
- Hiệu năng tốt trong code generation  

### Nhược điểm

- Khả năng reasoning chưa mạnh bằng các model mới

---

## 3.2 CodeLlama

CodeLlama là model coding do **Meta** phát triển.

### Đặc điểm

- Phiên bản mở rộng của LLaMA  
- Tối ưu cho code generation  

### Ưu điểm

- Open-source  
- Hỗ trợ nhiều ngôn ngữ lập trình  

### Nhược điểm

- Context window nhỏ hơn các model mới

---

## 3.3 StarCoder2

StarCoder2 được phát triển bởi **BigCode và HuggingFace**.

### Đặc điểm

- Training trên dataset code lớn  
- Hỗ trợ nhiều ngôn ngữ lập trình  

### Ưu điểm

- Open-source  
- Tích hợp tốt với hệ sinh thái HuggingFace  

### Nhược điểm

- Hiệu năng coding chưa mạnh bằng các model mới như Qwen hoặc DeepSeek

---

# 4. So sánh các Coding Models

| Model | Open Source | Coding Ability | Context Window | Self-Host |
|------|-------------|---------------|---------------|-----------|
| GPT-4 | ❌ | Rất mạnh | Lớn | ❌ |
| Gemini | ❌ | Rất mạnh | Rất lớn | ❌ |
| DeepSeek-Coder | ✅ | Tốt | Lớn | ✅ |
| CodeLlama | ✅ | Khá | Trung bình | ✅ |
| StarCoder2 | ✅ | Khá | Trung bình | ✅ |
| **Qwen2.5-Coder** | ✅ | **Rất tốt** | **128k** | ✅ |

Sau khi so sánh, nhóm quyết định lựa chọn **Qwen2.5-Coder**.

---

# 5. Model được lựa chọn: Qwen2.5-Coder

Qwen2.5-Coder là coding **Large Language Model** do **Alibaba Cloud** phát triển.

Model được huấn luyện trên:

- mã nguồn từ GitHub  
- tài liệu lập trình  
- nhiều ngôn ngữ lập trình khác nhau  

Các tác vụ model hỗ trợ:

- Code generation  
- Code completion  
- Code explanation  
- Code debugging  
- Code migration  

---

# 6. Lý do lựa chọn Qwen2.5-Coder

## 6.1 Hỗ trợ nhiều ngôn ngữ lập trình

Model hỗ trợ hơn **90+ ngôn ngữ**, bao gồm:

### Stack WordPress (hệ thống cũ)

- PHP  
- HTML  
- CSS  

### Stack hệ thống mới

- JavaScript  
- TypeScript  
- React  

Nhờ vậy model có khả năng **hiểu cấu trúc code WordPress và chuyển đổi sang React component**.

---

## 6.2 Context Window lớn

Qwen2.5-Coder hỗ trợ **context window lên đến 128k tokens**.

Điều này cho phép:

- đọc các file template WordPress dài  
- hiểu logic PHP  
- phân tích codebase lớn  

---

## 6.3 Khả năng Fine-Tune

Vì là **model open-source**, Qwen2.5-Coder có thể **fine-tune theo domain**.

Ví dụ dataset:

WordPress template → React component

### Training sample

Input:

Convert WordPress template to React component

```php
<h1><?php the_title(); ?></h1>
<p><?php the_content(); ?></p>
```

Output:

```jsx
function Post({ title, content }) {
  return (
    <>
      <h1>{title}</h1>
      <p>{content}</p>
    </>
  );
}
```

Fine-tuning giúp model:

- hiểu cấu trúc WordPress theme  
- hiểu React component  
- tăng độ chính xác khi migrate code  

---

# 7. Framework chạy Open-Source

Để triển khai các model open-source cần sử dụng **LLM Serving Framework**.

## 7.1 Ollama

Ollama là framework đơn giản để chạy LLM.

Ưu điểm:

- cài đặt nhanh  
- dễ sử dụng  
- có REST API  

Ví dụ:

```bash
ollama run qwen2.5-coder
```

Phù hợp cho **prototype và development**.

---

## 7.2 vLLM

vLLM là framework serving LLM hiệu năng cao.

Tính năng:

- tối ưu GPU memory  
- throughput cao  
- hỗ trợ OpenAI compatible API  

Ví dụ:

```bash
python -m vllm.entrypoints.openai.api_server \
--model Qwen/Qwen2.5-Coder-7B
```

➡ **vLLM được lựa chọn làm framework chính cho hệ thống.**

---

## 7.3 HuggingFace Text Generation Inference (TGI)

TGI là framework deploy LLM của HuggingFace.

Tính năng:

- deploy production  
- scale bằng Kubernetes  
- hỗ trợ quantization  

---

# 8. Hạ tầng GPU

Model **Qwen2.5-Coder-7B** có thể chạy trên các GPU phổ biến:

| GPU | VRAM |
|----|----|
| NVIDIA T4 | 16GB |
| NVIDIA L4 | 24GB |
| RTX 4090 | 24GB |

### Lựa chọn GPU

Nhóm lựa chọn **NVIDIA T4** vì:

- đủ VRAM chạy model 7B  
- chi phí thấp  
- phổ biến trên cloud GPU  

---

# 9. Nền tảng thuê GPU

Một số nền tảng hỗ trợ thuê GPU:

- RunPod  
- Vast.ai  
- Lambda Labs  

Chi phí trung bình:

**0.2 – 0.3 USD / giờ**

Nếu hệ thống chạy vài giờ mỗi ngày:

≈ **20 – 30 USD / tháng**

Chi phí này phù hợp cho:

- nghiên cứu  
- đồ án sinh viên  
- prototype system  

---

# 10. Kiến trúc triển khai hệ thống

Architecture đề xuất:

User  
↓  
AI Coding Service  
↓  
vLLM Serving Framework  
↓  
Qwen2.5-Coder-7B  
↓  
GPU NVIDIA T4  
↓  
Cloud GPU Provider (RunPod / Vast.ai)

---

# 11. Kết luận

Sau khi nghiên cứu các AI coding models và framework triển khai, nhóm lựa chọn kiến trúc hệ thống như sau:

Model: **Qwen2.5-Coder-7B**  
Framework: **vLLM**  
GPU: **NVIDIA T4 (16GB)**  
Cloud GPU: **RunPod hoặc Vast.ai**

Giải pháp này cho phép:

- chạy model open-source  
- self-host hệ thống  
- hỗ trợ fine-tune  
- chi phí vận hành thấp (~20–30 USD/tháng)

Hệ thống phù hợp để xây dựng **AI hỗ trợ tự động chuyển đổi code từ WordPress sang React**, giúp giảm đáng kể thời gian và công sức trong quá trình migrate hệ thống.