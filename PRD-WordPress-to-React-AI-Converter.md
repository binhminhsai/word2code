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
Luồng 1: Migrate and Refine WP to X-press

User                     X-press (AI)
 │                            │
 ├─ Import WP Codebase ──────►│
 │                            ├─ Analyze WP Source
 │                            ├─ Extract Structure & Data
 │◄──────── Parsed Structure ─┤
 │
 ├─ Nhập yêu cầu (Chat) ─────►│
 │                            ├─ Generate React Code
 │◄──────── Initial UI + Code ┤
 │
 ├─ Refine UI / Logic ───────►│
 │                            ├─ Apply Changes
 │                            ├─ Optimize Components
 │◄──────── Updated UI State ─┤
 │
 ├─ Export / Deploy ─────────►│
 │                            ├─ Build Output
 │                            ├─ Deploy App
 |                            ├─ Sync Wordpress
 │◄──────── Live App / URL ───┤
 │
 ▼
     X-press App Ready
```

### Bước 1 — Cài đặt plugin của X-press cung cấp vào Wordpress của mình
- Trên giao diện onboarding User được mời download file zip plugin lên Wordpress của họ hoặc cài plugin X-press Vibecode trong store Wordpress để automation workflow access and push "codebase, database infomation to github"
- Hệ thống chuyển người dùng sang màn hình chờ, để Hệ thống tự tạo "github 1" cho dự án và đấy toàn bộ source code Wordpress lên đó (gồm codebase để AI Migrate Frontend, database information để call dữ liệu)
- Sau khi cài đặt plugin và cấp quyền, hệ thống tự nhận diện được khách hàng ấy là chủ sỡ hữu của Github Wordpress nào qua gmail của Wordpress và hiển thị yêu cầu đăng nhập chính xác gmail admin đó vào X-press browser

### Bước 2 — Chat với X-press
#### Mục đích user vào Web X-press là để sửa component đang có ở Wordpress thành một dạng khác bằng AI chat thay vì sửa trực tiếp bằng Wordpress
- User chọn một một trong những github đang có đại diện cho từng dự án Wordpress của họ (về có nhiều trường hợp user có nhiều host Wordpress khác nhau cho những lĩnh vực khách nhau)
- Sau khi import github 1 thì hiện ra giao diện các theme trong kho Wordpress của mình, hệ thống render code php, user xem giao diện và khoanh vùng + comment những vùng cần sửa
- User, mô tả yêu cầu (ngôn ngữ tự nhiên) sửa tổng quan vào khung chat
- Hỗ trợ cả tiếng Việt và tiếng Anh

### Bước 3 — AI Code + Preview song song
- Màn hình chia đôi:
  - **Trái**: Panel AI với log tiến trình coding theo từng bước
  - **Phải**: Preview panel — ban đầu là skeleton loader, sau đó render live khi code xong
- User thấy AI "đang làm việc" theo thời gian thực
- User thấy thông báo AI đã chuyển đổi và code xong thì nhấn vào preview để xem giao diện mới đã chỉnh sửa
- User nhấn xem full thì hệ thống ra ngoài màn hình browser mới

### Bước 4 — Visual Edit
- Sau khi preview sẵn sàng, user hover chuột lên bất kỳ element nào
- Xuất hiện nút chỉnh sửa (icon bút/edit) ngay tại vị trí đó
- Click vào → mở panel chat chỉ nói về element đó
- User typing requirement in chat 
- AI code and sửa đúng component tương ứng, không ảnh hưởng phần còn lại

### Bước 5 — Public/Deploy
- User nhấn Pulic để đồng bộ thay đổi ở X-press sang Wordpress hiện tại
- User nhấn deploy thẳng lên server để người khác xem bằng domain của X-press (có hoặc không cần github 2)
- User setup thay đổi domain của bản thân

### Bước 6 - Daily usage X-press
- User được hệ thống đưa ra những lý do nên rời nền tảng Wordpress qua X-press để quản trị Website bán hàng và Landing page của mình
- User sẽ chọn Design element hoặc page của Website mình ở X-press thay vì design tay ở Wordpress
- User sẽ đăng bài và viết bài trên X-press, quản lý website như đăng sản phẩm, thanh toán, làm hoá đơn...trên X-press
- X-press cung cấp dashboard theo dõi hành vi của khách hàng cho Website ở nền tảng X-press 
- Trong giai đoạn đầu, nếu User chưa quen X-press thì khi họ thay đổi element gì ở Wordpress thì X-press đều thông báo để user commit ở X-press

---

## 5. Tính năng chi tiết (Feature Specifications)

### 5.1 Import Codebase & Khởi tạo (Import Module)

#### F-01: WordPress Plugin Integration
**Mô tả**: Plugin WordPress (X-press Vibecode) tự động thiết lập quyền truy cập repository, chuyển codebase và database lên hệ thống Github 1. 

**Workflow**:
- 1. Từ flow Onboarding, người dùng tải file zip plugin sau đó vào host Wordpress của mình để tải lên hoặc download trên store WP.
- 2. Plugin tiến hành automation thiết lập workflow, đóng gói source code (Themes, Plugins context ứng dụng) và database (hoặc cấu hình thông qua REST).
- 3. Tự động đẩy dữ liệu lên kho chứa (Github 1) được tạo riêng cho dự án trên hệ thống.
- 4. Xác thực và map định danh chủ sở hữu website thông qua Gmail quản trị viên (Admin WordPress), điều hướng và hiển thị yêu cầu liên kết đăng nhập chính xác trên trình duyệt.

**Acceptance Criteria**:
- Tự động push mã nguồn và thông tin database lên GitHub thành công.
- Hệ thống nhận diện đúng Gmail Admin WordPress để yêu cầu login đồng bộ trên trình duyệt X-press.

---

### 5.2 X-press Chat Interface (Tương tác khởi tạo)

#### F-02: Giao diện AI Chat & Khai báo yêu cầu
**Mô tả**: Giao diện cho phép chọn repo/dự án, xem trước layout thô và gửi request tuỳ chỉnh.

**Acceptance Criteria**:
- Cho phép người dùng chọn một trong những github đang có đại diện cho từng dự án Wordpress của họ (khi có nhiều host khác nhau).
- Khi chọn 1 trong những host đã có plugin của X-press thì user thấy những list page đã có trong Wordpress tổng
- User thấy được thêm lịch sử commit của Git với Wordpress và X-press
- Người dùng nhấn vào một trong các page đó thì system Cung cấp viewer để xem trước cấu trúc UI (render code PHP page và theme đã lấy).
- Cung cấp công cụ Annotated: người dùng khoanh vùng và comment chi tiết vào các khối giao diện (component) cần sửa đổi ở trong môi trường canvas của từng page đó.
- Khung chat AI: nhận mô tả yêu cầu (prompt) bằng ngôn ngữ tự nhiên về mong muốn chuyển đổi/sửa/thêm đổi tổng quan. (Hỗ trợ tiếng Việt và tiếng Anh).

---

### 5.3 Màn hình Split View (AI Code + Live Preview)

#### F-03: Theme Detection & Analysis codebase
**Mô tả**: AI đọc active theme, phân tích layout structure, viết code frontend tạo page, component, element và code backend Rest API tới database.

**Workflow**:
- 1. Đọc context các file → `template` và `stylesheet` → biết tên theme.
- 2. Tra cứu theme structure từ dữ liệu metadata của file gốc.
- 3. Parse cấu trúc template phân cấp (header.php, footer.php, page.php, single.php).
- 4. Phân tích nội dung CSS variables, color palette, typography.
- 5. Lập map chuyển đổi sang React component structure tương ứng.
- 6. Xây dựng cấu trúc backend để Rest API tới database lấy nội dung các bài blog, comment, sản phẩm, danh mục, tag, user, ...

**Acceptance Criteria**:
- Nhận diện Theme: Định danh chính xác tên, phiên bản và các file core của Active Theme từ GitHub 1.
- Phân tích Cấu trúc (Parsing): Parse thành công sơ đồ phân cấp PHP (Header, Footer, Page, Single, Archive) thành bản đồ cấu trúc (Layout Map).
- Trích xuất Design System: Thu thập đầy đủ các biến CSS (Colors, Typography, Spacing) và lưu trữ cho React (tạo một file nào đó).
- Mapping Frontend: Chuyển đổi sơ đồ Layout Map sang cây thư mục React Components và xây dụng Routing.
- Khởi tạo Backend API: Tự động tạo bộ REST API endpoints (Node.js/Express) kết nối trực tiếp vào WordPress Database hiện tại.
- Đầy đủ Resource: API phải fetch được toàn bộ dữ liệu từ các bảng: Posts, Postmeta, Categories, Tags, Users và Products (nếu có).
- Xử lý Ngoại lệ: Hệ thống có cơ chế Fallback (mẫu chuẩn) nếu phát hiện file PHP của theme gốc bị lỗi hoặc không đúng cấu trúc WordPress.
- Độ chính xác: Mã nguồn React được gen ra phải phản ánh đúng >90% cấu trúc UI cũ và các API Endpoint có tốc độ phản hồi <200ms.

#### F-04: Split View Layout & Tiến trình chạy song song
**Mô tả**: Hệ thống sử dụng chế độ Layout chia màn hình làm 2 panel (Panel tiến trình AI bên trái / Panel Preview render bên phải) để cung cấp UI feedback liên tục.

**F-04.1: Panel AI Progress (Trái - Hệ thống đang xử lý)**
***Acceptance Criteria***:
- Hiển thị tiến trình coding và generate logic: 
  - Phân tích cấu trúc thư mục & yêu cầu prompt.
  - Tái tạo theme và setup design system React.
  - Tạo các components React & setup routing theo context hiện có từ Github 1.
  - Text/Code streaming (log tiến trình).
- Icon trạng thái từng bước: hoàn tất, đang tải, pending.

**F-04.2: Preview Panel (Phải - Kết quả Render thực nghiệm)**
***Acceptance Criteria***:
- Ban đầu là giao diện khung (skeleton loader).
- Hiển thị giao diện tương ứng cho các checkpoint lớn như: Plan/ Generate Theme/ Generate Component/ Generate API/ Generate Routing/ Metric Evaluate/ Redesign.
- Nhận event lắng nghe trigger để build ra ứng dụng Live ngay khi code AI thực thi hoàn thành.
- Giao diện có hiển thị nút Expand ("Xem Full") để chuyển sang tab trình duyệt độc lập, xem UI ở chế độ trải nghiệm website.
- Khi preview đã sẵn sàng và render, hệ thống thông báo cho user (Toast).

---

### 5.4 Visual Editor (Tinh chỉnh Component bằng Chat)

#### F-05: Contextual Element-Level Editor
**Mô tả**: Cung cấp khả năng tinh chỉnh nhanh các chi tiết trên giao diện đã sinh ra bằng màn hình Canvas.

**Acceptance Criteria**:
- Khi user di chuột (hover) vào bất kỳ thành phần giao diện nào ở tab Preview, hệ thống sẽ bọc element với viền màu highlight (vd: border xanh).
- Hiển thị icon thao tác chỉnh sửa (bút / edit tooltip) ngay vị trí chuột để click.
- Click chuyển đổi sang trạng thái Focused-Edit: 
  - Giao diện hiển thị một popup comment riêng riêng chỉ nhận diện content và logic cho component đó.
  - User nhập yêu cầu (requirement typing).
  - Với nhiều yêu cầu, thì pannel bên tay phải canvas xuất hiện, sau khi comment xong user bấm nút "Run" hoặc typing vào global chat để AI sẽ ưu tiên xử lý theo thứ tự comment của user.
  - AI khởi động coding lại duy nhất vùng giới hạn của DOM tree và React Component đó, ngăn chặn thay đổi chéo.
  - Preview hot-reload lại bộ giao diện sau khi thay đổi (không load cả frame).

---

### 5.5 Deploy & Sync (Xuất bản và Đồng bộ)

#### F-06: 2nd Mode Deployment System (Public / Host)
**Mô tả**: Hành vi thay đổi trạng thái codebase sang chế độ Production.

**Workflow**:
**Public Mode**:
- 1. Hệ thống kiểm tra trạng thái tương đồng của codebase và database của X-press và Wordpress host qua (Github 1) để nhắc nhở người dùng commit.
- 2. User nhấn nút Public để đồng bộ những thay đổi của database và source code về Wordpress host của người dùng.
- 3. Hệ thống đồng bộ các thay đổi ở X-press trả ngược về codebase và database của site WordPress gốc đang chạy (pipeline AI lần 2 từ REACT sang PHP).
**Host Mode**:
- 1. Người dùng nhấn Deploy để sử dụng X-press như một nền tảng phát triển web độc lập.
- 2. Hệ thống deploy ra Internet sử dụng domain của X-press platform (hoặc cấu hình Custom Domain từ user). (Workflow có thể tạo repo thứ 2 là Github 2).
- 3. Khi người dùng đăng blog hoặc khách hàng comment trên Web thì hệ thống lưu về database để dùng chung với Wordpress. 


**Acceptance Criteria**:
- **Nút Public (Sync)**: Thực hiện đồng bộ các thay đổi ở X-press trả ngược về codebase và database của site WordPress gốc đang chạy. 
- **Nút Deploy (Hosting)**: Chuyển đổi trạng thái project thành ứng dụng React App và deploy ra Internet sử dụng domain của X-press platform (hoặc cấu hình Custom Domain từ user). (Workflow có thể tạo repo thứ 2 là Github 2).

---

### 5.6 Metrics & Daily Usage 

#### F-07: Dashboard Tracking & Management (Vibepress CMS / E-commerce module)
**Mô tả**: Phục vụ tương tác Daily Active user để giữ chân user lại trên X-press thay vì sử dụng WordPress Admin.
- X-press Admin cung cấp: trình đăng/viết bài, edit sản phẩm, hóa đơn thanh toán.
- Tracking & Analytics Dashboard báo cáo performance và tương tác người mua hàng.
- Logic đồng bộ notification: Trong giai đoạn mới làm quen vòng lặp, nếu user vô tình thay đổi data cơ sở bên WP dashboard, X-press hiển thị notice thông báo chênh lệch dữ liệu để yêu cầu người dùng commit / sync về chiều của X-press.

---

### 5.7 Integration Pipeline & Automation Tools

**X-press → Git → WP (for user workflow Sync)**:
```
User update code tại Component X-press Chat 
    → Vibepress Agent commit Github 2 (to deploy)
    → Sync webhook push file qua WordPress Plugin (wp to git)
    → WordPress Live đổi dạng (x-press -> AI pipeline 2-> git)
```

**Evaluate Migration Metrics Tracking**: Cung cấp baseline so sánh độ tiệm cận giao diện (Pixel diff tool) và tối ưu Lighthouse cho các tính năng hệ thống sau Migration.

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
| Create WordPress Website | LE ANH + VINH | 19/03 | Week 3 | Completed | Website mẫu để test |
| Push WP source code to Github 1 | LE ANH + VINH | 19/03 | Week 3 | Completed | Setup repo chuẩn |
| Test AI Gen Themes from Git + DB access | LE ANH | 20/03 | Week 3 | Completed | Verify AI đọc được theme |
| Build Metric evaluate migration performance | VINH | 20/03 | Week 3 | Completed | Định nghĩa baseline metrics |
| Case: AI Plan Frontend & Backend | LE ANH | 26/03 | Week 4 | Completed | Pipeline chart -> Source code Component tree + API plan |
| Case: Automation sync WP ↔ VP-CASSO ↔ Git | VINH | 26/03 | Week 4 | Delay | Pipeline chart -> Source code WP→Git1, Git1→VP, VP→Git2, VP->Git1, Git1→WP |
| Checkpoint 1 in Week | **All team** | 25/03 | Week 4 | Completed | Process: VINH show input github 1 (source) -> LANH show and final flow AI plan -> VINH start optimize metric compare migrate -> LANH start to merge (input Git1 + metric to agent) -> (Tool 1 for Agent)|
| Case: AI Gencode base plan & data | LE ANH | 26/03 | Week 4 | Completed | Pipeline chart -> React code generation |
| Optimize input and ouput Metric evaluate migration performance For LANH | VINH | 26/03 | Week 4 | Completed | Định nghĩa baseline metrics |
| Build Prototype Draft 1 | PM | 26/03 | Week 4 | Completed | Figma/static prototype |
| Checkpoint 2 in Week | **All team** | 26/03 | Week 4 | Completed | Process: VINH show metric -> LANH merge in pipeline gen code of AI -> Minh show prototype |
| **Merge system & Demo 1** | **All team** | **27/03** | *Week 4* | **Completed** | **Internal demo** |

**Done = Demo 1 chạy được**: Import Github 1 → AI gen vài component → hiển thị preview

---

### CHECKPOINT 2: Core Pipeline Working
**Thời gian**: Week 5 (28/03 – 03/04/2026)  
**Mục tiêu**: End-to-end pipeline hoạt động — Github 1 → AI → React preview → Github 2

| Task | Owner | Deadline | Mô tả | Status |
|------|-------|---------|-------|-------|
| Build tool evaluate performance dong va tinh | VINH | 31/03 | Nevigation & API query WP DB to compare | Completed |
| AI Open source gen được React and Nodejs | LE ANH | 31/03 | Reseach and run thu opensource tren Mac mini | Completed |
| Complete Tool Sync WP ≍ Git| VINH | 02/04| Testing 2 case | Delay |
| Test pipeline AI migrate full WP Page (Frontend/Rest API) | LE ANH | 02/04 | Nevigation/Blog/Comment/Store (WooCommerce)| Completed |
| Enhance prototype through requirement cua anh Diep | Minh | 02/04| Research (31.03) -> Redesign (02.04) | Delay |
| Push React code to Github 2 | VINH | 03/04 | Auto-commit sau khi gen xong | Completed |
| Live preview chạy được trong x-press with open source model | LE ANH + Vinh | 03/04 | iframe preview internal | Not Started |
| Case: Automation CI/CD Git to server | VINH | 03/04 | OAuth + clone flow | Delay |
| Case: AI Revise Frontend by chat | LE ANH | 03/04 | Chat edit loop | Delay |

**Done = Demo 2**: Import repo (refine UI/UX) → AI gen (open source) → Preview live & evaluation → Code có trên Github 2 (tool automation)

---

### CHECKPOINT 3: Content & AI Generation Engine
**Thời gian**: Week 6 (04/04 – 10/04/2026)  
**Mục tiêu**: Toàn bộ nội dung WP được migrate, AI gen đủ pages, routing đúng, CI/CD lên server

| Task | Owner | Deadline | Mô tả |
|------|-------|---------|-------|
| Testing Performance của Agentic AI worfklow WP -> Next.js | VINH | 07/04 | Đúng routing, page, UI, đủ ảnh, blog, comment, product |
| Testing Blog post + comment in Next.js code | LE ANH | 07/04 | Dynamic content render in database and sync to Wordpress |
| AI Revise Frontend by chat (UI only) | LE ANH | 08/04 | Chỉ sửa mỗi UI, UX tạo table để sau|
| Performance metrics dashboard | VINH | 08/04 | So sánh Lighthouse WP vs React, Rest API performance |
| Checkpoint 3 - MVP X-press | LE ANH + VINH | 08/04 | Demo full luồng từ WP -> Next.js -> AI refine by chart |

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
