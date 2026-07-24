# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 14h00–18h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.7, 1.2 và 1.8 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Hà Nội."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi? Ở mức nào phản hồi bắt đầu
kém mạch lạc?** (2–3 câu)
> Khi temperature tăng từ 0.0 lên 1.5, câu trả lời có xu hướng đa dạng, sáng tạo và khó dự đoán hơn. Với temperature 0.0, nội dung thường ổn định, ngắn gọn và tập trung vào một sự thật rõ ràng; trong khi ở mức 1.0–1.5, cách diễn đạt phong phú hơn nhưng đôi khi có thể dài dòng hoặc kém chính xác hơn.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho trợ lý soạn thảo hợp đồng pháp lý,
và bao nhiêu cho trợ lý viết slogan quảng cáo? Giải thích khác biệt.**
> Đối với trợ lý soạn thảo hợp đồng pháp lý, tôi sẽ đặt temperature khoảng 0.0–0.2 để mô hình trả lời ổn định, chính xác và hạn chế việc tự sáng tạo nội dung có thể gây sai sót pháp lý. Đối với trợ lý viết slogan quảng cáo, tôi sẽ đặt temperature khoảng 1.0–1.2 để tạo ra nhiều ý tưởng mới, đa dạng và sáng tạo hơn. Hai ứng dụng có mục tiêu khác nhau nên cần mức độ ngẫu nhiên khác nhau.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 20.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 2 lần,
mỗi lần trung bình ~500 token đầu ra.

**Ước tính chi phí mỗi ngày của model lớn so với model nhỏ cho workload này
(dựa trên bảng giá trong template). Nêu một trường hợp model lớn xứng đáng
với chi phí và một trường hợp model nhỏ là lựa chọn đúng:**
> GPT-4o có chi phí khoảng 200 USD/ngày, trong khi GPT-4o mini chỉ khoảng 12 USD/ngày, rẻ hơn rất nhiều. Model lớn phù hợp với các ứng dụng yêu cầu suy luận phức tạp như trợ lý pháp lý hoặc phân tích tài liệu chuyên môn. Model nhỏ phù hợp với chatbot CSKH, tóm tắt văn bản hoặc trả lời câu hỏi thông thường để tiết kiệm chi phí.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích máy học (machine learning) là gì?"** nhưng hai system prompt
khác nhau:
- "Bạn là một nhà thơ, trả lời mọi thứ bằng hình ảnh ví von, tránh thuật ngữ."
- "Bạn là kỹ sư phần mềm senior, trả lời chính xác, có ví dụ code khi phù hợp."

**Hai phản hồi khác nhau như thế nào (giọng văn, độ dài, mức kỹ thuật)?
Từ đó rút ra system prompt điều khiển được những khía cạnh nào của phản hồi?**
(3–4 câu)
> Với system prompt "nhà thơ", mô hình sử dụng ngôn ngữ giàu hình ảnh, nhiều phép ẩn dụ và hầu như không dùng thuật ngữ kỹ thuật. Với system prompt "kỹ sư phần mềm senior", câu trả lời chính xác hơn, có cấu trúc rõ ràng và giải thích bằng các khái niệm kỹ thuật, thậm chí kèm ví dụ code nếu cần. Điều này cho thấy system prompt có thể điều khiển phong cách viết, giọng văn, mức độ chi tiết, trình độ kỹ thuật và cách trình bày của mô hình.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~150 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Nếu dùng ước lượng thô để dự
toán ngân sách API cho ứng dụng tiếng Việt, bạn sẽ dự toán thiếu hay thừa —
và vì sao?**
> Khi so sánh, số token do tiktoken tính thường lớn hơn một chút so với cách ước lượng bằng số từ / 0.75, chênh lệch khoảng 10–20% (tùy đoạn văn). Nếu chỉ dùng công thức ước lượng để tính ngân sách API cho tiếng Việt thì rất dễ dự toán thiếu, vì tiếng Việt có nhiều dấu, ký tự Unicode và cách tách token của mô hình không hoàn toàn giống cách đếm từ thông thường.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Xét ba ứng dụng: (a) chatbot văn bản, (b) trợ lý giọng nói đọc to phản hồi,
(c) pipeline dịch tài liệu chạy ngầm ban đêm. Ứng dụng nào hưởng lợi nhiều
nhất từ streaming, ứng dụng nào không cần — và tại sao?** (1 đoạn văn)
> Streaming mang lại lợi ích lớn nhất cho chatbot văn bản và đặc biệt là trợ lý giọng nói, vì người dùng nhìn thấy hoặc nghe được phản hồi ngay khi mô hình bắt đầu sinh nội dung nên cảm giác chờ đợi ngắn hơn. Trong khi đó, pipeline dịch tài liệu chạy ngầm ban đêm hầu như không cần streaming vì người dùng chỉ quan tâm đến kết quả cuối cùng chứ không theo dõi quá trình sinh văn bản.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**Khi API quá tải và hàng nghìn client cùng retry, exponential backoff giúp
gì so với delay cố định? Tra cứu thêm: kỹ thuật "jitter" (thêm độ trễ ngẫu
nhiên) giải quyết vấn đề gì còn sót lại?**
> Exponential backoff giúp giảm số lượng yêu cầu gửi lại khi API đang quá tải bằng cách tăng dần thời gian chờ sau mỗi lần thất bại. Nếu tất cả client đều retry với khoảng thời gian cố định thì chúng có thể đồng loạt gửi yêu cầu lại và tiếp tục làm máy chủ quá tải. Kỹ thuật jitter thêm một khoảng thời gian ngẫu nhiên vào mỗi lần retry để các client không gửi request cùng lúc, từ đó giảm hiện tượng "thundering herd" và tăng khả năng hệ thống phục hồi.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Viết lại system prompt bạn dùng cho trợ lý của mình. Chỉ ra 2 chỗ trong
prompt mà nếu xóa đi, hành vi trợ lý sẽ thay đổi rõ rệt — và mô tả thay đổi
đó:**
> System prompt:"Bạn là một trợ lý AI thân thiện, luôn trả lời bằng tiếng Việt rõ ràng, giải thích từng bước và chỉ đưa ra thông tin có căn cứ. Nếu không chắc chắn thì hãy nói rằng bạn không biết thay vì tự suy đoán."Nếu bỏ câu "luôn trả lời bằng tiếng Việt", trợ lý có thể chuyển sang tiếng Anh hoặc trả lời lẫn nhiều ngôn ngữ. Nếu bỏ câu "nếu không chắc chắn thì hãy nói rằng bạn không biết", mô hình sẽ có xu hướng suy đoán nhiều hơn và dễ tạo ra thông tin không chính xác (hallucination).

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn giữ history 4 lượt cuối. Hãy mô tả một tình huống hội thoại
cụ thể mà giới hạn này khiến trợ lý trả lời sai/mất ngữ cảnh, và đề xuất một
cách khắc phục (ví dụ: tóm tắt các lượt cũ, tăng giới hạn có chọn lọc...):**
> Giả sử người dùng trao đổi khoảng 10 lượt để xây dựng một chương trình Python. Sau đó họ hỏi lại về một biến hoặc yêu cầu đã được nhắc ở những lượt đầu tiên. Do trợ lý chỉ lưu 4 lượt hội thoại gần nhất, thông tin cũ đã bị mất nên mô hình có thể trả lời sai hoặc yêu cầu người dùng cung cấp lại dữ liệu. Một cách khắc phục là tóm tắt các lượt hội thoại cũ thành một bản ghi ngắn và đưa bản tóm tắt đó vào context, hoặc tăng giới hạn history một cách có chọn lọc đối với các thông tin quan trọng.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên GitHub cá nhân và nộp link repo vào vlearn (theo hướng dẫn README)
