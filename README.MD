# Nội dung quyền riêng tư của Ngày xưa ơi - EDO Time

Trạng thái: **bản nháp theo mã nguồn và AAB hiện có, chưa xuất bản và chưa
xác minh tuân thủ trên production**. Ngày rà soát: 07/10/2026.

- [index.html](index.html): chính sách riêng cho Ngày xưa ơi - EDO Time.
- [delete-account.html](delete-account.html): trang hướng dẫn yêu cầu xóa tài
  khoản qua email, đồng thời hướng dẫn thao tác hiện có trong ứng dụng.

Hai trang dùng HTML/CSS tĩnh, phông chữ hệ thống, không có JavaScript, trình
theo dõi, biểu mẫu thu thập hoặc tài nguyên ngoài được tải tự động. Email đang
là chỗ điền; trang xóa chưa phải một kênh hỗ trợ đã hoạt động.

## Thông tin cần hoàn thiện trước khi công bố

Thay mọi chỗ điền ở **cả hai trang**, theo hoạt động thực tế:

| Chỗ điền | Nội dung cần xác nhận |
| --- | --- |
| `[TEN_NHA_PHAT_TRIEN]` | Tên nhà phát triển trên Play Store. Tên ứng dụng trên trang phải khớp tên ở trang thông tin Store. |
| `[EMAIL_HO_TRO]` | Hộp thư thật, được theo dõi, nhận câu hỏi quyền riêng tư và yêu cầu xóa tài khoản. Thay cả văn bản lẫn liên kết `mailto:`. |
| `[NHA_CUNG_CAP_HA_TANG_THUC_TE]` | Đơn vị chạy API, cơ sở dữ liệu, lưu log và backup. `time_server/fly.toml` cấu hình Fly.io ở Singapore; cấu hình chưa chứng minh toàn bộ hạ tầng đang hoạt động. |
| `[THOI_HAN_XU_LY_YEU_CAU_EMAIL]` | Thời hạn tiếp nhận, xác minh và hoàn tất; ai xử lý, cách trả lời khi có lỗi. Chưa có quy trình tiếp nhận email trong mã nguồn. |
| `[THOI_HAN_VA_QUY_TRINH_XOA_ANH]` | Thời hạn xóa tài nguyên Cloudinary, các bản sao/biến thể/CDN, giám sát và xử lý khi retry thất bại. Worker chạy khi server hoạt động; chu kỳ 60 giây không phải cam kết xóa trong 60 giây. |
| `[QUY_TRINH_VA_THOI_HAN_XOA_FIREBASE]` | Cách xử lý khi `currentUser.delete()` thất bại hoặc người dùng gửi yêu cầu ngoài ứng dụng. Backend hiện không có Firebase Admin deletion/retry. |
| `[DU_LIEU_MUC_DICH_VA_THOI_HAN_LOG_HO_TRO]` | Dữ liệu log/hỗ trợ thực sự giữ, mục đích và thời hạn. Không tự nhận giữ 7/30/90 ngày khi chưa có cấu hình/quy trình tương ứng. |
| `[CHINH_SACH_SAO_LUU_VA_XOA]` | Có backup nào, thời hạn, quyền truy cập, xử lý yêu cầu xóa và dữ liệu tái xuất hiện khi restore. Nếu không có backup, ghi rõ sau khi xác minh. |

Sau khi hoàn thiện, bỏ cảnh báo **Bản nháp**, đổi ngày cập nhật thành ngày công
bố thực tế và rà lại cam kết không bán dữ liệu, các nhà cung cấp, dữ liệu công
khai và đối tượng người dùng theo hoạt động ngoài mã nguồn. Không dùng URL chứa
chỗ điền hoặc nội dung chưa đúng để nộp Play Console.

## Quyền Android đã đối chiếu với AAB

Đã đọc cấu trúc manifest protobuf trong
`build/app/outputs/bundle/release/app-release.aab` và đối chiếu các phần tử
`uses-permission` với merged release manifest. Không chỉ đọc manifest nguồn.

- Mã ứng dụng: `com.eduino.timevn`; nhãn: `Ngày xưa ơi - EDO Time`.
- Phiên bản: `1.0.0`, version code `1`; target SDK `36`, min SDK `24`.
- SHA-256 của AAB được rà:
  `112987212b81d526877cca8725d99d33d8fb18bf43055707cf3d77e8a993f4f4`.
- API HTTPS theo cấu hình Fly có mặt trong mã Dart biên dịch của AAB. Chưa
  kiểm tra lưu lượng chạy thật hoặc mọi kết nối của SDK trên thiết bị.

| Quyền trong AAB | Nguồn / chức năng |
| --- | --- |
| `android.permission.INTERNET` | Manifest ứng dụng và thư viện đăng nhập; gọi API, gửi/tải ảnh, tải phông chữ. |
| `android.permission.ACCESS_NETWORK_STATE` | Firebase Auth/reCAPTCHA; trạng thái kết nối. |
| `android.permission.USE_BIOMETRIC` | AndroidX biometric qua thư viện secure storage. |
| `android.permission.USE_FINGERPRINT` | AndroidX biometric qua thư viện secure storage. |
| `com.google.android.providers.gsf.permission.READ_GSERVICES` | reCAPTCHA được Firebase Auth tích hợp; hỗ trợ dịch vụ Google. Có thư viện không chứng minh mọi tính năng của nó đang được gọi. |
| `com.eduino.timevn.DYNAMIC_RECEIVER_NOT_EXPORTED_PERMISSION` | AndroidX Core, quyền mức signature để bảo vệ receiver nội bộ; không phải quyền đọc dữ liệu cá nhân. |

Không thấy `CAMERA`, `RECORD_AUDIO`, quyền GPS, danh bạ/SMS/cuộc gọi,
`READ_MEDIA_IMAGES`, `READ_EXTERNAL_STORAGE`, `WRITE_EXTERNAL_STORAGE` hoặc
`AD_ID` trong các phần tử `uses-permission` của AAB này. `android.permission.DUMP`
chỉ là điều kiện quyền đặt trên receiver của thư viện ProfileInstaller, không
phải quyền ứng dụng xin. `<queries>` cho `PROCESS_TEXT` cũng không phải quyền
đọc danh bạ/dữ liệu của ứng dụng khác.

`LocalStorageService` dùng secure storage mặc định; phiên bản khóa trong
`pubspec.lock` là `flutter_secure_storage 10.3.1`, với `enforceBiometrics=false`.
Không thấy ứng dụng yêu cầu xác thực sinh trắc học hoặc thu thập mẫu vân tay.
Việc chọn ảnh dùng `ImageSource.gallery`, chỉ nhận tệp do người dùng chọn qua
giao diện Android; không có truy cập hàng loạt thư viện ảnh.

Khi build lại, phải đối chiếu permissions/dependencies của **AAB mới**. Bản rà
này không chứng minh quyền của mọi bản phát hành sau.

## Bằng chứng dữ liệu và giới hạn hiện tại

| Dữ liệu / hành vi | Nguồn đã đọc |
| --- | --- |
| Google → Firebase → Phoenix; hồ sơ tên/email/URL avatar, Firebase UID và ID ứng dụng | [Google identity provider](../../lib/screen/auth/service/google_identity_provider.dart), [Identity Toolkit](../../time_server/lib/time_server/firebase_auth/identity_toolkit.ex), [User schema](../../time_server/lib/time_server/accounts/user.ex), [Accounts](../../time_server/lib/time_server/accounts.ex). |
| Phiên 30 ngày, server chỉ lưu SHA-256 token; thông tin hồ sơ/phiên lưu bảo mật trên thiết bị | [Auth session storage](../../lib/screen/auth/service/auth_session_storage.dart), [Local storage](../../lib/core/service/local_storage_service.dart), [Accounts](../../time_server/lib/time_server/accounts.ex). Hết hạn không tự xóa bản ghi session. |
| Ảnh, tên tệp, tên/mô tả, người góp, giai đoạn; chủ đề/trạng thái/thời gian server quản lý | [Image picker](../../lib/screen/contribution/service/contribution_image_picker.dart), [Upload repository](../../lib/screen/contribution/repository/contribution_repository.dart), [Contribution schema](../../time_server/lib/time_server/contributions/contribution.ex). |
| EXIF có thể đi cùng ảnh, gồm GPS | `image_picker_android 0.8.13+17` trong `pubspec.lock`; `ExifDataCopier.java` sao chép GPS và metadata khi resize; backend upload chưa có bước loại bỏ EXIF. Đây là khả năng theo code, chưa thử tệp EXIF thật trên thiết bị/Cloudinary. |
| Ảnh được lưu và phân phối qua Cloudinary | [Cloudinary adapter](../../time_server/lib/time_server/media_storage/cloudinary.ex). Upload gửi tệp/tên tệp/MIME, không gửi hồ sơ Google như một metadata field. |
| Bảng công khai trả ID, tên, **email đầy đủ**, số ảnh; cho tìm email | [Contributors query](../../time_server/lib/time_server/contributors.ex), [Contributor row UI](../../lib/screen/contributor_ranking/widgets/photo_contributor_list_item.dart), [Router](../../time_server/lib/time_server_web/router.ex): `GET /api/contributors` không cần đăng nhập. Không có đồng ý riêng cho công bố email được tìm thấy trong luồng hiện tại. |
| Ảnh lưu cá nhân và thời điểm lưu | [Saved photo schema](../../time_server/lib/time_server/saved_photos/saved_photo.ex), [Router](../../time_server/lib/time_server_web/router.ex). |
| Nunito có thể tải từ Google | [App theme](../../lib/main.dart), `google_fonts 8.1.0`, runtime fetching mặc định bật; AAB này không có Nunito đóng gói. |
| Log mạng Flutter giới hạn ở debug, body/token được che | [Logging interceptor](../../lib/core/network/logging_interceptor.dart). Không suy ra server/Fly/Cloudinary không có log từ quy tắc này. |
| Xóa account Phoenix, contributions, session/saved-photo qua transaction/cascade; queue xóa Cloudinary | [Accounts](../../time_server/lib/time_server/accounts.ex), [Cleanup worker](../../time_server/lib/time_server/media_storage/cleanup.ex), [Auth/session migration](../../time_server/priv/repo/migrations/20260724151118_create_authentication_tables.exs), [Saved-photo migration](../../time_server/priv/repo/migrations/20260922000137_create_saved_photos.exs). |
| Firebase xóa từ client, có thể thất bại sau khi Phoenix đã xóa | [AppBloc](../../lib/bloc/app_bloc.dart), [Google identity provider](../../lib/screen/auth/service/google_identity_provider.dart). Chưa có server-side retry cho Firebase; không hứa xóa tất cả ngay lập tức. |
| Chưa có nội dung/đường dẫn chính sách trong ứng dụng | Tìm trong `lib`, router và `pubspec.yaml`: chưa thấy UI quyền riêng tư hoặc `url_launcher`. Google cho phép đường dẫn **hoặc văn bản** chính sách trong app; cần bổ sung trước bản phát hành nộp duyệt. |

Không thấy SDK quảng cáo, Analytics/Crashlytics, Messaging hoặc chức năng
thanh toán trong dependencies và các entry point đang dùng. `messagingSenderId`
trong cấu hình Firebase không tự chứng minh tích hợp Firebase Messaging.
Đối tượng độ tuổi trên Play Console và cấu hình Firebase/reCAPTCHA đang hoạt
động chưa được xác minh. Rà SDK tự thu thập, hạ tầng và cấu hình thực tế trước
khi chốt Data Safety.

## Gợi ý điền Data Safety từ hiện trạng

Đây là ánh xạ phục vụ rà soát, **không phải biểu mẫu đã nộp/được Google duyệt**.
Quyền Android và câu trả lời Data Safety là hai phạm vi khác nhau: không có
quyền GPS không chứng minh ảnh không chứa tọa độ.

| Nhóm trên Play Console | Căn cứ / mục đích / bắt buộc |
| --- | --- |
| Personal info → Name | Tên Google và tên người góp: thu thập; quản lý tài khoản và chức năng đóng góp. Không cần để xem nội dung công khai. |
| Personal info → Email address | Thu thập để đăng nhập/quản lý tài khoản; bắt buộc cho luồng Google login, tùy chọn so với việc dùng ứng dụng ở chế độ khách. **Email công khai ở bảng đóng góp hiện tại**. |
| Personal info → User IDs | Firebase UID, Google/account ID, ID nội bộ: thu thập để xác thực, liên kết tài khoản, ảnh và lượt lưu. |
| Photos and videos → Photos | Avatar URL/ảnh hồ sơ và ảnh tự chọn tải lên: thu thập; chức năng ứng dụng. Đóng góp ảnh là tùy chọn. |
| App activity → Other user-generated content | Tên/mô tả ảnh, tên người góp, năm/giai đoạn và nội dung tự nhập: thu thập; chức năng ứng dụng. |
| App activity → App interactions | Thao tác lưu ảnh, liên kết gửi ảnh và thời điểm hoạt động lưu trên máy chủ: thu thập; chức năng ứng dụng. Không có SDK phân tích hành vi được tìm thấy, nhưng vẫn có hoạt động được lưu cho tính năng. |
| Location → Precise location | Tệp tải lên có thể giữ GPS EXIF. Chưa nên chọn "không thu thập vị trí" chỉ vì không có permission. Cần xác minh upload thực tế và công bố nhóm vị trí tương ứng nếu truyền/lưu tọa độ người dùng/thiết bị; hoặc triển khai loại bỏ metadata rồi kiểm chứng trước khi đổi khai báo. |
| Dữ liệu kỹ thuật của SDK/hạ tầng | Firebase Auth nhận IP, user-agent, device metadata và Firebase App ID. Rà theo tài liệu SDK/cấu hình thật; không tự gọi Firebase UID là device ID hoặc tự khai IP là vị trí khi không có suy ra vị trí. Nếu SDK/hạ tầng suy ra vị trí từ IP hoặc tạo ID theo cài đặt, cần khai nhóm tương ứng. |

Đối với chia sẻ dữ liệu, Google phân biệt dịch vụ xử lý thay nhà phát triển
với bên dùng dữ liệu cho mục đích riêng. Việc chuyển đến Firebase/Cloudinary
cần được công bố trong chính sách dù có thể thuộc ngoại lệ "service provider"
trong biểu mẫu. Ngoại lệ hành động do người dùng chủ động cũng yêu cầu sự chia
sẻ phù hợp kỳ vọng hoặc công bố/đồng ý nổi bật. Không tự chọn "không chia sẻ"
cho email công khai khi luồng hiện tại chưa giải thích và xin đồng ý về việc
công bố email. Nếu không áp dụng được ngoại lệ, khai **shared** cho các trường
công khai tương ứng, hoặc thay đổi hành vi sản phẩm và kiểm chứng trước.

Tên/email/ID/ảnh/lượt lưu không phải dữ liệu chỉ xử lý tức thời: hệ thống lưu
chúng. Phương thức tạo tài khoản hiện là **OAuth/Google**. Câu trả lời "mã hóa
khi truyền" cần căn cứ vào bản phát hành HTTPS và lưu lượng SDK/hạ tầng thực
tế; cấu hình dự kiến không đủ chứng minh. Xóa tài khoản có đường trong app,
nhưng URL ngoài và quy trình hỗ trợ/Firebase/Cloudinary phải hoạt động trước
khi tuyên bố hoàn tất yêu cầu xóa.

## Đưa lên GitHub Pages

Để chỉ công bố các trang này, sao chép `index.html` và `delete-account.html`
sang repository dành riêng cho trang, giữ hai tệp cạnh nhau. Bật Pages cho
nhánh/thư mục chứa hai tệp và bật **Enforce HTTPS**. Ví dụ URL sau khi deploy:

- Chính sách: `https://<tai-khoan>.github.io/<repo>/`.
- Xóa tài khoản: `https://<tai-khoan>.github.io/<repo>/delete-account.html`.

Dùng URL trang HTML thực tế, không dùng URL `github.com/.../blob/...` hoặc
`raw.githubusercontent.com`. Không cần publish README bằng chứng này như
chính sách công khai. Nếu dùng Pages của repository hiện tại từ `/docs`, URL
chính sách sẽ có thêm `/privacy/`.

Trước khi nộp:

1. Hoàn thiện mọi chỗ điền và bỏ nhãn bản nháp sau khi các cam kết hoạt động.
2. Mở hai URL HTTPS ở cửa sổ riêng tư, không đăng nhập; kiểm tra trang HTML,
   liên kết email, SSL và truy cập công khai. Khả năng truy cập toàn cầu cần
   kiểm tra thực tế; chưa được xác minh ở lần rà này.
3. Cấu hình URL chính sách trên Play Console, bổ sung đường dẫn hoặc văn bản
   chính sách trong ứng dụng rồi build lại AAB. Trang HTML chưa tự thêm vào app.
4. Cấu hình URL yêu cầu xóa ở phần Data Safety/account deletion; kiểm tra hộp
   thư bằng một yêu cầu thực tế và hoàn tất xóa Phoenix, Firebase, Cloudinary,
   cache/backup theo quy trình đã công bố.
5. Chốt dữ liệu công khai (nhất là email), metadata ảnh, cấu hình SDK, độ tuổi
   đối tượng và câu trả lời Data Safety theo bản phát hành thực tế.

## Nguồn chính thức đã đối chiếu

- [Google Play: dữ liệu người dùng và chính sách quyền riêng tư](https://support.google.com/googleplay/android-developer/answer/10144311?hl=vi).
- [Google Play: Data Safety, nhóm dữ liệu và ngoại lệ sharing](https://support.google.com/googleplay/android-developer/answer/10787469?hl=en).
- [Google Play: yêu cầu xóa tài khoản và nguồn yêu cầu ngoài app](https://support.google.com/googleplay/android-developer/answer/13327111?hl=vi).
- [Firebase: dữ liệu SDK Android tự thu thập](https://firebase.google.com/docs/android/play-data-disclosure).
- [Firebase: quyền riêng tư và thời gian lưu dữ liệu](https://firebase.google.com/support/privacy).
- [Cloudinary: quyền riêng tư](https://cloudinary.com/privacy).
- [GitHub Pages: HTTPS](https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https).

Kiểm tra ở lần soạn này gồm mã nguồn, permissions trong AAB, cấu hình release
và cấu trúc/liên kết cục bộ của hai trang. Không thay đổi Flutter/Phoenix,
không chạy application tests cho thay đổi tài liệu, không triển khai Pages,
không gửi Play Console và chưa kiểm chứng xóa Firebase/Cloudinary hoặc thời
hạn log/backup trên production.
