# PRD: Vibepress — WordPress to React AI Migration Platform

**Version**: 2.0  
**Date**: March 19, 2026  
**Status**: Active  
**PM**: dawningchu1024@gmail.com  
**Team**: LE ANH (lengocanhpyne363@gmail.com) · VINH (luuvinh909@gmail.com)

---

## 1. Project Brief

> *"A Vibepress platform that migrates WordPress websites into a full-stack React architecture, supports live UI editing via chat, auto-deploys through GitHub CI/CD, and keeps both WordPress and React versions synchronized through a shared database."*

---

## 2. Actors & System Map

| Actor | Mô tả |
|-------|-------|
| **User / WordPress User** | Người dùng cuối sở hữu website WordPress |
| **Github 1** | Repo lưu source code WordPress gốc của user |
| **Vibepress Platform** | Hệ thống chính được xây dựng |
| **Vibepress Source Code** | React source được gen từ Github 1 |
| **Vibepress Agent** | AI engine đảm nhận việc đọc, phân tích và generate code |
| **Github 2** | Repo output lưu React code mới được tạo bởi Vibepress |

```
User / WP Site
     │
     ▼
  Github 1 ──────────────► Vibepress Platform
  (WP source)                     │
                         ┌────────┴─────────┐
                         │   Vibepress Agent │ (AI)
                         └────────┬─────────┘
                                  │
                          ┌───────▼────────┐
                          │  React App Gen  │
                          │  Shared DB Sync │
                          │  Live Preview   │
                          └───────┬────────┘
                                  │
                               Github 2
                             (React output)
```

---

## 3. Mục tiêu sản phẩm (Goals)

### Mục tiêu kinh doanh
- Là nền tảng SaaS tự động hóa toàn bộ quá trình migrate WordPress → React
- Rút ngắn thời gian migrate từ hàng tháng xuống dưới 1 ngày
- Duy trì cả hai phiên bản WP và React đồng bộ real-time qua shared database

### Mục tiêu kỹ thuật
- Import WordPress source từ Github 1 → AI phân tích → Gen React code → Push Github 2
- Bi-directional sync: WP ↔ Git tự động
- Chat-based UI editing với visual element selector
- GitHub CI/CD auto-deploy

### Chỉ số thành công
| Metric | Mục tiêu |
|--------|----------|
| Tỷ lệ migrate thành công | ≥ 85% với WP chuẩn |
| Thời gian hoàn thành 1 migration | < 30 phút |
| Visual accuracy vs bản gốc | ≥ 90% |
| Sync latency WP ↔ React | < 60 giây |

---

## 4. Hành trình người dùng (User Journey)

```
<<<<<<< Updated upstream
<<<<<<< Updated upstream
[Connect Github 1] → [AI Phân tích WP source] → [AI Gen React code]
       → [Preview live] → [Chat Edit UI] → [Export to Github 2] → [Auto Deploy]
=======
=======
>>>>>>> Stashed changes
[Import Codebase] → [Nhập yêu cầu qua Chat] → [AI Code + Preview song song] → [Visual Edit by choose,chat and AI] → [Export/Deploy]
```

### Bước 1 — Import Codebase
- User upload file `.sql` dump từ WordPress (hoặc automation workflow access codebase)
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
- User typing requirement in chat 
- AI code and sửa đúng component tương ứng, không ảnh hưởng phần còn lại

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
When user kéo thả file vào drop zone hoặc click upload or automation access and pull codebase
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
>>>>>>> Stashed changes
```

---

## 5. Tính năng chi tiết

### 5.1 Import Module — 3 Mode

#### Mode A: SQL File Upload
- User upload file `.sql` dump từ WordPress
- AI phân tích ngoại tuyến, không cần kết nối
- An toàn nhất, phù hợp khi có full DB export

#### Mode B: Database Direct Connect
- User cung cấp: host, port, username, password, database name
- Tool kết nối MySQL/MariaDB, query tuần tự:
  - `wp_options` → site config, active theme
  - `wp_posts` → posts, pages
  - `wp_postmeta` → custom fields, featured images
  - `wp_terms` → categories, tags
  - `wp_users` → authors
- Credentials không được lưu, xoá sau khi query xong
- Giới hạn: cần mở MySQL port (3306), không lấy được PHP theme files

#### Mode C: WordPress REST API
- User cung cấp domain + Application Password (WP 5.6+ built-in)
- Gọi các endpoint song song:
  - `/wp/v2/posts`, `/wp/v2/pages`, `/wp/v2/media`
  - `/wp/v2/categories`, `/wp/v2/tags`, `/wp/v2/themes`
- Không cần mở firewall, an toàn nhất cho hosting shared
- Giới hạn: không lấy được PHP template structure, menu cần plugin

#### Mode D: Github Repository Import *(Vibepress-specific)*
- User kết nối Github 1 (OAuth)
- Vibepress clone repo, đọc trực tiếp từ source code:
  - PHP theme files → map sang React component structure
  - `functions.php`, `style.css`, template hierarchy
  - `wp-content/themes/[active-theme]/`
- Đây là mode chính của Vibepress (khác với 3 mode trên)

---

### 5.2 AI Agent Pipeline

```
Github 1 (WP source)
    │
    ▼
[Bước 1] Phân tích cấu trúc thư mục & theme
    │
    ▼
[Bước 2] Parse PHP templates → Component tree plan
    │
    ▼
[Bước 3] Đọc database (Mode A/B/C) → Lấy nội dung
    │
    ▼
[Bước 4] AI gen React components + routing
    │
    ▼
[Bước 5] AI gen backend API để fetch DB
    │ (shared database — cả WP lẫn React đều dùng)
    ▼
[Bước 6] Build & Preview live
    │
    ▼
[Bước 7] Push to Github 2 + CI/CD deploy
```

**Về backend access database**: React app gen ra sẽ có backend layer (Node.js/Express) kết nối trực tiếp vào WordPress database hiện tại (shared DB). AI gen endpoint theo chuẩn REST để React query. WP vẫn chạy song song dùng chung DB đó. Không cần auth riêng nếu backend nằm cùng server — nếu khác server thì dùng credentials Mode B.

---

### 5.3 Sync Automation

#### WP → Git (for user)
```
User chỉnh sửa WP → Webhook trigger → Vibepress detect changes
    → Pull diff từ WP DB → Update React source → Push to Github 2
    → CI/CD rebuild → Deploy
```

#### Git → WP (for robot)
```
User chỉnh sửa React code qua Vibepress chat
    → Vibepress Agent update source → Push Github 2
    → Sync engine write changes back to WP DB
    → WP site cập nhật tương ứng
```

---

### 5.4 Visual Editor & Chat

- **Edit Mode toggle**: Bật/tắt chế độ chỉnh sửa visual
- **Element hover**: Highlight component với border xanh + tooltip tên component
- **Click to edit**: Mở chat panel với context đúng component
- **AI chỉ sửa đúng component được chọn**, phần còn lại không thay đổi
- **Property panel**: Quick-edit CSS thông dụng (font, color, spacing, border)
- **Preview refresh**: Tự động cập nhật sau khi AI sửa xong (< 5 giây)

<<<<<<< Updated upstream
---
=======
**UX Flow**:
1. User bật chế độ "Edit Mode" (toggle button ở thanh toolbar)
2. Hover chuột lên bất kỳ element nào → element bị highlight với border xanh
3. Tooltip nhỏ hiển thị tên component: "CardContent", "HeroSection", v.v.
4. Click vào element → mở panel chat sidebar, có sẵn context: "Đang chỉnh sửa: CardContent"
5. User gõ yêu cầu, AI start plan, coding chỉ sửa đúng component đó
6. Preview tự động refresh component vừa được sửa
>>>>>>> Stashed changes

### 5.5 Metrics — Evaluate Migration Performance

| Metric | Công thức / Cách đo |
|--------|---------------------|
| Visual Accuracy | Pixel diff screenshot WP vs React (tool: Percy / Chromatic) |
| Performance Score | Lighthouse score React vs WP (LCP, FID, CLS) |
| Content Coverage | % posts/pages được migrate thành công |
| Sync Latency | Thời gian từ khi thay đổi WP đến khi React cập nhật |
| Build Time | Thời gian từ import đến preview sẵn sàng |

---

### 5.6 Deploy & Domain Management *(VINH)*

Sau khi AI gen xong và user hài lòng với preview, Vibepress tự động deploy React site lên infrastructure của nền tảng.

**Domain mặc định (tự động cấp)**:
- Mỗi project được cấp subdomain: `[project-slug].vibepress.app`
- Không cần cấu hình, sẵn sàng ngay sau lần deploy đầu
- HTTPS tự động qua Let's Encrypt

**Custom domain (tùy chọn — user tích chọn)**:
- User bật toggle "Dùng domain riêng" trong Settings
- Nhập domain: `mysite.com`
- Vibepress hiển thị DNS records cần trỏ (CNAME hoặc A record)
- Sau khi DNS propagate → Vibepress verify và cấp SSL tự động
- Cả `vibepress.app` subdomain và custom domain đều hoạt động song song hoặc subdomain bị redirect về custom domain (user chọn)

**CI/CD Flow (VINH)**:
```
User approve preview
    → Vibepress trigger Github Actions (Github 2)
    → Build React app (pnpm build)
    → Deploy lên Vibepress hosting
    → Cập nhật DNS routing cho domain
    → Notify user: "Site live tại [domain]"
```

---

## 6. Ngoài phạm vi — Non-Goals (v1.0)

- ❌ WooCommerce / e-commerce
- ❌ WordPress plugin logic phức tạp (contact forms, sliders)
- ❌ Multi-site WordPress network
- ❌ Mobile app output
- ❌ Support theme builder (Elementor, Divi drag-drop logic)

---

## 7. Câu hỏi mở

| # | Câu hỏi | Owner | Deadline |
|---|---------|-------|---------|
| 1 | Shared DB: React backend dùng credentials nào để connect WP DB? | VINH | Week 4 |
| 2 | Github OAuth: dùng Vibepress app hay user tự tạo token? | VINH | Week 4 |
| 3 | AI model: GPT-4o vs Claude 3.5 cho code gen? | LE ANH | Week 4 |
| 4 | WP webhook: plugin tự build hay dùng WP Action Hooks? | VINH | Week 5 |
| 5 | Preview sandbox: isolated Docker container hay Replit environment? | VINH | Week 5 |
| 6 | Giới hạn repo size Github 1 được import? | VINH | Week 5 |

---

## 8. Roadmap & Checkpoints — 6 Tuần

> **Nguyên tắc**: 5 tuần build + 1 tuần polish/fix bug. Checkpoint 1 là tuần hiện tại (Week 3–4).

---

### CHECKPOINT 1: Ideation & Foundation Test
**Thời gian**: Week 3–4 (19/03 – 27/03/2026)  
**Mục tiêu**: Có WP site thật, source lên Github, AI thử gen được, prototype draft 1

| Task | Owner | Start | Launch | Status | Ghi chú |
|------|-------|-------|--------|--------|---------|
| Create WordPress Website | LE ANH + VINH | 19/03 | Week 3 | In Progress | Website mẫu để test |
| Push WP source code to Github 1 | LE ANH + VINH | 19/03 | Week 3 | In Progress | Setup repo chuẩn |
| Test AI Gen Themes from Git + DB access | LE ANH | 20/03 | Week 3 | In Progress | Verify AI đọc được theme |
| Build Metric evaluate migration performance | VINH | 20/03 | Week 3 | In Progress | Định nghĩa baseline metrics |
| Case: AI Plan Frontend & Backend | LE ANH | 26/03 | Week 4 | In Progress | Pipeline chart -> Source code Component tree + API plan |
| Case: Automation sync WP ↔ VP-CASSO ↔ Git | VINH | 26/03 | Week 4 | In Progress | Pipeline chart -> Source code WP→Git1, Git1→VP, VP→Git2, VP->Git1, Git1→WP |
| Checkpoint 1 in Week | **All team** | 25/03 | Week 4 | In Progress | Process: VINH show input github 1 (source) -> LANH show and final flow AI plan -> VINH start optimize metric compare migrate -> LANH start to merge (input Git1 + metric to agent) -> (Tool 1 for Agent)|
| Case: AI Gencode base plan & data | LE ANH | 26/03 | Week 4 | Not Started | Pipeline chart -> React code generation |
| Optimize input and ouput Metric evaluate migration performance For LANH | VINH | 26/03 | Week 4 | Not Started | Định nghĩa baseline metrics |
| Build Prototype Draft 1 | PM | 26/03 | Week 4 | Not Started | Figma/static prototype |
| Checkpoint 2 in Week | **All team** | 26/03 | Week 4 | Not Started | Process: VINH show metric -> LANH merge in pipeline gen code of AI -> Minh show prototype |
| Case: Automation CI/CD Git to server | VINH | 27/03 | Week 4 | Not Started | OAuth + clone flow |
| Case: AI Revise Frontend by chat | LE ANH | 27/03 | Week 4 | Not Started | Chat edit loop |
| **Merge system & Demo 1** | **All team** | **27/03** | *Week 4* | **Not Started** | **Internal demo** |

**Done = Demo 1 chạy được**: Import Github 1 → AI gen vài component → hiển thị preview

---

### CHECKPOINT 2: Core Pipeline Working
**Thời gian**: Week 5 (28/03 – 03/04/2026)  
**Mục tiêu**: End-to-end pipeline hoạt động — Github 1 → AI → React preview → Github 2

| Task | Owner | Deadline | Mô tả |
|------|-------|---------|-------|
| Github OAuth integration hoàn chỉnh | VINH | 01/04 | User connect repo không cần manual token |
| AI đọc được full PHP theme structure | LE ANH | 01/04 | Parse header/footer/template hierarchy |
| Gen được React components cơ bản | LE ANH | 02/04 | Header, Footer, Layout, Page shell |
| Backend layer gen + connect shared DB | VINH | 02/04 | Node/Express API query WP DB |
| Push React code to Github 2 | VINH | 03/04 | Auto-commit sau khi gen xong |
| Live preview chạy được trong Vibepress | LE ANH | 03/04 | iframe preview internal |

**Done = Demo 2**: Import repo → AI gen → Preview live → Code có trên Github 2

---

### CHECKPOINT 3: Content & AI Generation Engine
**Thời gian**: Week 6 (04/04 – 10/04/2026)  
**Mục tiêu**: Toàn bộ nội dung WP được migrate, AI gen đủ pages, routing đúng

| Task | Owner | Deadline | Mô tả |
|------|-------|---------|-------|
| Import đủ posts, pages, media | VINH | 07/04 | Query WP DB + map sang JSON |
| Routing đầy đủ: `/`, `/blog`, `/blog/:slug`, `/page/:slug` | LE ANH | 07/04 | React Router setup |
| Gen Blog listing + Single post page | LE ANH | 08/04 | Dynamic content render |
| Menu navigation migrate chính xác | LE ANH | 08/04 | Lấy từ wp_terms/menus |
| Media handling: ảnh, thumbnail | VINH | 09/04 | URL reference hoặc re-host |
| Performance metrics dashboard | VINH | 10/04 | So sánh Lighthouse WP vs React |
| Visual accuracy score tự động | LE ANH | 10/04 | Screenshot diff tool |

**Done = Demo 3**: Full WP site được migrate với đủ nội dung, đo được metrics

---

### CHECKPOINT 4: Sync Automation & CI/CD
**Thời gian**: Week 7 (11/04 – 17/04/2026)  
**Mục tiêu**: Hai chiều sync WP ↔ Git hoạt động, GitHub Actions CI/CD deploy được

| Task | Owner | Deadline | Mô tả |
|------|-------|---------|-------|
| WP → Git sync: webhook khi user update WP | VINH | 14/04 | Plugin hoặc WP hooks |
| Git → WP sync: Vibepress write back | VINH | 14/04 | API endpoint cập nhật WP DB |
| GitHub Actions workflow CI/CD | VINH | 15/04 | Auto build + deploy khi push Github 2 |
| Conflict resolution khi 2 bên cùng thay đổi | VINH | 16/04 | Last-write-wins hoặc queue |
| Shared DB architecture finalize | VINH | 16/04 | Cả WP lẫn React dùng cùng 1 DB |
| End-to-end sync test | LE ANH + VINH | 17/04 | Thay đổi WP → React cập nhật tự động |

**Done = Demo 4**: Sửa bài viết trên WP → React site tự động cập nhật sau < 60s

---

### CHECKPOINT 5: Visual Editor & Chat Polish
**Thời gian**: Week 8 (18/04 – 24/04/2026)  
**Mục tiêu**: Chat edit hoàn chỉnh, visual element selector hoạt động, UX mượt

| Task | Owner | Deadline | Mô tả |
|------|-------|---------|-------|
| Chat panel UI hoàn chỉnh | LE ANH | 21/04 | History, context, streaming response |
| Element inspector: hover highlight + click | LE ANH | 21/04 | iframe postMessage communication |
| AI chỉ sửa đúng component được chọn | LE ANH | 22/04 | Component-scoped prompt context |
| Property panel quick-edit CSS | LE ANH | 22/04 | Font, color, spacing, border |
| Preview auto-refresh sau edit | LE ANH | 23/04 | Hot reload component đúng |
| Split view: AI progress + Preview | LE ANH | 23/04 | UX giống Replit Agent |
| Mobile / Tablet preview toggle | LE ANH | 24/04 | iframe width responsive |
| UI polish toàn bộ Vibepress | PM | 24/04 | Design review + fixes |

**Done = Demo 5**: Click vào element → chat sửa → preview cập nhật real-time

---

### CHECKPOINT 6: Integration Complete & Full Demo
**Thời gian**: Week 9 (25/04 – 01/05/2026)  
**Mục tiêu**: Toàn bộ flow end-to-end hoạt động ổn định, demo được với user thật

| Task | Owner | Deadline | Mô tả |
|------|-------|---------|-------|
| End-to-end integration test toàn hệ thống | All | 28/04 | Happy path + edge cases |
| Fix blocker bugs từ Demo 5 | LE ANH + VINH | 29/04 | Critical bugs only |
| Performance optimization | VINH | 29/04 | Build time < 30 phút |
| Error handling & fallback | LE ANH + VINH | 30/04 | Graceful errors khi AI fail |
| Documentation nội bộ | PM | 30/04 | Setup guide cho dev |
| **Demo 6 — Full system demo** | **All** | **01/05** | **Chạy với WP site thật** |

**Done = Demo 6**: Toàn bộ flow: Github 1 → Import → AI Gen → Sync → Chat Edit → Deploy

---

### CHECKPOINT 7: Polish & Bug Fix (Final)
**Thời gian**: Week 10 (02/05 – 08/05/2026)  
**Mục tiêu**: Sản phẩm sẵn sàng cho user thật — không thêm feature mới

| Task | Owner | Deadline | Mô tả |
|------|-------|---------|-------|
| Bug fix từ feedback Demo 6 | LE ANH + VINH | 05/05 | Theo priority từ PM |
| UI/UX polish chi tiết | PM + LE ANH | 05/05 | Spacing, micro-interactions, loading states |
| Security review | VINH | 06/05 | Credentials handling, DB access |
| Performance benchmark final | VINH | 06/05 | Lighthouse, build time, sync latency |
| User onboarding flow | PM + LE ANH | 07/05 | First-time experience |
| **Final Demo & Sign-off** | **All** | **08/05** | **Product ready** |

---

## 9. Timeline Tổng quan

```
Week 3–4  │ CP1: Ideation & Foundation Test     │ Mar 19–27
Week 5    │ CP2: Core Pipeline Working           │ Mar 28 – Apr 3
Week 6    │ CP3: Content & AI Gen Engine         │ Apr 4–10
Week 7    │ CP4: Sync Automation & CI/CD         │ Apr 11–17
Week 8    │ CP5: Visual Editor & Chat            │ Apr 18–24
Week 9    │ CP6: Integration Complete            │ Apr 25 – May 1
Week 10   │ CP7: Polish & Bug Fix (Final)        │ May 2–8
```

---

## 10. Phân công tổng hợp

| Owner | Trách nhiệm chính |
|-------|------------------|
| **LE ANH** (lengocanhpyne363) | AI GEN framework toàn bộ — từ đọc WP source, lên plan, gen Frontend (React components, routing, UI editor) đến kết nối Backend (API layer, DB fetch, shared database integration) |
| **VINH** (luuvinh909) | Automation kết nối các tool (Github OAuth, WP webhook, sync engine) + Deploy: cấp domain Vibepress mặc định (`*.vibepress.app`), user có thể tích chọn custom domain riêng |
| **PM** (dawningchu1024) | Prototype, Design review, Task tracking, Demo coordination |

---

## 11. Rủi ro

| Rủi ro | Mức độ | Giải pháp |
|--------|--------|-----------|
| AI không tái tạo chính xác custom PHP theme | Cao | Fallback: clean React version + manual chat edit |
| WP host block remote DB/Git access | Cao | Hướng dẫn user dùng Mode A (SQL export) hoặc REST API |
| Sync conflict khi cả hai phía cùng edit | Trung bình | Queue + last-write-wins + conflict notification |
| AI gen code có bug, build fail | Trung bình | Auto retry 3 lần + hiển thị error rõ ràng |
| Chi phí AI API cao cho WP site lớn | Trung bình | Cache, chunking, dùng model nhỏ hơn cho tasks đơn giản |
| Demo 6 bị delay do scope creep | Cao | Freeze feature sau CP5, chỉ fix bug từ CP6 trở đi |
