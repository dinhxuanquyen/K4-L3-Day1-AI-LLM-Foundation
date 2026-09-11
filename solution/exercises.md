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

> Khi để temperature bằng 0.0, AI trả lời rõ ràng hơn, các lần hỏi đều trả các kết quả gần giống hệt nhau, khi tăng lên 0.5 đến 1.0 thí cách AI trả lời thay đổi, các câu từ có sự biến đổi, còn khi tăng lên 1.5 AI trả lời dài dòng hơn

### Câu 1.2 — Chọn temperature cho sản phẩm

**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**

> Khi làm chatbot hỗ trợ khách hàng nên để temperature thấp, khoảng 0.2-0.3, khi đó chatbot sẽ trả lời chính xác hơn, tránh bịa đặt khi trả lời với khách hàng

### Câu 1.3 — Đánh đổi chi phí

Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**

> Với bảng giá trong lab, chi phí output của GPT-4o là 0.010 USD/1K token, còn GPT-4o-mini là 0.0006 USD/1K token, nên GPT-4o đắt hơn khoảng 16.7 lần cho workload này. Workload có 10.000 _ 3 _ 350 = 10.500.000 token đầu ra/ngày, tương đương khoảng 105 USD/ngày với GPT-4o và 6.3 USD/ngày với mini nếu chỉ tính output. GPT-4o đáng dùng cho tác vụ cần suy luận phức tạp, chất lượng cao hoặc rủi ro sai lớn; mini phù hợp cho hỏi đáp đơn giản, phân loại, tóm tắt ngắn hoặc chatbot khối lượng lớn.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona

Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:

- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)

> Bảo máy đóng vai thành cô giáo tiểu học thì AI giảng ngắn, dùng từ dễ như giải thích cho học sinh. khi đổi sang vai chuyên gia tài chính từ AI phán đoán nhiều hơn dẫn đến việc trả lời dài hơn tuy nhiên câu trả lời theo tính minh bạch xác thực. System promt có ảnh hưởng đến hành vi model giống như các quy tắc prompt đơn giải thì AI trả lời tốt hơn còn những prompt phức tạp nhiều yêu cầu, AI trả lời dài dòng lan man và đôi khi không có tính xác thực chính xác

### Câu 2.2 — tiktoken vs đếm từ

Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**

> Đoạn tiếng Việt 88 từ của tôi đếm bằng tool ra 106 token, còn tính nhẩm kiểu số từ / 0.75 thì ra khoảng 117 token (lệch tầm 10%). Tiếng Việt tốn token hơn tiếng Anh vì có dấu, các công cụ đếm token hay phải tách một từ có dấu ra thành 2-3 mẩu nhỏ để đọc.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming

**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)

> Streaming giống như kiểu vừa gõ vừa hiện chữ, cực kỳ hợp cho chatbot để người dùng thấy máy đang trả lời ngay, không phải ngồi chờ xoay vòng vòng. Còn non-streaming hợp cho mấy việc xử lý ngầm, ví dụ như bắt máy phân loại dữ liệu rồi gửi sang phần mềm khác chạy tiếp.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?

**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**

> Lỗi mạng mà cứ thử lại liên tục (ví dụ 1 giây thử 1 lần) thì chẳng khác nào hàng nghìn người cùng bấm F5 liên tục làm sập luôn web. Chờ theo kiểu nhân lên (1s -> 2s -> 4s -> 8s) giúp hệ thống có thời gian thở và giãn bớt lưu lượng truy cập ra.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona

**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**

> System Prompt: "Bạn là trợ giảng thân thiện của lớp AI. Hãy trả lời bằng tiếng Việt, ngắn gọn, dễ hiểu và hướng dẫn từng bước một." Giải thích: Phải dặn "tiếng Việt" để máy không nói tiếng Anh. Thêm "ngắn gọn" để bớt tốn tiền token và đỡ dài dòng. Thêm "từng bước một" để người mới bắt đầu dễ làm theo.

### Câu 4.2 — Hạn chế & cải thiện

**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**

> Điểm yếu nhất là máy chỉ nhớ được 3 câu gần nhất, nói chuyện dài một lúc là nó quên mất đoạn đầu.Cách sửa: Làm thêm tính năng tóm tắt. Cứ sau vài câu trò chuyện, dặn máy tự tóm tắt lại các ý chính thành 1 câu ngắn rồi đính câu đó vào đầu đoạn chat tiếp theo, vừa đỡ quên vừa không tốn tiền lưu chữ dài.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
