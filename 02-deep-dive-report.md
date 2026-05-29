# 02 — Deep-Dive Report: Vinhomes Resident Concierge (Phase 3 & 5)

---

## 👥 Thông tin nhóm

| # | Họ và tên | MSSV |
|---|-----------|------|
| 1 | *Nguyễn Văn Dưỡng* | *2A202600967* |
| 2 | *Phùng Hữu Uy* | *2A202600886* |
| 3 | *Chu Hồng Minh* | *2A202600845* |

**Tên nhóm:** The Seven

---

## 🗳️ Quyết định lựa chọn bài toán

Nhóm quyết định chọn bài toán **"Vinhomes Resident Concierge — Trợ lý ảo cư dân Vinhomes"** để thực hiện Deep-Dive.

### Lý do lựa chọn:
* Bài toán có actor rõ ràng (cư dân + nhân viên BQL), workflow dễ vẽ, metric đo được.
* 60–70% cuộc gọi đến tổng đài BQL là FAQ lặp lại → AI giải quyết ngay, giải phóng nhân lực.
* Ranh giới an toàn phong phú (cấm bịa, cấm cam kết, bắt buộc escalate khi nhạy cảm) → phù hợp để stress-test prompt.
* Công nghệ LLM + RAG đơn giản, không cần Agent phức tạp, phù hợp scope hẹp.

### Lý do loại bỏ các thẻ khác:
* **Card #1 (Vinmec lễ tân):** Phần lớn giá trị đến từ OCR (computer vision) hơn là LLM. Bài toán thiên về nhận dạng ảnh, không phải xử lý ngôn ngữ tự nhiên thuần túy.
* **Card #3 (Vinhomes khiếu nại):** Tốt nhưng actor là nhân viên nội bộ, ít "wow" hơn. Bài toán Resident Concierge bao trùm cả phân loại khiếu nại (như một sub-function của escalate) nên chọn cái rộng hơn.

---

## 🏗️ Phase 3 — DEEP-DIVE

### 3.1. Current-State Workflow Mapping

Quy trình hiện tại khi cư dân Vinhomes cần hỗ trợ thông tin/dịch vụ:

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │     │ Bước 5       │
│ Cư dân phát  │     │ Tìm kiếm    │     │ Gọi hotline  │     │ Tổng đài     │     │ Được trả lời │
│ sinh thắc mắc│ ──→ │ trên App     │ ──→ │ BQL khu vực  │ ──→ │ tiếp nhận &  │ ──→ │ hoặc chuyển  │
│              │     │ (không thấy) │     │              │     │ tra cứu      │     │ lòng vòng    │
│              │     │              │     │              │     │              │     │              │
│ Ai: Cư dân  │     │ Ai: Cư dân  │     │ Ai: Cư dân  │     │ Ai: NV Tổng  │     │ Ai: NV BQL   │
│ ⏱ 1 phút     │     │ ⏱ 3–5 phút   │     │ ⏱ 5–15 phút  │     │ đài BQL      │     │ tòa nhà      │
│ In: Nhu cầu  │     │ In: App menu │     │ In: Số hotline│     │ ⏱ 5–10 phút  │     │ ⏱ 0–30 phút  │
│ Out: Câu hỏi │     │ Out: Không   │     │ Out: Kết nối │     │ In: Câu hỏi  │     │ In: Ticket   │
│              │     │ tìm thấy    │     │ tổng đài     │     │ Out: Trả lời │     │ Out: Giải    │
│              │     │              │     │ 🔴 Bottleneck │     │ hoặc chuyển  │     │ quyết/không  │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘

🔴 Bottleneck: Bước 3 — Cư dân phải chờ hotline (giờ cao điểm chờ 10–15 phút; ngoài giờ HC = KHÔNG có ai trả lời)
🔄 Handoff: Bước 4→5 — Tổng đài chuyển sang BQL tòa nhà, cư dân phải giải thích lại từ đầu

⏱ Tổng thời gian xử lý trung bình: 15–60 phút/câu hỏi (nếu trong giờ HC)
⏱ Ngoài giờ hành chính: KHÔNG THỂ giải quyết → chờ đến ngày hôm sau
```

### Các vấn đề chính của quy trình hiện tại:
1. **Không có self-service 24/7:** App Vinhomes Resident có menu tĩnh nhưng không có tìm kiếm thông minh, cư dân không tìm được thông tin cần.
2. **Hotline quá tải:** 60–70% cuộc gọi là FAQ lặp lại (giờ hồ bơi, phí quản lý, cách đăng ký thẻ xe...) nhưng vẫn cần người trả lời.
3. **Handoff mất thông tin:** Khi chuyển từ tổng đài → BQL tòa, cư dân phải kể lại vấn đề, gây bực bội.
4. **Ngoài giờ = bế tắc:** Cư dân gặp vấn đề buổi tối/cuối tuần không có ai hỗ trợ.

---

### 3.2. Problem Statement (6-field) — Vin Smart Future Standard

| Field | Nội dung |
|-------|----------|
| **1. Actor / Operator** | **Primary:** Cư dân Vinhomes (sử dụng App Vinhomes Resident). **Secondary:** Nhân viên tổng đài BQL khu vực (nhận escalation từ AI). |
| **2. Current Workflow** | Cư dân có thắc mắc → tìm trên App (menu tĩnh, không thấy) → gọi hotline BQL (chỉ giờ HC, hay quá tải) → tổng đài tra cứu thủ công → trả lời hoặc chuyển BQL tòa nhà. 5 bước, phụ thuộc hoàn toàn vào con người, mất 15–60 phút. |
| **3. Bottleneck** | Bước 3–4: Chờ hotline (10–15 phút giờ cao điểm, 0% ngoài giờ HC). 60–70% câu hỏi là FAQ lặp lại nhưng vẫn cần người trả lời vì không có kênh tự động. |
| **4. Business Impact** | Khu đô thị 5,000 hộ → ước tính 150–200 cuộc gọi/ngày. 70% là FAQ = ~130 cuộc/ngày lãng phí nhân lực tổng đài. Chi phí: ~3 nhân viên full-time chỉ để trả lời FAQ. Ngoài giờ HC: cư dân không được hỗ trợ → CSAT giảm, đánh giá 1-star trên App. |
| **5. Success Metric** | 1. Tự động giải quyết ≥ 60% câu hỏi cư dân không cần người (Automation Rate). 2. Giảm thời gian chờ phản hồi từ 15–60 phút → dưới 30 giây (Response Time). 3. Escalate đúng bộ phận ≥ 90% (Routing Accuracy). 4. Hoạt động 24/7 (Availability). |
| **6. Operational Boundary** | AI **ĐƯỢC PHÉP:** Tra cứu kho tài liệu chính thức (FAQ, bảng phí, quy định, thủ tục) và trả lời; hướng dẫn cư dân từng bước; phân loại + tóm tắt + escalate đến đúng ban quản lý. **TUYỆT ĐỐI CẤM:** Bịa thông tin không có trong tài liệu (anti-hallucination); cam kết thời gian sửa chữa/bồi thường/miễn phí; tư vấn pháp lý/tranh chấp căn hộ; xử lý giao dịch tài chính (thanh toán, hoàn tiền). **HITL BẮT BUỘC:** Sự cố khẩn cấp (cháy, rò gas, kẹt thang máy) → hiện hotline khẩn + chuyển người ngay; tranh chấp/khiếu nại phức tạp → escalate kèm tóm tắt. |

---

### 3.3. Future-State Flow & AI Fit

#### AI Fit Matrix — Quyết định:

| Giải pháp | Phù hợp? | Lý do |
|-----------|:---------:|-------|
| Rule / State-Machine | ❌ | Câu hỏi cư dân đa dạng, ngôn ngữ tự nhiên, không thể cover bằng if-else |
| **LLM Feature + RAG** | ✅ | Hiểu ngôn ngữ tự nhiên + tra cứu kho tài liệu chính thức → trả lời chính xác, chống hallucination |
| Agentic Loop | ❌ | Không cần tự trị — quy trình có cấu trúc rõ (trả lời hoặc escalate), rủi ro khi để Agent tự quyết quá cao trong môi trường cư dân |

**Quyết định:** Chọn **LLM Feature + RAG (Retrieval-Augmented Generation)**.

#### Future-State Flow:

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────────────────────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3                               │
│ Cư dân nhắn  │     │ 🔵 AI hiểu   │     │                                      │
│ tin trong App │ ──→ │ ý định +     │ ──→ │  ┌─ Trả lời được (FAQ/thủ tục)?      │
│ Resident     │     │ RAG tra kho  │     │  │                                    │
│              │     │ tài liệu     │     │  │  CÓ → 🔵 AI trả lời + trích nguồn │
│              │     │              │     │  │         → Giải quyết xong ✅        │
│              │     │              │     │  │                                    │
│              │     │              │     │  └─ KHÔNG (phức tạp/nhạy cảm/khẩn)?  │
│              │     │              │     │           ↓                           │
│              │     │              │     │     🟢 AI phân loại + tóm tắt        │
│              │     │              │     │         + escalate đúng BQL           │
│              │     │              │     │           ↓                           │
│              │     │              │     │     ↩️ Fallback: "Tôi chuyển bạn     │
│              │     │              │     │     đến BQL để hỗ trợ chính xác hơn" │
└──────────────┘     └──────────────┘     └──────────────────────────────────────┘

Ký hiệu:
🔵 AI Step — Tác vụ LLM + RAG xử lý tự động
🟢 Human Step (HITL) — Nhân viên BQL tiếp nhận escalation
↩️ Fallback — Khi AI không chắc chắn hoặc vấn đề nhạy cảm
```

#### Điểm khác biệt so với quy trình cũ:

| Tiêu chí | Hiện tại (Manual) | Tương lai (AI + RAG) |
|----------|:-----------------:|:--------------------:|
| Thời gian phản hồi | 15–60 phút | < 30 giây |
| Hoạt động | Giờ hành chính | 24/7 |
| FAQ lặp lại | Người trả lời | AI tự động (60–70%) |
| Handoff mất thông tin | Có | Không (AI tóm tắt kèm ticket) |
| Nhân lực tổng đài | 3 FTE cho FAQ | Giải phóng → xử lý việc phức tạp |

---

## 🏁 Phase 5 — EVALUATE

### AI Readiness Checklist:

| # | Câu hỏi | Trả lời |
|---|---------|---------|
| 1 | Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test? | ✅ **CÓ** — Vinhomes có sẵn: bảng phí quản lý, quy định nội khu, danh sách tiện ích + giờ hoạt động, quy trình thủ tục (thi công, đăng ký xe, chuyển nhượng). Đây là tài liệu công khai phát cho cư dân, dễ số hóa thành kho RAG. |
| 2 | Rủi ro khi AI sai có nằm trong tầm kiểm soát? | ✅ **CÓ** — AI sai = trả lời sai thông tin phí/giờ → cư dân bực nhưng không nguy hiểm tính mạng. Có Fallback: khi không chắc → escalate người. Không liên quan y tế/an toàn/tài chính. |
| 3 | Stakeholders sẵn sàng thay đổi quy trình? | ✅ **CÓ** — BQL Vinhomes đang chịu áp lực giảm chi phí vận hành + tăng CSAT. Cư dân muốn self-service nhanh hơn (khảo sát nội bộ cho thấy 78% cư dân muốn tra cứu tự động thay vì gọi điện). |

### Quyết định cuối cùng:

## ✅ **GO — Bắt đầu xây dựng Prototype với scope hẹp**

### Justification (Lý giải quyết định):

**Luận điểm kỹ thuật:**
1. **Data sẵn sàng:** Kho tài liệu dịch vụ Vinhomes đã tồn tại dưới dạng PDF/Word (bảng phí, quy định, FAQ). Chỉ cần vector hóa và đưa vào RAG pipeline — không cần thu thập data mới.
2. **Công nghệ đã chín:** LLM + RAG là pattern đã được chứng minh (ChatGPT Enterprise, Intercom Fin, Zendesk AI). Không phải R&D mới.
3. **Rủi ro thấp:** Sai = trả lời sai thông tin → cư dân hỏi lại hoặc gọi hotline. Không ảnh hưởng an toàn/tài chính. Có Fallback rõ ràng.
4. **HITL đảm bảo:** Mọi việc nhạy cảm đều escalate người, AI không tự quyết.

**Ước lượng chi phí (scope hẹp — 1 khu đô thị pilot):**

| Hạng mục | Chi phí ước tính |
|----------|-----------------|
| Vector DB + RAG pipeline (Pinecone/Weaviate) | ~$50–100/tháng |
| LLM API (Gemini/GPT-4o, ~5000 queries/ngày) | ~$200–400/tháng |
| Phát triển chatbot integration (2 engineers × 4 tuần) | ~$8,000–12,000 one-time |
| Số hóa tài liệu + QA kho dữ liệu | ~$2,000 one-time |
| **Tổng chi phí pilot 3 tháng** | **~$12,000–16,000** |

**So sánh với chi phí hiện tại:**
- 3 nhân viên tổng đài full-time × $500/tháng = $1,500/tháng = $18,000/năm
- AI giải phóng 60–70% workload → tiết kiệm ~$12,000/năm chỉ riêng nhân lực
- **ROI dương sau 12–16 tháng**, chưa tính giá trị CSAT tăng + hoạt động 24/7

**Scope pilot đề xuất:**
- 1 khu đô thị Vinhomes (ví dụ: Vinhomes Ocean Park)
- Chỉ FAQ + hướng dẫn thủ tục + escalate
- Chạy song song với hotline hiện tại (không thay thế ngay)
- Đánh giá sau 3 tháng trước khi mở rộng
