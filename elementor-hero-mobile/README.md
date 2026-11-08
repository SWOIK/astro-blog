# Scroll Hero (mobile) สำหรับ WordPress + Elementor

แปลงคลิป `hero-mobile_bg.mp4` (883 KB) เป็นสคริปต์ Canvas 2D ที่ **ใช้การ scroll ควบคุมแอนิเมชัน**
ไม่มี library และไม่ต้องอัปโหลดไฟล์ใดเพิ่ม ขนาดไฟล์ประมาณ 19 KB (gzip ประมาณ 6 KB)

ลำดับแอนิเมชันตามการเลื่อน: พื้นกระเบื้อง, อนุภาคปรากฏ, รวมตัว, เรียงเป็นโลโก้ "Fw", ค้างพร้อม glow,
ระเบิดเป็นวงแหวน, ตกกลับเป็นพื้น แล้วหน้าเว็บเลื่อนต่อไปส่วนถัดไปตามปกติ

## วิธีใช้ใน Elementor

1. สร้าง Container ใหม่ (ตั้ง **Content Width = Full Width**, padding = 0)
2. ลาก **HTML widget** ใส่ แล้ววางโค้ดจาก `hero-mobile.html` ทั้งไฟล์
3. ที่แท็บ Advanced ของ widget: margin/padding = 0
4. ที่ Container แท็บ Advanced ไปที่ Layout ตั้ง **Overflow = Default** (ห้ามเป็น Hidden เพราะ `position: sticky` จะพัง)
5. ถ้าต้องการให้แสดงเฉพาะมือถือ: Advanced → Responsive → Hide On Desktop / Hide On Tablet
6. แก้ข้อความที่ `swk-h1`, `swk-sub` และลิงก์ของปุ่ม `swk-cta` ในโค้ด

## ปรับแต่ง

| อยากปรับ | แก้ตรงไหน |
|---|---|
| ความยาวการเลื่อน | `--swk-len` ใน `.swk-hero` (300svh = เลื่อน 3 หน้าจอ ยิ่งมากยิ่งช้า) |
| มี fixed header | ตั้ง `--swk-top` เป็นความสูง header เช่น `72px` |
| สีปุ่ม | `--swk-accent` |
| โลโก้ | อาร์เรย์ `LOGO` (`.` ว่าง, `#` น้ำเงิน, `+` น้ำเงินอ่อน) แก้ได้ทีละช่อง |
| สีอนุภาคและพื้น | `C_BLUE`, `C_LIGHT`, `C_WHITE`, `C_GRAY`, `BG` |
| ถ้าไม่ต้องการไอคอนเมาส์ "Scroll" | ลบ `<div class="swk-hint">` |

## เรื่อง PageSpeed / SEO

- `h1`, คำอธิบาย และปุ่ม เป็น HTML จริง Google อ่านได้ และ canvas ไม่นับเป็นองค์ประกอบ LCP จึงไม่ถ่วงคะแนน
- canvas ใส่ `aria-hidden` เพราะเป็นของตกแต่ง
- ไม่มี request เพิ่ม ไม่ใช้ฟอนต์นอก ใช้ font ของระบบ
- วาดเฉพาะตอน scroll และหยุดเมื่อพ้นจอ (`IntersectionObserver`) จำกัด devicePixelRatio ไม่เกิน 2
- ผู้ใช้ที่เปิด `prefers-reduced-motion` จะเห็นภาพโลโก้นิ่งแทนแอนิเมชัน
- ถ้าปิด JavaScript ข้อความและปุ่มยังแสดงอยู่
- **ใน Elementor ให้มี `h1` แค่ตัวเดียวต่อหน้า** ถ้าหน้านั้นมี H1 อยู่แล้ว ให้เปลี่ยน `<h1 class="swk-h1">` เป็น `<h2 class="swk-h1">`

## ข้อควรระวัง

- ปลั๊กอิน cache/optimize (WP Rocket, LiteSpeed, Autoptimize) ที่มีตัวเลือก **Delay JavaScript** ให้เพิ่มข้อยกเว้นให้สคริปต์นี้ (เช่นคำว่า `swk-hero`) ไม่เช่นนั้นแอนิเมชันจะเริ่มช้า การ minify ปกติไม่มีปัญหา (โค้ดใช้ `;` ครบและไม่มีคอมเมนต์ `//`)
- ถ้า theme มี `overflow: hidden` ที่ `body` หรือ wrapper ครอบทั้งหน้า sticky จะไม่ทำงาน ให้ปรับ CSS ส่วนนั้น
- บนเบราว์เซอร์เก่าที่ไม่รู้จักหน่วย `svh` จะ fallback เป็น `vh`
- ผลทดสอบทำใน Chromium (จำลอง iPhone 13) ยังไม่ได้ทดสอบบน Safari จริงหรือบน Elementor จริง ควรเปิดดูบนมือถือจริงก่อนขึ้นเว็บ
