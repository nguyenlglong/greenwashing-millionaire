# Greenwashing Millionaire Live

Website trắc nghiệm trực tiếp 15 câu dựa trên bài tiểu luận Greenwashing của nhóm (mục 1.1, 1.2, 2.1–2.3, 4.1). 30–50 người/phòng; Host, Player và màn hình trình chiếu; PIN/QR; 20 giây/câu; 50:50; bảng xếp hạng và CSV.

## Chạy trên máy

Yêu cầu Node.js 20+.

```bash
npm install
npm start
```

Mở `http://localhost:3000`. Máy chủ và điện thoại cần truy cập được cùng địa chỉ. `localhost` chỉ hoạt động trên chính máy đó.

## Đưa lên Internet (khuyến nghị Railway / VPS)

1. Đưa thư mục dự án lên một repository GitHub riêng. Không tải lên thư mục `data/` hoặc token bí mật.
2. Trên Railway, tạo dịch vụ từ repository, lệnh khởi động `npm start`, đặt `PORT` theo cổng môi trường (Railway thường tự cung cấp).
3. Gắn **persistent volume** tại `/data`, đặt biến `DATA_DIR=/data` để lưu điểm qua các lần khởi động lại. Không dùng nhiều replicas với backend này.
4. Sau khi có tên miền HTTPS, đặt `PUBLIC_URL=https://ten-mien-cua-ban` để QR chính xác. Deploy lại.
5. Mở website bằng laptop, chọn **Tạo phòng**, chiếu QR/PIN. Sinh viên quét QR và nhập biệt danh.
6. Bấm **Bắt đầu**, sau mỗi câu bấm **Công bố đáp án** (hoặc chờ hết 20 giây), sau đó **Câu tiếp theo**. Cuối game xem TOP 3 và xuất CSV.
7. Chạy thử trên 2–3 điện thoại, sau đó thử tải 50 kết nối trước buổi học.

**Không dùng Vercel serverless** cho server Socket.IO này: cần tiến trình Node chạy liên tục. Có thể chuyển sang Supabase/managed realtime nếu muốn triển khai frontend Vercel, nhưng bản mã nguồn này dùng Express + Socket.IO + lưu JSON một tiến trình.

## Bảo mật và giới hạn

- Điểm và thời hạn trả lời được kiểm tra phía máy chủ; mỗi người chỉ trả lời một lần/câu.
- Host token lưu trong sessionStorage; không chia sẻ URL màn hình chiếu (có token Host) cho người chơi.
- Người chơi khôi phục phiên bằng token lưu trên trình duyệt của họ.
- Phòng được dọn sau 24 giờ. Dữ liệu biệt danh, điểm được lưu trong volume; cần xóa file `rooms.json` sau buổi chơi nếu không cần giữ.
- Chưa có hệ thống tài khoản/quản trị hoặc chống nhiều thiết bị/biệt danh khác nhau; sử dụng biệt danh lớp học và giám sát khi thi đấu.
- Một phiên bản triển khai một tiến trình, chưa có kiểm thử tải tự động 50 người và chưa được xác nhận hoạt động trên hosting công khai.

## Thang điểm

Câu 1–3: 100; 4–6: 200; 7–9: 300; 10–12: 500; 13–15: 1.000. Tối đa 6.300. Hòa điểm xét số câu đúng, rồi tổng thời gian trả lời đúng; bằng cả ba thì đồng hạng về tiêu chí (giao diện hiển thị theo thứ tự ổn định).

## Nội dung

Chỉnh sửa `questions.json` nếu muốn đổi câu hỏi; mỗi câu có `q` (câu hỏi), `a` (4 lựa chọn), `c` (chỉ số đáp án đúng 0–3), `e` (giải thích), `s` (mục tài liệu). Không sửa khi phòng đang diễn ra.
