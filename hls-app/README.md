# HLS 0.1 — Phòng đọc hiểu & Xưởng viết (prototype hoạt động)

Nền tảng thử nghiệm độc lập cho **hls.id.vn**, dành cho môn Ngữ văn lớp 6–12. Đây là bản tĩnh không có API key, thư viện bên thứ ba hoặc tracker.

## Chạy thử

Mở `index.html` trên trình duyệt; nên chạy qua một web server tĩnh để giống môi trường thực tế:

```bash
cd hls-v0.1
python3 -m http.server 8000
# Mở http://localhost:8000
```

## Các chức năng đã triển khai

- Dashboard, chọn khối lớp 6–12, giao diện responsive và menu trên điện thoại.
- Thư viện ngữ liệu: bài *Qua Đèo Ngang* (phạm vi công cộng), truyện tự biên soạn và thêm văn bản cá nhân; kèm câu hỏi đọc hiểu.
- Phòng đọc hiểu: chọn văn bản, bôi đoạn, tô ba màu, lưu dẫn chứng, trả lời câu hỏi, lưu cục bộ.
- Xưởng viết: 4 mô hình hỗ trợ (phân tích nghệ thuật, cảm nhận, nghị luận xã hội, bài phân tích); viết theo từng bước, chuyển dẫn chứng từ đọc sang viết, ghép nháp, chỉnh bài hoàn chỉnh, lưu lịch sử phiên bản, xuất TXT.
- Giáo viên tạo nhiệm vụ, xuất JSON; học sinh nhập nhiệm vụ, đọc/viết rồi xuất JSON bài nộp; giáo viên nhập và xem bài nộp.
- Sao lưu/khôi phục toàn bộ dữ liệu bằng JSON; hồ sơ hoạt động cục bộ.
- Không tích hợp dịch vụ AI hoặc chức năng chấm điểm tự động. Giao diện không tuyên bố thay thế giáo viên.

## Giới hạn quan trọng

**Dữ liệu lưu bằng localStorage trong thiết bị hiện tại; không có đăng nhập, phân quyền thật, đồng bộ nhiều máy hoặc nộp bài trực tuyến.** Nút “Góc giáo viên” chưa có kiểm soát truy cập — chỉ dùng như công cụ cá nhân trong giai đoạn thử nghiệm. Tệp JSON sao lưu/bài nộp chưa mã hóa: tránh điền thông tin nhạy cảm và không công bố tệp.

Trình duyệt có thể xóa dữ liệu cục bộ. Hãy dùng “Sao lưu” thường xuyên. Chưa có bảo đảm không mất dữ liệu như cơ sở dữ liệu máy chủ; trước khi sử dụng đại trà cần triển khai hệ thống tài khoản, D1/Worker, phân quyền và quy trình đồng ý/pháp lý phù hợp với học sinh.

## Triển khai lên Cloudflare Pages ở hls.id.vn

1. Tải thư mục này lên một **repository riêng** hoặc thư mục `hls-app` trong repo `nddungdn/hlstools` (không sửa `CNAME` hiện phục vụ `tools.hls.id.vn`).
2. Vào Cloudflare → Workers & Pages → Create → Pages → Connect to Git → chọn kho mã nguồn và nhánh. Nếu dùng repo chung với công cụ cũ, cấu hình **Root directory: `hls-app`**. Framework preset: `None`; Build command để trống; output directory: `.`.
3. Vào Pages project → Custom domains → Set up a domain → nhập `hls.id.vn`.
4. Vì `hls.id.vn` là tên miền gốc, cần thêm zone `hls.id.vn` vào Cloudflare và cấu hình nameserver của tên miền theo hướng dẫn Cloudflare. Không tự ý sửa zone/DNS nếu đang phục vụ `tools.hls.id.vn` mà chưa kiểm tra bản ghi hiện tại.
5. Kiểm tra chứng chỉ HTTPS, đường dẫn trang chủ, lưu bản nháp và import/export trên điện thoại.

Tài liệu chính thức: https://developers.cloudflare.com/pages/configuration/custom-domains/

## Hướng phát triển tiếp

- Backend Cloudflare Worker + D1 mới, xác thực/ủy quyền cho giáo viên và học sinh, giao/nộp trực tuyến, lưu phiên bản phía máy chủ.
- Bộ yêu cầu cần đạt 6–12 và rubric do giáo viên thẩm định; AI Gateway nhiều nhà cung cấp khi cần.
- Cần quy định bản quyền ngữ liệu, xử lí dữ liệu cá nhân và quy trình xóa/xuất dữ liệu trước khi triển khai với học sinh thật.

**Lưu ý:** Mô hình “Nêu – Dẫn – Giải – Kết” được sử dụng như phương pháp hỗ trợ phân tích nghệ thuật, không phải khung chấm cứng hay cách viết duy nhất.