# Lý thuyết — HTTP cơ bản, Status Codes, Headers, Cookie & Session

> Tài liệu chuẩn để dán lên GitHub / Notion. Có ví dụ raw HTTP, checklist, và chỗ đặt ảnh (thay bằng ảnh thực tế khi đăng).

---

## Mục lục
1. [HTTP là gì — Request / Response](#http-là-gì---request--response)  
2. [Methods: GET / POST (khác biệt & ví dụ)](#methods-get--post-khác-biệt--ví-dụ)  
3. [Status codes — nhóm & ý nghĩa](#status-codes---nhóm--ý-nghĩa)  
4. [Request / Response Headers — các header quan trọng](#request--response-headers---các-header-quan-trọng)  
5. [Cookie — cú pháp & flags](#cookie---cú-pháp--flags)  
6. [Session — server-side state & session ID](#session---server-side-state--session-id)  
7. [Ví dụ thực tế: flow login và debug bằng intercept](#ví-dụ-thực-tế-flow-login-và-debug-bằng-intercept)  
8. [Tóm tắt nhanh — những điểm phải nhớ](#tóm-tắt-nhanh---những-điểm-phải-nhớ)  
9. [Checklist / Cheatsheet nhanh](#checklist--cheatsheet-nhanh)

---

# HTTP là gì — Request / Response

**HTTP (HyperText Transfer Protocol)** là giao thức cho client (trình duyệt, mobile app) và server trao đổi dữ liệu theo mô hình *request → response*.  
Mặc định HTTP là **stateless** (server không nhớ trạng thái giữa các request), vì vậy cần cơ chế như *cookies* / *sessions* để duy trì trạng thái.

**Chu trình cơ bản:**
1. Client gửi **Request** (method, path, headers, optional body).  
2. Server xử lý → trả **Response** (status line, headers, body).  
3. Kết nối có thể đóng hoặc tái sử dụng (keep-alive).

**Ví dụ raw HTTP (GET):**
```http
Request:
GET /index.html HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html

Response:
HTTP/1.1 200 OK
Date: Thu, 01 Jan 2025 12:00:00 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 1256

<html> ... </html>
```

---

# Methods: GET / POST (khác biệt & ví dụ)

- **GET**
  - Mục đích: lấy resource (không thay đổi state trên server).  
  - Tham số nằm trong query string (`?q=term`).  
  - Ví dụ: `GET /search?q=burp HTTP/1.1`

- **POST**
  - Mục đích: gửi dữ liệu tới server (tạo/update resource, submit form, login).  
  - Body có thể là `application/x-www-form-urlencoded`, `multipart/form-data`, hoặc `application/json`.  
  - Ví dụ: form login:
```http
POST /login HTTP/1.1
Host: target.lab
Content-Type: application/x-www-form-urlencoded
Content-Length: 35

username=alice&password=secret123
```

**Ghi nhớ**: Không dùng GET cho thao tác làm thay đổi dữ liệu nhạy cảm (ví dụ login, update) vì query string dễ lộ trong logs/history.

---

# Status codes — nhóm & ý nghĩa

Status code là số 3 chữ số ở đầu response status line. Chữ số đầu xác định nhóm:

- **1xx** — Informational (hiếm dùng).  
- **2xx** — Success  
  - `200 OK` — yêu cầu thành công.  
  - `201 Created` — resource được tạo.  
  - `204 No Content` — thành công nhưng không có body.
- **3xx** — Redirection  
  - `301 Moved Permanently`, `302 Found` — redirect, client thường theo `Location`.
- **4xx** — Client error  
  - `400 Bad Request`, `401 Unauthorized` (chưa xác thực), `403 Forbidden` (bị cấm), `404 Not Found`.
- **5xx** — Server error  
  - `500 Internal Server Error`, `503 Service Unavailable`.

**Ví dụ: redirect sau login thành công**
```http
HTTP/1.1 302 Found
Location: /dashboard
Set-Cookie: session=xyz123; HttpOnly; Path=/; Secure
```

---

# Request / Response Headers — các header quan trọng

### Request headers (client → server)
- `Host`: tên host (bắt buộc HTTP/1.1).  
- `User-Agent`: thông tin client.  
- `Accept`: kiểu content client chấp nhận.  
- `Referer`: trang gốc (đôi khi server kiểm tra).  
- `Origin`: gốc gửi request (quan trọng với CORS/CSRF).  
- `Cookie`: cookie kèm theo.  
- `Content-Type`: kiểu dữ liệu body (`application/json`, `application/x-www-form-urlencoded`).  
- `Authorization`: token/bearer.

### Response headers (server → client)
- `Content-Type`: kiểu dữ liệu trả về.  
- `Content-Length`: độ dài body.  
- `Set-Cookie`: chỉ định cookie mới.  
- `Location`: dùng khi redirect (3xx).  
- `WWW-Authenticate`: dùng cho auth challenges.  
- `Cache-Control`, `Expires`: chính sách cache.  
- `X-Frame-Options`, `Content-Security-Policy`: header bảo mật bổ sung.

**Lưu ý debug login**: thường cần kiểm tra `Content-Type`, `Referer`/`Origin`, và `Cookie`/`Set-Cookie`. Nếu server validate `Origin`/`Referer` và bạn replay request mà không có, request có thể bị từ chối.

---

# Cookie — cú pháp & flags

Server gửi cookie bằng header `Set-Cookie`. Browser lưu và gửi lại cookie trong header `Cookie` cho các request tương lai.

**Ví dụ Set-Cookie:**
```http
Set-Cookie: session=xyz123; Path=/; HttpOnly; Secure; SameSite=Strict; Max-Age=3600
```

**Các flag quan trọng**
- `HttpOnly` — JS trên client không đọc được (giảm rủi ro XSS đánh cắp cookie).  
- `Secure` — cookie chỉ gửi trên kết nối HTTPS.  
- `SameSite` — `Strict` / `Lax` / `None`: kiểm soát gửi cookie trong các request cross-site (ảnh hưởng CSRF).  
- `Path`, `Domain` — phạm vi cookie.  
- `Max-Age` / `Expires` — thời hạn tồn tại.

**Cơ chế hoạt động**:
1. Sau login thành công server trả `Set-Cookie: session=...`.  
2. Trình duyệt tự kèm `Cookie: session=...` trong request tiếp theo tới cùng domain/path.

---

# Session — server-side state & session ID

Vì HTTP là stateless, server lưu **session** (object) để giữ trạng thái người dùng, và liên kết session đó bằng **session ID** đặt trong cookie.

**Flow điển hình:**
1. Client `POST /login` với credentials.  
2. Nếu đúng, server tạo session lưu server-side (ví dụ: Redis) — `{session_id: {user_id: 42, roles: [...]}}`.  
3. Server trả `Set-Cookie: session=abc123; HttpOnly; Secure`.  
4. Client gửi `Cookie: session=abc123` cho các request sau → server lookup session → xác định user.

**Bảo mật session**
- Luôn validate session server-side (đừng tin cookie raw).  
- Dùng `HttpOnly`, `Secure`.  
- Expire / rotate session ID khi cần (logout, privilege change).

---

# Ví dụ thực tế: flow login và debug bằng intercept

### Flow chuẩn:
1. GET `/login` → server trả HTML form, có thể kèm CSRF token hidden.  
2. Client submit form (POST `/login`) kèm `username`, `password`, `csrf_token`.  
3. Server validate CSRF + credentials.  
   - Nếu token sai/hết hạn → `403` hoặc `401`.  
   - Nếu cred sai → `401`.  
   - Nếu OK → tạo session + `302` redirect + `Set-Cookie: session=...`.

### Raw request/response (ví dụ)
**Request:**
```http
POST /login HTTP/1.1
Host: target.lab
Content-Type: application/x-www-form-urlencoded
Content-Length: 58
Referer: https://target.lab/login

username=alice&password=secret123&csrf=abcd1234
```

**Response (thành công):**
```http
HTTP/1.1 302 Found
Location: /dashboard
Set-Cookie: session=xyz123; HttpOnly; Path=/; Secure
```

**Response (csrf thiếu/không đúng):**
```http
HTTP/1.1 403 Forbidden
Content-Type: text/html

<html>Forbidden - invalid CSRF</html>
```

**Khi debug bằng Burp / proxy**:  
- Bắt request POST, kiểm tra `Content-Type`, có CSRF không, giữ nguyên `Origin`/`Referer` nếu server kiểm tra.  
- Thử sửa body (ví dụ đổi `username=admin`) và forward để quan sát thay đổi (302/401/403).  
- Thử sửa header `Cookie` (tamper session) để kiểm tra server-side session validation.

---

# Tóm tắt nhanh — những điểm phải nhớ

- **GET** lấy dữ liệu; **POST** gửi/ thay đổi.  
- **2xx** thành công, **3xx** redirect, **4xx** lỗi client, **5xx** lỗi server.  
- `Set-Cookie` tạo session id; browser gửi `Cookie` trong các request tiếp theo.  
- `HttpOnly` & `Secure` rất quan trọng để giảm rủi ro.  
- Khi intercept/replay, luôn kiểm tra `Origin`/`Referer`/`CSRF` — những thứ này thường gây replay thất bại.

---

# Checklist / Cheatsheet nhanh (copy-paste)

```text
— Trước khi intercept:
  * Proxy: 127.0.0.1:8080 (Burp default)
  * Cài Burp CA vào browser test (chỉ profile test)
— Khi intercept request login:
  * Xem Method: GET/POST?
  * Content-Type?
  * Body: form fields / JSON?
  * Headers: Origin / Referer / Cookie / Authorization
  * Có CSRF hidden field không?
— Sau forward:
  * Status code: 200/302/401/403?
  * Response headers: Set-Cookie? Location?
  * Nếu replay thất bại: check CSRF token (lấy token mới bằng GET form)
```

---

## Hình ảnh (gợi ý chèn)
> Thay `images/...` bằng đường dẫn ảnh bạn muốn dùng trên GitHub / Notion.

- Luồng Request/Response:  
  `![HTTP flow](images/http_flow.png)`

- Cheat-sheet Status Codes (infographic):  
  `![Status codes](images/status_codes.png)`

- Luồng Cookie & Session:  
  `![Cookie session flow](images/cookie_session.png)`

---

Mình đã soạn file Markdown này sẵn; mở file trên canvas để xem/ chỉnh sửa. Nếu muốn, mình sẽ:
- Thêm ảnh mẫu vào thư mục `images/` (mình sẽ chèn link ảnh).
- Xuất file `.md` để bạn tải về.

