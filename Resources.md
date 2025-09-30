# 🔗 Resources — Tài nguyên học theo Daily Plan (CTF Web Bootcamp)

## Tổng quan / đọc nền tảng
- **PortSwigger — Web Security Academy** — kho lab tương tác, lý thuyết + hands-on phù hợp Week 1–3. :contentReference[oaicite:0]{index=0}  
- **OWASP Juice Shop (GitHub)** — app giả mạo có sẵn nhiều lỗ hổng (dùng làm lab offline / CTF). :contentReference[oaicite:1]{index=1}  
- **OWASP Top Ten (tham khảo lỗi phổ biến)** — đọc song song để hiểu danh sách lỗ hổng cần biết. (Tìm trên owasp.org)

## Beginner CTF platforms (dành cho Week 1–2 practice)
- **picoCTF** — bài tập/CTF dành cho sinh viên & beginner (tuyệt vời cho Week 1–2). :contentReference[oaicite:2]{index=2}  
- **CTFlearn** — tập hợp challenge nhiều mức độ, beginner-friendly. :contentReference[oaicite:3]{index=3}  
- **TryHackMe — Web rooms** — các room hướng dẫn có host sẵn, thích hợp self-paced labs. :contentReference[oaicite:4]{index=4}

## Tools — cài & làm quen (dùng xuyên suốt cả 4 tuần)
- **Burp Suite (Community)** — proxy + Repeater/Intruder để intercept & tinh chỉnh request. (Download & docs). :contentReference[oaicite:5]{index=5}  
- **sqlmap** — tool tự động test & exploit SQLi (dùng cho Week 2 SQLi labs). :contentReference[oaicite:6]{index=6}  
- **ffuf** — fast web fuzzer cho fuzzing / dir brute (Week 4 recon). :contentReference[oaicite:7]{index=7}  
- **dirsearch / gobuster** — directory brute tools (recon). (Tìm trên GitHub / package repo).  
- **wordlists** — sử dụng wordlists phổ biến (SecLists / custom lists). :contentReference[oaicite:8]{index=8}

## Advanced & specialized learning (Week 3)
- **PortSwigger advanced labs / training** — SSRF, file upload, advanced XSS, etc. :contentReference[oaicite:9]{index=9}  
- **Juice Shop CTF tooling** — dùng Juice Shop khi muốn deploy lab offline / self-hosted CTF. :contentReference[oaicite:10]{index=10}

## How to map resources → Daily Plan
- **Tuần 1 (HTTP, DevTools, Cookie/Session)**  
  - Read: PortSwigger HTTP basics. :contentReference[oaicite:11]{index=11}  
  - Practice: TryHackMe Web Security Essentials (intro room). :contentReference[oaicite:12]{index=12}

- **Tuần 2 (XSS, SQLi)**  
  - Learn & Labs: PortSwigger XSS + SQLi labs (do từng lab, copy payloads into writeup). :contentReference[oaicite:13]{index=13}  
  - Extra Practice: picoCTF & CTFlearn web XSS/SQLi tasks. :contentReference[oaicite:14]{index=14}  
  - Tooling: cài `sqlmap` để học cách tự động hóa discovery/extraction. :contentReference[oaicite:15]{index=15}

- **Tuần 3 (LFI, File Upload, Command Injection, SSRF)**  
  - PortSwigger advanced labs (SSRF, file upload). :contentReference[oaicite:16]{index=16}  
  - Juice Shop — deploy local instance, chơi các challenge tương ứng. :contentReference[oaicite:17]{index=17}

- **Tuần 4 (Recon, Fuzzing, Mini-CTF)**  
  - Recon tools: `ffuf` (wordlists), `dirsearch`/`gobuster`. :contentReference[oaicite:18]{index=18}  
  - Mini-CTF: lấy challenge từ PicoCTF / CTFlearn / Juice Shop → chấm điểm theo plan. :contentReference[oaicite:19]{index=19}

## Quick install / get-started commands (suggested)
- Burp Suite Community: download & cài theo hướng dẫn (PortSwigger). :contentReference[oaicite:20]{index=20}  
- sqlmap: `git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git` → `python3 sqlmap/sqlmap.py -h`. :contentReference[oaicite:21]{index=21}  
- ffuf: `go install github.com/ffuf/ffuf/v2@latest` (hoặc cài binary từ releases). :contentReference[oaicite:22]{index=22}  
- Juice Shop: `git clone https://github.com/juice-shop/juice-shop.git` → deploy theo README. :contentReference[oaicite:23]{index=23}

## Suggested reading / videos (extra)
- PortSwigger research & Web Security Academy videos — dễ hiểu, cập nhật. :contentReference[oaicite:24]{index=24}  
- Blog posts / writeups từ CTFlearn / Medium — tham khảo cách viết writeup & PoC. :contentReference[oaicite:25]{index=25}

---

## Gợi ý push lên GitHub
- Tạo `resources.md` trong repo, copy phần trên.  
- Trong README: link tới `resources.md` theo từng tuần (Week 1 → link PortSwigger lab cụ thể, Week 2 → link PicoCTF tasks, v.v.).  
- Thêm phần **How to run Juice Shop locally** (kèm docker command) để sinh viên có môi trường offline.

