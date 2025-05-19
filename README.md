# Cập nhật Packages An Toàn Với NPM Check Updates

Bạn mở lại dự án cũ, `npm install`, và rồi: 200+ warnings, vài chục vulnerabilities, kèm theo cảm giác "ôi thôi...".
Tin tốt là có cách để xử lý chúng gọn gàng.
Còn Tin xấu? Là nếu bạn bỏ qua, lỗi sẽ rình rập từ những nơi bạn không ngờ tới. Dù vẫn chạy được, nhưng việc sử dụng các phiên bản lỗi thời có thể là một rủi ro tiềm tàng về bảo mật và hiệu năng. Việc cập nhật các gói đúng cách không chỉ giúp dự án ổn định hơn, mà còn tiết kiệm thời gian sửa lỗi về sau. [NPM Check Updates](https://www.npmjs.com/package/npm-check-updates) (ncu) là một công cụ dòng lệnh (CLI) giúp bạn cập nhật các dependencies trong `package.json` **một cách an toàn và có kiểm soát**.

Trong bài viết này, tôi sẽ chia sẻ quy trình 5 bước để cập nhật dependencies mà không phá vỡ codebase, cùng những lưu ý quan trọng khi áp dụng `ncu` vào dự án thực tế.

## 1. Cài đặt NPM Check Updates

Thông thường, cách tốt nhất là cài đặt NPM check updates toàn cục. (Bạn cũng có thể chạy nó bằng NPX nếu muốn).

```bash
npm install -g npm-check-updates
```

## 2. Run NPM Check Updates

Di chuyển đến thư mục chứa dự án của bạn bằng lệnh `cd` và thực hiện chạy lệnh sau:

```bash
npx ncu
```

Lệnh này sẽ liệt kê những gói nào trong package.json có phiên bản mới hơn. Kết quả hiển thị sẽ có dạng như sau:

```bash
[====================] 23/23 100%

 @rollup/plugin-commonjs            ^26.0.1  →   ^28.0.3
 @types/chrome                     ^0.0.268  →  ^0.0.323
 @types/react                      ^18.2.66  →   ^19.1.4
 @types/react-dom                  ^18.2.22  →   ^19.1.5
 @typescript-eslint/eslint-plugin    ^7.2.0  →   ^8.32.1
 @typescript-eslint/parser           ^7.2.0  →   ^8.32.1
 @vitejs/plugin-react-swc            ^3.5.0  →    ^3.9.0
 autoprefixer                      ^10.4.19  →  ^10.4.21
 browserify                         ^17.0.0  →   ^17.0.1
 daisyui                            ^4.11.1  →   ^5.0.35
 eslint                             ^8.57.0  →   ^9.27.0
 eslint-plugin-react-hooks           ^4.6.0  →    ^5.2.0
 eslint-plugin-react-refresh         ^0.4.6  →   ^0.4.20
 postcss                            ^8.4.38  →    ^8.5.3
 react                              ^18.2.0  →   ^19.1.0
 react-daisyui                       ^5.0.0  →    ^5.0.5
 react-dom                          ^18.2.0  →   ^19.1.0
 react-router-dom                   ^6.23.1  →    ^7.6.0
 tailwindcss                         ^3.4.3  →    ^4.1.7
 typescript                          ^5.2.2  →    ^5.8.3
 uuid                                ^9.0.1  →   ^11.1.0
 vite                                ^5.2.0  →    ^6.3.5
 vite-tsconfig-paths                 ^4.3.2  →    ^5.1.4

 ```

Phiên bản hiện tại được hiển thị bên trái và phiên bản mới nhất ở bên phải. NPM tuân thủ quy tắc phiên bản ngữ nghĩa `(semantic versioning)`, giúp chúng ta nhanh chóng xác định các bản vá, cập nhật nhỏ hoặc cập nhật lớn cần xử lý.

🔍 Lưu ý về Quy tắc Phiên bản Ngữ nghĩa (Semantic Versioning):

Ý nghĩa từng phần:

- **PATCH** (số bên phải):  
  👉 Bản vá lỗi — sửa lỗi nhỏ, không ảnh hưởng đến tính năng hiện có.

- **MINOR** (số ở giữa):  
  👉 Tính năng mới — được thêm vào mà vẫn **tương thích ngược** (không phá vỡ code cũ).

- **MAJOR** (số bên trái):  
  👉 Thay đổi lớn — có thể **gây xung đột**, không tương thích với phiên bản trước.

---

> [!WARNING]
> Khi cập nhật phiên bản, bạn nên đặc biệt cẩn thận với thay đổi ở phần **MAJOR**, vì chúng có thể làm hỏng ứng dụng nếu không kiểm tra kỹ.

## 3. Update Patches

Trước tiên, chúng ta sẽ cập nhật **tất cả các bản vá (patches)**.  
Giả sử các tác giả gói tuân thủ quy tắc **Semantic Versioning**, thì việc cập nhật này **sẽ không gây ra lỗi** gì nghiêm trọng.

```bash
npx ncu -u -t patch
```

Sau đó:
1. Chạy lại lệnh cài đặt `npm i` để áp dụng cập nhật.
2. Kiểm tra xem ứng dụng vẫn chạy ổn không.
3. Nếu mọi thứ hoạt động bình thường, chúng ta sẽ commit thay đổi — giúp dễ dàng quay lại (revert) nếu cần thiết trong tương lai.

## 4. Update Minor Versions

Tiếp theo, chúng ta sẽ cập nhật **tất cả các phiên bản phụ (minor versions)**. Giống như bản vá (patch), nếu các tác giả gói tuân thủ **Semantic Versioning**, thì việc cập nhật **phiên bản phụ** cũng **không gây ra lỗi** nghiêm trọng.

```bash
npx ncu -u -t minor
```

Sau đó, chạy lệnh `npm i` để cài đặt các gói mới, kiểm tra xem hệ thống vẫn hoạt động bình thường, và commit các thay đổi (để có thể khôi phục nếu cần).

## 5. Update Major Versions

Cuối cùng, cập nhật tất cả các phiên bản chính. Trước khi cập nhật, bạn cần đọc kỹ tài liệu ghi chú phát hành (release notes) để xem phiên bản mới ảnh hưởng thế nào đến dự án. Sau khi hiểu rõ tác động của cập nhật, hãy cập nhật từng thay đổi lớn trong một commit riêng biệt.

Với NCU, bạn có thể lọc một gói cụ thể bằng cờ --filter hoặc -f. Ví dụ, giả sử chúng ta bắt đầu với gói `tailwindcss`, chạy lệnh:

```bash
npx ncu -u -f tailwindcss
```

Sau đó, chạy lệnh `npm i` để cài đặt các gói vừa được cập nhật. Tiếp theo, kiểm tra xem hệ thống vẫn hoạt động ổn định và tiến hành commit các thay đổi (để dễ dàng khôi phục nếu cần). Khi hoàn tất, tiếp tục chuyển sang nhóm gói tiếp theo và lặp lại quy trình cho từng cấp phiên bản tiếp theo (minor, major).

## Kết luận
Việc cập nhật các gói trong dự án không chỉ giúp chúng ta tận dụng được các tính năng mới mà còn đảm bảo hệ thống luôn an toàn và ổn định hơn nhờ các bản vá lỗi và bản cập nhật bảo mật. Sử dụng `NPM Check Updates` kết hợp với quy trình cập nhật có kiểm soát theo từng cấp phiên bản (patch, minor, major) sẽ giúp bạn giảm thiểu rủi ro phát sinh lỗi khi nâng cấp. Hãy luôn kiểm tra kỹ sau mỗi lần cập nhật, commit các thay đổi riêng biệt để dễ dàng quay lại khi cần, đồng thời đọc kỹ tài liệu phát hành của các gói để hiểu rõ những thay đổi quan trọng.

Chúc bạn thành công trong việc duy trì và phát triển dự án một cách an toàn, hiệu quả!