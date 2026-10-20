# SpeakFlow AI — Website học tiếng Anh (MVP nâng cấp)

Ứng dụng học tiếng Anh **A1–C2** có thể sử dụng ngay, giao diện tiếng Việt và nội dung học tiếng Anh. Xây bằng **HTML/CSS/JavaScript** thuần, server Node.js là tùy chọn. Không cần cài thư viện npm, không phụ thuộc CDN để chạy phần học offline.

## Bắt đầu trong 1 phút

**Không cài đặt:** Giải nén gói ZIP, mở `index.html` bằng Chrome hoặc Edge. Có thể học từ vựng, flashcards, quiz, hội thoại có kịch bản, IELTS/TOEIC, xem tiến độ và sao lưu dữ liệu. Quyền dùng microphone và khả năng lưu dữ liệu khi mở `file://` phụ thuộc trình duyệt. Nếu gặp hạn chế, dùng server localhost như bên dưới.

**Cách ổn định trên máy tính:** Cài Node.js 20+, mở Terminal trong thư mục đã giải nén và chạy:

```bash
node server.js
```

Truy cập **http://localhost:4173** trên máy tính.

**File 1 trang:** Gói phát hành có `SpeakFlow_AI_Offline.html` chứa HTML, CSS và JavaScript trong một file để xem/ôn tập nhanh. Cũng không có backend AI khi chạy offline.

## Các phần hoàn thiện

| Module | Tính năng thực tế |
|---|---|
| Dashboard | Lịch trình gợi ý, tiến độ 7 ngày, chuỗi ngày học, lối tắt vào từng bài |
| Vocabulary | **156 mục từ/cụm từ**, **42 nhóm chủ đề** (đếm từ dữ liệu hiện tại), phân loại A1–C2, IPA, dịch, ví dụ, collocation, tìm kiếm, lọc, lưu từ, nghe bằng TTS của trình duyệt |
| Smart Flashcards | Lật thẻ, đánh giá mức nhớ (4 mức), lịch ôn cơ bản và lưu tiến độ |
| Vocabulary Quiz | **Mới:** bài trắc nghiệm 10 câu theo trình độ, giải thích ví dụ sau mỗi câu, xem lại câu đúng/sai, lưu lượt hoàn thành |
| Speaking Partner | **12 tình huống giao tiếp**, trả lời bằng văn bản/nhận diện giọng nói trên trình duyệt (nếu được hỗ trợ), hội thoại demo có kịch bản hoặc hỏi tiếp bằng OpenAI API nếu server có khóa |
| IELTS Speaking | Part 1, Part 2, Part 3, đề và bài mẫu; đồng hồ; lưu bài; **mới: nhận xét transcript bằng AI khi có API** |
| TOEIC Speaking | 5 dạng luyện mô phỏng, bài mẫu/gợi ý, bấm giờ, lưu bài, nhận xét AI nếu có API |
| Pronunciation Lab | Minimal pairs, shadowing, nghe và ghi âm để tự đối chiếu; không có hệ thống chấm âm vị |
| Progress | Hoạt động học, quiz đã làm, lịch sử hội thoại, bài thi, dữ liệu ôn từ |
| Settings | Cá nhân hóa mục tiêu, CEFR, thời gian, TTS, xuất/nhập JSON, xóa dữ liệu |

**Lưu ý:** Nhãn trình độ CEFR là mục tiêu giảng dạy mang tính tham khảo. Các bài IELTS/TOEIC là bài tự biên soạn để luyện tập, không phải đề thi chính thức.

## Kết nối OpenAI (tùy chọn)

Ứng dụng **không có API key được cung cấp sẵn**. Khi đặt `OPENAI_API_KEY` trong biến môi trường của **máy chủ**, server sẽ tự động báo đã kết nối. Khóa API tuyệt đối không được đặt trong trang HTML, tệp JavaScript frontend hay gửi cho người khác.

```bash
# macOS / Linux:
OPENAI_API_KEY="your-key" node server.js

# Windows PowerShell:
$env:OPENAI_API_KEY="your-key"; node server.js
```

Có thể đặt `OPENAI_MODEL` nếu muốn chọn model khác; mặc định là `gpt-4.1-mini`. Có thể phát sinh chi phí sử dụng OpenAI API. Để đổi cổng, đặt biến môi trường `PORT`.

Server sử dụng OpenAI Responses API từ server, không để lộ khóa qua trình duyệt. Các endpoint:

- `GET /api/config`: trả trạng thái kết nối AI (không hiển thị khóa).
- `POST /api/chat`: câu trả lời theo chủ đề, trò chuyện tự nhiên khi API khả dụng.
- `POST /api/evaluate`: phản hồi cụ thể bằng tiếng Việt dựa trên **nội dung transcript** IELTS/TOEIC. Không tự nhận đã nghe phát âm, không chấm điểm thi chính thức.

Nếu không có khóa hoặc kết nối API thất bại, website vẫn hoạt động với các tính năng offline. Website không thể cung cấp hội thoại AI thực sự khi dùng file offline đơn lẻ.

## Quyền riêng tư và giới hạn

- Thư viện từ và tiến độ học được lưu ở **localStorage** của trình duyệt; **chưa có tài khoản hay đồng bộ nhiều thiết bị**. Nên xuất JSON sao lưu định kỳ.
- Ghi âm dự phòng (nếu trình duyệt không có SpeechRecognition) chỉ là bản ghi tạm trong trình duyệt. Không upload file audio lên OpenAI ở phiên bản này.
- Nếu bật chế độ AI, transcript văn bản của buổi học được gửi đến backend và dịch vụ API để tạo câu trả lời hoặc nhận xét.
- **Phân tích ngữ pháp demo là heuristic**, không thay thế giảng viên. Không đánh giá phát âm, độ trôi chảy từ raw audio hoặc đưa ra điểm Band IELTS/TOEIC chính thức.
- Bộ 156 mục từ là tập mẫu hữu ích cho MVP, **chưa phải thư viện hoàn chỉnh hàng nghìn từ**. Có thể mở rộng qua `data.js`.
- Chưa triển khai hosting/tên miền công khai. Nếu đưa lên Internet, cần xác thực người dùng, giới hạn tần suất API, ngân sách, bảo vệ dữ liệu cá nhân và kiểm thử bảo mật nâng cao.

## Thử nhanh

1. Dashboard → Từ vựng → tìm **opportunity**, lưu yêu thích.
2. Flashcards → lật thẻ → chọn **Đã nhớ**.
3. **Quiz từ vựng** → chọn A2 → làm hết 10 câu → xem kết quả.
4. Speaking → chọn **Job interview** → bắt đầu → trả lời tiếng Anh → kết thúc & lưu.
5. IELTS → Part 2 → bấm **Chuẩn bị** → **Bắt đầu nói** → nhập hoặc nhận transcript → Lưu.
6. Tiến độ → xem lịch sử. Cài đặt → xuất file JSON để sao lưu.
7. Trên Chrome/Edge hoặc điện thoại có quyền microphone, thử ghi âm; nếu không hỗ trợ, dùng bàn phím.

## Các tệp

```text
speakflow-ai/
  index.html      Giao diện và điều hướng
  styles.css      Thiết kế responsive
  data.js         Từ vựng, tình huống, đề luyện, bài mẫu
  app.js          Quiz, luyện nói, từ vựng, lưu tiến độ
  server.js       Server tĩnh + OpenAI API proxy (tùy chọn)
  favicon.svg     Biểu tượng
  README.md       Hướng dẫn
  tests/          Kiểm thử API và cấu trúc dữ liệu
```

## Bản mở nhanh không cần cài đặt

Tệp `SpeakFlow_AI_Mo_Ngay.html` nằm bên ngoài thư mục mã nguồn. Chỉ cần tải tệp đó về và mở bằng trình duyệt; có thể luyện Vocabulary, Flashcards, Quiz, hội thoại demo, IELTS/TOEIC và phát âm. Trong tệp này không có backend nên AI live không khả dụng. Chrome/Edge trên máy tính là lựa chọn dễ dùng nhất. Safari/iOS có thể hạn chế nhận diện lời nói. Nếu trình duyệt không lưu được Local Storage từ tệp trực tiếp, hãy chạy server Node như hướng dẫn trên.

Để bật AI thật qua terminal khi đã có API key riêng:

- macOS/Linux: `OPENAI_API_KEY="your-secret-key" node server.js`
- Windows PowerShell: `$env:OPENAI_API_KEY="your-secret-key"; node server.js`

Chỉ đặt khóa trong terminal, không đưa vào `app.js`, `index.html` hay tệp HTML mở nhanh. OpenAI API có thể có chi phí sử dụng. Không có tài khoản hay đồng bộ cloud trong bản hiện tại.

## Bản mở nhanh không cần cài đặt

Tệp `SpeakFlow_AI_Mo_Ngay.html` nằm bên ngoài thư mục mã nguồn. Chỉ cần tải tệp đó về và mở bằng trình duyệt; có thể luyện Vocabulary, Flashcards, Quiz, hội thoại demo, IELTS/TOEIC và phát âm. Trong tệp này không có backend nên AI live không khả dụng. Chrome/Edge trên máy tính là lựa chọn dễ dùng nhất. Safari/iOS có thể hạn chế nhận diện lời nói. Nếu trình duyệt không lưu được Local Storage từ tệp trực tiếp, hãy chạy server Node như hướng dẫn trên.

Để bật AI thật qua terminal khi đã có API key riêng:

- macOS/Linux: `OPENAI_API_KEY="your-secret-key" node server.js`
- Windows PowerShell: `$env:OPENAI_API_KEY="your-secret-key"; node server.js`

Chỉ đặt khóa trong terminal, không đưa vào `app.js`, `index.html` hay tệp HTML mở nhanh. OpenAI API có thể có chi phí sử dụng. Không có tài khoản hay đồng bộ cloud trong bản hiện tại.
