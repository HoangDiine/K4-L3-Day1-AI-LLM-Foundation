# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Temperature thấp cho câu trả lời ổn định và ít thay đổi hơn. Khi tăng temperature, cách diễn đạt đa dạng và sáng tạo hơn, nhưng đôi khi cũng dễ lan man hoặc kém chính xác hơn.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Mình sẽ để khoảng 0.2–0.4. Mức này giúp bot trả lời nhất quán, đúng chính sách nhưng vẫn tự nhiên.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Mỗi ngày có khoảng 10,5 triệu output token. GPT-4o tốn khoảng 105 USD/ngày, còn mini khoảng 6,3 USD/ngày, tức đắt hơn gần 17 lần. GPT-4o phù hợp cho việc cần suy luận phức tạp; mini hợp với FAQ và câu hỏi lặp lại.
---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Persona giáo viên dùng từ đơn giản, câu ngắn và ví dụ gần gũi nên dễ hiểu hơn. Persona chuyên gia tài chính thường dài hơn, dùng các từ như phi tập trung, sổ cái phân tán và xác thực giao dịch. System prompt quyết định giọng điệu, độ sâu và cách model chọn ví dụ.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Với đoạn văn mình thử, tiktoken đếm nhiều hơn ước lượng số từ/0.75 khoảng 15%. Tiếng Việt có dấu và cách tách từ không giống tiếng Anh, nên một từ hoặc một phần từ có thể bị tách thành nhiều token hơn.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng khi câu trả lời dài hoặc người dùng đang chờ trực tiếp, vì họ thấy bot phản hồi ngay thay vì nhìn màn hình đứng yên. Non-streaming phù hợp khi chỉ cần lấy kết quả hoàn chỉnh để xử lý tiếp, ví dụ lưu vào cơ sở dữ liệu hoặc tạo báo cáo tự động.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giảm áp lực cho server bằng cách cho mỗi lần thử lại chờ lâu hơn. Nếu hàng nghìn client cùng chờ cố định 1 giây rồi gọi lại, chúng có thể tạo thêm một đợt quá tải cùng lúc.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Mình chọn persona: "Bạn là trợ giảng AI thân thiện, trả lời ngắn gọn bằng tiếng Việt và đưa ví dụ đơn giản khi cần." Cụm "ngắn gọn" giúp câu trả lời dễ đọc, còn "bằng tiếng Việt" giúp phù hợp với người học trong lớp.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là bot chỉ nhớ 3 lượt gần nhất nên dễ quên ngữ cảnh cũ. Có thể cải thiện bằng cách tóm tắt các lượt cũ thành một đoạn ngắn và gửi đoạn tóm tắt đó cùng history ở các lần gọi sau.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
