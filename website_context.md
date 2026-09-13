# Website Context

> เอกสารนี้เป็น context แบบย่อของเว็บไซต์ เพื่อใช้ประกอบการแก้ไขหรือเพิ่ม feature ในอนาคต
> ก่อนเริ่มแก้ไขเว็บไซต์ ให้อ่านไฟล์นี้ก่อน แล้วค่อยเปิดเฉพาะ source code ที่เกี่ยวข้องกับงานนั้น

**สร้างเอกสารเมื่อ:** 2026-09-09
**ตรวจสอบ code ล่าสุดเมื่อ:** 2026-09-13 (อ้างอิง commit `13ffe0a` บน branch `main` + การสลับวันตาม "แผน D" ในวันเดียวกัน)

---

## 1. Project Overview

- **ชื่อโปรเจกต์:** Beijing Autumn Odyssey 2026 (ชื่อไฟล์ repo: `beijing_planner`)
- **วัตถุประสงค์ของเว็บไซต์:** เว็บไซต์แผนการเดินทางท่องเที่ยวปักกิ่งส่วนตัวแบบ interactive สำหรับทริป 8 วัน (17–24 ตุลาคม 2026) ประกอบด้วยตารางเวลารายวัน, คู่มือร้านอาหาร, เช็คลิสต์เตรียมตัว/จองตั๋ว, และคู่มือจองตั๋วแบบละเอียด (วัน-เวลาที่ต้องกดจองจริง)
- **กลุ่มผู้ใช้งาน:** ผู้จัดทริป/ครอบครัวผู้เดินทาง (2 ผู้ใหญ่ + เด็ก 1 คน) ใช้เป็น reference ส่วนตัวระหว่างวางแผนและระหว่างเดินทางจริง ไม่ใช่เว็บสาธารณะสำหรับผู้ใช้ทั่วไป
- **สถานะปัจจุบันของเว็บไซต์:** ใช้งานได้จริง (live) มีการแก้ไขเนื้อหาต่อเนื่องหลายรอบ (ดู git log) เนื้อหาเป็น static ล้วน ไม่มี backend
- **URL / deployment environment:** `https://supparerkk.github.io/beijing_planner/` (GitHub Pages, deploy จาก branch `main` โดยตรง ไม่มี build step)
- **Framework / language:** ไม่มี framework — เขียนด้วย HTML + CSS + Vanilla JavaScript ล้วนในไฟล์เดียว (`index.html`)
- **Package manager:** ไม่มี (ไม่มี `package.json`, ไม่มี dependency ที่ติดตั้งผ่าน npm/yarn/pnpm)
- **Build / run commands:** ไม่มี build step — เปิด `index.html` ตรงในเบราว์เซอร์ได้เลย หรือ deploy ผ่าน GitHub Pages อัตโนมัติเมื่อ push เข้า `main`
- **โครงสร้างระบบโดยสรุป:** Single-file static website — 1 ไฟล์ HTML (~16,600 บรรทัด, ~1.1MB) รวม `<style>` (CSS ทั้งหมด) และ `<script>` (JS ทั้งหมด) ไว้ในไฟล์เดียว ไม่มี routing จริง (ใช้ tab-switching ด้วย JS แทนหน้าแยก), ไม่มี backend/database/API, ไม่มี authentication, ข้อมูล state เดียวที่ persist คือ `localStorage` สำหรับสถานะ checkbox ในเช็คลิสต์

---

## 2. Tech Stack

| ส่วน | เทคโนโลยี/Library | เวอร์ชัน | ใช้ทำอะไร | ไฟล์ที่เกี่ยวข้อง |
|---|---|---:|---|---|
| Frontend | Vanilla HTML5 + CSS3 + JavaScript (ES6) | - | โครงสร้างเว็บทั้งหมด ไม่มี framework (React/Vue ฯลฯ) | `index.html` |
| Styling | Native CSS ด้วย CSS Custom Properties (design tokens) | - | ธีมสี, layout, responsive design, animation | `index.html` `<style>` บรรทัด 20–2342 |
| Font | Google Fonts (Inter, Playfair Display, Prompt, Chonburi) | - | โหลดผ่าน `<link>` จาก `fonts.googleapis.com` | `index.html` บรรทัด 13–15 |
| Icons | Font Awesome | 6.4.0 (CDN) | ไอคอนทั้งเว็บ (`<i class="fa-solid ...">`) | `index.html` บรรทัด 18 (โหลดจาก cdnjs.cloudflare.com) |
| Routing | ไม่มี routing จริง — ใช้ JS tab-switching แทน | - | สลับการแสดงผล "หน้า" ด้วย `.day-card` + `setActiveDay()` | `index.html` บรรทัด 16295–16452 |
| State management | ไม่มี state library — ใช้ DOM class toggling + `localStorage` | - | เก็บสถานะ checkbox ของเช็คลิสต์ | `index.html` บรรทัด 16544–16578 |
| Form / Validation | ไม่มี (ไม่มีฟอร์มในเว็บไซต์) | - | - | ไม่พบใน codebase |
| Backend / API | ไม่มี | - | เว็บเป็น static content ล้วน ไม่มีการเรียก API ใด ๆ | ไม่พบใน codebase |
| Database | ไม่มี | - | - | ไม่พบใน codebase |
| Authentication | ไม่มี | - | - | ไม่พบใน codebase |
| Analytics / Monitoring | ไม่มี | - | ไม่มี Google Analytics/Tag Manager หรือ monitoring script ใด ๆ | ไม่พบใน codebase |
| Deployment | GitHub Pages | - | Deploy อัตโนมัติจาก branch `main` (repo `Supparerkk/beijing_planner`) | git remote `origin` |
| Image assets | PNG ภาพนิ่ง (ไม่ใช่ external CDN) | - | รูปร้านอาหารและภาพพื้นหลัง hero | `images/*.png`, `beijing_hero_bg.png` |

---

## 3. Project Structure

```text
beijing_planner/
├── index.html              # ไฟล์เดียวที่มีทั้งเว็บ: HTML + <style> (CSS) + <script> (JS)
├── website_context.md      # เอกสารนี้
├── beijing_hero_bg.png     # ภาพพื้นหลัง hero section (ใช้ผ่าน CSS background-image)
├── images/                 # รูปภาพร้านอาหาร 20 ร้าน ใช้ในแท็บ "ลายแทงร้านอร่อย"
│   ├── siji_minfu.png
│   ├── nanmen_shunrou.png
│   ├── bc_bakery.png
│   ├── bao_shi_fu.png
│   ├── xiaodiao_litang.png
│   ├── fangzhuanchang.png
│   ├── grandma_home.png
│   ├── yaoji_chaogan.png
│   ├── jin_ding_xuan.png
│   ├── dadong.png
│   ├── jubaoyuan.png
│   ├── najia_xiaoguan.png
│   ├── juqi.png
│   ├── nanjing_impressions.png
│   ├── chuan_ban.png
│   ├── heytea.png
│   ├── sexy_tea.png
│   ├── baoyuan_dumplings.png
│   └── ...                 # รวม 18 ไฟล์ .png (ดูรายการเต็มด้วย `ls images/`)
├── .claude/                 # การตั้งค่า Claude Code local (permission settings) — ไม่เกี่ยวกับตัวเว็บ
└── .git/
```

**ไม่พบ:** `package.json`, `README`, `.env` / `.env.example`, โฟลเดอร์ `src/`, `docs/`, `public/`, build config ใด ๆ (webpack/vite/next.config ฯลฯ), test folder

**หมายเหตุ:** เนื่องจากไม่มีโฟลเดอร์ `docs/` มาก่อน จึงบันทึกไฟล์นี้ไว้ที่ root ของโปรเจกต์ตามกติกาที่กำหนด

### โครงสร้างภายใน `index.html` (แบ่งตามบรรทัด)

| ช่วงบรรทัด (ประมาณ) | ส่วน |
|---|---|
| 1–19 | `<head>`: meta tags, Google Fonts, Font Awesome CDN |
| 20–2342 | `<style>`: CSS ทั้งหมดของเว็บ (ดูหัวข้อ 8) |
| 2344–2359 | เปิด `<body>`, mouse-spotlight layer, gradient mesh background |
| 2360–~2470 | Hero section: countdown timer, hero meta badges, pixel-art walking animation box |
| ~2470–12700 | SVG pixel-art landmark graphics (`lm-svg-1` ถึง `lm-svg-8`) — ภาพพิกเซลของแลนด์มาร์คแต่ละวัน ใช้พื้นที่ไฟล์เยอะที่สุด (rect หลายพันตัว) |
| 12711–12725 | `<nav class="mobile-day-nav">` — แถบเมนูเลื่อนแนวนอนสำหรับมือถือ |
| 12727–~12810 | `<aside class="sidebar">` — เมนูเลือกวัน (desktop) เท่านั้น (แผนที่ interactive และกล่องข้อควรจำถูกลบเมื่อ 2026-09-10 ดู Change Log หัวข้อ 13) |
| 12907–14113 | `<section class="timeline-container">` — การ์ดแต่ละวัน `#day1`–`#day8` |
| 14114–14980 | `<article id="restaurants">` — แท็บ "ลายแทง 20 ร้านอร่อย" |
| 14973–15493 | `<article id="checklist">` — แท็บรวม "🎫 คู่มือจองปักกิ่ง 2026" (เดิมแยกเป็น `#checklist` + `#booking` 2 แท็บ รวมเป็นแท็บเดียวเมื่อ 2026-09-10 ดู Change Log หัวข้อ 13) |
| ~15507–15965 | `<script>`: JS ทั้งหมดของเว็บ (ดูหัวข้อ 6) — เลขบรรทัดขยับขึ้นจากเดิมเพราะการรวมแท็บทำให้ไฟล์สั้นลง ควร `grep` หาจุดเริ่มต้นจริงแทนอ้างอิงเลขบรรทัดตรงๆ |

---

## 4. Pages and Routing

**ไม่มี routing จริงในความหมายของ SPA framework** — เว็บเป็นไฟล์เดียว ทุก "หน้า" คือ `<article class="day-card">` ที่ซ่อน/แสดงด้วย JS class `.active-day` / `.collapsed` ผ่านฟังก์ชัน `setActiveDay(dayId)` ไม่มีการเปลี่ยน URL จริง (ไม่มี History API / hash routing แม้ `<a href="#day1">` จะมี `#` แต่ JS จะ `e.preventDefault()` แล้วสลับด้วย class เสมอ)

| "Route" (article id) | หน้า/เนื้อหา | จุดประสงค์ | Component สำคัญ | Data source | หมายเหตุ |
|---|---|---|---|---|---|
| `#day1` | วันที่ 1 · 17 ต.ค. | เดินทางถึงปักกิ่ง, เที่ยว Qianmen | `.timeline`, `.day-status-strip` | เนื้อหา hardcode ในไฟล์ | ระดับ 🟩 เดินน้อย |
| `#day2` | วันที่ 2 · 18 ต.ค. | Lama Temple + Confucius Temple + 798 Art + Sanlitun | เหมือนด้านบน | เนื้อหา hardcode | ระดับ 🟨 ปานกลาง — ย้ายมาจากวันพุธเดิม (แผน D 2026-09-13) |
| `#day3` | วันที่ 3 · 19 ต.ค. | Mutianyu Great Wall (+ บล็อก "แผนสำรองบ่ายวันจันทร์" 4 ตัวเลือก) | เหมือนด้านบน + `.attraction-callout` | เนื้อหา hardcode | ระดับ 🟥 — ย้ายมาจากวันอาทิตย์เดิม (แผน D 2026-09-13) เพราะวันจันทร์คนน้อยที่สุดและเป็นวันที่สถานที่อื่นปิด |
| `#day4` | วันที่ 4 · 20 ต.ค. | Forbidden City + Tiananmen Square + Jingshan Park | เหมือนด้านบน | เนื้อหา hardcode | ระดับ 🟥 — ย้ายมาจากวันจันทร์เดิม (Forbidden City ปิดทุกวันจันทร์) |
| `#day5` | วันที่ 5 · 21 ต.ค. | Temple of Heaven + Wangfujing + โชว์กายกรรม Chaoyang Theatre | เหมือนด้านบน | เนื้อหา hardcode | ระดับ 🟨 ปานกลาง — ย้ายมาจากวันศุกร์เดิม ให้เป็นวันพักก่อน Universal (แผน D 2026-09-13) |
| `#day6` | วันที่ 6 · 22 ต.ค. | Universal Beijing Resort | เหมือนด้านบน | เนื้อหา hardcode | ระดับ 🟥 — **ล็อกวันตายตัว ห้ามย้าย** (ตกลงกับผู้ใช้ไว้ชัดเจน) |
| `#day7` | วันที่ 7 · 23 ต.ค. | Summer Palace + ตรอก Hutong/Houhai | เหมือนด้านบน | เนื้อหา hardcode | ระดับ 🟥 — ย้ายมาจากวันจันทร์เดิม เพื่อให้ตกวันศุกร์ที่คนน้อยกว่า (แผน D 2026-09-13) |
| `#day8` | วันที่ 8 · 24 ต.ค. | เช็คเอาท์ & เดินทางกลับกรุงเทพฯ | เหมือนด้านบน | เนื้อหา hardcode | ระดับ 🟩 เดินน้อย |
| `#restaurants` | ลายแทง 20 ร้านอร่อยกระแสโซเชียล | รวมร้านอาหารแนะนำ 20 ร้าน พร้อมรูป/ราคา/พิกัด | `.restaurant-card` × 20 | เนื้อหา hardcode + รูปจาก `images/` | ใช้ `val-highlight-day` เชื่อมโยงร้านกับวันที่ในแผน |
| `#checklist` | 🎫 คู่มือจองปักกิ่ง 2026 (เดิมแยกเป็น "เช็คลิสต์เตรียมตัว" + "คู่มือจองตั๋ว" 2 แท็บ รวมเป็นแท็บเดียวเมื่อ 2026-09-10) | Checklist แบบ interactive (บันทึกสถานะผ่าน `localStorage`) แบ่งเป็น 4 เฟสตามลำดับเวลาใช้งานจริง: 1) ก่อนจอง 2) วันจอง (ตาราง fixed-date + flexible + booking-card ต่อสถานที่ + สรุปงบ) 3) ก่อนเดินทาง 4) ระหว่างทริป | `.checklist-item`, `.booking-card`, progress bar | เนื้อหา hardcode (คำนวณ deadline ด้วยมือ ไม่ใช่ JS) | ตัด Food Guide table เต็ม/Drink Checklist/งบอาหาร (ซ้ำกับ `#restaurants`), Inventory summary และ Booking Calendar timeline (ซ้ำกับตารางเปรียบเทียบใหม่), ตาราง Itinerary Review วิเคราะห์เชิงลึก (ข้อสรุปสะท้อนอยู่ในตารางใหม่แล้ว) ออกทั้งหมด — คงเฉพาะ Must-Book/Queue Alert (ย้ายไปเฟส 4 ระหว่างทริป) |

**Protected route / dynamic route / redirect:** ไม่มี — ทุกส่วนเป็น static content เข้าถึงได้เท่ากันหมด ไม่มีการ auth หรือ permission ใด ๆ

**หน้าที่ยังสร้างไม่เสร็จหรือถูกปิดไว้:** ไม่พบ — ทุกแท็บมีเนื้อหาสมบูรณ์

---

## 5. Components Inventory

ไม่มี component framework (React/Vue) — "component" ในที่นี้หมายถึงชุด CSS class + โครงสร้าง HTML ที่ถูกใช้ซ้ำหลายจุดในไฟล์เดียวกัน

| Component (CSS class) | นิยามอยู่ที่ (บรรทัด CSS) | หน้าที่ | โครงสร้าง/Attributes สำคัญ | ใช้ที่ไหน | ข้อควรระวัง |
|---|---|---|---|---|---|
| `.day-card` | 693–845 | การ์ดเนื้อหาแต่ละ "หน้า" (วันเที่ยว/ร้านอาหาร/เช็คลิสต์/booking) | ต้องมี `id` ไม่ซ้ำ, class `.active-day`/`.collapsed` ควบคุมโดย JS | ทุก `<article>` ใน `#main-content` | การเพิ่ม `.day-card` ใหม่จะถูก JS หยิบเข้า `dayCards` NodeList อัตโนมัติจาก `querySelectorAll`, **แต่ต้องเพิ่ม nav link (`.day-tab`/`.mobile-day-btn`) เองด้วยมือ** ไม่มีระบบ auto-generate |
| `.timeline` / `.timeline-item` | 846–965 | แสดงตารางเวลารายกิจกรรมในแต่ละวัน แบบเส้น timeline พร้อม marker สี | class ย่อย `activity-transport` / `activity-food` / `activity-sightseeing` / `activity-shopping` / `activity-hotel` กำหนดสี marker/เวลา | ทุก `#day1`–`#day8` | ต้องมี class กิจกรรมที่ถูกต้องมิฉะนั้นจะไม่มีสี (fallback เป็นสี default) |
| `.attraction-callout` | 966–1060 | การ์ดไฮไลท์สถานที่ท่องเที่ยวแบบเน้น (กรอบทอง) ภายใน timeline-item | `.callout-header`, `.callout-title`, `.callout-tag`, `.callout-body`, `.callout-bullets` | ภายใน `.timeline-item` ของสถานที่สำคัญ | ใช้เฉพาะสถานที่ไฮไลท์ ไม่ใช่ทุก timeline-item |
| `.day-status-strip` | 2105–2131 (12b) | แถบสรุป 3 บรรทัดใต้ tips-box ของแต่ละวัน: สถานะเปิดสถานที่ / ระดับกิจกรรม / เหตุผลการจัดวัน | `<span><i>...</i> ข้อความ</span>` × 3 | ทุก `#day1`–`#day8` (เพิ่มเข้ามาระหว่างงานแก้ปัญหา Forbidden City ปิดวันจันทร์) | Component ใหม่ที่ออกแบบให้เข้ากับ `.tips-warnings-box`/`.checklist-item` — ถ้าจะย้ายวันเที่ยวอีกในอนาคต ต้องอัปเดตข้อความ 3 บรรทัดนี้ทุกครั้ง |
| `.tips-warnings-box` | ค้นหาด้วย `grep "tips-warnings-box"` | กล่องคำแนะนำ/คำเตือนแบบกรอบเส้นประ พร้อมไอคอน | `.tips-content`, `<strong>`, `<p>` | ทุกวัน + restaurants + checklist + booking | ใช้สีกรอบ default เป็นแดง ปรับ inline style ได้ (เช่น restaurants ใช้สีฟ้า) |
| `.difficulty-badge` | ค้นหาด้วย `grep "difficulty-badge"` | ป้ายระดับความหนักของวัน | class ย่อย `diff-easy` / `diff-medium` / `diff-heavy` / `diff-veryheavy` | header ของแต่ละ `.day-card` + `.day-tab` (sidebar) | **ต้องอัปเดตพร้อมกัน 2 จุด**: badge ใน header ของ day-card และ badge ใน sidebar `.day-tab` — ถ้าลืมจุดใดจุดหนึ่งจะไม่ตรงกัน |
| `.checklist-section` / `.checklist-item` | 1215–1412 (Section 11) | ระบบเช็คลิสต์แบบ checkbox ที่บันทึกสถานะใน `localStorage` | `.checklist-cb-wrapper` > `input.checklist-checkbox` + `.checklist-checkmark`, `.checklist-content` > `label` + `.checklist-meta` | `#checklist` เกือบทั้งหมด, และ Food Guide checkbox (Must Book/Queue Alert) | **checkbox `id` ต้องไม่ซ้ำกันทั้งไฟล์** เพราะ JS ผูก progress bar กับ `document.querySelectorAll('.checklist-checkbox')` แบบ global (ไม่ scope ตามหน้า) — เพิ่ม checkboxใหม่ต้องตั้ง id ไม่ชนของเดิม |
| `.checklist-badge` | 1369–1392 | ป้ายความเร่งด่วนในแต่ละ checklist-item | class ย่อย `badge-urgent` (แดง) / `badge-warning` (เหลือง) / `badge-info` (ฟ้า) / `badge-free` (เขียว) | `#checklist` | - |
| `.restaurant-card` | 1413–1669 (Section 12) | การ์ดร้านอาหารแต่ละร้าน พร้อมรูป, badge, meta grid | `.restaurant-image-wrapper > img`, `.restaurant-badges`, `.restaurant-meta-grid > .restaurant-meta-item`, `.val-highlight-day` (span สีทองอ้างอิงวันในแผน) | `#restaurants` (20 การ์ด) | `.val-highlight-day` เป็นจุดที่ต้อง sync กับวันที่จริงเสมอเมื่อมีการสลับวันเที่ยว (เคยเป็นจุดพลาดมาแล้วในอดีต) |
| `.pixel-landmark` / `.lm-svg-N` (N=1–8) | 1670–1973 (pixel art) | ภาพพิกเซลอาร์ตของแลนด์มาร์คแต่ละวัน แสดงใน hero box แบบ cross-fade ตามวันที่เลือก | `id="lm-svg-{dayNum}"` ต้องตรงกับเลขวันเป๊ะ ๆ, ใช้ `.active`/`.leaving` class สลับ | Hero section, sync ผ่าน `syncPixelArt()` | **ห้ามวาดใหม่เฉย ๆ เวลาสลับเนื้อหาแต่ละวัน** — ถ้าสลับว่าวันไหนไปเที่ยวที่ไหน ต้องเปลี่ยน `id` ของ svg (และ comment กำกับ) ให้ตรงกับแลนด์มาร์คจริงของวันนั้น ไม่ใช่วาดภาพใหม่ |
| `.booking-card` / `.inventory-grid` / `.booking-cal` | ภายใน Section 12 (Booking Dossier CSS) | การ์ดรายละเอียดการจองตั๋วรายสถานที่ | `.booking-card-head`, `.booking-meta-grid > .booking-meta-item`, `.urgency-pill` (`urgency-high`/`urgency-med`/`urgency-low`), class `card-critical`/`flag-day` สำหรับเน้นรายการวิกฤต | `#booking` Section A–C | ต้องอัปเดตวันที่/deadline ทุกครั้งที่มีการย้ายวันเที่ยว — ปัจจุบัน sync กับแผนล่าสุดแล้ว (สลับวันตาม "แผน D" 2026-09-13) |
| `.day-title-box` / `.day-header` | 693–845 | หัวข้อของแต่ละการ์ด (ชื่อวัน, theme, difficulty badge) | `h2.day-number-title`, `span.date`, `span.day-main-theme` | ทุก `.day-card` | - |

**ห้ามแก้โดยไม่ทดสอบผลกระทบ:**
- `setActiveDay()` / `syncPixelArt()` (บรรทัด 16295–16523) — เป็น engine หลักที่ผูกทุกอย่างเข้าด้วยกัน (tab, pixel art, mobile accordion) แก้ผิดจุดเดียวจะกระทบทั้งเว็บ
- checkbox `id` ใด ๆ ภายใต้ `.checklist-checkbox` — ซ้ำกันจะทำให้ progress bar และ `localStorage` เพี้ยน
- `id="lm-svg-N"` ของ pixel art SVG — ต้องเป็นเลข 1–8 ไม่ซ้ำกัน (มี ~1,000+ `<rect>` ต่อภาพ ห้ามลบทิ้งโดยไม่ได้ตั้งใจ)

---

## 6. Data Flow and State

ไม่มี global state library, ไม่มี API call ทั้งเว็บเป็น static content ที่ hardcode ไว้ใน HTML โดยตรง data flow ที่มีจริงมีเพียง 2 เส้นทาง:

### 6.1 Tab/Day Switching (ไม่มีการ fetch ข้อมูล เป็นแค่ show/hide)

```text
User คลิก .day-tab หรือ .mobile-day-btn
→ e.preventDefault() + อ่าน data-target attribute
→ setActiveDay(dayId) [บรรทัด 16327]
   ├─ toggle .active class บน tabs/buttons ที่ตรง data-target
   ├─ toggle .active-day / .collapsed บน .day-card ที่ id ตรงกัน
   ├─ toggle .visible บน .timeline-item ภายในการ์ดที่ active (สำหรับ CSS fade-in)
   └─ syncPixelArt(dayId) → เปลี่ยน #pixel-day-title ข้อความ + สลับ .active/.leaving บน lm-svg-N
→ scrollIntoView() ไปยังตำแหน่งที่เหมาะสม (mobile scroll ไปการ์ด, desktop scroll ไป #main-content)
```

- Auto-slideshow: `startAutoSlide()` เรียก `syncPixelArt()` วนทุก 5 วินาที (บรรทัด 16307–16325) — เปลี่ยนเฉพาะภาพ pixel-art ใน hero ไม่กระทบการ์ดเนื้อหาจริง
- Initial load: `initLayout()` (บรรทัด 16387) อ่าน `.day-tab.active` ที่ hardcode ไว้ใน HTML (ปัจจุบันคือ `day1`) เป็นค่าเริ่มต้น

### 6.2 Checklist State (localStorage)

```text
User คลิก checkbox (.checklist-checkbox)
→ change event listener [บรรทัด 16571]
→ localStorage.setItem(checkbox.id, checkbox.checked)
→ updateChecklistProgress() → คำนวณ % จาก checkedCount / checklistCbs.length (นับ checkbox ทั้งไฟล์ ไม่แยกตามแท็บ)
→ อัปเดต #checklist-progress-text และ #checklist-progress-bar (แสดงเฉพาะใน header ของแท็บ #checklist)

โหลดหน้าเว็บใหม่:
→ วน checklistCbs ทั้งหมด → localStorage.getItem(checkbox.id) → set checkbox.checked ตามค่าที่บันทึกไว้
```

- **Global state ที่ใช้:** `localStorage` (browser-native, per-origin) — key คือ checkbox `id` ตรง ๆ, value เป็น string `'true'`/`'false'`
- **Local state ที่สำคัญ:** `currentAnimationDay` (JS variable, บรรทัด 16125) ติดตามว่า pixel-art กำลังแสดงวันไหน
- **Cache / query library:** ไม่มี
- **Form state และ validation:** ไม่มี (ไม่มีฟอร์มในเว็บไซต์)
- **จุดที่ข้อมูลถูก transform/format:** ไม่มี — ราคา/วันที่ทั้งหมดเป็นข้อความ hardcode คำนวณด้วยมือไว้ล่วงหน้าแล้วในเนื้อหา ไม่มีการคำนวณแบบ dynamic ใน JS (ยกเว้น countdown timer)
- **Loading / error / empty state:** ไม่มี (ไม่มีการโหลดข้อมูล async ใด ๆ)
- **Side effect / asynchronous process ที่มี:**
  - `setInterval(updateCountdown, 1000)` — นับถอยหลังสู่วันเดินทาง (target date hardcode: `October 17, 2026 10:10:00`)
  - `requestAnimationFrame(animateParticles)` — วาด particle animation บน canvas พื้นหลัง hero
  - `IntersectionObserver` (บรรทัด 16534) — เพิ่ม `.visible` class ให้ `.timeline-item` เมื่อ scroll เข้ามาในจอ (ใช้เสริม fade-in effect)

---

## 7. API, Backend and Integrations

**ไม่มี API, backend, database, authentication, payment, storage provider หรือ third-party integration ใด ๆ ในโปรเจกต์นี้**

สิ่งเดียวที่โหลดจากภายนอกคือ static asset (ไม่ใช่ API call):

| Feature | Endpoint / Service | Method | Request | Response | Auth | Source file |
|---|---|---|---|---|---|---|
| Web font | `fonts.googleapis.com` / `fonts.gstatic.com` | GET (ผ่าน `<link>`) | - | ไฟล์ font | ไม่มี | `index.html` บรรทัด 13–15 |
| Icon library | `cdnjs.cloudflare.com` (Font Awesome 6.4.0) | GET (ผ่าน `<link>`) | - | ไฟล์ CSS | ไม่มี | `index.html` บรรทัด 18 |

ข้อมูลจองตั๋ว/ราคา/เวลาต่าง ๆ ในแท็บ "คู่มือจองตั๋ว" (`#booking`) **เป็นผลจากการค้นคว้าด้วยมือแล้วพิมพ์ hardcode ลงในหน้าเว็บ** ไม่ได้ดึงจาก API ของหน่วยงานจริง (เช่น ticket.dpm.org.cn) — ถ้าข้อมูลราคา/เวลาเปลี่ยนแปลง ต้องแก้ข้อความในไฟล์ด้วยมือเสมอ

---

## 8. Styling and Design System

- **วิธี styling ที่ใช้:** Native CSS ล้วน เขียนอยู่ใน `<style>` tag เดียวกลางไฟล์ (บรรทัด 20–2342) ไม่มี CSS-in-JS, ไม่มี Tailwind/Bootstrap/Material UI หรือ CSS framework ใด ๆ ไม่มี CSS Modules (เพราะไม่มี build step)
- **Global CSS / theme file:** ไม่มีไฟล์แยก — ธีมทั้งหมดกำหนดผ่าน CSS Custom Properties ใน `:root` (บรรทัด 24–58)

### Design Tokens หลัก (จาก `:root`)

| ตัวแปร | ค่า | ใช้สำหรับ |
|---|---|---|
| `--bg-dark` | `#0a0f1e` | สีพื้นหลังหลักของเว็บ (โทนกลางคืน) |
| `--color-gold` | `#C9A84C` | สี accent หลัก (หัวข้อ, highlight, badge สำคัญ) |
| `--color-sky-blue` | `#38bdf8` | accent รอง (transport, ลิงก์, badge ข้อมูล) |
| `--color-soft-red` | `#f87171` | คำเตือน, urgency สูง, booking |
| `--color-purple` | `#a855f7` | กิจกรรมช้อปปิ้ง |
| `--color-orange` | `#f97316` | กิจกรรมอาหาร, ระดับกิจกรรมปานกลาง (🟨) |
| `--color-green` | `#10b981` | สถานะเปิด/สำเร็จ, กิจกรรมโรงแรม, ระดับกิจกรรมน้อย (🟩) |
| `--text-main` | `#f3f4f6` | สีตัวอักษรหลัก |
| `--text-muted` | `#9ca3af` | สีตัวอักษรรอง |
| `--glass-bg` / `--glass-border` | `rgba(255,255,255,0.05)` / `rgba(255,255,255,0.08)` | พื้นผิวกระจกฝ้า (glassmorphism) ของการ์ดต่าง ๆ |
| `--font-headings` | `'Playfair Display', 'Chonburi', Georgia, serif` | หัวข้อทั้งหมด |
| `--font-body` | `'Prompt', 'Inter', sans-serif` | เนื้อหาทั่วไป (รองรับภาษาไทย) |

- **Component library:** ไม่มี (เขียน component ด้วยมือทั้งหมดเป็น CSS class ตามหัวข้อ 5)
- **Icon library:** Font Awesome 6.4.0 (`fa-solid`, `fa-regular` classes)
- **Responsive breakpoint หลัก:** `768px` (mobile/desktop switch — ใช้ตรวจสอบใน JS ด้วย `window.innerWidth < 768` เช่นกัน ไม่ใช่แค่ CSS media query) และ `991px` (ซ่อนเมนู `.day-tabs` บนจอเล็ก ใช้ `.mobile-day-nav` แทน) ดู CSS media query เพิ่มเติมที่ Section 10 (บรรทัด 1061–1214)
- **แนวทางการออกแบบที่ควรรักษาไว้:** โทนมืด (dark theme) หรูหรา สไตล์ "แผนที่เดินทางพรีเมียม" (ไม่มีแผนที่ interactive จริงในเว็บแล้วตั้งแต่ 2026-09-10 แต่ยังคงธีมภาพลักษณ์นี้ไว้), ใช้ glassmorphism (พื้นผิวโปร่งแสงมีขอบบาง), ใช้สี accent (gold/sky-blue/red/purple/orange/green) แบบมีความหมายเฉพาะ (ไม่ใช่สุ่มสี) ตามหัวข้อ/ประเภทกิจกรรม, ใช้ font คู่ Playfair Display (หัว) + Prompt (เนื้อหาไทย)
- **ไฟล์ที่ควรแก้เมื่อเปลี่ยนสี/ฟอนต์/theme/layout/responsive:**
  - เปลี่ยนสีธีมหลัก → แก้ `:root` (บรรทัด 24–58) เท่านั้น เพราะทุก component อ้างอิงผ่าน CSS variable
  - เปลี่ยนฟอนต์ → แก้ `--font-headings`/`--font-body` ใน `:root` และ `<link>` Google Fonts บรรทัด 15
  - เปลี่ยน layout หลัก (sidebar/grid) → Section 5 "CONTENT WRAPPER & GRID SYSTEM" (บรรทัด 473–692)
  - เปลี่ยน responsive/mobile → Section 10 "MOBILE SPECIFIC & RESPONSIVE DESIGN" (บรรทัด 1061–1214)

---

## 9. Feature Map: แก้อะไรต้องดูไฟล์ไหน

> ทั้งหมดอยู่ในไฟล์เดียว `index.html` — คอลัมน์ "ไฟล์" หมายถึงช่วงบรรทัดภายในไฟล์นั้น

| หากต้องการแก้ไขเรื่องนี้ | เริ่มดูส่วนนี้ก่อน | บรรทัดโดยประมาณ | ข้อควรตรวจสอบ |
|---|---|---|---|
| เพิ่มวันเที่ยวใหม่ / วันที่ 9 | คัดลอกโครงสร้าง `<article class="day-card" id="dayN">` จากวันที่ใกล้เคียง | 12907–14113 | ต้องเพิ่ม `.day-tab` ใน sidebar (12731–12813), `.mobile-day-btn` (12711–12725), เพิ่ม `id="lm-svg-N"` ใหม่ในภาพ pixel art, เพิ่ม entry ใน `pixelDayTitles` (16126–16135) |
| แก้ navigation / menu | Sidebar `.day-tabs` (desktop) และ `.mobile-day-nav` (mobile) | 12711–12813 | ต้องแก้ทั้ง 2 ที่พร้อมกันให้ label ตรงกัน (emoji มือถือ vs ชื่อเต็ม desktop) |
| แก้ UI/เนื้อหาหน้าเดิม (วันเที่ยว) | หา `id="dayN"` ที่ต้องการ แล้วแก้ `.timeline-item` ภายใน | 12910–14113 | อย่าลืมอัปเดต `.day-status-strip` (3 บรรทัดสถานะ) ให้ตรงกับเนื้อหาใหม่ |
| เพิ่ม form | ไม่มีระบบฟอร์มอยู่แล้ว ต้องสร้างใหม่ทั้งหมด | - | ไม่มี pattern เดิมให้อ้างอิง — ต้องออกแบบใหม่ |
| แก้ validation | ไม่มี validation logic ในโปรเจกต์ | - | - |
| เรียก API ใหม่ | ไม่มี pattern การเรียก API ในโปรเจกต์เลย (ไม่มี `fetch`/`XMLHttpRequest`) | - | ถ้าจะเพิ่มต้องเขียนใหม่ทั้งหมด รวมถึง loading/error state |
| แก้ database model | ไม่มี database | - | - |
| แก้ login / permission | ไม่มีระบบ authentication | - | - |
| แก้ dashboard / chart | ไม่มี chart library ใช้อยู่ — ใกล้เคียงที่สุดคือ progress bar ของเช็คลิสต์ | 16544–16578 | - |
| แก้ responsive / mobile | Section 10 ใน `<style>` + breakpoint check ใน JS | CSS: 1061–1214, JS: `window.innerWidth < 768` (หลายจุด) | breakpoint หลักคือ 768px (layout) และ 991px (สลับเมนู `.day-tabs`/`.mobile-day-nav`) — ต้องเทสทั้งสองจุด |
| แก้ theme / สี / font | `:root` custom properties | 24–58 | ทุก component อ้างอิงผ่าน variable — แก้จุดเดียวมีผลทั้งเว็บ |
| แก้เช็คลิสต์ (เพิ่ม/ลบรายการ) | `#checklist` article, ระบบ `.checklist-item` | 14981–15660 | checkbox `id` ต้องไม่ซ้ำกับที่มีอยู่ทั้งไฟล์ (ตรวจด้วย `grep 'id="chk_'`) |
| แก้ข้อมูลร้านอาหาร | `#restaurants` article, `.restaurant-card` | 14114–14980 | ถ้าร้านผูกกับวันเที่ยว ต้องอัปเดต `.val-highlight-day` ให้ตรงกับวันจริง |
| แก้ข้อมูลจองตั๋ว/deadline | `#booking` article, `.booking-card` / `.inventory-grid` / `.booking-cal` | 15661–16121 | ต้องคำนวณวันที่ deadline ด้วยมือใหม่ทุกครั้งที่มีการย้ายวันเที่ยว — ไม่มี auto-calculation |
| แก้ pixel-art / hero animation | SVG `lm-svg-N` + `pixelDayTitles` + `syncPixelArt()` | SVG: ~2470–12700, JS: 16126–16168 | ห้ามวาดภาพใหม่เมื่อสลับเนื้อหาแต่ละวัน ให้สลับ `id`/comment ของ SVG ที่มีอยู่แทน |
| deploy เว็บไซต์ | ไม่มีขั้นตอนพิเศษ | - | `git push` ไปที่ branch `main` แล้ว GitHub Pages จะ deploy อัตโนมัติ (ไม่มี CI/CD config ให้เห็นใน repo) |

---

## 10. Environment Variables and Configuration

**ไม่พบ environment variable หรือไฟล์ config ใด ๆ ในโปรเจกต์** (ไม่มี `.env`, `.env.example`, `config.js`, หรือค่าที่ต้องตั้งค่าก่อนรัน)

| Variable / Config | ใช้สำหรับอะไร | อยู่ในไฟล์ใด | จำเป็นใน environment ไหน | หมายเหตุ |
|---|---|---|---|---|
| ไม่พบใน codebase | - | - | - | โปรเจกต์นี้ไม่ต้องการ environment variable ใด ๆ เพราะเป็น static site ล้วน ไม่มี backend/API key ที่ต้องซ่อน |

**Technical debt:** ไม่มี `.env.example` เพราะไม่จำเป็น — ไม่ถือเป็น debt ในบริบทโปรเจกต์นี้

---

## 11. Important Rules and Constraints

### Convention ที่พบในโค้ด

- **การตั้งชื่อไฟล์ภาพร้านอาหาร:** `images/{ชื่อร้านแบบ snake_case ภาษาอังกฤษ}.png` (เช่น `siji_minfu.png`, `nanmen_shunrou.png`)
- **การตั้งชื่อ checkbox id:** prefix ตามหมวด เช่น `chk_d{N}_{ชื่อสถานที่ย่อ}` (เช่น `chk_d4_forbidden`), `chk_food_*` (Food Guide), `chk_bk_*` (Booking action list), `chk_review_*` (Itinerary Review action list) — **ต้อง unique ทั้งไฟล์**
- **การตั้งชื่อ SVG landmark id:** `lm-svg-{เลขวัน 1-8}` ต้องตรงกับวันที่แลนด์มาร์คนั้นถูกใช้จริงในปัจจุบัน (ไม่ใช่เลขวันตอนที่วาดครั้งแรก)
- **รูปแบบวันที่:** ใช้ปฏิทินไทย/สากลผสม เช่น `19 ต.ค. 2026` หรือ `วันจันทร์ที่ 19 ตุลาคม 2026` — ไม่มี ISO date format หรือ timezone library ใช้งาน (ยกเว้น countdown timer ที่ใช้ `new Date('October 17, 2026 10:10:00')` แบบ local browser timezone)
- **การจัดการเงิน/สกุลเงิน:** ราคาทั้งหมดอ้างอิงเป็น CNY (หยวนจีน) ก่อน แล้วแปลงเป็น THB ด้วยอัตราคงที่ **1 CNY = 4.90 THB** (ตรวจสอบล่าสุด 9 ก.ย. 2026 จาก xe.com mid-market) — ค่านี้ hardcode พิมพ์ซ้ำในหลายจุดของไฟล์ ไม่ได้ผูกเป็นตัวแปรกลาง หากอัตราแลกเปลี่ยนเปลี่ยนแปลงมาก ต้องแก้ไขด้วยมือทีละจุด (ค้นด้วย `grep "THB"` หรือ `grep "4.90"`)
- **Locale:** ภาษาไทยเป็นหลักทั้งเว็บ (`<html lang="th">`), มีศัพท์เฉพาะ/ชื่อสถานที่ภาษาอังกฤษ-จีนปนอยู่ตามความเหมาะสม

### Permission / Role
- ไม่มีระบบ role หรือ permission — เว็บเปิดให้ทุกคนที่มีลิงก์เข้าถึงได้เท่ากันหมด (ไม่มี login)

### ข้อจำกัดด้าน security
- ไม่มีการรับ input จากผู้ใช้ที่ส่งกลับไปยัง server ใด ๆ (ไม่มีฟอร์ม, ไม่มี backend) จึงไม่มีความเสี่ยง XSS/injection จากฝั่ง user โดยตรง
- `localStorage` ใช้เก็บเฉพาะสถานะ checkbox (boolean) ไม่มีข้อมูลอ่อนไหว

### ข้อจำกัด performance
- ไฟล์ SVG pixel-art (8 ภาพ) มี `<rect>` รวมกันหลายพันตัว ทำให้ไฟล์ `index.html` มีขนาดใหญ่ (~1.1MB) — โหลดครั้งแรกอาจช้าบนอินเทอร์เน็ตช้า แต่ทุกอย่าง render ฝั่ง client ล้วน ไม่มี network request เพิ่มเติมหลังโหลดเสร็จ (ยกเว้น font/icon CDN)
- Particle animation ใช้ `requestAnimationFrame` วิ่งตลอดเวลา (ไม่มีการหยุดเมื่อ tab ไม่ active) — เป็น performance overhead เล็กน้อยที่ยังไม่ได้ optimize

### Browser/device support
- ไม่พบการระบุ browser support list หรือ polyfill ใด ๆ ใน codebase — ใช้ modern JS (ES6 class, arrow function, `IntersectionObserver`) ซึ่งต้องการเบราว์เซอร์ที่ค่อนข้างใหม่ (ไม่รองรับ IE11)
- Responsive design รองรับ mobile ผ่าน breakpoint 768px/991px (ดูหัวข้อ 8)

### สิ่งที่ห้ามแก้โดยไม่ทดสอบผลกระทบ
ดูหัวข้อ 5 ท้ายตาราง Components Inventory (setActiveDay/syncPixelArt, checkbox id, lm-svg id)

### Known bugs / technical debt ที่พบระหว่างสำรวจ codebase

1. **`.flight-banner` CSS ไม่ถูกใช้งาน (orphaned CSS):**
   มี CSS rule `.flight-banner` / `.flight-banner-bottom` (บรรทัด 154–238) แต่ไม่พบการใช้ class นี้ใน HTML เลย (`grep` ไม่เจอ) — คาดว่าเป็นของเก่าที่ถูกแทนที่ด้วย `.hero-meta` + countdown timer แล้วไม่ได้ลบ CSS ทิ้ง ไม่กระทบการทำงาน แต่เพิ่ม dead code

2. **HTML comment ใน Booking Dossier ไม่ตรงกับ heading จริง:**
   Comment `<!-- Section B: Booking calendar -->` (ก่อนบรรทัด ~15923) อยู่เหนือ heading ที่เขียนว่า "C) Booking Calendar" — comment label เพี้ยนจากตัวอักษรจริง 1 อักษร ไม่กระทบการทำงาน เป็นแค่ความสับสนเวลาอ่าน source

3. **Booking deadline เป็นตัวเลขคำนวณด้วยมือ ไม่มี auto-recalculation:**
   ทุกครั้งที่มีการย้ายวันเที่ยวของสถานที่ใด ต้องไปคำนวณวันจองใหม่ด้วยมือใน `#booking` (เช่น "จองล่วงหน้า 7 วัน" ต้องนับวันเองแล้วพิมพ์ทับ) — ไม่มี JS ช่วยคำนวณ มีความเสี่ยงตัวเลขไม่ sync ถ้าลืมอัปเดตจุดใดจุดหนึ่ง (มีบันทึกจุดที่ต้องแก้พร้อมกันไว้ในหัวข้อ 9)

---

## 12. Testing and Validation

- **วิธี run project ใน local:** เปิดไฟล์ `index.html` ด้วยเบราว์เซอร์โดยตรง (double-click หรือ `start index.html` บน Windows) ไม่ต้องมี local server หรือ build step ใด ๆ — อย่างไรก็ตาม CDN font/icon requires internet connection
- **คำสั่ง build:** ไม่มี (ไม่พบใน codebase)
- **คำสั่ง lint:** ไม่มี (ไม่พบใน codebase — ไม่มี ESLint/Stylelint config)
- **คำสั่ง test:** ไม่มี (ไม่พบ test file หรือ test framework ใด ๆ ในโปรเจกต์)
- **วิธีตรวจสอบ feature สำคัญหลังแก้ไข (manual):**
  1. เปิดไฟล์ในเบราว์เซอร์ ตรวจว่าไม่มี error ใน DevTools Console
  2. คลิกทุก tab (`.day-tab` และ `.mobile-day-btn`) ตรวจว่าเนื้อหาสลับถูกต้อง, pixel-art เปลี่ยนตาม
  3. ย่อขนาดหน้าจอ/ใช้ DevTools responsive mode ทดสอบที่ breakpoint 768px และ 991px
  4. ในแท็บ `#checklist` ลองติ๊ก checkbox แล้ว refresh หน้าเว็บ ตรวจว่าสถานะยังอยู่ (localStorage) และ progress bar อัปเดตถูกต้อง
  5. ตรวจสอบ HTML ด้วยสายตา/เครื่องมือว่า tag เปิด-ปิดสมดุล (โปรเจกต์นี้ไม่มี HTML validator ติดตั้งไว้ — แนะนำใช้ `python -c` script นับ `<div>`/`</div>` หรือ paste ผ่าน W3C validator ก่อน commit งานใหญ่)
- **Test files / test framework ที่มี:** ไม่มี
- **Checklist ขั้นต่ำก่อน commit/deploy:**
  - [ ] ไม่มี syntax error ใน DevTools Console เมื่อเปิดหน้าเว็บ
  - [ ] Tag HTML เปิด-ปิดสมดุล (โดยเฉพาะ `<div>`, `<article>`, `<table>`)
  - [ ] checkbox `id` ใหม่ (ถ้ามี) ไม่ซ้ำกับของเดิม
  - [ ] ถ้าย้าย/เพิ่ม/ลบวันเที่ยว: อัปเดตครบทุกจุดตามหัวข้อ 9 (sidebar tab, mobile nav, pixel-art id, `pixelDayTitles`, `.val-highlight-day` ในร้านอาหาร, checkbox id ใน checklist, วันที่ใน booking dossier)
  - [ ] ทดสอบคลิกทุกแท็บอย่างน้อย 1 รอบก่อน push

---

## 13. Change Log for AI / Developers

| วันที่ | สิ่งที่แก้ | ไฟล์ที่เปลี่ยน | ผลกระทบ | วิธีทดสอบ | ผู้แก้ |
|---|---|---|---|---|---|
| 2026-09-09 | สร้างเอกสาร `website_context.md` ฉบับแรก | `website_context.md` (ใหม่) | ไม่มีผลต่อพฤติกรรมเว็บไซต์ (เอกสารอ้างอิงเท่านั้น) | อ่านทวนเนื้อหาเทียบกับ `index.html` | Claude (AI) |
| 2026-09-10 | รวมแท็บ `#checklist` และ `#booking` เป็นแท็บเดียว (`#checklist` — "🎫 คู่มือจองปักกิ่ง 2026") เรียงเนื้อหาใหม่เป็น 4 เฟส: ก่อนจอง → วันจอง (ตารางเทียบวัน-เวลาแบบตายตัว/ยืดหยุ่น + booking-card ต่อสถานที่ + สรุปงบ) → ก่อนเดินทาง → ระหว่างทริป ตัดเนื้อหาซ้ำซ้อน (ตาราง Food Guide เต็ม/Drink Checklist/งบอาหาร ซ้ำกับแท็บ `#restaurants`, ตารางวิเคราะห์ Itinerary Review ที่ข้อสรุปถูกสะท้อนในตารางใหม่แล้ว, Inventory summary ที่ซ้ำกับ booking-card, Booking Calendar timeline ที่ซ้ำกับตารางใหม่ที่เรียงตามวันจองอยู่แล้ว) | `index.html` บรรทัด ~14973–15493 (แท็บรวมใหม่), sidebar nav บรรทัด ~12799, mobile nav บรรทัด ~12722 | ลบ `id="booking"` ทั้งหมดออกจากไฟล์ — nav เหลือปุ่ม/ลิงก์เดียวชี้ไป `data-target="checklist"` เท่านั้น checkbox id ทั้งหมด unique (verify ด้วย `grep -oE 'id="chk_[a-zA-Z0-9_]+"' index.html \| sort \| uniq -d`) | เปิดไฟล์ตรวจ tab ใหม่, กด checkbox ทดสอบ progress bar, ตรวจไม่มี `#booking` residual (`grep -n '#booking' index.html`) | Claude (AI) |
| 2026-09-10 | ทดลองทำ visual redesign "Modern Beijing Travel Journal" (theme สว่าง, ฟอนต์ใหม่, บีบอัดรูป ฯลฯ) แบบ uncommitted แล้วผู้ใช้ตัดสินว่าแย่กว่าดีไซน์เดิม จึง **revert `index.html`/`website_context.md`/`images/*` กลับสู่ commit ล่าสุดทั้งหมด** (ไม่มีร่องรอยของ redesign นั้นหลงเหลือ) จากนั้นลบ 2 ส่วนออกจากดีไซน์เดิมอย่างถาวร: (1) "พื้นที่ท่องเที่ยวตามแผนที่" — sidebar map panel ทั้งหมด (SVG `.map-zone` 6 โซน, `.map-legend`, ฟังก์ชัน JS `syncMapHighlight()` และการเรียกใช้ใน `setActiveDay()`) (2) "ข้อควรจำสำคัญ" — `.reminders-panel` พร้อม `.reminder-item` 3 รายการ (Forbidden City, Mutianyu Toboggan, Universal Beijing) ลบ CSS ที่เกี่ยวข้องทั้งหมด (Section 6 เดิม, media query ที่ซ่อน `.map-panel` บนจอเล็ก) ไม่มี asset ไฟล์แยกให้ลบ (แผนที่เป็น inline SVG, reminders เป็นข้อความล้วน) sidebar ตอนนี้เหลือแค่เมนูเลือกวัน (desktop) อย่างเดียว | `index.html` (`:root`→ท้ายไฟล์ ทั้งไฟล์ revert, แล้วลบ sidebar HTML บรรทัด ~12807–12895 เดิม, CSS Section 6 บรรทัด ~486–692 เดิม, media query บรรทัด ~1304 เดิม, JS Section 6 "INTERACTIVE MAP SYNCING" บรรทัด ~15646–15714 เดิม), `website_context.md` (revert แล้วอัปเดตให้ตรงกับโครงสร้างใหม่) | sidebar เหลือ 1 การ์ด (`.day-tabs`) จากเดิม 3 การ์ด — ไม่กระทบ itinerary/checklist/booking/restaurants เลย breakpoint 991px ยังทำงานถูกต้อง (sidebar ยุบเป็น `height:auto` บนจอเล็กอยู่แล้วจึงไม่มีช่องว่างเกิดขึ้น) | `node --check` ผ่าน, tag balance (`<article>`/`</article>` 10/10, `<section>`/`</section>` 2/2) ผ่าน, grep หา `map-panel\|map-zone\|syncMapHighlight\|reminders-panel\|reminder-item` ไม่พบเหลือ | Claude (AI) |
| 2026-09-13 | **สลับวันเที่ยวตาม "แผน D"** เพื่อให้ทั้งกำแพงเมืองจีนและพระราชวังฤดูร้อนตกวันธรรมดาที่คนน้อย โดยล็อกพระราชวังต้องห้าม (อ. 20 ต.ค.) และ Universal (พฤ. 22 ต.ค.) ไว้เหมือนเดิม — หมุนเนื้อหา 4 วันแบบวน: `day5`(วัดลามะ/798)→`day2` · `day2`(กำแพงเมืองจีน)→`day3` · `day7`(หอบูชาฟ้า/Wangfujing/กายกรรม)→`day5` · `day3`(พระราชวังฤดูร้อน/Hutong)→`day7` · เหตุผล: วันจันทร์ 19 ต.ค. มีเพียงกำแพงเมืองจีนกับพระราชวังฤดูร้อนที่เปิด (ต้องห้าม/วัดขงจื๊อ/จุดชมในหอบูชาฟ้า ปิดจันทร์) จึงให้กำแพงลงวันจันทร์ (คนน้อยสุด) และย้ายพระราชวังฤดูร้อนไปศุกร์ 23 ต.ค. ได้จังหวะหนัก-เบาเป็น 🟩🟨🟥🟥🟨🟥🟥🟩 ไม่มีวันเดินเยอะ 3 วันติดอีก · **เพิ่มบล็อกใหม่ "แผนสำรองบ่ายวันจันทร์"** ในวันกำแพงเมืองจีน (แทนที่ Yandaixie/Houhai ที่ย้ายไปอยู่กับพระราชวังฤดูร้อนแล้ว) เสนอ 4 ตัวเลือกที่เปิดวันจันทร์: Ditan Park (ใบแปะก๊วย), Olympic Park, Beihai Park, Liangma River | `index.html`: day-card `#day2/#day3/#day5/#day7` (หมุนทั้งบล็อก + แก้หัวข้อวัน/วันที่), `.day-status-strip` 4 วัน, `.difficulty-badge` ใน sidebar `.day-tab` 4 จุด, mobile nav emoji 4 ปุ่ม, `pixelDayTitles` + `id="lm-svg-N"` และ comment กำกับ (หมุน id ตามแลนด์มาร์ค ไม่วาดใหม่), `.val-highlight-day` 20 จุดในแท็บ `#restaurants`, ตาราง fixed-date/flexible + `.booking-card` 7 ใบในแท็บ `#checklist`, ข้อความเชื่อมวัน (day1 "วันรุ่งขึ้น", day5 "พรุ่งนี้เช็คเอาท์"→"พรุ่งนี้ Universal", day7 เพิ่มเตือนเก็บกระเป๋า) | ไม่กระทบ `#day1/#day4/#day6/#day8` · วันจอง/deadline ที่เลื่อน: Lama+Confucius (21→18 ต.ค.), Mutianyu + รถส่วนตัว (18→19 ต.ค.), หอบูชาฟ้า + โชว์กายกรรม (23→21 ต.ค.), พระราชวังฤดูร้อน (19→23 ต.ค.) พร้อมเลื่อนวันเปิดจองตาม offset เดิมของแต่ละที่ · checkbox id ทั้งหมดคงเดิม (localStorage ไม่เพี้ยน) | `node --check` ผ่าน · tag balance `<article>` 10/10, `<section>` 2/2, `<span>` 657/657, `<div>` 890/889 (ต่างกัน 1 ตัวเป็นของเดิมก่อนแก้ ยืนยันกับไฟล์สำรอง) · ไม่มี `id="chk_*"` ซ้ำ · `id="lm-svg-1..8"` ครบไม่ซ้ำ | Claude (AI) |

> เพิ่มแถวใหม่ทุกครั้งที่มีการเปลี่ยนแปลงโครงสร้างสำคัญ (route/component/API/config/rule) ในโปรเจกต์

---

## 14. Quick Start for Future Tasks

1. อ่าน `website_context.md` (ไฟล์นี้) ก่อนเสมอ
2. ระบุ feature หรือหน้าที่ต้องการแก้ (เช่น "ย้ายวันเที่ยว", "เพิ่มร้านอาหาร", "แก้สี theme")
3. เปิดอ่านเฉพาะช่วงบรรทัดจากหัวข้อ 9 "Feature Map" — ไม่ต้องอ่านทั้งไฟล์ 16,600 บรรทัด
4. ตรวจสอบ dependency ระหว่างจุดต่าง ๆ ตามหัวข้อ 5/9 (เช่น สลับวันเที่ยว = ต้องแก้ 6-7 จุดพร้อมกัน ไม่ใช่แค่เนื้อหา `.day-card`)
5. แก้เฉพาะส่วนที่จำเป็น — ใช้ `grep`/search หา pattern ที่มีอยู่แล้วก่อนสร้าง component ใหม่ซ้ำซ้อน
6. ไม่มี lint/test/build ให้รัน — ทดสอบด้วยการเปิดเบราว์เซอร์จริงตาม checklist หัวข้อ 12
7. อัปเดต `website_context.md` และ Change Log (หัวข้อ 13) หากเปลี่ยนโครงสร้าง, "route" (day-card ใหม่), component, styling token, หรือ rule สำคัญ

---

## 15. Files to Read First

โปรเจกต์นี้มีไฟล์เนื้อหาเดียว จึงเรียงตาม "ช่วงที่ควรอ่านก่อน" ภายในไฟล์นั้นแทน:

1. `website_context.md` (ไฟล์นี้) — ภาพรวมทั้งหมดก่อนแตะ code จริง
2. `index.html` บรรทัด 1–58 — meta tags และ design tokens (`:root`) จำเป็นสำหรับงานด้าน styling ทุกชนิด
3. `index.html` บรรทัด 16295–16523 — JS engine หลัก (`setActiveDay`, `syncPixelArt`) จำเป็นสำหรับงานด้าน navigation/interaction ทุกชนิด
4. `index.html` บรรทัด 12907–14113 — โครงสร้างการ์ดวันเที่ยว `#day1`–`#day8` จำเป็นสำหรับงานแก้เนื้อหาทริป
5. `index.html` บรรทัด 12731–12813 — sidebar day-tabs (จุดที่มักลืมอัปเดตคู่กับเนื้อหา)
6. `index.html` บรรทัด 14981–15660 — แท็บ `#checklist` (ระบบ checkbox + localStorage + Food Guide + Itinerary Review)
7. `index.html` บรรทัด 15661–16121 — แท็บ `#booking` (Booking Dossier — deadline การจองทุกสถานที่)
8. `index.html` บรรทัด 14114–14980 — แท็บ `#restaurants` (ร้านอาหาร 20 ร้าน)
9. `index.html` บรรทัด 1215–1669 — CSS ของระบบ checklist และ restaurant card
10. `.git log` — ประวัติการแก้ไขเนื้อหา (ใช้เข้าใจบริบทว่าทำไมเนื้อหาบางจุดถึงเป็นแบบนี้ เช่น ทำไม Forbidden City ถึงอยู่วันที่ 4 ไม่ใช่วันที่ 3 และทำไมกำแพงเมืองจีนถึงอยู่วันจันทร์)

---

## Open Questions / Needs Verification

- ไม่พบไฟล์ README หรือเอกสารอ้างอิงเดิมในโปรเจกต์ ก่อนสร้างไฟล์นี้ — เอกสารนี้จึงเป็นฉบับแรกทั้งหมด ไม่มีข้อมูลเดิมให้ merge
- ไม่มี CI/CD config (`.github/workflows/`) ปรากฏใน repo แม้จะ deploy ผ่าน GitHub Pages ได้จริง — สันนิษฐานว่า GitHub Pages ถูกตั้งค่าให้ serve จาก branch `main` root โดยตรงผ่าน repository settings (ไม่ใช่ Actions) แต่ยังไม่ได้ตรวจสอบ repository settings จริงเพื่อยืนยัน 100%
- ไม่สามารถยืนยันได้จาก codebase ว่ามีการทดสอบบนเบราว์เซอร์ใดบ้างมาก่อน (Chrome/Safari/Firefox/มือถือจริง) — ไม่มีบันทึกไว้ในโค้ดหรือ commit message
