# PostureAI

แอปติดตามท่านั่งแบบ local-first ใช้กล้องและ MediaPipe Pose เพื่อประมวลผลบนอุปกรณ์ของผู้ใช้โดยตรง ไม่มีการบันทึกหรืออัปโหลดภาพ/วิดีโอจากกล้อง

## เริ่มใช้งาน

ต้องมี Node.js 22 ขึ้นไป (ทดสอบกับ Node.js 24 แล้ว)

```powershell
npm start
```

เปิด [http://localhost:3000](http://localhost:3000) แล้วกด **เริ่มการวิเคราะห์** และอนุญาตการใช้กล้อง

> ห้ามเปิด `index.html` โดยตรงผ่าน `file://` เพราะ Browser จะไม่อนุญาตการเข้าถึงกล้อง ต้องเปิดผ่าน `localhost` หรือ HTTPS เสมอ

## ข้อมูลและความเป็นส่วนตัว

- AI pose detection ทำงานใน browser ของผู้ใช้ด้วย MediaPipe
- วิดีโอไม่ถูกส่งไปยัง server และไม่ถูกเก็บลงดิสก์
- Server local เก็บเพียงคะแนน, มุมโดยประมาณ, เวลา และรายการแจ้งเตือนใน `E:\PostureAI\data\postureai.sqlite`
- กด **ลบข้อมูลในเครื่อง** ในแอปเพื่อล้างประวัติทั้งหมด
- คะแนนนี้เป็นเครื่องมือช่วยปรับพฤติกรรม ไม่ใช่อุปกรณ์หรือคำวินิจฉัยทางการแพทย์

## โครงสร้าง

- `server.mjs` — local HTTP server และ REST API สำหรับ SQLite
- `app.js` — กล้อง, MediaPipe Pose, การคำนวณคะแนน และการบันทึกผล
- `app.css` — responsive application UI
- `E:\PostureAI\data\postureai.sqlite` — สร้างอัตโนมัติครั้งแรกที่รัน (ไม่ถูก commit)

เปิดดูตารางด้วย **DB Browser for SQLite** ได้ที่ `E:\PostureAI\data\postureai.sqlite` ขณะ server รันอยู่ระบบใช้ WAL mode เพื่อให้เปิดดูฐานข้อมูลได้โดยไม่ขัดขวางการบันทึกตามปกติ

## ขอบเขตเวอร์ชันนี้

เป็นแอปสำหรับใช้งานบนเครื่องเดียว ข้อมูลไม่ซิงก์ข้ามเครื่องและไม่มีระบบบัญชีผู้ใช้ หากต้องการใช้งานหลายคน/ออนไลน์ ควรเพิ่ม authentication, HTTPS, database server และนโยบายสิทธิ์ข้อมูลก่อนนำขึ้น public internet
