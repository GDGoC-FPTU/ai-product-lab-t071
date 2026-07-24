# 02-deep-dive-report.md — Vin Smart Future (Bài nhóm - 40 điểm)

> **Tên nhóm:** ____________________
> **Thành viên nhóm:**
> | Họ và tên | MSSV |  


 
 

> |-----------|------|
> |Trương Minh Hoàng |2A202602004 | 
> |Đỗ Nhật Minh  | 2A202601085| |
> |Trần Đức Thiện | 2A202602032 | 
> |Dương Minh Quân |2A202601903 | 
> |Lê Hồng Đức|2A202601313 | 
> |Phạm Sĩ Đức| 2A202601601 | 
> **Ngày:** 24/07/2026

---

## 🗳️ Quyết định lựa chọn bài toán

**Bài toán được chọn:** Phân loại và trả lời yêu cầu bảo hành xe VinFast qua email/tổng đài

**Công ty thành viên:** VinFast

**Lý do lựa chọn:**
- Pain point rõ ràng: 40 nhân viên CSKH xử lý 200+ email/ngày
- Bottleneck cụ thể: Phân loại thủ công theo 8 loại mất 8-15 phút/yêu cầu
- Metric đo lường được: Giảm thời gian từ 22 phút → 5 phút
- Rủi ro thấp: Có HITL (nhân viên review trước khi gửi)

---

## 1. Problem Statement (6-field)

| Field | Nội dung chi tiết |
|-------|-------------------|
| **1. Actor / Operator** | Đội ngũ CSKH VinFast — 40 nhân viên tổng đài viên + 5 Team Lead. Mỗi nhân viên xử lý trung bình 200+ email và 50-80 cuộc gọi/ngày. |
| **2. Current Workflow** | Khi khách hàng gửi email hoặc gọi tổng đài báo sự cố bảo hành:<br>1. Ghi nhận mã VIN, mô tả lỗi<br>2. Đăng nhập DMS tra cứu lịch sử bảo dưỡng<br>3. Phân loại thủ công vào 8 loại vấn đề<br>4. Soạn phản hồi mẫu + chuyển phòng ban kỹ thuật<br>5. Team Lead review trước khi gửi<br>**Tổng thời gian: 22 phút/lượt** |
| **3. Bottleneck** | Bước 2 & 3 (mất 13 phút): Tra cứu DMS mất thời gian, phân loại thủ công dễ nhầm lẫn giữa các loại lỗi (đặc biệt lỗi phần mềm vs phần cứng). Tỷ lệ phân loại sai ~15%. |
| **4. Business Impact** | ~200 yêu cầu/ngày × 22 phút = **73 giờ làm việc/ngày** cho cả team.<br>Tỷ lệ phân loại sai 15% → chuyển sai phòng ban → khiếu nại khách hàng tăng.<br>Chi phí tuyển dụng thêm nhân viên: ~600 triệu/tháng. |
| **5. Success Metric** | 1. Giảm thời gian xử lý: **22 phút → dưới 5 phút/yêu cầu**<br>2. Giảm tỷ lệ phân loại sai: **15% → dưới 3%**<br>3. Tăng throughput: **200 → 600+ email/ngày**<br>4. Tăng NPS CSKH: **45 → 60+ điểm** |
| **6. Operational Boundary** | **ĐƯỢC PHÉP:** Đọc nội dung email/cuộc gọi, tra cứu mã VIN trong DMS, phân loại 8 category, soạn phản hồi draft.<br><br>**CẤM:**<br>- Tự động gửi phản hồi cho khách hàng mà không có CSKH phê duyệt (HITL bắt buộc)<br>- Đưa ra chẩn đoán kỹ thuật cuối cùng (chỉ phân loại)<br>- Truy cập thông tin cá nhân nhạy cảm (tài khoản ngân hàng, CMND) |

---

## 2. Future-State Flow & AI Fit

### AI Fit Matrix

| Tiêu chí | Rule/State-Machine | LLM Feature | Agentic Loop |
|-----------|-------------------|-------------|--------------|
| Độ phức tạp | Thấp | Trung bình | Cao |
| Xử lý ngôn ngữ tự nhiên | ❌ Không tốt | ✅ Rất tốt | ✅ Rất tốt |
| Rủi ro khi sai | Thấp | Trung bình | Cao |
| Chi phí vận hành | Thấp | Trung bình | Cao |
| **Phù hợp?** | ❌ Không đủ | ✅ **CHỌN** | ❌ Thừa |

**Quyết định:** **LLM Feature** — Quy trình có cấu trúc cố định, rủi ro khi phân loại sai có thể xử lý qua HITL.

### Future-State Flow (Quy trình tương lai)

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│                         QUY TRÌNH TƯƠNG LAI (FUTURE STATE)                  │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌──────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────────┐ │
│  │ Bước 1  │    │ Bước 2       │    │ Bước 3       │    │ Bước 4       │ │
│  │ Nhận    │───▶│ 🔵 AI Auto   │───▶│ 🔵 AI Draft  │───▶│ 🟢 CSKH      │ │
│  │ email/   │    │ Tra cứu DMS  │    │ Phân loại + │    │ Review &     │ │
│  │ cuộc gọi│    │ + Phân loại  │    │ Soạn phản   │    │ Phê duyệt   │ │
│  │ (giữ    │    │ tự động      │    │ hồi sơ bộ   │    │ (HITL)       │ │
│  │  nguyên)│    │              │    │              │    │              │ │
│  └──────────┘    └──────────────┘    └──────────────┘    └──────────────┘ │
│                                                             │              │
│                                                             ▼              │
│                                                     ┌──────────────┐      │
│                                                     │ Bước 5       │      │
│                                                     │ Gửi phản hồi │      │
│                                                     │ cho khách    │      │
│                                                     └──────────────┘      │
│                                                                             │
│  🔵 = AI Step (LLM Feature)                                                 │
│  🟢 = Human Step (HITL - Human-in-the-loop)                                 │
│                                                                             │
│  ↩️ FALLBACK: Nếu AI draft không rõ ràng/confidence thấp → CSKH xử lý     │
│     thủ công như quy trình cũ.                                              │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

### So sánh Before vs After

| Tiêu chí | Before (Thủ công) | After (AI-assisted) |
|----------|-------------------|---------------------|
| Thời gian xử lý | 22 phút/yêu cầu | 5 phút/yêu cầu |
| Tỷ lệ phân loại sai | 15% | < 3% |
| Throughput/ngày | 200 email | 600+ email |
| Số bước trong flow | 5 bước | 5 bước (giữ nguyên) |

---

## 3. Evaluate (Phase 5)

### AI Readiness Checklist

| # | Tiêu chí | Status | Ghi chú |
|---|----------|--------|---------|
| 1 | Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? | ✅ Có | 200+ email/ngày, có transcript cuộc gọi |
| 2 | Rủi ro khi AI sai có nằm trong tầm kiểm soát? | ✅ Có | HITL (CSKH review) + Fallback |
| 3 | Stakeholders sẵn sàng thay đổi quy trình làm việc cũ? | ✅ Có | Team Lead và CSKH đều mong muốn giảm tải |
| 4 | Đã xác định rõ Operational Boundary? | ✅ Có | 3 rules cấm, HITL bắt buộc |
| 5 | Có đủ nguồn lực để maintain AI system? | ⚠️ Cần xác nhận | Cần 1 AI Engineer part-time |

### Quyết định cuối cùng

## ✅ **GO**

### Luận điểm kỹ thuật:

| Tiêu chí | Phân tích | Kết luận |
|----------|----------|----------|
| **Bài toán cụ thể** | Pain point ảnh hưởng 40 nhân viên, 200+ khách hàng/ngày | ✅ Rõ ràng |
| **Metric đo lường** | 4 metric cụ thể: thời gian, tỷ lệ sai, throughput, NPS | ✅ Đo được |
| **Độ phức tạp** | LLM Feature — prompt engineering, không cần fine-tuning | ✅ Vừa phải |
| **Rủi ro** | HITL bắt buộc + Fallback — rủi ro khi AI sai được kiểm soát | ✅ Thấp |
| **Data readiness** | Có logs email và transcript cuộc gọi sẵn có | ✅ Sẵn sàng |

### Ước lượng chi phí:

| Hạng mục | Chi phí ước tính |
|----------|-----------------|
| Gemini 2.5 Flash API | ~$300/tháng (200 req × 500 tokens × $0.003) |
| AI Engineer (1 part-time) | ~$500/tháng |
| **Tổng chi phí/tháng** | **~$800/tháng** |
| Tiết kiệm (không tuyển thêm 2 nhân viên) | ~$1,200/tháng |
| **ROI** | **Dương ngay tháng đầu tiên** |

### Kế hoạch triển khai:

| Tuần | Mục tiêu |
|------|----------|
| Tuần 1-2 | Thu thập 1000 mẫu email, thiết kế prompt chain |
| Tuần 3-4 | Implement Gemini API, A/B testing |
| Tuần 5 | Pilot với 5 nhân viên CSKH |
| Tuần 6 | Full rollout nếu pilot đạt ≥80% metric target |

---

## 📋 Tài liệu đính kèm

- Sơ đồ workflow hiện tại: `04-workflow-diagram.png` (vẽ tay, chụp ảnh)
- Prompt prototype: `starter-code/prompt_prototype.py`

---

*Báo cáo Deep-Dive — Lab 02: AI Product Scoping — Vin Smart Future — Ngày 24/07/2026*