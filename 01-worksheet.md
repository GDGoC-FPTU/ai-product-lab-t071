# Lab 02 — Worksheet: AI Product Scoping (Vin Smart Future)

> **Họ và tên:** Phạm Sỹ Đức
> **MSSV:** 2A202601601
> **Ngày:** 24/07/2026
> **Nhóm/Công việc:** AI Engineer tại Vin Smart Future

---

## 🏛️ 1. Bối cảnh thực tế: Vin Smart Future (Vingroup)

**Vingroup** — Tập đoàn tư nhân lớn nhất Việt Nam — vừa sáp nhập toàn bộ các phòng ban công nghệ thuộc các công ty thành viên thành một đơn vị công nghệ thống nhất mang tên **Vin Smart Future**. 

Nhiệm vụ của **Vin Smart Future** là xây dựng các giải pháp AI, số hóa, và tự động hóa cốt lõi để nâng cao hiệu suất vận hành và trải nghiệm khách hàng xuyên suốt các công ty thành viên:
* 🚗 **VinFast:** Hệ thống xe điện thông minh (EV), trợ lý AI ảo trong xe, dự đoán bảo trì pin, và quản lý chuỗi cung ứng sản xuất.
* 🚕 **Xanh SM (GSM):** Vận hành đội xe taxi/xe máy điện thông minh, điều vận thông minh (Smart Dispatching), tối ưu hóa lộ trình di chuyển.
* 🏢 **Vinhomes:** Quản lý đô thị thông minh (Smart Cities), trợ lý cư dân thông minh, tối ưu hóa mức tiêu thụ năng lượng.
* 🏥 **Vinmec:** Y tế thông minh, chẩn đoán hình ảnh bằng AI, tối ưu hóa quản lý hồ sơ bệnh án.
* 🎢 **Vinpearl / VinWonders:** Trải nghiệm du lịch số hóa, quản lý phòng và luồng khách thông minh tại các khu vui chơi.

Trong buổi Lab hôm nay, nhóm của bạn sẽ đóng vai trò là **AI Product Engineer** tại **Vin Smart Future**, tiến hành tìm kiếm, scoping, phân tích độ khả thi, thiết lập ranh giới vận hành, và xây dựng một **bản mẫu kỹ thuật (prompt prototype)** cho một bài toán cụ thể thuộc một trong những mảng kinh doanh trên.

---

## 📊 2. Cơ cấu tính điểm bài lab

### 👥 Điểm nhóm (60 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **G1. Workflow Mapping** | 20 | Problem Deep-Dive | Vẽ chi tiết quy trình hiện tại: các bước, handoff, thời gian, bottleneck |
| **G2. Problem Statement** | 20 | Problem Deep-Dive | Problem Statement 6-field bám sát thực tế, metric có số và ranh giới rõ ràng |
| **G3. AI Fit & Future Flow** | 10 | Problem Deep-Dive | So sánh Rule vs LLM vs Agent, future flow có bước AI, ranh giới và Fallback |
| **G4. Decision Quality** | 10 | Problem Deep-Dive | Quyết định Go/Not Yet/No-Go trung thực và có chứng cứ rõ ràng |

### 👤 Điểm cá nhân (40 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **I1. Scan & Cards** | 15 | Quick Cards | Liệt kê 5 problems sử dụng 3 lenses, hoàn thiện 3 quick cards chất lượng |
| **I2. Prototyping** | 10 | 02-lab/ | Chạy thử nghiệm programmatic prompt prototype thành công |
| **I3. AI Log & Reflection** | 15 | 03-ai-log.md | Phản ánh trung thực về việc dùng AI làm thought-partner (giúp gì, sai gì, sửa gì) |

---

# 🚀 Phase 0 — worked Example: Xanh SM Intelligent Dispatcher (15 min)

*Giảng viên walk-through ví dụ thực tế từ Vin Smart Future để bạn hiểu rõ cách scoping một bài toán AI.*
Đọc chi tiết worked example tại file [02-deliverable-example.md](02-deliverable-example.md).

---

# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | VinFast | Lặp lại (Repetitive) | So khớp hóa đơn mua linh kiện với lô nhập kho |
| 2 | VinFast | Tốn thời gian | Tổng hợp báo cáo tồn kho linh kiện cho 5 nhà máy |
| 3 | VinFast | AI-upgrade | Cải thiện Trợ lý AI ảo trong xe để trả lời HDSD và bảo trì |
| 4 | VinFast | Lặp lại (Repetitive) | Phân loại và trả lời yêu cầu bảo hành qua email/tổng đài |
| 5 | VinFast | AI-upgrade (Predictive) | Dự đoán lỗi pin/battery degradation cho đội xe |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Tổng hợp báo cáo tồn kho linh kiện từ 5 ERP       │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên Production Planner (8-10 ng) │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Export ERP ──> 2. Copy-paste Excel ──> 3. Chuẩn hóa ──> 4. Pivot │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2&3 (⏱ 45-60 phút/lượt)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Tự động chuẩn hóa dữ liệu │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   Giảm thời gian tổng hợp từ 60 phút ──> under 10 phút      │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Phân loại và trả lời yêu cầu bảo hành VinFast     │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? 40 nhân viên CSKH (200+ ticket/ngày)   │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Nhận log ──> 2. Tra cứu DMS ──> 3. Phân loại ──> 4. Trả lời │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2&3 (⏱ 12 phút/lượt)  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Trích xuất lỗi & phân loại │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   Giảm thời gian xử lý yêu cầu từ 12 min ──> under 3 min    │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Dự đoán lỗi pin (battery degradation)             │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Kỹ thuật viên bảo trì, Fleet Manager   │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Lấy Data ──> 2. KTV Phân tích ──> 3. Xe hỏng ──> 4. Sửa │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 (⏱ Phát hiện trễ 30 ngày)│
│ AI có thể nhảy vào hỗ trợ ở bước nào? Cảnh báo sớm qua telemetry│
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   Phát hiện lỗi sớm 30-60 ngày trước khi xe hỏng            │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘
```

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*

---

# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping (25 min)
**Vẽ quy trình hiện tại (Sơ đồ text):** Bài toán Phân loại & Trả lời Bảo hành VinFast
1. Nhận email/cuộc gọi khách hàng 🔄
2. 🔴 Tra cứu hệ thống DMS (Lịch sử bảo dưỡng) - **Bottleneck**
3. 🔴 Phân loại thủ công vào 8 loại vấn đề - **Bottleneck**
4. Soạn phản hồi và gửi 🔄
* Thời gian vận hành trung bình: **Tổng cộng = 12 phút/lượt**.

## 3.2. Problem Statement (6-field) & Metrics (15 min)
Điền đầy đủ 6 trường thông tin của bài toán:

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | 40 nhân viên CSKH VinFast (xử lý 200+ yêu cầu/ngày). |
| **2. Current Workflow** | Đọc khiếu nại, tra cứu mã VIN trên DMS, phân loại thủ công, soạn email/ticket phản hồi mẫu. |
| **3. Bottleneck** | Bước tra cứu DMS và phân loại ngôn ngữ tự nhiên tốn 8-15 phút, dễ sai sót giữa lỗi phần mềm/phần cứng. |
| **4. Business Impact** | Chậm trễ SLA phản hồi (quá 24h), throughput thấp, khách hàng phàn nàn và giảm CSAT. |
| **5. Success Metric** | Phân loại chính xác 95% yêu cầu dưới 10 giây/ticket, giảm thời gian xử lý tổng xuống dưới 3 phút. |
| **6. Operational Boundary** | AI chỉ được phép gán nhãn loại lỗi và soạn nháp [DRAFT_ONLY]. TUYỆT ĐỐI KHÔNG tự động gửi cho khách hàng. Mọi bản nháp phải có người duyệt (HITL). |

## 3.3. Future-State Flow & AI Fit (25 min)
* **Xác định mức AI Fit (AI-Fit Matrix):** Giải pháp thuộc nhóm nào? [ ] Rule / State-Machine [x] LLM Feature [ ] Agentic Loop.
* **Vẽ Future-State Flow:**
  * 1. Hệ thống nhận email tự động.
  * 2. 🔵 **AI Step:** LLM trích xuất mã VIN, phân loại lỗi và sinh bản nháp trả lời.
  * 3. 🟢 **Human Step (HITL):** Nhân viên CSKH đọc lại bản nháp, chỉnh sửa và bấm nút gửi.
  * 4. ↩️ **Fallback:** Nếu LLM độ tự tin thấp hoặc input quá phức tạp, trả về nhãn `UNABLE_TO_CLASSIFY` để nhân viên tự làm từ đầu.

---

# 💻 Phase 4 — TECHNICAL PROMPT PROTOTYPE (Nhóm, 30 min)

Để đảm bảo kỹ sư của Vin Smart Future luôn giữ vững năng lực lập trình, nhóm của bạn sẽ tiến hành **lập trình bản mẫu prompt** trực tiếp trên **Gemini 2.5 Flash** bằng Python để stress-test hệ thống.

### Hướng dẫn thực hiện:
1. Mở file [starter-code/prompt_prototype.py](starter-code/prompt_prototype.py) bằng VS Code/Cursor.
2. Hoàn thiện các nội dung sau:
   * **System Prompt:** Viết chỉ thị cực kỳ nghiêm ngặt quy định vai trò, nhiệm vụ, định dạng output và **Operational Boundary (Ranh giới cấm)** của mô hình.
   * **Structured Output:** Định nghĩa định dạng JSON output rõ ràng.
   * **Adversarial Test Cases:** Viết ít nhất 3 prompts "tấn công" (Adversarial inputs) cố tình dụ AI vượt ranh giới hoặc đưa ra câu trả lời không được phép để kiểm tra xem ranh giới của bạn có thực sự vững chắc.
3. Chạy file python:
   ```bash
   python3 prompt_prototype.py
   ```
4. Kiểm tra xem các ranh giới an toàn có bị LLM phá vỡ hay không và ghi lại kết quả vào worksheet.

---

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:
1. [x] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? (Lịch sử ticket/email cũ)
2. [x] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)? (Nhờ có Human-in-the-loop review bản draft)
3. [x] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? (Bộ phận CSKH đang quá tải và cần giải pháp)

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> **GO.** Bài toán có đủ lượng dữ liệu lịch sử để phát triển và test prompt ngay lập tức. Rủi ro hoạt động (AI sinh phản hồi sai) được rào kỹ bằng ranh giới (Operational Boundary) yêu cầu mọi output phải gắn mác `[DRAFT_ONLY]` và phải qua con người duyệt (HITL). Success metric rõ ràng (rút ngắn thời gian từ 12 phút xuống dưới 3 phút/ticket) sẽ tiết kiệm nhân lực đáng kể, đảm bảo ROI cao để xây dựng một bản MVP cho quy trình CSKH.

---

# 📝 Phase 6 — REFLECTION (Cá nhân)
*Ghi nhận phản ánh của cá nhân bạn về việc phối hợp với AI trong buổi học hôm nay vào file `03-ai-log.md`.*
