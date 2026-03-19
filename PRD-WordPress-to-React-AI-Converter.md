# PRD: WordPress → React AI Converter

**Version**: 1.0  
**Date**: March 18, 2026  
**Status**: Draft  
**Author**: Product Team  

---

## 1. Tổng quan sản phẩm (Product Overview)

### Tên sản phẩm
**WP2React** — AI-powered WordPress to React Converter

### Tuyên bố vấn đề (Problem Statement)
Hàng triệu doanh nghiệp đang vận hành website trên WordPress nhưng muốn chuyển sang React để có hiệu suất cao hơn, khả năng tùy biến tốt hơn và trải nghiệm người dùng hiện đại hơn. Quá trình chuyển đổi hiện tại đòi hỏi nhiều tháng phát triển thủ công, chi phí cao, dễ mất nội dung và không đảm bảo độ chính xác về giao diện. Không có công cụ nào trên thị trường có thể:

- Đọc trực tiếp database WordPress và tái tạo giao diện pixel-perfect
- Giữ nguyên toàn bộ nội dung (bài viết, trang, menu, v.v.)
- Cho phép chỉnh sửa trực tiếp từ giao diện visual
- Thực hiện tất cả chỉ trong một phiên chat với AI

---

## 2. Mục tiêu (Goals)

### Mục tiêu kinh doanh
- Phục vụ phân khúc SME/agency muốn migrate khỏi WordPress
- Rút ngắn thời gian chuyển đổi từ hàng tháng xuống còn vài giờ
- Tạo sản phẩm SaaS có thể charge per-conversion hoặc subscription

### Mục tiêu người dùng
- Import database WordPress → nhận React app hoàn chỉnh trong < 30 phút
- Không cần viết code thủ công
- Preview real-time ngay trong ứng dụng
- Chỉnh sửa trực tiếp bằng cách click vào element trên giao diện

### Chỉ số thành công (Success Metrics)
| Metric | Mục tiêu |
|--------|----------|
| Tỷ lệ chuyển đổi thành công | ≥ 85% với database WordPress chuẩn |
| Thời gian tạo site hoàn chỉnh | < 30 phút từ import đến preview |
| Pixel accuracy so với bản gốc | ≥ 90% visual match |
| CSAT sau lần dùng đầu | ≥ 4.2/5 |

---

## 3. Hành trình người dùng (User Journey)

```
[Import Database] → [Nhập yêu cầu qua Chat] → [AI Code + Preview song song] → [Visual Edit] → [Export/Deploy]
```

### Bước 1 — Import Database
- User upload file `.sql` dump từ WordPress (hoặc paste connection string)
- Hệ thống tự phân tích cấu trúc: posts, pages, themes, menus, media, plugins
- Hiển thị preview tóm tắt: "Tìm thấy 47 bài viết, 12 trang tĩnh, 3 menu"

### Bước 2 — Chat với AI
- Giao diện chat đơn giản, user mô tả yêu cầu (ngôn ngữ tự nhiên)
- Ví dụ: "Chuyển sang React, giữ nguyên giao diện, dùng màu xanh navy"
- Hỗ trợ cả tiếng Việt và tiếng Anh

### Bước 3 — AI Code + Preview song song
- Màn hình chia đôi:
  - **Trái**: Panel AI với log tiến trình coding theo từng bước
  - **Phải**: Preview panel — ban đầu là skeleton loader, sau đó render live khi code xong
- User thấy AI "đang làm việc" theo thời gian thực

### Bước 4 — Visual Edit
- Sau khi preview sẵn sàng, user hover chuột lên bất kỳ element nào
- Xuất hiện nút chỉnh sửa (icon bút/edit) ngay tại vị trí đó
- Click vào → mở panel chat chỉ nói về element đó
- AI chỉ sửa đúng component tương ứng, không ảnh hưởng phần còn lại

### Bước 5 — Export/Deploy
- Download toàn bộ React project dưới dạng .zip
- Hoặc deploy thẳng lên Vercel/Netlify trong 1 click

---

## 4. Tính năng chi tiết (Feature Specifications)

### 4.1 Import Module

#### F-01: WordPress Database Import
**Mô tả**: Nhận và phân tích WordPress database dump

**Acceptance Criteria** (Gherkin):
```gherkin
Given user đã có file .sql từ WordPress
When user kéo thả file vào drop zone hoặc click upload
Then hệ thống validate file: phát hiện bảng wp_posts, wp_options, wp_users
And hiển thị summary card: số posts, pages, media attachments, active theme
And enable nút "Bắt đầu chuyển đổi"

Given file upload không phải WordPress database
When hệ thống phân tích
Then hiển thị error message rõ ràng: "Không phát hiện cấu trúc WordPress. Vui lòng kiểm tra lại."
```

**Dữ liệu được trích xuất**:
- `wp_posts`: Bài viết, trang, menus, media
- `wp_options`: Theme settings, site title, description, permalinks
- `wp_postmeta`: Custom fields, featured images
- `wp_terms` + `wp_term_taxonomy`: Categories, tags
- `wp_users`: Author info
- CSS/theme files được reference trong DB

---

#### F-02: Theme Detection & Analysis
**Mô tả**: AI đọc active theme, phân tích layout structure

**Logic**:
1. Đọc `wp_options` → `template` và `stylesheet` → biết tên theme
2. Tra cứu theme structure từ database metadata
3. Parse PHP template hierarchy (header.php, footer.php, page.php, single.php)
4. Trích xuất CSS variables, color palette, typography
5. Map sang React component structure

---

### 4.2 Chat Interface

#### F-03: AI Chat Panel
**Mô tả**: Giao diện chat để user ra lệnh

**Yêu cầu UI**:
```
┌─────────────────────────────┐
│  📎 database.sql  ✓ Đã load │
├─────────────────────────────┤
│                             │
│  [Lịch sử chat]             │
│                             │
├─────────────────────────────┤
│  ✏️ Nhập yêu cầu...    [↵] │
└─────────────────────────────┘
```

**Acceptance Criteria**:
```gherkin
Given database đã được import thành công
When user gõ lệnh và nhấn Enter
Then AI phân tích yêu cầu và bắt đầu generate code
And giao diện chuyển sang màn hình Split View (F-04)
And chat history vẫn hiển thị ở panel trái
```

**Các lệnh được hỗ trợ (ví dụ)**:
- "Chuyển toàn bộ sang React, giữ nguyên giao diện"
- "Đổi font sang Inter, màu primary sang #2563EB"
- "Thêm dark mode"
- "Tối ưu mobile responsive cho trang chủ"

---

### 4.3 Split View — AI Code + Preview

#### F-04: Split View Layout
**Mô tả**: Màn hình chia đôi hiển thị quá trình AI code và preview

**Layout**:
```
┌────────────────────┬────────────────────┐
│  AI Progress Panel │   Preview Panel    │
│                    │                    │
│  ✓ Phân tích DB   │  [Skeleton loader] │
│  ✓ Tạo components │                    │
│  ⏳ Build routes  │  → Live preview    │
│  ○ Optimize CSS   │     khi xong       │
│                    │                    │
│  [Log detail...]  │                    │
└────────────────────┴────────────────────┘
```

#### F-05: AI Progress Panel (Trái)
**Mô tả**: Hiển thị tiến trình AI coding theo thời gian thực

**Các bước tiến trình**:
1. 🔍 Phân tích cấu trúc database
2. 🎨 Tái tạo theme và design system
3. 📄 Tạo các React components (Header, Footer, Hero, Card...)
4. 🔗 Build routing và navigation
5. 📝 Import toàn bộ nội dung (posts, pages)
6. 🖼️ Xử lý media và hình ảnh
7. ✅ Hoàn tất — Preview sẵn sàng

**Hiển thị**:
- Progress bar tổng thể
- Từng bước có icon trạng thái: ✓ (xong), ⏳ (đang xử lý), ○ (chưa bắt đầu)
- Code snippet nhỏ hiển thị live khi từng file được tạo (kiểu terminal typing effect)
- Thời gian ước tính còn lại

**Acceptance Criteria**:
```gherkin
Given AI đang generate code
When một component được tạo xong
Then panel trái cập nhật real-time: hiển thị tên file và số dòng code
And progress bar tăng tương ứng

Given toàn bộ code đã được generate
When preview panel render xong
Then hiển thị toast: "Preview sẵn sàng! Click vào bất kỳ phần nào để chỉnh sửa"
```

---

#### F-06: Preview Panel (Phải)
**Mô tả**: Live preview website React đã được generate

**Trạng thái**:
| Trạng thái | Hiển thị |
|-----------|---------|
| Đang generate | Skeleton loader giống layout trang gốc |
| Đang build | Spinner + "Đang render..." |
| Sẵn sàng | iframe nhúng preview live |
| Lỗi | Error card + nút "Thử lại" |

**Responsive Preview Toggle**:
- Nút chọn: Desktop / Tablet / Mobile
- Thay đổi width của iframe tương ứng

**Acceptance Criteria**:
```gherkin
Given code đã generate xong
When preview render lần đầu
Then iframe load trang chủ của site mới
And tất cả nội dung (text, ảnh, menu) hiển thị đúng với bản WordPress gốc
And layout responsive hoạt động trên 3 breakpoint

Given user click nút "Mobile"
When preview panel cập nhật
Then iframe thu hẹp về width 375px và hiển thị mobile layout
```

---

### 4.4 Visual Editing

#### F-07: Element Inspector & Visual Editor
**Mô tả**: Cho phép user trỏ vào element trên preview để yêu cầu AI chỉnh sửa

**UX Flow**:
1. User bật chế độ "Edit Mode" (toggle button ở thanh toolbar)
2. Hover chuột lên bất kỳ element nào → element bị highlight với border xanh
3. Tooltip nhỏ hiển thị tên component: "CardContent", "HeroSection", v.v.
4. Click vào element → mở panel chat sidebar, có sẵn context: "Đang chỉnh sửa: CardContent"
5. User gõ yêu cầu, AI chỉ sửa đúng component đó
6. Preview tự động refresh component vừa được sửa

**Tham khảo giao diện**: Tương tự hình ảnh đính kèm — khi click vào element "CardContent" sẽ hiển thị panel bên phải với các thuộc tính (font size, color, border, margin...) và một ô chat để ra lệnh AI

**Acceptance Criteria**:
```gherkin
Given preview đang hiển thị và Edit Mode được bật
When user hover chuột lên một card component
Then card được highlight với border xanh dương
And tooltip hiển thị: "CardContent — Click để chỉnh sửa"

Given user đã click vào một element
When panel chỉnh sửa mở ra
Then hiển thị tên component và vị trí trong cây component (breadcrumb)
And ô chat có placeholder: "Bạn muốn thay đổi gì cho phần này?"
And AI chỉ nhận context của component đó, không ảnh hưởng phần khác

Given user nhập lệnh "đổi background sang màu xanh navy"
When AI xử lý
Then chỉ component được chọn thay đổi màu
And preview cập nhật trong < 5 giây
And phần còn lại của trang không thay đổi
```

---

#### F-08: Property Panel (Quick Edit)
**Mô tả**: Panel bên phải cho phép chỉnh sửa nhanh các thuộc tính CSS thông dụng mà không cần chat

**Các thuộc tính hỗ trợ**:
- Typography: Font size, font weight, font family, text alignment
- Colors: Text color, background color, opacity
- Spacing: Margin, padding (top/right/bottom/left)
- Border: Width, color, radius
- Layout: Display, flex properties

**Thiết kế tham khảo**: Giống panel trong ảnh đính kèm — các field input rõ ràng, color picker inline, nút "Discard" và "Save changes"

---

### 4.5 Content Management

#### F-09: Blog & Post Import
**Mô tả**: Đảm bảo toàn bộ bài viết WordPress được chuyển đổi chính xác

**Dữ liệu được giữ lại**:
- Tiêu đề, nội dung (HTML → React components)
- Ngày đăng, ngày sửa
- Author name và avatar
- Featured image
- Categories và tags
- Custom excerpt
- SEO metadata (nếu có Yoast SEO)
- Comments (tùy chọn)

**Content Routing**:
- `/blog` → Blog listing page
- `/blog/:slug` → Single post page
- `/category/:slug` → Category archive
- `/page/:slug` → Static pages

---

### 4.6 Export & Deploy

#### F-10: Export React Project
**Mô tả**: Tải về toàn bộ React project

**Cấu trúc output**:
```
my-site/
├── src/
│   ├── components/
│   │   ├── Header.tsx
│   │   ├── Footer.tsx
│   │   ├── BlogCard.tsx
│   │   └── ...
│   ├── pages/
│   │   ├── Home.tsx
│   │   ├── Blog.tsx
│   │   ├── Post.tsx
│   │   └── ...
│   ├── data/
│   │   ├── posts.json
│   │   └── pages.json
│   └── styles/
│       └── globals.css
├── public/
│   └── images/
├── package.json
└── README.md
```

---

## 5. Ngoài phạm vi (Non-Goals)

Những gì **KHÔNG** nằm trong phạm vi v1.0:

- ❌ Hỗ trợ WooCommerce (e-commerce) — scope riêng cho v2
- ❌ Convert WordPress plugin logic (contact forms, sliders, gallery plugins)
- ❌ Real-time sync với WordPress database gốc sau khi export
- ❌ Support multi-site WordPress network
- ❌ Mobile app output (chỉ React web)
- ❌ Backend API generation (output là static React + JSON)
- ❌ Custom Gutenberg blocks phức tạp

---

## 6. Câu hỏi mở (Open Questions)

| # | Câu hỏi | Người trả lời | Deadline |
|---|---------|--------------|---------|
| 1 | Giới hạn kích thước file SQL upload? (Gợi ý: 500MB) | Tech Lead | — |
| 2 | Mô hình AI nào để dùng cho code generation? (GPT-4o vs Claude 3.5?) | Tech Lead | — |
| 3 | Pricing model: per-conversion hay subscription? | CEO | — |
| 4 | Có cần lưu project history để user quay lại chỉnh sau không? | PM | — |
| 5 | Auth/login bắt buộc hay dùng được ngay không cần đăng ký? | CEO | — |
| 6 | Media files được xử lý thế nào? (re-host hay dùng URL gốc?) | Tech Lead | — |

---

## 7. Kiến trúc kỹ thuật cấp cao (High-Level Technical Architecture)

```
Frontend (React + Vite)
├── Chat Interface
├── Split View (Progress + Preview)
└── Visual Editor (iframe + element inspector)

Backend (Node.js/Express)
├── /api/import      — Nhận và parse SQL dump
├── /api/generate    — Gọi AI API, stream tiến trình qua SSE
├── /api/preview     — Serve React build trong sandbox
├── /api/edit        — Nhận lệnh chỉnh sửa element cụ thể
└── /api/export      — Tạo zip và trả về

AI Layer
├── WordPress DB Analyzer (rule-based + AI)
├── Code Generator (LLM: GPT-4o / Claude)
└── Visual Diff Engine (so sánh bản gốc vs bản mới)

Storage
├── PostgreSQL: user sessions, project metadata
└── Object Storage: uploaded SQL files, generated code, media
```

**Streaming Architecture**: Tiến trình generate code được push real-time qua Server-Sent Events (SSE) để panel trái cập nhật từng bước mà không cần polling.

---

## 8. Rủi ro (Risks)

| Rủi ro | Mức độ | Giải pháp |
|--------|--------|-----------|
| Theme quá phức tạp / custom — AI không tái tạo được chính xác | Cao | Fallback: tạo giao diện "clean version" gần giống + cho phép manual edit |
| Kích thước DB quá lớn (>1GB) gây timeout | Trung bình | Chunk processing, progress save, resume support |
| AI generate code có bug không chạy được | Trung bình | Auto-run build check, retry loop tối đa 3 lần |
| Bảo mật: dữ liệu WordPress chứa thông tin nhạy cảm | Cao | Encrypt at rest, auto-delete file sau 24h, không log content |
| Chi phí AI API quá cao cho conversion lớn | Trung bình | Cache kết quả tương tự, optimize prompt, dùng model nhỏ hơn cho tasks đơn giản |

---

## 9. Roadmap

### Now (v1.0 — MVP)
| Initiative | Mục tiêu |
|-----------|---------|
| Crawl html, css từ URL + Fetch API | Tái tạo nội dung cơ bản |
| Import SQL + parse WordPress DB | Tái tạo nội dung cơ bản |
| AI generate React code | 5 theme phổ biến (Divi, Astra, OceanWP...) |
| Split view: Progress + Preview | UX core loop |
| Visual Editor (element click + chat) | Chỉnh sửa trực quan |
| Export .zip | Nhận sản phẩm cuối |

### Next (v1.5)
- Deployment 1-click lên Vercel/Netlify
- Lưu project history (revisit sau nhiều ngày)
- Hỗ trợ thêm 20+ WordPress themes
- Dark mode output support

### Later (v2.0+)
- WooCommerce support
- Backend API generation (Headless WordPress option)
- Team collaboration (nhiều người cùng edit)
- White-label cho agencies

---

## 10. Giao diện tham khảo

Dựa theo ảnh đính kèm từ Replit Agent interface, giao diện mục tiêu bao gồm:

1. **Chat + Import panel** (trái) — Giống Replit Agent chat nhưng có khu vực drop file SQL phía trên
2. **Preview panel** (giữa/phải) — iframe website được generate, có toolbar chọn device size
3. **Properties panel** (ngoài cùng phải, bật khi click element) — Tương tự panel trong ảnh: Text properties (font size, weight, alignment), Appearance (color, background, opacity, border, radius), Layout (margin, padding), nút Discard/Save changes

**Tông màu & Style**: Clean, dark sidebar, white/light preview area — tương tự giao diện Replit/Figma professional tool

---

*PRD này được viết theo format Linear Project Spec, phù hợp cho team eng-heavy với scope rõ ràng. Cần review với Tech Lead trước khi bắt đầu sprint planning.*
