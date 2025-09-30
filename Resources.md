# 🔗 Resources — Tài nguyên học theo Daily Plan (CTF Web Bootcamp)

## Tổng quan / nền tảng
- [PortSwigger — Web Security Academy](https://portswigger.net/web-security) — kho lab tương tác, lý thuyết + hands-on (rất phù hợp Week 1–3).  
- [OWASP Juice Shop (GitHub)](https://github.com/OWASP/juice-shop) — ứng dụng dễ deploy chứa nhiều lỗ hổng để practice offline/self-hosted.  
- [OWASP Top Ten](https://owasp.org/www-project-top-ten/) — danh sách 10 lỗ hổng web phổ biến, nên đọc song song.

## Beginner CTF platforms (dành cho Week 1–2 practice)
- [picoCTF](https://picoctf.org) — CTF dành cho sinh viên & beginner, nhiều challenge Web cơ bản.  
- [CTFlearn](https://ctflearn.com) — tập hợp challenge nhiều mức độ, beginner-friendly.  
- [TryHackMe](https://tryhackme.com) — rooms & guided tasks; có nhiều bài Web cho người mới.

## Tools — cài & làm quen (dùng xuyên suốt cả 4 tuần)
- [Burp Suite (Community)](https://portswigger.net/burp/communitydownload) — proxy + Repeater/Intruder để intercept & tinh chỉnh request.  
- [sqlmap (GitHub)](https://github.com/sqlmapproject/sqlmap) — tool tự động test & exploit SQLi.  
- [ffuf (GitHub)](https://github.com/ffuf/ffuf) — fast web fuzzer cho fuzzing / dir brute.  
- [dirsearch (GitHub)](https://github.com/maurosoria/dirsearch) — directory brute force scanner.  
- [gobuster (GitHub)](https://github.com/OJ/gobuster) — tool brute-forcing directories/files.  
- [SecLists (wordlists)](https://github.com/danielmiessler/SecLists) — tập hợp wordlist hay dùng cho fuzzing/bruteforce.

## Advanced & specialized learning (Week 3)
- PortSwigger advanced labs (xem trực tiếp trong Web Security Academy — các bài SSRF, file upload, advanced XSS).  
- Juice Shop — deploy local instance để làm lab thực tế và custom challenges.

## How to map resources → Daily Plan (gợi ý)
- **Tuần 1 (HTTP, DevTools, Cookie/Session)**  
  - Read: PortSwigger HTTP basics.  
  - Practice: TryHackMe Web Security Essentials rooms.

- **Tuần 2 (XSS, SQLi)**  
  - Labs: PortSwigger XSS + SQLi.  
  - Extra: picoCTF & CTFlearn XSS/SQLi tasks.  
  - Tooling: dùng `sqlmap` để học tự động hóa discovery/extraction.

- **Tuần 3 (LFI, File Upload, Command Injection, SSRF)**  
  - Labs: PortSwigger advanced labs + Juice Shop exercises.

- **Tuần 4 (Recon, Fuzzing, Mini-CTF)**  
  - Tools: `ffuf`, `dirsearch`, `gobuster` + SecLists.  
  - Mini-CTF: lấy challenge từ picoCTF / CTFlearn / Juice Shop.

## Quick start — Tóm tắt công cụ & hướng dẫn bắt đầu (mô tả)

Phần này mô tả nhanh mục đích của từng công cụ và gợi ý cách bắt đầu mà không đi sâu vào lệnh cài đặt.

- **Burp Suite (Community)**  
  Công cụ proxy/interception rất phổ biến để quan sát và tinh chỉnh HTTP(S) requests. Dùng để intercept traffic từ trình duyệt, thử payload bằng Repeater, và thực hiện các thử nghiệm thủ công. Bắt đầu bằng cách cài Burp và cấu hình trình duyệt dùng proxy (localhost:8080 thường thấy).

- **sqlmap**  
  Công cụ tự động hoá phát hiện và khai thác SQL Injection. Rất hữu ích để học cách các dạng SQLi (error/union/blind) hoạt động trong practice. Dùng sqlmap trên các target lab được phép để thấy cách khai thác và trích xuất dữ liệu.

- **ffuf**  
  Fast web fuzzer dùng để tìm endpoint/hidden file bằng wordlists. Rất phù hợp cho bước recon (Week 4) khi cần enumerating directory hoặc parameters nhanh.

- **dirsearch / gobuster**  
  Công cụ brute-force đường dẫn và file trên web server. Thường dùng cùng SecLists để tìm các endpoint, admin panels, và file ẩn.

- **SecLists (wordlists)**  
  Tập hợp wordlist phổ biến dùng cho fuzzing, directory brute-force và password lists. Nên giữ một vài danh sách thường dùng (common dirs, extensions, params) để chạy với ffuf/gobuster.

- **Juice Shop**  
  Ứng dụng web giả lập nhiều lỗ hổng, phù hợp deploy local để luyện tập. Dùng khi bạn muốn có môi trường offline/self-hosted cho lab hoặc mini-CTF.

### Môi trường khuyến nghị
- Dùng một máy/VM hoặc WSL (Linux) để cài công cụ, hoặc Docker container để chạy Juice Shop.  
- Dùng browser riêng (profile) cấu hình proxy cho Burp để tránh ảnh hưởng profile chính.  
- Luôn kiểm tra **scope**: chỉ chạy các công cụ trên môi trường lab / target bạn được phép.


