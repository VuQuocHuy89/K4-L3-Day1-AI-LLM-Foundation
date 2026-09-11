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
> Ở temperature 0.0 câu trả lời trực tiếp, khuôn mẫu và ít sáng tạo. Khi tăng temperature lên 
thì câu trả lời có cách diễn đạt đa dạng và sáng tạo hơn nhưng cũng dài dòng lan man hơn.Có thể thấp nếu cần trả lời ngắn gọn trực diện thì dùng temperature thấp còn khi muốn sáng tạo thì dùng mức cao.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt temperature ở mức vừa phải khoảng 0.2-0.4 cho chatbot hỗ trợ khách hàng. Ở mức này câu trả lời sáng tạo ở mức vừa phải nhưng ổn định, rõ ràng và ít tạo các thông tin không chính xác.
Chatbot hộ trộ khách hàng nên ưu tiên độ ổn định, chính xác thay vì sự sáng tạo.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Với GPT-4o đắt hơn khoảng 16,7 lần khi tốn khoảng 105 USD/ngày trong khi đó GPT-4o-mini chỉ tốn khoảng 6,3 USD/ngày. GPT-4o phù hợp với các yêu cầu phức tạp đòi hỏi phân tích chuyên xâu và xử lý tác vụ khó. Ngược lại GPT-4o-mini phù hợp với câu hỏi đơn giải, thường gặp phù hợp với câu hỏi cơ bản.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với giáo viên, model cố gắng giải thích blockchain bằng cách đơn giản, gần gũi, dễ hiểu. Còn với chuyên gia tài chính, model sử dụng cách diễn đạt chuyên sâu hơn có dùng các thuật ngữ chuyên nghành. System promt ảnh hướng đến vai trò, đối tượng người đọc, từ vựng và độ dài của của câu trả lời.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Tôi đã cho đoạn văn 101 từ và cách ước lượng số từ / 0.75, đoạn văn có khoảng 134.67 token. Hàm count_tokens bằng tiktoken đếm được 113 token, nên hai kết quả chênh lệch khoảng 16.09%. Tiếng Việt tốn token hơn vì tiếng Việt có dấu, từ ghép, dấu câu và có cách tách subword riêng vì vậy số token thực tế  có sự khác với ước lượng dựa trên số từ một cách thử công.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng khi model cần tạo câu trả lời dài và muốn câu trả lời xuất hiện ngay lập tức, trong khi đó non-streaming phù hợp hơn với các câu lời ngắn, các tác vụ có mức logic cao cần xử lý toàn bộ kết quả trước khi hiển thị hoặc các tác vụ không cần giao tiếp theo thời gian thực.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp các client giãn thời gian chờ sau mỗi lần retry, giúp giảm áp lực lên API khi hệ thống đang quá tải và tăng cơ hội request thành công sau khi dịch vụ hồi phục. Nếu hàng nghìn client đều retry sau đúng một khoảng thời gian cố định, có thể gửi request đồng loạt, tạo ra hiện tượng thundering herd và làm hệ thống tiếp tục quá tải.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Bạn là một trợ lý AI chuyên nghiệp có nhiều kinh nghiệm. Hãy giải thích bằng tiếng Việt, dùng ngôn ngữ dễ hiểu, chia vấn đề phức tạp thành các bước nhỏ kèm theo ví dụ trực quan. Nếu câu hỏi chưa rõ, hãy hỏi lại thay vì đoán, hãy trả lời đầy đủ súc tích. Tôi sử dụng cụm "trợ lý AI chuyên nghiệp có nhiều kinh nghiệm" để định hướng giọng điệu gần gũi và khuyến khích người học đặt câu hỏi. Yêu cầu "chia vấn đề phức tạp thành các bước nhỏ" giúp câu trả lời dễ hiểu hơn. Việc chỉ định trả lời bằng tiếng Việt giúp kết quả phù hợp với người dùng.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất của trợ lý là history chỉ lưu tối đa ba lượt hội thoại gần nhất, nên nó có thể quên các thông tin đã trao đổi từ trước đó. Có thể cải thiện bằng cách lưu lịch sử vào cơ sở dữ liệu hoặc file, sau đó dùng kỹ thuật tóm tắt hội thoại để giữ lại các ý chính thay vì lưu toàn bộ nội dung. Sau đó mỗi khi gặp history dài thì model sẽ thực hiện theo các bước nhau vậy.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
