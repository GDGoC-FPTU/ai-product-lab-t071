# 03-ai-log.md — AI Interaction Log (Bài cá nhân - 15 điểm)

> **Họ và tên:** [ĐỖ Nhật Minh]  
> **MSSV:** [26A202601085]  
> **Ngày:** 24/07/2026  
> **Công ty thành viên được chọn để SCAN:** **VinFast**

---

## 1. AI giúp gì?

Trong Lab 02, tôi dùng AI như một **thought-partner** để đặt câu hỏi, phản biện giả định và chuẩn hóa cách trình bày. AI không thay tôi chọn bài toán hay đưa ra quyết định vận hành cuối cùng.

### 1.1 Brainstorm theo 4 lenses

- Tôi yêu cầu AI gợi ý pain point theo bốn lenses của worksheet: lặp lại, tốn thời gian, AI-upgrade và stakeholder pain.
- AI giúp mở rộng danh sách sang năm bối cảnh khác nhau: đối chiếu hóa đơn sạc VinFast, phản ánh cư dân Vinhomes, email đặt phòng đoàn Vinpearl, tóm tắt xuất viện Vinmec và phân tích hủy chuyến Xanh SM.
- Sau đó tôi tự sàng lọc theo ba tiêu chí: có actor rõ ràng, có workflow hiện tại mô tả được, và có metric có thể đo từ dữ liệu vận hành. Vì vậy, tôi chọn Card #2 — phân loại và điều hướng phản ánh cư dân Vinhomes làm ứng viên ưu tiên.

### 1.2 Hoàn thiện Quick Problem Cards

- AI hỗ trợ chuyển ý tưởng ban đầu thành cấu trúc Card: actor, quy trình 3–5 bước, bottleneck, AI step, metric và kiến trúc sơ bộ.
- Với phản ánh cư dân, AI giúp tách hai phần công việc: LLM đọc tiếng Việt tự do để trích xuất loại sự cố/vị trí/mức độ khẩn; rule-based router đối chiếu taxonomy, tòa nhà và SLA để gợi ý đội xử lý.
- Tôi cũng dùng AI để kiểm tra metric. Ví dụ, thay vì chỉ viết “xử lý nhanh hơn”, card đã có ngưỡng cụ thể: phân loại đúng lần đầu ≥85%, giảm thời gian phân luồng từ 8 phút xuống ≤2 phút và tỷ lệ chuyển lại <8%.

### 1.3 Phản biện kiến trúc và ranh giới

- Tôi yêu cầu AI đóng vai Trưởng vận hành để tìm rủi ro khi tự động hóa ticket. Phản biện quan trọng nhất là không để AI tự cam kết chi phí, tự đóng ticket hoặc gửi thông báo cho cư dân.
- Từ phản biện đó, tôi xác định mô hình phù hợp là **Rule + LLM Feature**, không phải agent tự trị: nhân viên CSKH duyệt ticket nháp trước khi tạo hoặc gửi, còn trường hợp khẩn/độ tin cậy thấp phải chuyển người trực xử lý.

---

## 2. AI sai gì?

### 2.1 Hallucination — trình bày số ước tính như dữ liệu nội bộ

Khi được hỏi về quy mô vận hành và hiệu quả của quy trình Vinhomes, AI đưa ra các con số như thời gian xử lý ticket và tỷ lệ phân loại như thể là số liệu thực tế. Tuy nhiên, AI không có quyền truy cập dashboard hoặc log ticket nội bộ để xác nhận các số này.

Đây là một sai lệch quan trọng vì các KPI giả định, nếu không được gắn nhãn, có thể làm Problem Statement trông có căn cứ hơn thực tế. Tôi không dùng các con số đó như facts. Trong `01-problem-scan.md`, các số 8 phút/ticket, 85% và 2 phút được ghi rõ là **ước tính phục vụ scoping**, cần đo baseline bằng ticket lịch sử trước khi pilot.

**Bài học:** AI có thể giúp đặt giả thuyết KPI, nhưng dữ liệu kinh doanh phải được kiểm chứng bằng nguồn vận hành hoặc nguồn chính thức.

### 2.2 Over-engineering — đề xuất agent tự chuyển ticket và tự trả lời

Ở lần gợi ý đầu, AI đề xuất một agent có thể đọc phản ánh, tự chọn nhà thầu, gửi phản hồi và tự đóng ticket khi cho rằng vấn đề đã xong. Cách này quá rộng so với phạm vi bài toán:

- Nội dung phản ánh có thể liên quan đến an toàn, tranh chấp hoặc phí dịch vụ.
- Một ticket route sai có thể làm chậm xử lý sự cố thực tế.
- Không có cơ sở để agent tự đánh giá việc sự cố đã được giải quyết.

Vì vậy, “tự động hóa toàn bộ” không phải là mục tiêu đúng. Giá trị chính của AI ở đây là giảm thời gian đọc hiểu và tạo bản nháp có cấu trúc, còn quyết định gửi/chuyển/đóng vẫn thuộc về nhân viên.

### 2.3 Ranh giới prompt ban đầu chưa đủ chặt

Prompt ban đầu chỉ nêu “phân loại phản ánh và trả lời thân thiện”. Chỉ dẫn này quá mơ hồ: AI có thể suy đoán căn hộ còn thiếu, hứa thời gian xử lý hoặc coi một phản ánh khẩn là ticket thông thường. Những hành vi này không phù hợp với CSKH vận hành.

AI đã không tự đặt đủ các điều kiện dừng như confidence thấp, thiếu trường bắt buộc và từ khóa khẩn cấp. Tôi phải bổ sung ranh giới rõ ràng và thiết kế fallback thay vì chỉ kỳ vọng model “tự hiểu”.

---

## 3. Sửa đổi ra sao?

### 3.1 Phân biệt fact và giả định

**Trước:** Dùng câu hỏi mở kiểu “thời gian xử lý ticket Vinhomes là bao nhiêu?” và dễ nhận được câu trả lời nghe hợp lý nhưng không có nguồn.

**Sau:** Tôi yêu cầu AI tách riêng ba nhãn: `FACT_CITED`, `ASSUMPTION_FOR_PILOT` và `DATA_NEEDED`. Các con số chưa có nguồn được ghi là giả định; các dữ liệu cần xác minh gồm taxonomy ticket hiện có, tỷ lệ route đúng baseline, thời gian theo từng loại sự cố và số lần chuyển tiếp ticket.

### 3.2 Viết lại system prompt theo nguyên tắc draft-only

**Trước:**

```text
Đọc phản ánh cư dân, phân loại và trả lời nhanh chóng.
```

**Sau:**

```text
Bạn là trợ lý tạo BẢN NHÁP ticket cho nhân viên CSKH Vinhomes.
Chỉ trích xuất: loại sự cố, tòa/căn hộ nếu được cung cấp,
mức độ khẩn, thông tin còn thiếu và tuyến xử lý được đề xuất.

Không được gửi tin cho cư dân, không tự tạo/đóng ticket, không cam kết
chi phí hoặc thời hạn xử lý, không suy đoán dữ liệu thiếu.
Trả về JSON theo schema cố định kèm confidence.
Nếu confidence thấp, thiếu vị trí, hoặc có dấu hiệu khẩn cấp, trả về
NEEDS_HUMAN_REVIEW.
```

### 3.3 Bổ sung rule-based guardrails và fallback

Tôi không để ranh giới chỉ nằm trong prompt. Luồng xử lý dự kiến được sửa thành:

```text
Phản ánh cư dân
→ Rule kiểm tra từ khóa khẩn cấp / dữ liệu định danh bắt buộc
→ LLM tạo JSON ticket nháp
→ Validate schema + confidence + mapping taxonomy
→ Nhân viên CSKH review và quyết định tạo/chuyển ticket
→ Fallback form thủ công nếu thiếu dữ liệu hoặc kết quả không hợp lệ
```

Như vậy, LLM không trực tiếp thực hiện hành động có tác động ra bên ngoài. Rule xử lý các điều kiện xác định; con người xử lý ngoại lệ và mọi quyết định ảnh hưởng đến cư dân.

### 3.4 Chọn LLM Feature thay vì Agentic Loop

| Tiêu chí | Agent tự trị | Rule + LLM Feature |
|---|---|---|
| Mục tiêu | Tự xử lý toàn bộ ticket | Tạo và phân luồng ticket nháp |
| Khả năng kiểm soát | Thấp hơn, nhiều hành động tự động | Cao, có rule và HITL |
| Rủi ro sai sót | Cao khi thông tin mơ hồ/nhạy cảm | Thấp hơn vì có bước duyệt |
| Phù hợp giai đoạn pilot | Không phù hợp | **Phù hợp** |
| **Quyết định** | ❌ Không chọn | ✅ **Chọn** |

---

## 4. Tổng kết

| Khía cạnh | Nhận xét cá nhân |
|---|---|
| **AI làm tốt** | Brainstorm, biến mô tả thành workflow, phản biện metric và gợi ý cấu trúc output. |
| **AI làm chưa tốt** | Có thể bịa mức độ chắc chắn của dữ liệu, đề xuất kiến trúc quá phức tạp và bỏ sót điều kiện dừng. |
| **Cách dùng hiệu quả** | Dùng AI để tạo giả thuyết và phản biện; kiểm chứng fact bằng dữ liệu; đặt rule, schema, HITL và fallback trước khi tích hợp. |
| **Bài học chính** | AI nên hỗ trợ nhân viên ra quyết định, không thay thế quyền quyết định trong các luồng có rủi ro vận hành. |

---

*Bài cá nhân — Lab 02: AI Product Scoping — Hoàng Trường Minh — Ngày 24/07/2026*
