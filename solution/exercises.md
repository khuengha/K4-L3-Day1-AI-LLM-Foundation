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
> Khi temperature tăng từ 0.0 lên 1.5, phản hồi có xu hướng đa dạng và sáng tạo hơn về cách diễn đạt, trong khi temperature thấp cho câu trả lời ổn định và dễ dự đoán hơn. Qua bốn phản hồi, nội dung chính vẫn khá giống nhau và đều tập trung vào Hang Sơn Đoòng, vậy với prompt này mức temperature chưa làm thay đổi đáng kể chủ đề được chọn.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ chọn temperature = 0.2–0.3 cho chatbot hỗ trợ khách hàng. Mức này giúp câu trả lời ổn định, chính xác và nhất quán, hạn chế việc mô hình sáng tạo quá mức hoặc đưa ra thông tin không phù hợp, nhưng vẫn đủ tự nhiên khi giao tiếp với khách hàng.


### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Workload mỗi ngày là: 10.000 × 3 × 350 = 10.500.000 token đầu ra/ngày. Với giá output hiện tại: GPT-4o: $10 / 1 triệu token → khoảng **$105/ngày** và GPT-4o-mini: $0,60 / 1 triệu token → khoảng **$6,30/ngày**. Như vậy, nếu chỉ xét token đầu ra, **GPT-4o đắt hơn GPT-4o-mini khoảng 16,7 lần**. GPT-4o xứng đáng với chi phí khi xử lý các yêu cầu phức tạp cần khả năng suy luận và độ chính xác cao, ví dụ phân tích một khiếu nại khách hàng phức tạp để đưa ra hướng xử lý phù hợp. Ngược lại, GPT-4o-mini phù hợp với các tác vụ số lượng lớn và đơn giản như FAQ, phân loại yêu cầu, trả lời câu hỏi phổ biến hoặc tra cứu thông tin cơ bản.


---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Phản hồi với vai trò giáo viên tiểu học dùng từ ngữ đơn giản, dễ hiểu và sử dụng ví dụ “cuốn sổ ghi điểm” để giúp trẻ hình dung blockchain. Trong khi đó, phản hồi với vai trò chuyên gia tài chính dài và chuyên sâu hơn, sử dụng nhiều thuật ngữ kỹ thuật như DLT, decentralization, nodes, hash và immutability. System prompt đã định hướng rõ cách model lựa chọn từ vựng, mức độ chi tiết và cách giải thích phù hợp với từng đối tượng. Điều này cho thấy cùng một câu hỏi nhưng thay đổi persona có thể làm thay đổi đáng kể phong cách và nội dung câu trả lời.


### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Đoạn văn tiếng Việt được chọn có khoảng 120 từ. Theo cách ước lượng số từ / 0.75, số token dự kiến là khoảng 160 token, trong khi tiktoken đếm được 175 token, cao hơn khoảng **9,38%**. Sự chênh lệch xảy ra vì token không tương ứng trực tiếp với một từ; một từ tiếng Việt có dấu hoặc một số cụm từ có thể bị tokenizer tách thành nhiều token. Vì vậy, tiếng Việt thường có tỷ lệ token trên từ cao hơn tiếng Anh, đặc biệt khi tokenizer được tối ưu tốt hơn cho các mẫu từ phổ biến trong tiếng Anh.


---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất với các chatbot hoặc ứng dụng tạo nội dung dài, vì người dùng có thể thấy phản hồi xuất hiện từng phần ngay lập tức thay vì phải chờ toàn bộ câu trả lời hoàn thành, nhờ đó cảm giác phản hồi nhanh và tự nhiên hơn. Non-streaming phù hợp hơn khi kết quả ngắn, cần xử lý toàn bộ dữ liệu trước khi hiển thị, hoặc khi ứng dụng cần nhận một response hoàn chỉnh để lưu trữ, phân tích hay kiểm tra trước khi trả về cho người dùng.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff tăng dần thời gian chờ giữa các lần retry, giúp giảm số lượng request gửi lại khi API đang quá tải và tạo thời gian để hệ thống phục hồi. So với delay cố định, cách này hạn chế việc liên tục gây thêm áp lực lên server khi lỗi vẫn chưa được xử lý. Nếu hàng nghìn client cùng retry sau một khoảng cố định như 1 giây, chúng có thể đồng loạt gửi request trở lại cùng thời điểm, tạo ra một đợt tải lớn mới và khiến API tiếp tục quá tải. Vì vậy, trong thực tế exponential backoff thường được kết hợp thêm random jitter để các client không retry cùng lúc.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Tôi chọn persona là **trợ lý học tập về AI và lập trình Python dành cho người mới bắt đầu**. System prompt: **"Bạn là trợ lý học tập về AI và Python. Hãy giải thích bằng tiếng Việt, ngắn gọn, dễ hiểu, ưu tiên ví dụ thực tế và hướng dẫn từng bước khi có code. Nếu không chắc chắn về thông tin, hãy nói rõ thay vì tự suy đoán."** Tôi yêu cầu “trả lời ngắn gọn, dễ hiểu” vì đối tượng sử dụng là người mới học, giúp tránh việc câu trả lời quá dài hoặc có quá nhiều thuật ngữ khó. Việc chỉ định “bằng tiếng Việt” giúp phản hồi nhất quán với ngôn ngữ của người dùng, còn yêu cầu “không chắc thì nói rõ” nhằm hạn chế model đưa ra thông tin sai nhưng trình bày như một sự thật.


### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất của trợ lý hiện tại là **chỉ lưu 3 lượt hội thoại gần nhất**, do history = history[-6:] chỉ giữ lại 6 message gồm 3 message của user và 3 message của assistant. Vì vậy, khi cuộc hội thoại dài hơn, trợ lý có thể quên các thông tin hoặc yêu cầu đã được đề cập trước đó. Một cải thiện cụ thể là bổ sung cơ chế **bộ nhớ dài hạn hoặc tóm tắt hội thoại**. Khi history vượt quá số lượng cho phép, chương trình có thể dùng model để tóm tắt các lượt hội thoại cũ thành một đoạn ngắn và lưu đoạn tóm tắt đó vào context cùng với các lượt gần nhất. Cách này giúp trợ lý vẫn nhớ được những thông tin quan trọng mà không làm số token đầu vào tăng quá nhiều.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
