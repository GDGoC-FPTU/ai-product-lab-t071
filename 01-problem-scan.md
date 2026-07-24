# 01-problem-scan.md — Phase 1 & 2 Individual Work

> **Họ và tên:** [Lê Hồng Đức]  
> **MSSV:** [2A202601313]  
> **Ngày:** 24/07/2026  
> **Công ty thành viên được chọn để SCAN:** **VinFast**

---

## 🔍 Phase 1 — SCAN: Bảng Quét Cơ Hội (Tập trung vào VinFast)

Sử dụng 4 Lenses để quét bài toán từ các công ty thành viên Vingroup, **tập trung chính vào VinFast**.

| # | Công ty thành viên | Lens | Mô tả bài toán |
|---|---------------------|------|----------------|
| 1 | **VinFast** | Lặp lại (Repetitive) | So khớp hóa đơn mua linh kiện với lô nhập kho hàng ngày. Nhân viên kho phải đối chiếu thủ công hàng trăm dòng Excel giữa phiếu nhập kho và hóa đơn từ nhà cung cấp. |
| 2 | **VinFast** | Tốn thời gian (Time-consuming) | Tổng hợp báo cáo tồn kho linh kiện cho 5 nhà máy sản xuất. Nhân viên planner phải ghép dữ liệu từ 5 hệ thống ERP khác nhau vào một file Excel tổng hợp mất 2-3 giờ mỗi ngày. |
| 3 | **VinFast** | AI-upgrade | Trợ lý AI ảo trong xe VinFast hiện tại chỉ trả lời được các câu lệnh đơn giản (mở nhạc, bật điều hòa). Không xử lý được các câu hỏi phức tạp về hướng dẫn sử dụng xe, bảo trì, hoặc tình huống khẩn cấp. |
| 4 | **VinFast** | Stakeholder Pain | Khách hàng phàn nàn về việc hệ thống dự đoán trạm sạc còn trống trên app VinFast hiển thị sai/trễ, dẫn đến phải chờ 20-40 phút tại trạm sạc đông đúc. |
| 5 | **VinFast** | Lặp lại (Repetitive) | Phân loại và trả lời yêu cầu bảo hành qua email/tổng đài. Nhân viên CSKH phải đọc 200+ email/ngày, đối chiếu mã VIN, kiểm tra lịch sử bảo dưỡng, và phân loại thủ công theo 8 loại vấn đề. |
| 6 | **VinFast** | AI-upgrade (Predictive) | Dự đoán lỗi pin/battery degradation cho đội xe VinFast đang vận hành. Hiện tại chỉ phát hiện khi xe đã hỏng, chi phí sửa chữa trung bình 30-50 triệu/xe. |
| 7 | **VinFast** | Stakeholder Pain | Đại lý và nhân viên sales mất 30-45 phút soạn báo giá tùy chỉnh cho khách hàng doanh nghiệp (B2B) khi mua 50-200 xe, bao gồm tính toán ưu đãi, thuế, và các option. |

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

---

### QUICK PROBLEM CARD #1

**Bài toán:** Tổng hợp báo cáo tồn kho linh kiện từ 5 hệ thống ERP khác nhau tại VinFast

**Công ty thành viên:** [x] VinFast [ ] Xanh SM [ ] Vinhomes [ ] Vinmec [ ] Vinpearl

**Ai đang đau (Actor)?**
- Nhân viên Production Planner tại bộ phận Supply Chain
- Số lượng: 8-10 người, mỗi người phụ trách 1 nhà máy
- Thực hiện tác vụ này 2 lần/ngày (sáng và chiều)

**Workflow thủ công hiện tại (5 bước):**
1. **Đăng nhập hệ thống ERP nhà máy 1** → Export file Excel tồn kho
2. **Đăng nhập hệ thống ERP nhà máy 2-5** (lần lượt) → Export 4 file Excel riêng biệt
3. **Mở file Excel tổng hợp** → Copy dữ liệu từ 5 file vào 5 sheet
4. **Chuẩn hóa định dạng** →统一 SKU naming, đơn vị tính, loại bỏ trùng lặp
5. **Tạo Pivot Table** → Tính tổng tồn kho, xuất báo cáo PDF gửi cho quản lý

**Bước nào tốn thời gian/lỗi nhất?**
- **Bước 3 & 4** (⏱ **45-60 phút/lượt**) — Thao tác copy-paste thủ công dễ sai sót (sai dòng, thiếu cột, trùng dữ liệu), đặc biệt khi có 2000+ SKU.

**AI có thể nhảy vào hỗ trợ ở bước nào?**
- **Bước 3 & 4**: AI (LLM) có thể đọc 5 file Excel đầu vào, chuẩn hóa SKU naming và đơn vị tính tự động, ghép vào một file JSON/CSV duy nhất.

**Đo thành công bằng gì (Metric có số)?**
- ✅ Giảm thời gian tổng hợp từ **60 phút → dưới 10 phút**
- ✅ Giảm tỷ lệ lỗi copy-paste từ **~5% → dưới 0.5%**
- ✅ Số lượng báo cáo có thể tạo/ngày: từ 2 lần → 6 lần (real-time)

**Quick Architecture:** [ ] No AI [ ] Rule [x] LLM [ ] Agent

---

### QUICK PROBLEM CARD #2

**Bài toán:** Phân loại và trả lời email/tổng đài yêu cầu bảo hành xe VinFast

**Công ty thành viên:** [x] VinFast [ ] Xanh SM [ ] Vinhomes [ ] Vinmec [ ] Vinpearl

**Ai đang đau (Actor)?**
- Đội ngũ Chăm sóc Khách hàng (CSKH) VinFast — 40 nhân viên
- Trung bình mỗi nhân viên nhận 200+ email/ngày và 50-80 cuộc gọi tổng đài
- Team Lead phải review lại trước khi chuyển tiếp đến kỹ thuật viên

**Workflow thủ công hiện tại (4 bước):**
1. **Nhận email/cuộc gọi** → Ghi nhận mã VIN, số điện thoại, mô tả lỗi
2. **Tra cứu hệ thống DMS** → Kiểm tra lịch sử bảo dưỡng, bảo hành còn hạn không
3. **Phân loại thủ công** theo 8 loại vấn đề (pin, động cơ, màn hình, phần mềm, sạc, nội thất, khác...)
4. **Soạn phản hồi mẫu** → Gửi khách hàng + chuyển phòng ban kỹ thuật xử lý

**Bước nào tốn thời gian/lỗi nhất?**
- **Bước 2 & 3** (⏱ **8-15 phút/yêu cầu**) — Tra cứu DMS mất thời gian, phân loại thủ công dễ nhầm lẫn giữa các loại lỗi (đặc biệt lỗi phần mềm vs phần cứng).

**AI có thể nhảy vào hỗ trợ ở bước nào?**
- **Bước 2 & 3**: LLM đọc nội dung email/mô tả lỗi từ cuộc gọi (transcript), tự động tra cứu mã VIN trong DMS, phân loại vấn đề theo 8 category, và soạn phản hồi sơ bộ.

**Đo thành công bằng gì (Metric có số)?**
- ✅ Giảm thời gian xử lý yêu cầu từ **12 phút → dưới 3 phút/yêu cầu**
- ✅ Tăng throughput CSKH từ **200 email/ngày → 600+ email/ngày**
- ✅ Giảm tỷ lệ phân loại sai từ **~15% → dưới 5%**

**Quick Architecture:** [ ] No AI [ ] Rule [x] LLM [ ] Agent

---

### QUICK PROBLEM CARD #3

**Bài toán:** Dự đoán lỗi pin (battery degradation/anomaly) cho đội xe VinFast đang vận hành

**Công ty thành viên:** [x] VinFast [ ] Xanh SM [ ] Vinhomes [ ] Vinmec [ ] Vinpearl

**Ai đang đau (Actor)?**
- Đội ngũ Kỹ thuật viên Bảo trì tại Trung tâm Dịch vụ VinFast
- Quản lý Đội xe (Fleet Manager) cho khách hàng doanh nghiệp
- Bộ phận Customer Retention — chịu trách nhiệm về trải nghiệm sau bán

**Workflow thủ công hiện tại (5 bước):**
1. **Thu thập dữ liệu telemetry** từ xe (dung lượng pin, nhiệt độ, số lần sạc, quãng đường) mỗi 24h
2. **Kỹ thuật viên phân tích thủ công** → Đối chiếu với bảng tiêu chuẩn nhà máy
3. **Gọi điện khảo sát khách hàng** → Hỏi về cảm nhận thực tế (nếu có triệu chứng)
4. **Phát hiện lỗi khi xe đã hỏng** → Đưa vào xưởng sửa chữa
5. **Báo giá sửa chữa** → Email cho khách hàng + xếp lịch sửa (lead time 3-7 ngày)

**Bước nào tốn thời gian/lỗi nhất?**
- **Bước 2 & 4** (⏱ **Phát hiện muộn 30-90 ngày**) — Lỗi chỉ phát hiện khi xe đã hỏng hoàn toàn, chi phí sửa chữa cao 30-50 triệu/xe, khách hàng mất niềm tin, tăng tỷ lệ churn.

**AI có thể nhảy vào hỗ trợ ở bước nào?**
- **Bước 2 & 3**: ML/LLM Agent phân tích dữ liệu telemetry, so sánh với pattern lỗi đã biết, đưa ra cảnh báo sớm 30-60 ngày trước khi xe hỏng, tự động gửi notification cho khách hàng và book lịch bảo trì.

**Đo thành công bằng gì (Metric có số)?**
- ✅ Phát hiện lỗi sớm **30-60 ngày trước** khi xe hỏng (thay vì 0 ngày)
- ✅ Giảm chi phí sửa chữa khẩn cấp từ **40 triệu/xe → dưới 5 triệu/xe**
- ✅ Giảm tỷ lệ xe hỏng trên đường từ **3% → dưới 0.5%**
- ✅ Tăng Customer Retention Rate từ **75% → 90%+**

**Quick Architecture:** [ ] No AI [ ] Rule [ ] LLM [x] Agent

---

## 📊 Tổng kết

| Card | Bài toán | Công ty | Lens chính | Metric cải thiện |
|------|----------|---------|------------|------------------|
| #1 | Tổng hợp báo cáo tồn kho ERP | VinFast | Tốn thời gian | 60 phút → 10 phút |
| #2 | Phân loại & trả lời yêu cầu bảo hành | VinFast | Lặp lại | 12 phút → 3 phút |
| #3 | Dự đoán lỗi pin đội xe | VinFast | AI-upgrade (Predictive) | Phát hiện sớm 30-60 ngày |

---

*Tất cả 3 cards đều tập trung vào VinFast, sử dụng 3 lenses khác nhau: Tốn thời gian, Lặp lại, AI-upgrade.*

*Bài cá nhân Phase 1 & 2 — Lab 02: AI Product Scoping — Vin Smart Future*
