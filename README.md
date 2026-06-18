# 🚀 Mysite2026 (Django Blog Project)

ยินดีต้อนรับสู่โปรเจกต์ **Mysite2026** ซึ่งพัฒนาด้วยโครงสร้างของ Django Web Framework (เวอร์ชัน 5.2.x) ในโปรเจกต์นี้จะประกอบด้วยแอปพลิเคชันหลักคือ **blog** สำหรับจัดการบทความ และหน้าทดสอบระบบ เช่น หน้าเช็ค IP ของผู้ใช้งาน (`/info/`)

---

## 🛠️ คำสั่งการเริ่มต้นทำงาน (Quick Start)

สามารถรันโปรเจกต์บนเครื่องและเปิดเซิร์ฟเวอร์เพื่อให้เข้าถึงจากภายในเครือข่ายด้วยคำสั่งต่อไปนี้:

### 1. การเปิดใช้งาน Virtual Environment
สำหรับ Windows (PowerShell):
```powershell
.\.venv\Scripts\activate
```

### 2. การรันเซิร์ฟเวอร์เพื่อทดสอบ
สามารถเลือกรันผ่านตัวแปรมาตรฐานหรือใช้ `uv` สำหรับประมวลผลด่วน:
* **รันแบบทั่วไป (พอร์ต 80 เพื่อใช้ IP เครื่องตรงๆ):**
  ```bash
  python manage.py runserver 0.0.0.0:80
  ```
* **รันผ่านเครื่องมือ `uv`:**
  ```bash
  uv run manage.py runserver 0.0.0.0:80
  ```

---

## 📂 โครงสร้างโปรเจกต์ที่สำคัญ (Project Structure)

* 📁 **[mysite/](file:///d:/WebDev2026/mysite2026/mysite)** — โฟลเดอร์การตั้งค่าหลักของโปรเจกต์
  * 📄 [settings.py](file:///d:/WebDev2026/mysite2026/mysite/settings.py) — การตั้งค่าระบบ แอปที่ติดตั้ง (`INSTALLED_APPS`), และ `ALLOWED_HOSTS = ["*"]`
  * 📄 [urls.py](file:///d:/WebDev2026/mysite2026/mysite/urls.py) — จัดการเส้นทางการเชื่อมโยงหลัก (รวมถึงเส้นทาง `/info/` และ `/admin/`)
* 📁 **[blog/](file:///d:/WebDev2026/mysite2026/blog)** — แอปพลิเคชันบล็อกบทความ
  * 📄 [views.py](file:///d:/WebDev2026/mysite2026/blog/views.py) — ตรรกะและฟังก์ชันการแสดงผล (เช่น `hello_world`)
  * 📄 [urls.py](file:///d:/WebDev2026/mysite2026/blog/urls.py) — เส้นทางจำลองของแอป `blog`

---

## 📨 สรุปสาระสำคัญ: HTTP Request & Response

ในการพัฒนาเว็บแอปพลิเคชัน ข้อมูลจะถูกรับส่งระหว่างผู้ใช้งาน (Client) และเซิร์ฟเวอร์ผ่านโปรโตคอล HTTP เสมอ โดยสรุปเนื้อหาสำคัญได้ดังนี้:

### 🏗️ โครงสร้างของ HTTP Request
ประกอบด้วย 4 ส่วนสำคัญตามลำดับ:

```text
[Request Line]   -> GET /v1/users HTTP/1.1
[HTTP Headers]   -> Host: example.com
                    Authorization: Bearer xyz123
                    Accept: application/json
[Empty Line]     -> (บรรทัดว่าง 1 บรรทัด เพื่อแยกส่วนหัวกับเนื้อหา)
[Request Body]   -> { "name": "John Doe", "email": "john@example.com" }
```

1. **Request Line**: บอกคำสั่งพื้นฐาน (Method, Path/URL, HTTP Version)
2. **HTTP Headers**: ข้อมูลเพิ่มเติม เช่น `Content-Type`, `Authorization`, และ `User-Agent`
3. **Empty Line**: บรรทัดว่าง 1 บรรทัด เพื่อคั่นบอกการสิ้นสุด Header
4. **Request Body**: ข้อมูลที่เราส่งไปประมวลผล (จำเป็นสำหรับ `POST`, `PUT`, `PATCH`)

### 🛠️ การจับคู่ HTTP Methods กับระบบ CRUD
| HTTP Method | การทำงาน (Action) | การจับคู่ระบบ CRUD | มี Request Body? |
| :--- | :--- | :--- | :--- |
| **`GET`** | ขอรับ/ดึงข้อมูลจากเซิร์ฟเวอร์ | **R**ead (อ่าน) | ❌ ไม่มี |
| **`POST`** | ส่งข้อมูลไปสร้างสิ่งใหม่ | **C**reate (สร้าง) |  มี |
| **`PUT`** | ส่งข้อมูลไปแก้ไขแทนที่ตัวเดิมทั้งหมด | **U**pdate (อัปเดต ทั้งชุด) |  มี |
| **`PATCH`** | ส่งข้อมูลไปแก้ไขบางส่วน | **U**pdate (อัปเดต บางจุด) |  มี |
| **`DELETE`** | สั่งลบข้อมูลที่ระบุ | **D**elete (ลบ) | ❌ ไม่มี (ส่วนใหญ่) |

---

## 🚀 สรุปสาระสำคัญ: HTTP Status Codes
รหัสตัวเลข 3 หลักที่เซิร์ฟเวอร์ตอบกลับมาเพื่อแจ้งผลลัพธ์:

### 📂 ประเภทของรหัสสถานะ (Status Categories)
* **`1xx` (Informational)**: ได้รับคำขอแล้ว กำลังดำเนินการ
* **`2xx` (Success)**: ดำเนินการเสร็จสมบูรณ์เรียบร้อย (เช่น `200 OK`, `201 Created`)
* **`3xx` (Redirection)**: ต้องดำเนินการเปลี่ยนเส้นทาง (เช่น `301 Moved Permanently`, `302 Found`)
* **`4xx` (Client Error)**: ข้อผิดพลาดที่ฝั่งผู้ใช้งาน (เช่น `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`)
* **`5xx` (Server Error)**: ข้อผิดพลาดที่ฝั่งเซิร์ฟเวอร์ (เช่น `500 Internal Server Error`, `503 Service Unavailable`)