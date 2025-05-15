# Cập nhật Packages An Toàn Với NPM Check Updates

Khi quay lại một dự án web cũ, việc cập nhật các gói (packages) là rất quan trọng để:

- Nhận được các **tính năng mới**
- Áp dụng các **bản sửa lỗi (bug fixes)**
- Vá các **lỗi bảo mật**

[NPM Check Updates](https://www.npmjs.com/package/npm-check-updates) (ncu) là một công cụ dòng lệnh (CLI) giúp bạn cập nhật các dependencies trong `package.json` **một cách an toàn và có kiểm soát**.

Dưới đây là cách tiếp cận chung của tôi khi cần cập nhật một dự án sử dụng `npm`:

## 1. Cài đặt NCU toàn cục

```bash
npm install -g npm-check-updates
