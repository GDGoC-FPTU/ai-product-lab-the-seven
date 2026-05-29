# 01 — Problem Scan & Quick Cards (Cá nhân)

---

## 🔍 Phase 1 — SCAN: Bảng quét cơ hội AI tại Vin Smart Future

Sử dụng **4 Lenses** để quét qua hoạt động vận hành của các công ty thành viên Vingroup:

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | Vinmec | Tốn thời gian | Nhân viên lễ tân đăng ký khám & nhập thông tin bệnh nhân thủ công vào hệ thống HIS (8–12 phút/bệnh nhân) |
| 2 | VinUni | Lặp lại | Giảng viên chấm bài thi tự luận & bài luận sinh viên lặp đi lặp lại mỗi kỳ (20–30 phút/bài) |
| 3 | Vinhomes | Pain từ người khác | Nhân viên Call Center tiếp nhận & phân loại khiếu nại cư dân, cư dân phàn nàn chờ phản hồi 1–3 ngày |
| 4 | Vinhomes | Tốn thời gian | Chuyên viên BQL xử lý quy trình xin phép thi công nội thất cư dân, kiểm tra hồ sơ thủ công (2–3 ngày/đơn) |
| 5 | Vinmec | AI-upgrade | Nhân viên phòng xét nghiệm tổng hợp & diễn giải kết quả xét nghiệm cho bệnh nhân (15–20 phút/kết quả), phản hồi còn rập khuôn |
| 6 | Vinhomes | AI-upgrade | Cư dân không có kênh tự tra cứu dịch vụ/thủ tục 24/7; phải gọi hotline BQL (chỉ giờ hành chính), 60–70% là câu hỏi lặp lại |

---

## 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

### Quick Problem Card #1 — Vinmec: Đăng ký khám & nhập thông tin bệnh nhân

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Nhân viên lễ tân Vinmec nhập thủ công    │
│ thông tin bệnh nhân mới vào hệ thống HIS khi đăng ký khám, │
│ gây ùn tắc hàng chờ vào giờ cao điểm.                      │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│ - Nhân viên lễ tân (quá tải 80–100 bệnh nhân/ca sáng)      │
│ - Bệnh nhân (chờ đợi 15–20 phút chỉ để đăng ký)           │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Bệnh nhân đưa CCCD / thẻ BHYT tại quầy               │
│   → 2. Lễ tân nhập tay: họ tên, ngày sinh, địa chỉ, SĐT   │
│   → 3. Lễ tân hỏi triệu chứng & lý do khám                │
│   → 4. Tra cứu lịch bác sĩ, chọn khoa phù hợp            │
│   → 5. In phiếu khám & hướng dẫn bệnh nhân đến phòng chờ  │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                           │
│ Bước 2–3 (⏱ 6–8 phút/bệnh nhân). Lễ tân hay nhập sai      │
│ chính tả tên, nhầm mã BHYT khi gõ nhanh.                   │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                      │
│ - Bước 2: OCR quét CCCD/BHYT → auto-fill thông tin         │
│ - Bước 3–4: NLP phân loại triệu chứng → gợi ý khoa/bác sĩ │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ "Giảm thời gian đăng ký từ 10 phút xuống dưới 3 phút/     │
│ bệnh nhân. Giảm tỉ lệ nhập sai thông tin từ 8% xuống <1%."│
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
│ (OCR + NLP phân loại triệu chứng, lễ tân chỉ xác nhận)    │
└─────────────────────────────────────────────────────────────┘
```

---

### Quick Problem Card #3 — Vinhomes: Tiếp nhận & phân loại khiếu nại cư dân

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Nhân viên Call Center Vinhomes tiếp nhận  │
│ khiếu nại cư dân qua App/hotline, phân loại thủ công và    │
│ chuyển đúng bộ phận xử lý, khiến cư dân chờ 1–3 ngày.     │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│ - Nhân viên CSKH (xử lý 200+ ticket/ngày, dễ phân loại    │
│   nhầm khi quá tải)                                        │
│ - Cư dân (phàn nàn chờ phản hồi quá lâu, đánh giá 1-star) │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Cư dân gửi khiếu nại qua App Vinhomes / gọi hotline  │
│   → 2. Nhân viên đọc/nghe nội dung, xác định loại vấn đề  │
│   → 3. Phân loại thủ công (điện/nước/thang máy/an ninh/    │
│        tiếng ồn/rác thải/bãi xe...)                        │
│   → 4. Chuyển ticket đến đúng ban quản lý tòa nhà          │
│   → 5. Soạn tin nhắn xác nhận đã tiếp nhận gửi cư dân     │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                           │
│ Bước 2–4 (⏱ 8–15 phút/khiếu nại). Nhân viên hay phân loại │
│ nhầm khi nội dung mô tả mơ hồ.                             │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                      │
│ - Bước 2–3: NLP đọc nội dung → auto phân loại danh mục     │
│ - Bước 4: Auto-route đến đúng bộ phận dựa trên phân loại  │
│ - Bước 5: LLM draft tin nhắn phản hồi cá nhân hóa         │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ "Giảm thời gian phân loại & route từ 15 phút xuống dưới   │
│ 30 giây. Giảm thời gian phản hồi cư dân từ 1–3 ngày xuống│
│ dưới 2 giờ. Tỉ lệ phân loại đúng đạt ≥ 95%."             │
│                                                             │
│ Quick Architecture: [x] LLM Feature                         │
│ (NLP phân loại + auto-route + draft response, HITL duyệt)  │
└─────────────────────────────────────────────────────────────┘
```

---

### Quick Problem Card #6 — Vinhomes: Trợ lý ảo cư dân (Resident Concierge) ⭐ *Chọn cho Deep-Dive*

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #6                                       │
│                                                             │
│ Bài toán (1 câu): Cư dân Vinhomes không có kênh tự tra     │
│ cứu thông tin dịch vụ/thủ tục 24/7; phải gọi hotline BQL  │
│ (chỉ giờ hành chính, hay quá tải) cho cả những câu hỏi    │
│ lặp lại đơn giản.                                          │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau (Actor)?                                        │
│ - Cư dân (chờ lâu, ngoài giờ HC không ai hỗ trợ)          │
│ - Nhân viên tổng đài BQL (60–70% cuộc gọi là FAQ lặp lại) │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cư dân có thắc mắc (giờ hồ bơi? phí? thủ tục?)       │
│   → 2. Tự tìm trong app, menu rối → không thấy            │
│   → 3. Gọi hotline BQL / nhắn tin, chờ tổng đài           │
│   → 4. Được trả lời hoặc bị chuyển lòng vòng nhiều bộ phận│
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                           │
│ Bước 3–4 (⏱ chờ vài phút đến vài giờ; ngoài giờ HC = 0).  │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                      │
│ - Bước 1–2: Chatbot RAG trả lời ngay từ kho tài liệu      │
│   chính thức (FAQ, phí, thủ tục, giờ tiện ích)            │
│ - Bước 3–4: Việc phức tạp → AI phân loại + escalate đúng  │
│   ban quản lý khu vực kèm tóm tắt                         │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│ "Tự động giải quyết ≥ 60% câu hỏi không cần người. Giảm   │
│ thời gian chờ phản hồi từ vài giờ xuống dưới 30 giây.    │
│ Escalate đúng bộ phận ≥ 90%."                             │
│                                                             │
│ Quick Architecture: [x] LLM Feature (+ RAG)                 │
│ (Chatbot RAG tra kho tài liệu chính thức + escalate, có   │
│ HITL cho việc nhạy cảm)                                    │
└─────────────────────────────────────────────────────────────┘
```

> **Ghi chú:** Nhóm chọn **Card #6 — Vinhomes Resident Concierge** làm bài toán Deep-Dive ở Phase 3 (xem `02-deep-dive-report.md`).
