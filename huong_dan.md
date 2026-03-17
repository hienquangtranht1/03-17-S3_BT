# Hướng dẫn test API và chụp ảnh RS256 trên Postman

Dưới đây là các bước chi tiết để bạn tự thực hiện trên Postman và **chụp lại màn hình** để nộp bài:

## 1. Chụp ảnh chức năng Login bằng Postman (RS256)
- **Bước 1:** Mở ứng dụng Postman, tạo một Request mới.
- **Bước 2:** Đổi phương thức từ `GET` thành `POST`.
- **Bước 3:** Nhập URL: `http://localhost:3000/api/v1/auth/login` (đảm bảo server node.js đang chạy bằng lệnh `npm start`).
- **Bước 4:** Chọn tab **Body**, chọn **raw** và chọn định dạng **JSON**.
- **Bước 5:** Dán nội dung sau vào ô trống:
  ```json
  {
      "username": "admin",
      "password": "123"
  }
  ```
  *(Lưu ý bạn có thể dùng tài khoản nào đã tạo trong database của bạn).*
- **Bước 6:** Bấm nút **Send**. 
- **Bước 7:** Quan sát hộp phản hồi (Response) phía dưới, nó sẽ hiện ra một đoạn text dài (đó là JWT được mã hóa bằng RS256).
👉 **CHỤP ẢNH 1:** Chụp toàn bộ giao diện Postman lúc này (chứa cả URL, Body request và Token response) để chứng minh login thành công ra token.

## 2. Chụp ảnh chức năng /me (Xác thực RS256)
- **Bước 1:** Vẫn ở Postman, tạo một Request MỚI (hoặc sửa request cũ).
- **Bước 2:** Phương thức chọn là `GET`.
- **Bước 3:** Nhập URL: `http://localhost:3000/api/v1/auth/me`.
- **Bước 4:** Chọn tab **Cookies** (tuỳ chọn) hoặc tab **Headers** để kiểm tra (Tuy nhiên Postman bản mới đã tự động lưu Cookie `LOGIN_NNPTUD_S3` từ lúc login ở trên, nên bạn chỉ cần bấm Send luôn).
- **Bước 5:** Bấm nút **Send**.
- **Bước 6:** Khung Response lúc này sẽ trả về dữ liệu của người dùng login (chứa `_id`, `username`, `email`...).
👉 **CHỤP ẢNH 2:** Chụp toàn bộ giao diện Postman lúc này để chứng minh đã có thể truy cập `/me` đọc được token.


## 4. Test chức năng Change Password
- **Method:** `POST`
- **URL:** `http://localhost:3000/api/v1/auth/changepassword`
- **Headers:** Cookie sẽ tự động gửi kèm nhờ Postman (yêu cầu đăng nhập).
- **Body (raw JSON):**
  ```json
  {
      "oldpassword": "123",
      "newpassword": "newpassword123"
  }
  ```
- **Xác thực logic (Validate):**
  - Nếu `newpassword` nhắn hơn 6 ký tự: Nhận về lõi 400 `mat khau moi phai tu 6 ky tu tro len`.
  - Nếu `oldpassword` không đúng: Nhận mã 403 `mat khau cu khong dung`.
  - Thành công: API phản hồi `doi mat khau thanh cong` và hash mật khẩu được cập nhật trong MongoDB.

*Việc cuối cùng bạn cần làm là truy cập Postman thực hiện lại các endpoint trên và tự chụp ảnh lưu lại theo mong muốn của đề bài.*
