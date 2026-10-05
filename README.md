# 🎓 LUMA Distributed AI Platform — Defense Mastery Hub

> **Interactive Web Applications & Academic Preparation Hub for Image Processing Oral Defense**  
> *สื่อการเรียนรู้เชิงโต้ตอบสำหรับเตรียมสอบปากเปล่าวิชา Image Processing (ครอบคลุมทั้ง Frontend, Backend และ AI Server)*

[![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-Live%20Online-brightgreen?style=flat-square&logo=github)](https://kong19565.github.io/luma-defense-mastery/)
[![Architecture](https://img.shields.io/badge/Architecture-3--Node%20Distributed%20Cluster-blue?style=flat-square)](https://kong19565.github.io/luma-defense-mastery/)
[![License](https://img.shields.io/badge/Academic%20Prep-Oral%20Defense%202026-orange?style=flat-square)](#)

---

## 🌐 เข้าใช้งานระบบออนไลน์ (Live Website)

สามารถเปิดศึกษาและฝึกซ้อมตอบคำถามได้จากทุกอุปกรณ์ (มือถือ, แท็บเล็ต, แล็ปท็อป) ผ่าน GitHub Pages:

### 👉 **[https://kong19565.github.io/luma-defense-mastery/](https://kong19565.github.io/luma-defense-mastery/)**

---

## 📑 สรุป 3 โมดูลหลักและเกตเวย์ในเว็บแอปพลิเคชัน

| โหนด / องค์ประกอบ | เทคโนโลยีหลัก | บทบาทและฟังก์ชันเด่นสำหรับซ้อมสอบ | ลิงก์ตรง |
| :--- | :--- | :--- | :---: |
| **🌐 Node 1: Frontend Client** | React 18, Vite, Canvas Inpainting, Zustand, React Query | • Inpaint Canvas Coordinate Transformation Math<br>• Black/White Alpha Mask Generation (#FFFFFF บน #000000)<br>• กรอง LoRA ตามตระกูลสถาปัตยกรรมโมเดลด้วย `families.js`<br>• Optimistic UI & Adaptive Exponential Backoff Polling<br>• ห้องซ้อมสอบปากเปล่า 11 ข้อ | [เปิด Node 1](https://kong19565.github.io/luma-defense-mastery/frontend.html) |
| **⚙️ Node 2: Backend Core** | FastAPI, SQLAlchemy, SQLite WAL, OpenCV, Webhook Receiver | • OpenCV Color Dodge Formula Math Lab: `min(255, (I × 256) / (255 - B + 1))`<br>• Color Splash via HSV Color Space Separation<br>• SQLite WAL Concurrency (ป้องกัน Database Locked)<br>• Multi-Node Webhook Pipeline & Timing Attack Guard<br>• ห้องซ้อมสอบปากเปล่า 11 ข้อ | [เปิด Node 2](https://kong19565.github.io/luma-defense-mastery/backend.html) |
| **🤖 Node 3: AI Inference Server** | WebUI Forge API, Latent Diffusion, WebP Optimizer, PyTorch CUDA | • ซอร์สโค้ดจริง 100% ครบ 39 ฟังก์ชัน (รวม `prompt_builder.py`)<br>• Model Family Compatibility Matrix (`sd15`, `illustrious_xl`, `pony_xl`)<br>• Auto-skip LoRA Guard ป้องกันภาพเสียเมื่อเลือก LoRA ข้ามตระกูล<br>• Dual-mode Task Cancellation (Soft Cancel vs Hard Cancel)<br>• Watchdog Timeout 120s & 3-Tier VRAM Purge (Zero Leak)<br>• ห้องซ้อมสอบปากเปล่า 12 ข้อ | [เปิด Node 3](https://kong19565.github.io/luma-defense-mastery/ai-server.html) |
| **🛡️ DevOps Ingress Gateway** | Nginx Reverse Proxy, PowerShell, Batch Automation | • Reverse Proxy Ingress (Port 80 ➔ Port 7860)<br>• Forwarding กุญแจลับความปลอดภัยด้วย `underscores_in_headers on;`<br>• รองรับ Base64 รูปภาพขนาดใหญ่ `client_max_body_size 50M;`<br>• Timeout ป้องกัน Gateway ตัดสายล่วงหน้า `proxy_read_timeout 300s;` | [เปิด LUMA Hub](https://kong19565.github.io/luma-defense-mastery/) |

---

## ⚡ แผนผังสถาปัตยกรรมมัลติโหนด (Distributed Cluster Flow)

```text
[ Browser Client ]
       │
       ▼ (1. วาด Mask ขาว-ดำ + กรอง LoRA ด้วย families.js + ส่งคำขอพร้อม JWT)
[ Node 1: Frontend (พอร์ต 5173) ]
       │
       ▼ (2. POST /api/generations ข้ามพอร์ต)
[ Node 2: Backend Core (พอร์ต 8000) ] ──> บันทึกงานสถานะ 'queued' ลง SQLite WAL
       │
       ▼ (3. Asynchronous Dispatch ผ่าน Nginx Ingress Gateway พอร์ต 80 พร้อม Header X-LUMA-INTERNAL-SECRET)
[ DevOps Gateway: Nginx Reverse Proxy (พอร์ต 80 ➔ พอร์ต 7860) ]
       │
       ▼ (ส่งต่อคำขอไปยัง FastAPI Wrapper)
[ Node 3: AI Inference Server (พอร์ต 7860) ]
       │
       ├── FIFO Task Queue ──> ล็อก Concurrency = 1 (เซฟ VRAM 8GB)
       ├── Model Family Guard ──> Auto-skip LoRA หากข้ามตระกูล (SD1.5 vs SDXL/Pony)
       ├── เติม Trigger Words & ทำ Deduplication
       ├── รัน Latent Denoising บนการ์ดจอ RTX 3070 ผ่าน WebUI Forge (:7861)
       ├── บีบอัดภาพด้วย libwebp Method 6 (Quality 92) ลดขนาดภาพลง ~80%
       └── ล้าง VRAM ทันทีในบล็อก finally (Three-Tier Memory Purge)
       │
       ▼ (4. ยิง Webhook Callback พร้อม Secret Key กลับไป Node 2)
[ Node 2: Webhook Receiver (/api/callback) ]
       │
       ├── บันทึกไฟล์ภาพ WebP ลง Disk: outputs/<task_id>/output.webp
       └── ปรับสถานะงานในฐานข้อมูลเป็น 'completed' พร้อมบันทึก Actual Seed
       │
       ▼ (5. Adaptive Polling รับผลลัพธ์ไปเรนเดอร์บนหน้าจอ)
[ Node 1: Result Stage Display ]
```

---

## 📦 การใช้งานแบบออฟไลน์ (Offline Standalone ZIP)

หากต้องการนำไปเปิดดูแบบออฟไลน์โดยไม่ต้องเชื่อมต่ออินเทอร์เน็ต สามารถดาวน์โหลดไฟล์ ZIP จากโฟลเดอร์ `downloads/` ในคลังนี้:
* `downloads/LUMA_Frontend_Defense_Mastery.zip`
* `downloads/LUMA_Backend_Defense_Mastery.zip`
* `downloads/LUMA_AI_Server_Defense_Mastery.zip`

แตกไฟล์แล้วดับเบิลคลิกเปิด `index.html` ผ่าน Google Chrome หรือเบราว์เซอร์ใดก็ได้ ใช้งานได้ 100% ทันทีโดยไม่ต้องรันเซิร์ฟเวอร์หรือติดตั้งโปรแกรมเพิ่มเติม

---

## 👨‍💻 ข้อมูลผู้จัดทำ (Author & Academic Credits)

* **สถาบัน:** ภาควิชาวิศวกรรมคอมพิวเตอร์ / มหาวิทยาลัย (ชั้นปีที่ 3 เทอม 1)
* **รายวิชา:** Image Processing (การประมวลผลภาพดิจิทัล)
* **โครงงาน:** LUMA — Multi-Node Distributed Generative AI & Image Editing Platform
* **ผู้จัดทำ:** Apisak Kongphakdee (GitHub: [@Kong19565](https://github.com/Kong19565))
