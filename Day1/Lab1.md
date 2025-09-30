# Thực hành — Day 1 (Burp + Intercept Login)

## Mục lục
1. [Môi trường & chuẩn bị](#môi-trường--chuẩn-bị)
2. [Cấu hình Burp Proxy & trình duyệt (step-by-step)](#cấu-hình-burp-proxy--trình-duyệt-step-by-step)
3. [Bắt và phân tích request login](#bắt-và-phân-tích-request-login)
4. [Thay payload / tamper cookie — ví dụ thực tế](#thay-payload--tamper-cookie--ví-dụ-thực-tế)
5. [Dùng Repeater để thử nghiệm nhiều biến thể](#dùng-repeater-để-thử-nghiệm-nhiều-biến-thể)
6. [Các tình huống thường gặp (CSRF, JSON, XHR/AJAX)](#các-tình-huống-thường-gặp-csrf-json-xhrajax)
7. [Bài tập thực hành (có đáp án & hướng kiểm tra)](#bài-tập-thực-hành-có-đáp-án--hướng-kiểm-tra)

---

## Môi trường & chuẩn bị
- Máy ảo (Windows11/Kali) 
- Burp Suite Community [Download](https://portswigger.net/burp/communitydownload)

---

## Cấu hình Burp Proxy & trình duyệt (step-by-step)

1. **Khởi động Burp**
   - Mở Burp → `Proxy` → `Options` → đảm bảo có listener `127.0.0.1:8080`.

2. **Thiết lập trình duyệt dùng proxy**
   - Dùng FoxyProxy: thêm rule HTTP proxy `127.0.0.1:8080` → bật khi test.
   - Hoặc cấu hình thủ công trong Settings → Proxy → Manual proxy `127.0.0.1:8080`.

3. **Cài Burp CA vào trình duyệt (HTTPS)**
   - Trong trình duyệt (route qua Burp) mở `http://burp` → Download CA certificate.
   - Import CA vào Firefox (Preferences → Certificates → Import) hoặc vào Windows Trusted Root (Chrome).
   - **Chú ý:** chỉ cài CA trên profile test.

---

## Bắt và phân tích request login

1. **Bật Intercept**
   - Burp → Proxy → Intercept → `Intercept is on`.

2. **Trên trình duyệt:** mở trang login lab, nhập credentials mẫu (ví dụ `alice / password123`) → Submit.

3. **Quan sát request bị bắt** (ví dụ một POST form):
```http
POST /login HTTP/1.1
Host: target.lab
User-Agent: ...
Content-Type: application/x-www-form-urlencoded
Content-Length: 58
Referer: https://target.lab/login
Cookie: csrftoken=abcd1234

username=alice&password=password123&csrf=abcd1234
```

4. **Phân tích nhanh:**
- Method: POST hay GET?  
- Content-Type: application/x-www-form-urlencoded / application/json?  
- Headers quan trọng: `Origin`, `Referer`, `Cookie`.  
- Body: tên field (username/password) là gì? Có CSRF token hidden?

---

## Thay payload / tamper cookie — ví dụ thực tế

### 1) Thay username trước khi forward
- Trong Intercept, sửa body:
```
username=admin&password=password123&csrf=abcd1234
```
- Nhấn **Forward**.

**Quan sát:** server trả 302 + `Set-Cookie` nếu thành công; 401/403 nếu không.

### 2) Thay cookie session
- Intercept request tới `/dashboard` (sau login thành công) và sửa header `Cookie`:
```
Cookie: session=malicioussession456
```
- Forward và quan sát server trả 401/403 hoặc redirect về /login.

### 3) Ví dụ raw response (thành công)
```http
HTTP/1.1 302 Found
Location: /dashboard
Set-Cookie: session=xyz123; HttpOnly; Secure; Path=/
```

### 4) Ví dụ raw response (CSRF sai)
```http
HTTP/1.1 403 Forbidden
Content-Type: text/html

<html>Forbidden - invalid CSRF</html>
```

---

## Dùng Repeater để thử nghiệm nhiều biến thể

1. Chọn request trong HTTP history hoặc Intercept → Right-click → **Send to Repeater**.
2. Trong Repeater: sửa nhanh body/headers → nhấn **Send** nhiều lần.
3. So sánh responses ở panel bên phải; dùng **Compare Responses** để thấy khác biệt.

**Khi dùng Repeater:** nhớ refresh (GET form) để lấy CSRF token mới nếu cần.

---

## Các tình huống thường gặp (CSRF, JSON, XHR/AJAX)

### CSRF token (hidden)
- Token có thể là hidden input trong form hoặc nằm trong cookie.
- Nếu replay từ Repeater token cũ thường expired → GET lại form để lấy token mới.

### Login dùng JSON
```http
POST /api/login HTTP/1.1
Content-Type: application/json

{"username":"alice","password":"secret"}
```
- Sửa JSON trong Repeater tương tự.

### AJAX / XHR / fetch
- Mở DevTools → Network → filter XHR để xem.
- Burp vẫn intercept nếu trình duyệt route qua proxy; kiểm tra header `Origin`/`X-Requested-With`.

### SameSite cookie
- Nếu cookie `SameSite=Strict`, cookie sẽ không gửi trong cross-site requests — lưu ý khi test redirect/cross-site.

---

## Bài tập thực hành (có đáp án & hướng kiểm tra)

### Exercise 1 — Intercept & modify credentials (Basic)
**Yêu cầu:** Chặn POST /login, đổi `username=user1` → `username=admin`, forward.
**Mục tiêu:** Quan sát status code, Set-Cookie, Location.

**Hướng kiểm tra / đáp án:**
- Thành công: `HTTP/1.1 302 Found` + `Set-Cookie: session=...` (đi tới /dashboard).
- Thất bại: `401`/`403` hoặc message "Login failed".

---

### Exercise 2 — Tamper session cookie
**Yêu cầu:** Sau login thành công, truy cập /dashboard; chặn request và đổi `Cookie: session=` thành giá trị ngẫu nhiên.
**Mục tiêu:** Kiểm tra server validate session.

**Hướng kiểm tra / đáp án:**
- Kết quả đúng: server trả `401` hoặc redirect về /login.
- Nếu server vẫn trả nội dung dashboard => nghi vấn session không validate (ghi chú & dừng test trên môi trường không cho phép).

---

### Exercise 3 — CSRF replay
**Yêu cầu:** Từ Repeater, gửi POST /login dùng CSRF token cũ.
**Mục tiêu:** Hiểu token expiry.

**Hướng kiểm tra / đáp án:**
- Nếu token expired → `403 Forbidden` (hoặc message tương tự).
- Để thành công: GET lại /login để lấy token mới, chèn token mới vào body rồi gửi.

---

### Exercise 4 — Bypass client-side validation
**Yêu cầu:** Form có JS validate (min length). Thử submit request via Repeater với giá trị ngắn hơn.
**Mục tiêu:** Kiểm tra server-side validation.

**Hướng kiểm tra / đáp án:**
- Nếu server validate → response `400`/`422`/`401`.
- Nếu server không validate → payload accepted (cần note và báo cáo trên môi trường cho phép).

---


