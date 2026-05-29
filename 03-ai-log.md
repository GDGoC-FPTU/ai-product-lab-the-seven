# 03 — AI Log & Reflection (Cá nhân)

> Nhật ký chiêm nghiệm về việc tương tác với AI trong suốt buổi Lab 02: AI Product Scoping.
> 
> **Ghi chú:** Cả nhóm (3 thành viên) cùng thảo luận với AI trong quá trình làm bài. Mỗi người phụ trách research 2 mảng kinh doanh khác nhau, sau đó tổng hợp ý tưởng và cùng dùng AI để đánh giá, phản biện, và chọn lọc.

---

## 🤖 AI giúp gì trong buổi lab?

### 1. Brainstorm ý tưởng bài toán (Phase 1 — cả nhóm)
Nhóm chia nhau research: Thành viên 1 phụ trách VinFast + Xanh SM, Thành viên 2 phụ trách Vinhomes + Vinpearl, Thành viên 3 phụ trách Vinmec + VinUni. Mỗi người sử dụng AI để brainstorm, research các pain point vận hành tại mảng được giao. Cụ thể, chúng tôi yêu cầu AI gợi ý các quy trình thủ công tốn thời gian. AI giúp mở rộng góc nhìn sang nhiều mảng mà bản thân chưa có kinh nghiệm trực tiếp. Tổng cộng nhóm thu được ~15 ý tưởng từ 6 công ty thành viên.

### 2. Đánh giá và sàng lọc ý tưởng (Phase 1→2 — cả nhóm)
Sau khi cả nhóm tổng hợp được ~15 ý tưởng, chúng tôi cùng nhờ AI chấm điểm theo 5 trục (Business Need, LLM Fit, Feasibility, Metric, Boundary richness) để so sánh khách quan. AI giúp loại nhanh các ý tưởng không phù hợp LLM (bài toán vật lý, optimization thuần) và highlight 3 ý tưởng mạnh nhất. Quá trình này giúp cả nhóm đồng thuận nhanh thay vì tranh luận chủ quan.

### 3. Phản biện logic sản phẩm (Phase 2→3 — thảo luận nhóm)
Khi nhóm đề xuất ý tưởng "chẩn đoán lỗi xe VinFast qua app cho khách hàng", một thành viên đặt câu hỏi: "Tại sao khách phải dùng app thay vì mang ra garage?" Chúng tôi đưa câu hỏi này cho AI phân tích, và AI giúp nhận ra rằng cùng một công nghệ, đặt sai actor (khách hàng end-user) thì vô lý, đặt đúng actor (Service Advisor nội bộ) thì có giá trị. Cả nhóm cùng rút ra bài học quan trọng nhất về tư duy "Problem First, AI Second".
Chúng tôi cũng thảo luận về việc dùng AI vào hỗ trợ chuẩn đoán bệnh, tóm tắt hồ sơ bệnh án cho bệnh nhân, nhưng vì rủi ro có thể xảy ra sẽ gây thiệt hại lớn đến bệnh nhân và bệnh viện nên chúng tôi quyết định không chọn nó.

### 4. Thu hẹp scope (Phase 3 — quyết định nhóm)
Khi nhóm muốn làm "trợ lý toàn hệ sinh thái Vin", nhưng scope quá rộng và chúng tôi quyết định thu hẹp về "Vinhomes Resident Concierge" — giới hạn trong 1 khu đô thị, chỉ 3 chức năng (tư vấn FAQ, hướng dẫn thủ tục, escalate). Cả nhóm thảo luận và đồng ý rằng scope hẹp mới khả thi, và quyết định chốt ý tưởng này cho Deep-Dive.

---

## ❌ AI sai gì?

### 1. Đề xuất bài toán vật lý không phù hợp LLM
Ban đầu khi brainstorm, AI gợi ý một số bài toán thiên về hardware/vật lý mà LLM không giải quyết được, ví dụ: "Hệ thống AI phát hiện hành vi phá hoại trạm sạc" (cần computer vision, không phải NLP), "Tối ưu điều phối xe real-time" (bài toán optimization, dùng thuật toán tốt hơn LLM). Nếu không sàng lọc kỹ, nhóm có thể chọn nhầm bài toán không khả thi cho Phase 4 (prompt prototype).

### 2. Không tự nhận ra vấn đề actor
Khi tôi hỏi AI về ý tưởng VinFast chẩn đoán lỗi xe, ban đầu AI trình bày nó như một ý tưởng tốt mà không tự phát hiện vấn đề actor. Chỉ khi tôi chủ động hỏi "tại sao khách phải dùng app thay vì ra garage?" thì AI mới phân tích sâu và thừa nhận điểm yếu. Điều này cho thấy AI không tự phản biện — cần người đặt câu hỏi đúng.

### 3. Xu hướng "nói có" với mọi ý tưởng
AI có xu hướng khen mọi ý tưởng đều "hay" và "khả thi" trước khi được ép phải so sánh trực tiếp. Nếu hỏi từng ý tưởng riêng lẻ, AI sẽ nói cái nào cũng tốt. Chỉ khi bắt AI chấm điểm song song trên cùng bảng tiêu chí thì mới thấy rõ sự khác biệt.

---

## 🔧 Sửa đổi ra sao?

### 1. Ép AI chấm điểm định lượng thay vì nhận xét định tính
Thay vì hỏi "ý tưởng này có tốt không?", tôi chuyển sang: "Chấm điểm 14 ý tưởng này theo 5 trục, thang 1-5, lập bảng so sánh." Kết quả khách quan hơn nhiều — thấy rõ ý tưởng nào thực sự mạnh.

### 2. Đặt câu hỏi phản biện chủ động
Tôi học được rằng phải chủ động hỏi AI kiểu "devil's advocate": "Tại sao ý tưởng này KHÔNG nên làm?", "Actor có thực sự dùng sản phẩm này không?", "Rule-based có giải quyết được không?" Khi bị ép phản biện, AI cho ra phân tích sắc bén hơn nhiều so với khi được hỏi thuận chiều.

### 3. Giới hạn scope ngay từ đầu trong prompt
Khi brainstorm, tôi thêm constraint vào prompt: "Chỉ gợi ý bài toán mà LLM/NLP giải quyết được, loại bỏ bài toán cần computer vision, robotics, hoặc optimization thuần." Điều này giúp AI không đề xuất các bài toán ngoài khả năng của prompt prototype.

---

## 💡 Bài học rút ra

1. **AI là thought-partner, không phải decision-maker.** AI brainstorm tốt nhưng không tự phản biện — cần người đặt câu hỏi đúng.
2. **Problem First, AI Second.** Cùng một công nghệ, đặt sai actor/context thì sản phẩm vô dụng. Luôn hỏi: "Ai dùng? Họ có thực sự cần không? Họ có thể tự làm bằng cách khác đơn giản hơn không?"
3. **Scope kills projects.** Ý tưởng "toàn hệ sinh thái" nghe hay nhưng không khả thi. Thu hẹp về 1 actor + 1 context + 3 chức năng cụ thể mới triển khai được.
4. **Ép AI so sánh, đừng hỏi từng cái.** Bảng chấm điểm song song > nhận xét riêng lẻ.
