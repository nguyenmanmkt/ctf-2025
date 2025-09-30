# 🗓 Lộ trình cấp tốc CTF Web (4 tuần)

## Tuần 1 — Nền tảng cơ bản  
**Mục tiêu:** Hiểu cách hoạt động của web & làm quen công cụ cơ bản.

**Nội dung**
- HTTP request/response, cookie, session, header  
- Cách đọc source HTML, JS cơ bản  
- Cài & làm quen: Burp Suite, Postman, DevTools (F12)

**Thực hành**
- TryHackMe / PortSwigger Web Academy → module *HTTP Basics*  
- Giải challenge easy kiểu “View source”, `robots.txt`, hidden parameter

---

## Tuần 2 — Lỗ hổng Web phổ biến (Easy → Medium)  
**Mục tiêu:** Thành thạo các lỗi web hay gặp trong CTF.

**Nội dung**
- XSS (stored, reflected, DOM-based)  
- SQL Injection (error-based, union-based, blind)  
- File Inclusion: LFI / RFI

**Thực hành**
- Lab PortSwigger (XSS + SQLi)  
- Root-Me → Web – Client/Server (easy)  
- Một số bài CTFd như PicoCTF Web (SQLi / XSS cơ bản)

---

## Tuần 3 — Lỗ hổng Web nâng cao (Medium → Hard)  
**Mục tiêu:** Tiếp cận lỗi khó hơn & kết hợp nhiều kỹ năng.

**Nội dung**
- Command Injection & bypass file upload  
- SSRF (Server-Side Request Forgery)  
- Deserialization / IDOR  
- Authentication bypass / Logic bug

**Thực hành**
- PortSwigger Advanced Labs (SSRF, Upload)  
- CTFd bài medium/hard → HackTheBox Web challenges

---

## Tuần 4 — CTF Practice + Teamwork  
**Mục tiêu:** Làm quen thi thực tế, rèn teamwork.

**Nội dung**
- Phân chia vai trò trong team:
  1. Recon & fuzzing (dirsearch, gobuster, ffuf)  
  2. Payload & exploitation (sqlmap, Burp, script nhỏ)  
  3. Đọc hint, quản lý notes/writeup, thử nhiều hướng
- Tập giải set CTF mini (PicoCTF, CTFlearn, một số đề VN)  
- Viết writeup ngắn gọn sau mỗi bài

---

## 🔧 Công cụ cần làm quen
- **Fuzzing:** `ffuf`, `dirsearch`, `gobuster`  
- **Exploitation:** Burp Suite (Intruder, Repeater), `sqlmap`  
- **Note & Teamwork:** Obsidian / Notion / Google Docs (ghi flag & payload)

---

## 🎯 Kết quả sau 1 tháng
- Team nắm chắc web cơ bản, giải được ~70–80% bài *easy* + *medium*  
- Biết phân vai trong CTF  
- Có kho writeup riêng để ôn thi

---
