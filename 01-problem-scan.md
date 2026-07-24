# 📄 File: 01-problem-scan.md
**Họ và tên:** [Dương Minh Quân]  
**MSSV:** [2A202601903]  
Ngày: 24/07/2026
**Đơn vị:** Vin Smart Future — AI Product Scoping Lab  

---

# 🔍 Phase 1 — SCAN: Bảng Quét Cơ Hội AI Vingroup

Sử dụng **4 Lenses** (Lặp lại, Tốn thời gian, AI-upgrade, Stakeholder Pain) để quét qua hoạt động vận hành của các công ty thành viên thuộc Vingroup.

### 📝 Bảng 5 Bài Toán Vận Hành (SCAN List):

| # | Subsidiary | Lens | Mô tả ngắn bài toán / Bottleneck |
|---|------------|------|----------------------------------|
| 1 | **Xanh SM (GSM)** | Tốn thời gian & Stakeholder Pain | Điều phối viên xử lý thủ công sự cố tài xế báo pin yếu/hết pin giữa đường: tra cứu bản đồ, kiểm tra trụ sạc VinFast còn trống, soạn tin nhắn chỉ dẫn hoặc gọi cứu hộ di động (mất 15 phút/lượt). |
| 2 | **Vinhomes** | AI-upgrade & Tốn thời gian | Ban quản lý mất nhiều thời gian đọc và phân loại thủ công phản ánh của cư dân trên App Vinhomes Resident, dẫn đến trả lời rập khuôn, chậm trễ SLA (mất 12 tiếng để phản hồi ban đầu). |
| 3 | **Vinmec** | Pain từ người khác & Tốn thời gian | Bác sĩ mất quá nhiều thời gian (20–30 phút/bệnh nhân) đọc EHR (Bệnh án điện tử) và kết quả xét nghiệm để viết bản Tóm tắt hồ sơ xuất viện (Discharge Summary) gây quá tải hành chính. |
| 4 | **VinFast** | Lặp lại | So khớp và đối chiếu thủ công dữ liệu hóa đơn sạc điện hằng tuần từ hàng nghìn trụ sạc đối tác ngoài với telemetry sạc pin thực tế của xe. |
| 5 | **Vinpearl** | Tốn thời gian | Đọc và trích xuất thông tin từ email đặt phòng theo đoàn (Group Booking) phức tạp từ các công ty lữ hành để kiểm tra quỹ phòng trống và draft lệnh book. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

Chọn top 3 bài toán tiềm năng nhất từ Phase 1: **Card #1 (Xanh SM Sự cố sạc)**, **Card #2 (Vinhomes CSKH Cư dân)**, **Card #3 (Vinmec Hồ sơ xuất viện)**.

---

### ┌─────────────────────────────────────────────────────────────┐
### │ QUICK PROBLEM CARD #1                                       │
```text
Bài toán (1 câu): Điều phối viên xử lý thủ công các báo cáo sự cố hết pin/pin yếu thực địa của tài xế Xanh SM để chỉ dẫn trạm sạc VinFast phù hợp hoặc điều xe cứu hộ.

Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes
                    [ ] Vinmec   [ ] Khác (Ghi rõ)________

Ai đang đau (Actor)? Điều phối viên (Dispatcher) quá tải giờ cao điểm & Tài xế Xanh SM (chờ đợi, lo lắng cạn pin).

Workflow thủ công hiện tại (5 bước):
  1. Nhận cuộc gọi/tin báo sự cố pin ──> 2. Tra cứu tọa độ GPS xe trên bản đồ ──> 3. Tra cứu Dashboard trạm sạc VinFast còn trụ trống ──> 4. Soạn tin nhắn hướng dẫn đường đi gửi qua App tài xế ──> 5. Gọi xe cứu hộ nếu pin < 5%.

Bước nào tốn thời gian/lỗi nhất? Bước 3 & 4 (⏱ 10 phút/lượt)
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 & 4 (Tự động pull tọa độ xe, kiểm tra API trạm sạc, draft tin nhắn hướng dẫn đường đi chuẩn xác).

Đo thành công bằng gì (Metric có số)?
Giảm tổng thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút/lượt; Tỉ lệ hướng dẫn đúng loại trụ sạc còn trống đạt 98%.

Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2 │
code
Text
Bài toán (1 câu): Hệ thống phân loại tự động intent/độ khẩn cấp và draft phản hồi ban đầu cho các ý kiến/khiếu nại của cư dân trên App Vinhomes Resident.

Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes
                    [ ] Vinmec   [ ] Khác (Ghi rõ)________

Ai đang đau (Actor)? Chuyên viên CSKH / Ban Quản Lý tòa nhà Vinhomes (quá tải phản ánh) & Cư dân (chờ phản hồi lâu).

Workflow thủ công hiện tại (4 bước):
  1. Đọc khiếu nại của cư dân trên App ──> 2. Phân loại thủ công (Sửa chữa, Tiếng ồn, An ninh, Phí dịch vụ) ──> 3. Chuyển tiếp (Route) yêu cầu tới BQL tòa nhà ──> 4. Viết email/tin nhắn phản hồi tiến độ cho cư dân.

Bước nào tốn thời gian/lỗi nhất? Bước 2 & 4 (⏱ 12 phút/yêu cầu)
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 4 (Tự động phân loại chính xác nhóm sự cố, đánh dấu độ khẩn cấp và soạn nháp tin nhắn phản hồi lịch sự theo đúng chuẩn Vinhomes).

Đo thành công bằng gì (Metric có số)?
Rút ngắn thời gian CSKH xử lý ban đầu từ 12 phút ──> dưới 2 phút; Giảm SLA phản hồi cư dân từ 12 giờ ──> dưới 1 giờ.

Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3 │
code
Text
Bài toán (1 câu): Trích xuất thông tin lâm sàng từ Bệnh án điện tử (EHR) để tự động soạn thảo bản Tóm tắt hồ sơ xuất viện (Discharge Summary) bằng ngôn ngữ dễ hiểu cho bệnh nhân.

Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes
                    [x] Vinmec   [ ] Khác (Ghi rõ)________

Ai đang đau (Actor)? Bác sĩ điều trị Vinmec (kiệt sức vì công việc hành chính) & Bệnh nhân xuất viện (đọc hồ sơ chuyên môn khó hiểu).

Workflow thủ công hiện tại (4 bước):
  1. Đọc lại toàn bộ lịch sử khám, xét nghiệm trên EHR ──> 2. Tổng hợp danh mục thuốc và chỉ định xuất viện ──> 3. Soạn thảo văn bản Tóm tắt xuất viện và dặn dò tái khám bằng từ ngữ bình dân ──> 4. Bác sĩ rà soát và ký xác nhận.

Bước nào tốn thời gian/lỗi nhất? Bước 1 & 3 (⏱ 25 phút/bệnh nhân)
AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1 & 3 (Trích xuất tự động dữ liệu lâm sàng, draft bản tóm tắt xuất viện để bác sĩ duyệt).

Đo thành công bằng gì (Metric có số)?
Giảm thời gian bác sĩ soạn thảo hồ sơ xuất viện từ 25 phút ──> dưới 5 phút/bệnh nhân (Bác sĩ chỉ cần review & ký duyệt).

Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM Feature  [ ] Agent
└─────────────────────────────────────────────────────────────┘
