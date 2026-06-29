# Handoff — Smart Office Skills Website (iPhenomenon)

เว็บไซต์แสดง 24 AI Skills (9 หมวด) สำหรับนักเรียน — เปิดดู / Copy Prompt / Download .zip ได้
ดีไซน์ที่เลือกใช้: **Hero = Split, Card = Accent** (ตั้งเป็น default แล้ว)

## งานที่ต้องทำต่อ
1. Deploy เว็บนี้ขึ้น host แบบ static (Netlify / Vercel / Cloudflare Pages / GitHub Pages)
2. ตรวจว่า Copy Prompt และ Download .zip ทำงานได้บน production
3. (ถ้าต้องการ) ต่อ custom domain ของ iPhenomenon

## โครงสร้างไฟล์ (ส่งทั้งโฟลเดอร์)
```
Smart Office Skills.dc.html   ← หน้าเว็บหลัก (เปิดได้ตรงๆ ในเบราว์เซอร์)
support.js                    ← runtime ของ Design Component (ห้ามแก้/ห้ามลบ)
skills-data.js               ← ข้อมูล 24 สกิล + เนื้อหา SKILL.md ทั้งหมด (window.SMART_OFFICE_DATA)
assets/
  SmartOffice_24Skills_Guide.docx   ← คู่มือฉบับเต็ม (ปุ่ม "ดูคู่มือทั้งหมด" ลิงก์มาที่นี่)
  skills/
    H1-S1_tax-guide-by-business.zip ... H9-S3_sop-prompt-library.zip  (24 ไฟล์)
```

## สถาปัตยกรรม (อ่านก่อนแก้)
- หน้าเว็บเป็น **Design Component** ไฟล์เดียว: `Smart Office Skills.dc.html`
  - มี 2 ส่วน: template (markup) + logic class (`class Component extends DCLogic`)
  - โหลด `support.js` เป็น runtime — เปิดเป็นไฟล์ static ได้เลย ไม่ต้อง build step
- ข้อมูลสกิลทั้งหมดอยู่ใน `skills-data.js` เป็น `window.SMART_OFFICE_DATA` (array ของ 9 หมวด)
  - แต่ละสกิล: `{ code, name, slug, team, desc, ex[], zip, md }`
  - `md` = เนื้อหา SKILL.md เต็ม (ใช้ตอน Copy), `zip` = path ไฟล์ดาวน์โหลด, `team` = badge Teamwork
- ปุ่ม Copy ใช้ `navigator.clipboard` (มี fallback execCommand), Download เป็น `<a download href=...>`

## วิธีแก้เนื้อหา (ที่พบบ่อย)
- **แก้คำอธิบาย / ตัวอย่างคำสั่ง / ชื่อสกิล** → แก้ใน `skills-data.js`
- **แก้เนื้อหา Prompt (SKILL.md)** → แก้ field `md` ใน `skills-data.js` และอัปเดตไฟล์ .zip ใน `assets/skills/` ให้ตรงกัน
- **เพิ่ม/ลบสกิล** → เพิ่ม object ใน `skills-data.js` + วางไฟล์ .zip ใน `assets/skills/` + อัปเดต `count`/หมวด
- **ปรับ Teamwork badge** → toggle field `team: true/false` ใน `skills-data.js`
  - ปัจจุบันติด team=true ที่: H1-S2, H2-S1, H3-S2, H5-S4, H6-S1, H6-S2, H6-S3, H8-S1
    (เป็นการประเมินจากเนื้อหา — เจ้าของคอนเทนต์ควรยืนยันรายการนี้)

## Brand colors (iPhenomenon)
Magenta `#C040C0` · Deep Purple `#5B1A8B` · Purple `#8B2FC9` · Lavender `#EDE9FE` ·
Cream `#FFF5E6` (page bg) · Gold `#FFC700` · Teamwork Blue `#1D4ED8`
Font: Noto Sans Thai (UI), IBM Plex Mono (code/slash command)

## หมายเหตุ deploy
- เป็น static ทั้งหมด ไม่มี backend — อัปโหลดทั้งโฟลเดอร์ได้เลย
- ต้องเสิร์ฟผ่าน http(s) (อย่าเปิดด้วย file://) เพราะ `skills-data.js` โหลดผ่าน `<script src>`
- ตั้ง `Smart Office Skills.dc.html` เป็นหน้า index (หรือ rename เป็น index.html ก็ได้ ขอแค่ให้ support.js / skills-data.js / assets อยู่ path เดิม)
- ทางเลือก: ถ้าอยากได้ไฟล์เดียวจบ ขอให้ทีมเดิม export เป็น standalone HTML ได้ (แต่ไฟล์ .zip ดาวน์โหลดต้องอยู่แยกเสมอ)
