> **Họ và tên:** [ĐỖ Nhật Minh]  
> **MSSV:** [26A202601085]  
> **Ngày:** 24/07/2026  
> **Công ty thành viên được chọn để SCAN:** **VinFast**
# 01 — Problem Scan: Vin Smart Future

> **Bài cá nhân — Lab 02: AI Product Scoping**  
> Các thời gian, sản lượng và chỉ số dưới đây là **ước tính phục vụ scoping ban đầu**, không phải số liệu vận hành nội bộ đã được xác nhận. Trước khi triển khai cần đo baseline trên dữ liệu thực tế.

---

# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội

Tôi dùng 4 lenses (lặp lại, tốn thời gian, AI-upgrade và stakeholder pain) để quét các luồng vận hành có lượng công việc đủ lớn, đầu vào tương đối rõ và có thể đo kết quả.

| # | Công ty thành viên | Lens | Mô tả ngắn bài toán / bottleneck |
|---:|---|---|---|
| 1 | **VinFast** | Lặp lại | Đối chiếu hóa đơn sạc điện từ đối tác với log phiên sạc và bảng giá. Nhân viên tài chính phải kiểm tra từng mã trạm, thời gian, kWh và đơn giá; các dòng lệch cần tra cứu lại thủ công. |
| 2 | **Vinhomes** | Tốn thời gian | Phân loại và điều hướng phản ánh cư dân trên ứng dụng (mất nước, hỏng đèn, thang máy, tiếng ồn…) đến đúng ban quản lý/nhà thầu. Nhân viên CSKH đọc tự do từng tin, hỏi lại thông tin thiếu và chuyển ticket bằng tay. |
| 3 | **Vinpearl** | AI-upgrade | Xử lý email đặt phòng đoàn có ngôn ngữ tự do: trích ngày ở, số khách, loại phòng, yêu cầu đặc biệt rồi kiểm tra tồn phòng và soạn phản hồi. Phản hồi hiện có thể chậm, không nhất quán giữa ca trực. |
| 4 | **Vinmec** | Stakeholder Pain | Bác sĩ dành nhiều thời gian tổng hợp bệnh án, xét nghiệm và dặn dò để soạn tóm tắt xuất viện dễ hiểu. Bệnh nhân chờ lâu, còn bác sĩ chịu áp lực hoàn tất hồ sơ; đây là luồng nhạy cảm, bắt buộc bác sĩ duyệt. |
| 5 | **Xanh SM** | Tốn thời gian | Phân tích lý do hủy chuyến từ ghi chú tài xế và transcript cuộc gọi để nhận diện nhóm nguyên nhân (điểm đón khó tìm, giá, chờ lâu, lỗi ứng dụng…). Báo cáo thủ công chậm nên đội vận hành khó phát hiện xu hướng sớm. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Tôi chọn **#1, #2 và #3** vì có đầu vào xác định, có thể thử nghiệm theo phạm vi hẹp, và có metric vận hành rõ. Card #4 có giá trị cao nhưng rủi ro y tế lớn; card #5 phù hợp phân tích offline nhưng tác động không tức thời bằng ba card được chọn.

## Card #1 — VinFast: Đối chiếu hóa đơn sạc điện đối tác

| Mục | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Tự động đối chiếu các dòng hóa đơn sạc của đối tác với log phiên sạc và bảng giá để nhân viên chỉ cần xử lý ngoại lệ. |
| **Công ty thành viên** | **VinFast** |
| **Ai đang đau (Actor/Operator)?** | Chuyên viên tài chính/đối soát; bộ phận vận hành trạm phải trả lời các truy vấn khi một dòng bị lệch. |
| **Workflow thủ công hiện tại** | 1. Nhận file hóa đơn từ đối tác → 2. Xuất log phiên sạc từ hệ thống → 3. Mở bảng giá/khuyến mại áp dụng → 4. So sánh thủ công mã trạm, thời gian, kWh, đơn giá và tổng tiền → 5. Gắn cờ, hỏi đối tác/vận hành và chốt đối soát. |
| **Bước tốn thời gian/lỗi nhất** | Bước 4: ghép và kiểm tra từng dòng khi mã trạm hoặc thời gian ghi nhận không đồng nhất (**ước tính 6 phút/dòng ngoại lệ**); dễ bỏ sót hoặc ghép nhầm. |
| **AI có thể hỗ trợ ở đâu?** | Không cần LLM cho phép tính. Rule/ETL chuẩn hóa mã, áp dụng tolerance cho thời gian-kWh và tự động ghép các dòng khớp; dashboard chỉ xếp hàng các ngoại lệ để nhân viên quyết định. |
| **Metric thành công** | Tự động khớp **≥95%** dòng hợp lệ; giảm thời gian xử lý một dòng ngoại lệ từ **6 phút xuống ≤2 phút**; tỷ lệ đối soát bị phải sửa sau khi chốt **<1%**. |
| **Quick Architecture** | **[x] Rule / state-machine** · [ ] No AI · [ ] LLM · [ ] Agent. Đây là bài toán dữ liệu có quy tắc xác định; dùng LLM sẽ khó kiểm toán hơn. |
| **Ranh giới tối thiểu** | Hệ thống không tự duyệt thanh toán hoặc sửa hóa đơn; mọi dòng không khớp/giá trị vượt ngưỡng phải do nhân viên đối soát phê duyệt. |

## Card #2 — Vinhomes: Phân loại và điều hướng phản ánh cư dân

| Mục | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Tạo ticket có cấu trúc và đề xuất đúng tuyến xử lý từ phản ánh tiếng Việt tự do của cư dân để giảm thời gian phân luồng. |
| **Công ty thành viên** | **Vinhomes** |
| **Ai đang đau (Actor/Operator)?** | Nhân viên CSKH/ban quản lý tòa; cư dân chờ phản hồi khi sự cố bị chuyển sai bộ phận. |
| **Workflow thủ công hiện tại** | 1. Cư dân gửi tin/ảnh qua app → 2. CSKH đọc, xác định tòa/căn hộ và loại vấn đề → 3. Hỏi lại nếu thiếu vị trí hoặc mức độ khẩn → 4. Tạo ticket và chuyển ban quản lý/nhà thầu → 5. Theo dõi phản hồi, cập nhật cư dân. |
| **Bước tốn thời gian/lỗi nhất** | Bước 2–4: diễn giải tin nhắn mơ hồ và route ticket (**ước tính 8 phút/ticket**); ticket chuyển sai làm tăng một vòng handoff. |
| **AI có thể hỗ trợ ở đâu?** | LLM trích xuất loại sự cố, vị trí, mức độ khẩn và tạo bản nháp ticket; rule-based router kiểm tra tòa nhà/danh mục/ngoài giờ rồi gợi ý đúng đội nhận. Nhân viên duyệt trước khi gửi. |
| **Metric thành công** | **≥85%** ticket được phân loại đúng ngay lần đầu; giảm thời gian tạo và phân luồng từ **8 phút xuống ≤2 phút**; giảm tỷ lệ ticket bị chuyển lại xuống **<8%**. |
| **Quick Architecture** | [ ] No AI · **[x] Rule + LLM Feature** · [ ] Agent. LLM xử lý ngôn ngữ tự do, còn danh mục, SLA và tuyến chuyển dùng rule có thể kiểm tra. |
| **Ranh giới tối thiểu** | AI chỉ tạo **draft**, không cam kết bồi thường/chi phí, không tự đóng ticket, không đưa thông tin cư dân sang ticket khác; ticket khẩn hoặc độ tin cậy thấp phải ưu tiên người trực duyệt. |

## Card #3 — Vinpearl: Trích xuất yêu cầu từ email đặt phòng đoàn

| Mục | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Đọc email đặt phòng đoàn để trích thông tin đặt chỗ, kiểm tra tồn phòng và tạo bản nháp phản hồi nhất quán cho nhân viên reservations. |
| **Công ty thành viên** | **Vinpearl / VinWonders** |
| **Ai đang đau (Actor/Operator)?** | Nhân viên reservations/sales; đối tác lữ hành chờ báo giá hoặc xác nhận khi email có nhiều phương án ngày, phòng và dịch vụ đi kèm. |
| **Workflow thủ công hiện tại** | 1. Nhận email/đính kèm → 2. Đọc và ghi lại ngày đến–đi, số khách, loại phòng, ngân sách/yêu cầu → 3. Mở PMS kiểm tra tồn phòng và chính sách → 4. Soạn email hỏi lại hoặc báo giá → 5. Trưởng ca/nhân viên gửi phản hồi và cập nhật hệ thống. |
| **Bước tốn thời gian/lỗi nhất** | Bước 2 và 4 (**ước tính 12 phút/yêu cầu**): email thường thiếu trường hoặc có nhiều lựa chọn, dễ gõ sai ngày/số khách và soạn phản hồi không đồng nhất. |
| **AI có thể hỗ trợ ở đâu?** | LLM trích xuất sang biểu mẫu có schema, phát hiện trường thiếu và soạn email nháp theo dữ liệu PMS/chính sách đã cấp. API PMS chỉ trả dữ liệu; nhân viên kiểm tra rồi mới tạo booking hoặc gửi email. |
| **Metric thành công** | **≥90%** trường cốt lõi (ngày, số khách, loại phòng) trích xuất đúng sau kiểm tra; giảm thời gian tạo bản nháp từ **12 phút xuống ≤3 phút**; phản hồi đầu tiên trong **≤15 phút** cho **≥80%** email trong giờ làm việc. |
| **Quick Architecture** | [ ] No AI · [ ] Rule · **[x] LLM Feature** · [ ] Agent. Quy trình có điểm dừng rõ; không cần agent tự đặt phòng hay tự gửi thư. |
| **Ranh giới tối thiểu** | AI không được tự xác nhận giá, giữ phòng, tạo booking hay gửi email; không bịa tồn phòng/chính sách. Thiếu dữ liệu hoặc thông tin mâu thuẫn phải tạo câu hỏi làm rõ để nhân viên duyệt. |

---

## Ghi chú ưu tiên

Nếu phải chọn một vấn đề để deep-dive, tôi ưu tiên **Card #2 — Vinhomes phân loại và điều hướng phản ánh cư dân**: giá trị thời gian và chất lượng dịch vụ xuất hiện hàng ngày, AI có vai trò rõ với tiếng Việt tự do, trong khi rule-based routing và human-in-the-loop giúp kiểm soát rủi ro. Cần xác minh trước khi làm pilot: taxonomy ticket hiện có, tỷ lệ route đúng baseline, dữ liệu tòa/căn hộ và chính sách xử lý thông tin cá nhân.
