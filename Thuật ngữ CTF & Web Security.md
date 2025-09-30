# 🔎 Glossary — Thuật ngữ CTF & Web Security (Dành cho sinh viên)

> Bản chú giải ngắn gọn các thuật ngữ thường gặp khi luyện CTF Web.  
> Mỗi mục gồm: **Định nghĩa ngắn**, **Tại sao quan trọng** và **Ví dụ / Ghi chú thực hành**.

---

## Challenge
**Định nghĩa:** Bài toán/hình thức thử thách trong CTF (ví dụ: Web, Crypto, Forensics...).  
**Tại sao quan trọng:** Là đối tượng bạn sẽ giải để lấy flag.  
**Ví dụ:** Một web page có input cho phép chèn script → challenge XSS.

---

## Flag
**Định nghĩa:** Chuỗi ký tự (ví dụ `FLAG{...}`) là "khoản thưởng" khi giải xong challenge.  
**Tại sao quan trọng:** Là bằng chứng bạn đã khai thác thành công lỗ hổng.  
**Ghi chú:** Luôn lưu flag vào writeup cùng với cách thu được.

---

## Payload
**Định nghĩa:** Dữ liệu/chuỗi được gửi vào ứng dụng để khai thác (ví dụ `<script>alert(1)</script>` cho XSS).  
**Tại sao quan trọng:** Payload đúng = exploit thành công.  
**Ví dụ:** Payload SQLi: `' OR '1'='1' -- `

---

## Writeup
**Định nghĩa:** Bài viết mô tả cách bạn giải challenge (recon, steps, payload, flag, root cause, fix).  
**Tại sao quan trọng:** Giúp học lại, chia sẻ kiến thức và ghi lại bằng chứng.  
**Ghi chú:** Viết ngắn, rõ ràng: Steps + payload + flag + remediation.

---

## Recon (Reconnaissance)
**Định nghĩa:** Giai đoạn thu thập thông tin: URLs, parameters, headers, public files.  
**Tại sao quan trọng:** Biết nhiều thông tin giúp tìm lỗ hổng nhanh hơn.  
**Công cụ:** `ffuf`, `dirsearch`, browser DevTools.

---

## Fuzzing
**Định nghĩa:** Tự động gửi nhiều thử nghiệm (requests) vào mục tiêu để tìm đường dẫn/param ẩn hoặc input dễ bị lỗi.  
**Tại sao quan trọng:** Tìm được những endpoint ẩn, file, hoặc input bỏ sót kiểm tra.  
**Công cụ:** `ffuf`, `gobuster`, `burp intruder`.

---

## Burp Suite (Proxy/Repeater/Intruder)
**Định nghĩa:** Bộ công cụ proxy để intercept, chỉnh sửa request, lặp payload.  
**Tại sao quan trọng:** Công cụ chính để thao tác request/response thủ công.  
**Ghi chú:** Dùng Repeater để tune payload, Proxy để intercept.

---

## SQL Injection (SQLi)
**Định nghĩa:** Khai thác input không kiểm soát để chạy câu lệnh SQL tùy ý trên DB.  
**Tại sao quan trọng:** Có thể rò rỉ dữ liệu, lấy flag, thậm chí remote code.  
**Ví dụ:** Error-based, Union-based, Blind (boolean/time-based).  
**Công cụ:** `sqlmap` để tự động.

---

## XSS (Cross-Site Scripting)
**Định nghĩa:** Chèn script vào trang web khiến trình duyệt nạn nhân chạy mã độc.  
**Tại sao quan trọng:** Đánh cắp cookie/session, hiển thị alert (test) hoặc lấy flag.  
**Loại:** Reflected, Stored, DOM-based.  
**Payload test:** `<script>alert(1)</script>`

---

## LFI / RFI (Local/Remote File Inclusion)
**Định nghĩa:** Lấy nội dung file trên server (LFI) hoặc include file từ URL (RFI).  
**Tại sao quan trọng:** Đọc file nhạy cảm (`/etc/passwd`) hoặc thực thi code.  
**Ghi chú:** Kết hợp log poisoning để thực thi shell.

---

## File Upload Vulnerability
**Định nghĩa:** Upload file nhưng kiểm tra không đủ → có thể upload webshell.  
**Tại sao quan trọng:** Có thể đặt script/weaponized file để remote code execution.  
**Ghi chú:** Thử bypass filter bằng đổi extension, MIME, content.

---

## Command Injection
**Định nghĩa:** Input được ghép vào lệnh hệ thống và không được kiểm soát → chạy lệnh tùy ý.  
**Tại sao quan trọng:** Truy cập shell, lấy flag.  
**Ví dụ:** `; ls -la` hoặc `$(whoami)`

---

## Reverse Shell
**Định nghĩa:** Kết nối shell ngược từ target về máy attacker (listener).  
**Tại sao quan trọng:** Cho phép tương tác trực tiếp với hệ thống bị khai thác.  
**Ví dụ listener:** `nc -lvnp 4444`

---

## SSRF (Server-Side Request Forgery)
**Định nghĩa:** Buộc server thực hiện request đến URL nội bộ/ngoại vi (ví dụ metadata service).  
**Tại sao quan trọng:** Lộ thông tin nội bộ, lấy token, tấn công mạng nội bộ.  
**Ví dụ:** `http://169.254.169.254/` (metadata AWS).

---

## IDOR (Insecure Direct Object Reference)
**Định nghĩa:** Truy cập tài nguyên bằng ID mà không kiểm tra quyền (ví dụ `?file_id=123`).  
**Tại sao quan trọng:** Đọc/ghi dữ liệu người khác.  
**Ghi chú:** Thử thay ID để kiểm tra.

---

## Authentication Bypass / Logic Bug
**Định nghĩa:** Lỗi trong logic ứng dụng cho phép vượt qua xác thực hoặc thao tác trái phép.  
**Tại sao quan trọng:** Có thể chiếm quyền tài khoản, tăng quyền.  
**Ví dụ:** Reset token predictable, giá trị isAdmin trong cookie client-side.

---

## Exploit / PoC (Proof of Concept)
**Định nghĩa:** Mã hoặc chuỗi thao tác chứng minh lỗ hổng tồn tại.  
**Tại sao quan trọng:** Bằng chứng để viết writeup hoặc báo cáo.  
**Ghi chú:** PoC nên an toàn, không gây hại hệ thống.

---

## Root / Privilege Escalation
**Định nghĩa:** Lấy quyền cao hơn trên hệ thống (root trên Linux).  
**Tại sao quan trọng:** Toàn quyền hệ thống, có thể lấy mọi file.  
**Ghi chú:** Trong CTF web hiếm nhưng có thể khi shell đã được có.

---

## CVE
**Định nghĩa:** Common Vulnerabilities and Exposures — ID chuẩn cho lỗ hổng công khai.  
**Tại sao quan trọng:** Tham khảo cách exploit/patch.  

---

## Jeopardy vs Attack-Defense
**Jeopardy:** Nhiều challenge theo category, đội làm bài để lấy điểm.  
**Attack-Defense:** Đội vừa tấn công vừa bảo vệ dịch vụ của mình.  
**Ghi chú:** Bootcamp thường luyện Jeopardy Web.

---

## Scope / Rules (Trong CTF)
**Định nghĩa:** Giới hạn hệ thống & luật chơi (cấm tấn công ngoài scope).  
**Tại sao quan trọng:** Tuân thủ pháp luật & đạo đức.  
**Ghi chú:** Luôn tấn công chỉ trong lab/target được phép.

---

## Responsible Disclosure
**Định nghĩa:** Quy trình báo lỗ hổng cho chủ hệ thống một cách có trách nhiệm.  
**Tại sao quan trọng:** Tránh gây hại, giúp patch kịp thời.  

---

## Proxy / Intercept / Repeater
**Proxy (Burp):** Chặn request/response giữa browser và server.  
**Intercept:** Bắt request để chỉnh sửa trước khi gửi.  
**Repeater:** Gửi lại request đã chỉnh nhiều lần để tinh chỉnh payload.

---

## Enumeration
**Định nghĩa:** Liệt kê các mục tiêu khả dĩ (endpoints, params, users).  
**Tại sao quan trọng:** Bước cơ bản để khai thác.  
**Công cụ:** `ffuf`, `dirsearch`, `gobuster`.

---

## Brute-force
**Định nghĩa:** Thử nhiều giá trị (passwords, tokens) tự động.  
**Tại sao quan trọng:** Có thể crack password nhưng tốn thời gian; cẩn trọng (rate-limit).  
**Công cụ:** `hydra`, Burp Intruder.

---

## Fingerprinting
**Định nghĩa:** Nhận diện công nghệ server (ngôn ngữ, framework, version).  
**Tại sao quan trọng:** Giúp chọn payload đúng, biết lỗi hay exploit phù hợp.  
**Công cụ:** `whatweb`, `wappalyzer` extension.

---

## Sanitization / Input Validation / Escape
**Định nghĩa:** Cách xử lý input để loại bỏ/escape kí tự nguy hiểm.  
**Tại sao quan trọng:** Là cách fix lỗ hổng (ví dụ XSS/SQLi).  
**Ghi chú:** Luôn nói đến server-side validation + prepared statements.

---

## Parameter / Endpoint
**Parameter:** Giá trị truyền tới server (`id`, `q`, `file`).  
**Endpoint:** URL cụ thể cung cấp chức năng (`/login`, `/upload`).  
**Ghi chú:** Kiểm tra cả query params, POST body, headers.

---

## Sandbox / Lab
**Định nghĩa:** Môi trường thử nghiệm an toàn (ví dụ JuiceShop, PortSwigger labs).  
**Tại sao quan trọng:** Nơi luyện thực hành mà không phạm pháp.  
**Ghi chú:** Luôn dùng lab được phép.

---

## Score (Điểm)
**Định nghĩa:** Điểm thưởng khi submit đúng flag.  
**Tại sao quan trọng:** Thể hiện hiệu quả thi đấu; thường challenge khó hơn = nhiều điểm hơn.

---

## Misc (Một số từ vựng ngắn)
- **PoC:** Proof of Concept (xem Exploit).  
- **WAF:** Web Application Firewall — có thể chặn payload, cần bypass.  
- **Log Poisoning:** Ghi payload vào log rồi LFI đọc log để thực thi.  
- **Rate-limit:** Giới hạn số request; ảnh hưởng brute-force/fuzzing.  

---
