# Quyền riêng tư của Ngày xưa ơi - EDO Time

Ngày cập nhật: **07/10/2026**. Mã Android: `com.eduino.timevn`.

- [index.html](index.html): chính sách quyền riêng tư.
- [delete-account.html](delete-account.html): yêu cầu xóa tài khoản qua email
  và hướng dẫn xóa trong ứng dụng.

Hai trang dùng HTML/CSS tĩnh, phông chữ hệ thống; không có JavaScript,
trình theo dõi, biểu mẫu thu thập hay tài nguyên ngoài tự tải.
Nút gửi yêu cầu mở ứng dụng email; người dùng cần gửi email để yêu cầu
được tiếp nhận. Địa chỉ cũng hiển thị để gửi thủ công.

## Thông tin đã cập nhật

- Email quyền riêng tư, hỗ trợ và yêu cầu xóa: **eduino.info@gmail.com**.
- Thời hạn xử lý yêu cầu xóa tài khoản và dữ liệu ứng dụng: **tối đa 7 ngày**.
  Mốc bắt đầu thời hạn đang chờ xác nhận.
- Hạ tầng, nhật ký và sao lưu được mô tả ở mức cơ bản vì chủ ứng dụng chưa
  xác nhận nhà cung cấp và cấu hình lưu trữ. Nội dung không cam kết thời
  hạn log/backup cụ thể hoặc khẳng định không có bản sao lưu.
- Thời gian lưu dữ liệu của nhà cung cấp được trình bày riêng: Firebase
  có thể xử lý dữ liệu xác thực trong hệ thống hoạt động/sao lưu đến
  180 ngày sau khi bắt đầu xóa; CDN Cloudinary tiêu chuẩn có thể giữ
  bản sao đến 30 ngày nếu chưa được làm mất hiệu lực.

## Còn chờ chủ ứng dụng xác nhận

1. Tên nhà phát triển trên Google Play; tên app có giữ nguyên
   **Ngày xưa ơi - EDO Time** không?
2. Mốc 7 ngày tính từ khi nhận yêu cầu hay khi xác minh chủ tài khoản?
   Ai theo dõi hộp thư và hoàn tất xóa Cloudinary/Firebase khi tự động
   thất bại hoặc người dùng gửi yêu cầu ngoài ứng dụng?

Hai trang còn `[TEN_NHA_PHAT_TRIEN]`, `[MOC_BAT_DAU_THOI_HAN_XOA]`
và nhãn **Bản nháp**. Hoàn thiện hai thông tin này và quy trình xử lý
trước khi bỏ nhãn và sử dụng URL để nộp Play Console.

Mô tả lưu trữ cơ bản không phải bằng chứng log/backup trên production
đã được kiểm tra. Cần đối chiếu cấu hình thực tế và cập nhật chính sách
nếu có dữ liệu giữ lại hoặc thời hạn khác.

## Bật GitHub Pages cho repository này

Hai tệp HTML đã nằm ở thư mục gốc, không cần sao chép sang `/docs`.

1. Commit và push các thay đổi đã hoàn thiện lên nhánh `main`.
2. Trong repository GitHub, mở **Settings → Pages**.
3. Chọn **Deploy from a branch → main → / (root) → Save**.
4. Khi site được xuất bản, bật **Enforce HTTPS** nếu tùy chọn hiển thị.
5. Mở cả hai URL ở cửa sổ riêng tư, kiểm tra không yêu cầu đăng nhập,
   nội dung mới nhất và liên kết gửi email.

Với remote hiện tại `Tudolla/time_app_privacy` và không dùng tên miền riêng:

- Chính sách: <https://tudolla.github.io/time_app_privacy/>.
- Yêu cầu xóa: <https://tudolla.github.io/time_app_privacy/delete-account.html>.

**Kiểm tra trực tiếp ngày 07/10/2026:** cả hai URL trên trả HTTP 404.
Đây là URL dự kiến; chưa dùng để nộp khi trang chưa hoạt động.
Dùng URL Pages trong Settings nếu GitHub hiển thị URL khác.

`README.MD` và `problems-report.html` là tài liệu nội bộ, không phải
URL chính sách hoặc URL yêu cầu xóa để nhập vào Play Console.

## Đối chiếu trước khi gửi Google Play

- Chính sách cần URL công khai và đường dẫn hoặc nội dung trong ứng dụng.
  Tìm kiếm mã nguồn hiện tại chưa thấy đường dẫn/nội dung chính sách trong
  `lib` của TimeTravel; tạo trang HTML không tự bổ sung vào app.
- URL yêu cầu xóa nhập ở phần Data Safety/account deletion. Google cho phép
  kênh email hỗ trợ; hộp thư và quy trình xử lý phải hoạt động thực tế.
- Mã nguồn hiện tại xóa tài khoản Phoenix và dữ liệu liên quan; gửi ảnh
  Cloudinary vào hàng đợi xóa có thử lại. Firebase được yêu cầu xóa từ
  client và có thể thất bại; cần người xử lý yêu cầu qua email.
- API bảng đóng góp vẫn trả email đầy đủ, tên, ID và số ảnh công khai.
  Chính sách đã công bố hành vi này; câu trả lời Data Safety và thông báo
  trong app cần phản ánh đúng cách công bố dữ liệu.
- Ảnh tải lên có thể chứa EXIF, bao gồm tọa độ. Không suy ra không thu thập
  vị trí chỉ từ việc app không xin quyền GPS. Kiểm tra upload thực tế trước
  khi chốt mục vị trí trong Data Safety.
- Kiểm tra nhà cung cấp, độ tuổi đối tượng, log/backup và xóa dữ liệu
  production theo cấu hình thực tế. Nội dung tài liệu không chứng minh
  Google đã duyệt ứng dụng hoặc quy trình xóa đã chạy thành công.

## Nguồn chính thức

- [Google Play: dữ liệu người dùng và chính sách quyền riêng tư](https://support.google.com/googleplay/android-developer/answer/10144311?hl=vi).
- [Google Play: Data Safety](https://support.google.com/googleplay/android-developer/answer/10787469?hl=en).
- [Google Play: yêu cầu xóa tài khoản, gồm kênh email](https://support.google.com/googleplay/android-developer/answer/13327111?hl=vi).
- [Firebase: quyền riêng tư và thời gian lưu dữ liệu](https://firebase.google.com/support/privacy).
- [Cloudinary: bộ nhớ đệm sau khi xóa ảnh](https://cloudinary.com/documentation/invalidate_cached_media_assets_on_the_cdn).
- [GitHub Pages: chọn nhánh và thư mục xuất bản](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site).
- [GitHub Pages: HTTPS](https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https).
