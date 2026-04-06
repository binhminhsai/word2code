# Đề Xuất: Pipeline AI cho Bài Toán Migrate WordPress → React

## 1. Mục tiêu

Cho mốc **demo cuối tháng 4/2026**, mục tiêu của nhóm là xây dựng một pipeline AI hỗ trợ **migrate giao diện WordPress sang React**.

Để cân bằng giữa **chất lượng, chi phí và tốc độ phát triển**, nhóm đề xuất sử dụng **pipeline hybrid gồm nhiều model AI**, thay vì self-host model lớn ở giai đoạn này.

---

## 2. Kiến trúc Model Đề Xuất

Pipeline đề xuất sử dụng **2 loại model AI** cho các nhiệm vụ khác nhau:

| Bước trong Pipeline      | Model                         | Vai trò                                              |
| ------------------------ | ----------------------------- | ---------------------------------------------------- |
| Phân tích & lập kế hoạch | GPT-5.2                       | Phân tích cấu trúc WordPress và lập kế hoạch migrate |
| Sinh code                | Devstral 2 hoặc GPT-5.3 Codex | Sinh React components                                |
| Sửa lỗi code             | Devstral 2 hoặc GPT-5.3 Codex | Fix lỗi TypeScript / import / props                  |
| Review code              | GPT-5.2                       | Kiểm tra code có bám theo plan và kiến trúc không    |

---

## 3. Hai Lựa Chọn Cho Bước Generate Code

### Option 1: Devstral 2 — _Cost/Performance tốt_

Devstral 2 là model code agent của Mistral AI, được thiết kế cho các tác vụ software engineering.

**Ưu điểm:**

- Chi phí thấp
- Sinh code nhanh
- Hỗ trợ chỉnh sửa nhiều file
- Phù hợp cho pipeline generate code hàng loạt

**Nhược điểm:**

- Reasoning code không mạnh bằng các model cao cấp hơn

**Phù hợp khi:**

- Cần tối ưu chi phí
- Pipeline cần chạy nhiều lần

---

### Option 2: GPT-5.3 Codex — _Chất lượng cao nhất_

GPT-5.3 Codex là model coding agent của OpenAI, được thiết kế cho các tác vụ software engineering phức tạp. Model này đạt kết quả **state-of-the-art trên các benchmark coding như SWE-Bench Pro và Terminal-Bench**.

**Ưu điểm:**

- Rất mạnh ở multi-file refactor
- Sửa bug tốt
- Hiểu cấu trúc repository
- Phù hợp cho coding agent workflow
- Context window lớn (~400k tokens), tối ưu cho workflow coding dài

**Nhược điểm:**

- Chi phí cao hơn Devstral
- Latency lớn hơn

---

## 4. Pipeline Đề Xuất

```
Plan     → GPT-5.2
Gen code → Devstral 2 hoặc GPT-5.3 Codex
Fix code → Devstral 2 hoặc GPT-5.3 Codex
Review   → GPT-5.2
```

---

## 5. Chiến Lược Tối Ưu Chi Phí (Hybrid)

Dùng **model rẻ để generate code** và **model mạnh để fix bug**:

```
Plan     → GPT-5.2
Gen code → Devstral 2
Fix code → GPT-5.3 Codex
Review   → GPT-5.2
```

Cách này giúp:

- Giảm chi phí pipeline
- Vẫn giữ chất lượng debugging cao

---

## 6. Vì Sao Không Self-Host Model Lúc Này

Một số model code agent yêu cầu **GPU mạnh để self-host**, thường ở mức **multi-GPU H100 class**.

Chi phí cloud GPU hiện tại:

```
$8 – $13 / giờ cho 4 GPU H100
```

Chưa bao gồm: setup inference server, monitoring, autoscaling, vận hành hệ thống.

Với deadline demo gần, sử dụng **API của nhà cung cấp model** sẽ giảm rủi ro vận hành hạ tầng.

---

## 7. Quyết Định Đề Xuất

Cho mốc **demo cuối tháng 4/2026**, đề xuất:

- Sử dụng **GPT-5.2 cho plan và review**
- Cho bước generate code có **hai lựa chọn:**
  - **Devstral 2** — tối ưu chi phí
  - **GPT-5.3 Codex** — tối ưu chất lượng

Mentor có thể lựa chọn model phù hợp tùy theo ưu tiên chi phí hoặc chất lượng.

---

## 8. Tóm Tắt

```
GPT-5.2        → planning + review
Devstral 2     → generate code + fix code  (phương án tiết kiệm)
GPT-5.3 Codex  → generate code + fix code  (phương án chất lượng cao)
```

Phương án này giúp cân bằng giữa:

- Chất lượng code
- Chi phí vận hành
- Độ đơn giản của hạ tầng
- Khả năng mở rộng pipeline sau demo

---

## 9. Ước Tính Chi Phí AI Cho Pipeline

Token estimate cho **1 lần migrate một page WordPress**:

| Bước     | Model    | Input       | Output     | Chi phí           |
| -------- | -------- | ----------- | ---------- | ----------------- |
| Planning | GPT-5.2  | ~10k tokens | ~2k tokens | ~$0.04            |
| Gen code | Devstral | ~12k tokens | ~6k tokens | ~$0.015           |
| Fix code | Devstral | —           | —          | ~$0.01            |
| Review   | GPT-5.2  | —           | —          | ~$0.03            |
| **Tổng** |          |             |            | **~$0.10 / page** |

---

## 10. Ước Tính Chi Phí Cho Demo

Giả sử demo migrate **20 WordPress pages**:

```
20 × $0.10 ≈ $2
```

Nếu pipeline chạy nhiều lần để refine code:

```
≈ $10 – $30
```

---

## 11. AI Budget Đề Xuất Cho Demo

| Kịch bản                     | Chi phí        |
| ---------------------------- | -------------- |
| Ước tính cơ bản              | ~$10 – $30     |
| **Budget đề xuất (an toàn)** | **$50 – $100** |

---

## 12. API Cần Sử Dụng

| API         | Model      |
| ----------- | ---------- |
| OpenAI API  | GPT-5.2    |
| Mistral API | Devstral 2 |

> Không cần thuê GPU.

---

## 13. Kết Luận

Đối với **demo cuối tháng 4/2026**:

| Hạng mục       | Quyết định |
| -------------- | ---------- |
| Infrastructure | Dùng API   |
| AI budget      | $50 – $100 |
| Self-host GPU  | Không cần  |

**Pipeline cuối cùng:**

```
Plan   → GPT-5.2
Gen    → Devstral 2
Fix    → Devstral 2
Review → GPT-5.2
```

**Ưu điểm:**

- Chi phí AI rất thấp
- Không cần vận hành GPU
- Triển khai nhanh
- Ổn định cho demo
- Dễ mở rộng pipeline sau này
