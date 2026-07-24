# 03 — AI Log & Reflection (Cá nhân)

## Phase 6 — Reflection: Phối hợp với AI trong buổi Lab

### 1. AI đã giúp gì?

- **Brainstorm ở Phase 1 (Scan):** Dùng AI để quét nhanh qua các mảng kinh doanh (VinFast, Xanh SM, Vinhomes, Vinmec, Vinpearl) theo 4 lenses thay vì chỉ nghĩ trong phạm vi 1-2 mảng quen thuộc. AI giúp mình có đủ 5 bài toán đa dạng công ty thành viên thay vì dồn hết vào một chỗ.
- **Chuẩn hóa cấu trúc Quick Card:** AI giúp điền đúng format 6 trường (Actor, Workflow, Bottleneck, Metric, Architecture...) nhất quán giữa 3 card, tránh việc quên trường hoặc viết metric mơ hồ không có số.
- **Gợi ý Metric có số:** Ở card #2 (Vinmec), lúc đầu mình chỉ viết "giảm thời gian soạn tóm tắt", AI gợi ý thêm cả điều kiện ràng buộc an toàn (100% vẫn qua bác sĩ ký duyệt) — nếu không có AI nhắc, mình dễ bỏ sót phần Operational Boundary quan trọng này.

### 2. AI sai/chưa chuẩn ở đâu?

- Bản nháp đầu tiên của card #1 (VinFast), AI đề xuất Architecture là "Agent" cho việc đối chiếu mã lỗi BMS — nhưng đây thực chất là một bài toán tra cứu/matching có cấu trúc cố định (catalog lỗi đã biết trước), không cần vòng lặp tự trị. Mình đã sửa lại thành **LLM Feature** vì rủi ro thấp và không cần AI tự quyết định hành động tiếp theo.
- Ở card #3 (Vinpearl), số liệu thời gian ban đầu AI đưa ra hơi chung chung ("vài phút"), mình phải tự ước lượng lại thành con số cụ thể hơn (8 phút/lượt) dựa trên mô tả quy trình 4 bước để metric có căn cứ thực tế hơn, tránh con số bịa đặt không kiểm chứng được.

### 3. Mình đã sửa/kiểm chứng lại như thế nào?

- Đối chiếu lại từng Quick Card với tiêu chí "AI có thể tốt hơn Rule-based không" — loại bỏ các đề xuất Agent không cần thiết, chỉ giữ LLM Feature cho các bài toán có input/output rõ ràng, rủi ro kiểm soát được qua HITL.
- Tự kiểm tra tính nhất quán giữa Bottleneck và AI-support-step trong mỗi card để đảm bảo AI được đặt đúng vào bước gây tắc nghẽn thực sự, không phải bước bất kỳ.
- Rà lại toàn bộ 3 card để đảm bảo mỗi card đều có Operational Boundary ngầm định (HITL, không tự động gửi/quyết định) phù hợp với tinh thần ranh giới an toàn của bài Lab, dù worksheet Phase 2 không bắt buộc ghi rõ trường này.

### 4. Nhận xét chung

AI là công cụ brainstorm và chuẩn hóa cấu trúc hiệu quả, giúp tiết kiệm thời gian ở giai đoạn Scan và soạn thảo Quick Card. Tuy nhiên, việc chọn đúng AI-Fit Architecture (Rule vs LLM vs Agent) và ước lượng số liệu thực tế vẫn cần tư duy phản biện của bản thân — AI có xu hướng đề xuất giải pháp "nặng đô" hơn mức cần thiết (ví dụ Agent) nếu không được yêu cầu giải thích rõ lý do lựa chọn.
