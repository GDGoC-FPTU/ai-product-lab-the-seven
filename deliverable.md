## 🏛️ Bối cảnh: Tôi là ai?

Tôi là Uy, AI Engineer tại Vin Smart Future. Nhóm chúng tôi được giao nhiệm vụ phối hợp với Khối Vận Hành của Vinhomes để tìm kiếm các cơ hội tối ưu hóa bằng trí tuệ nhân tạo.

Thông qua khảo sát thực địa tại hệ thống vận hành và chăm sóc cư dân, tôi nhận thấy đội ngũ vận hành và điều phối đang chịu áp lực rất lớn trong các khung giờ cao điểm. Điều này dẫn đến việc giảm hiệu suất xử lý yêu cầu, gia tăng độ trễ phản hồi và ảnh hưởng trực tiếp đến trải nghiệm cư dân.

Bài toán tôi mang đến buổi Lab hôm nay xuất phát từ chính quan sát thực tế này trong quá trình vận hành tại Vinhomes.
---

# 🔍 Phase 1 — SCAN: Tìm kiếm cơ hội (Cá nhân)

Dùng **4 Lenses** quét qua vận hành của các công ty thành viên Vingroup.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Vin Bus** | Pain từ người khác | Khách hàng đợi lâu , chuyến xe đông người trong khi chuyến xe gần tương tự ít người. |
| 2 | **Xanh SM** | Pain từ người khác | Khách hàng đợi lâu , chuyến xa không hài lòng , khác hàng ngại đánh giá và lẳng nặng không dùng cho lần sau nữa. |
| 3 | **VinUni** | Pain từ người khác | Hệ thống mạng của trường , cũng như các dịch vụ khác. |
| 4 | **Vinhomes** | Lặp lại | Chưa có hệ thống hướng dẫn sử dụng các dịch vụ của VinHomes. |
| 5 | **Vinmec** | Pain từ người khác | Bác sĩ mất quá nhiều thời gian viết tóm tắt hồ sơ xuất viện (mất 20-30 phút/bệnh nhân, bác sĩ phàn nàn vì quá tải). |
| 6 | **Xanh SM** | Tốn thời gian | Tóm tắt lý do khách hàng hủy chuyến từ cuộc gọi ghi âm và ghi chú của tài xế để tìm pattern lỗi hệ thống. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)

Chọn top 3 từ danh sách SCAN: **#4 (Vinhomes hỗ trợ khách hàng), #2 (Xanh SM trải nghiệm không tốt), #6 (Xanh SM Hủy chuyến).**

## Thẻ bài toán tiêu biểu: Card #4 — Vinhomes hỗ trợ cư dân và khách hàng.

```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Cơ dân hoặc khoác hàng gặp một sự cố và mmuốn được giải quyết trong thời gian ngắn nhất .                       │
│                                                             │
│ Ai đang đau? Cơ dân , khách hàng chờ đợi, BQL quá tải hoặc không nắm bắt được thông tin     │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Cư dân gặp vấn đề và cần giải quyết nhanh nhất              │
│   → 2. Cư dân cần tìm người có thẩm quyền và hiểu biết để hỏi│
│   → 3. Tra cứu thông trên app thông tin và các dịch vụ liên quan   │
│   → 4. Viết tin nhắn hoặc gọi và đợi phản hồi    │
│   → 5. Liên hệ người hộ trợ         │
│                                                             │
│ Bước nào tốn nhất? Bước 2-3 (⏱ 12 phút/lượt)                │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3              │
│ (Tra cứu thông tin tìm đúng người liên hệ ) │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút.      │
│                                                             │
│ Quick Architecture: [x] LLM Feature (Tự động soạn chỉ dẫn)   │
└─────────────────────────────────────────────────────────────┘
```

---

# 🗳️ Quyết định lựa chọn của nhóm:
Nhóm quyết định chọn bài toán **"Card #4 — Vinhomes hỗ trợ cư dân và khách hàng"** để thực hiện Deep-Dive.

## Lý do lựa chọn và loại bỏ các thẻ khác:
Card #2 — Xanh SM (Trải nghiệm khách hàng không tốt):
Vấn đề chủ yếu mang tính trải nghiệm cảm xúc (sentiment-driven) hơn là một điểm nghẽn vận hành cụ thể. Dữ liệu phản hồi khách hàng hiện tại thường chưa được chuẩn hóa, phân tán nhiều nguồn và khó truy xuất nguyên nhân gốc (root-cause). Do đó, giải pháp AI trong trường hợp này chủ yếu dừng ở mức phân tích hậu kỳ (analytics / NLP sentiment mining) thay vì can thiệp trực tiếp vào vận hành real-time. Vì vậy, ROI ngắn hạn trong vận hành thấp hơn so với các bài toán tối ưu hóa luồng CSKH tại Vinhomes.
Card #6 — Xanh SM (Phân tích lý do hủy chuyến):
Đây là bài toán thuộc nhóm offline analytics / post-event analysis, xử lý sau khi chuyến xe đã bị hủy nên không tác động trực tiếp đến quyết định điều phối tại thời điểm thực (dispatch moment). Giá trị chính của bài toán nằm ở việc tổng hợp dữ liệu, tìm pattern và cải thiện hệ thống trong dài hạn. Tuy nhiên, do không giải quyết trực tiếp các “pain point” thời gian thực trong vận hành, nên mức độ ưu tiên thấp hơn so với các bài toán can thiệp trực tiếp vào luồng vận hành.
---


