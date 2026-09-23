# design.md 
ต้นแบบ: https://fdnet.dhammakaya.network/services-new/
ขอบเขต: design tokens, viewport และ accessibility baseline

---

## 1. Viewport 

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

กฎที่ต้องทำตาม:
  ```css
  @media (max-width: 768px) {
    input, select, textarea { font-size: 16px; }
  }
  ```
---

## 2. Design tokens

### 2.1 สี

```css
:root {
  /* Brand */
  --clr-primary:       #4a90b8; /* ปุ่ม, ลิงก์, แถบ nav, eyebrow */
  --clr-primary-dark:  #2e6f96; /* ชื่อหน่วยงาน, section title */
  --clr-primary-pale:  #eef6fb; /* พื้น hover / active, badge blue */
  --clr-accent:        #c9a84c; /* ทอง — badge "ใหม่" */

  /* Surface */
  --clr-white:   #ffffff;
  --clr-bg:      #f5f8fc; /* พื้นหลังหน้า */
  --clr-surface: #ffffff; /* พื้น card */
  --clr-border:  #dde8f2;

  /* Text */
  --clr-text:       #1e2c3a;
  --clr-text-muted: #5a7186;
  --clr-text-light: #8dafc7; /* placeholder, ตัวคั่น */

  /* Status */
  --clr-success: #1f8c5b;
  --clr-warning: #d97706;
  --clr-danger:  #c0392b;
}
```

สีที่ใช้ตรงๆ (ไม่ได้เป็นตัวแปร) แต่เป็นส่วนหนึ่งของระบบ:

| ใช้กับ | ค่า |
|---|---|
| พื้น footer | `#0e2338` |
| ข้อความบน footer | `rgba(255,255,255,.72)` / ลิงก์ `.58` / hover `#fff` |
| Hero overlay | `linear-gradient(135deg, rgba(8,47,73,.82) 0%, rgba(13,77,128,.70) 100%)` |
| ข้อความบน hero | หัวข้อ `#fff`, eyebrow `rgba(255,255,255,.7)`, คำอธิบาย `.78` |
| ข้อความบนแถบ nav | `rgba(255,255,255,.88)`, hover/active พื้น `rgba(255,255,255,.15)` |
| ปุ่ม "หน้าแดง" (บน hero) | ขอบ `rgba(255,0,0,.6)`, hover พื้น `rgba(220,80,80,.40)`, ไอคอน `#ffb3b3` |
| Badge green | พื้น `#d1fae5` / ตัวอักษร `#065f46` |
| Badge amber | พื้น `#fef3c7` / ตัวอักษร `#92400e` |
| Highlight คำค้น | `#fef9c3` |
| Tint ของเงา / focus ring | `rgb(26,111,168)` (= `#1a6fa8`) |

### 2.2 Typography

```css
:root {
  --font-th: 'Sarabun', sans-serif; /* ฟอนต์หลักทั้งหน้า */
  --font-en: 'Sarabun', sans-serif; 
}

html { font-size: 100%; } 
body { line-height: 1.65; }
```
โหลดฟอนต์จาก Google Fonts ใส่ใน `<head>` ก่อน `<style>`:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@400;500;600;700&display=swap" rel="stylesheet">
```

Type scale (1rem = 16px ที่ค่าเริ่มต้นของเบราว์เซอร์):

| บทบาท | size | px | weight | line-height |
|---|---|---|---|---|
| H1 (hero title) | 1.55rem | 24.8 | 600 | 1.3 |
| H1 บนจอเล็ก | 1.2rem | 19.2 | 600 | 1.3 |
| H2 (intro title) | 1.3rem | 20.8 | 600 | 1.4 |
| Section title / ชื่อหน่วยงานใน header | 1.05rem | 16.8 | 600 | 1.2 |
| Body | 1rem | 16 | 400 | 1.65 |
| คำอธิบาย / ย่อหน้า | .9rem | 14.4 | 400 | 1.7–1.8 |
| Nav link | .86rem | 13.76 | 500 | — |
| Dropdown link / footer | .85rem | 13.6 | 400 | — |
| Eyebrow (hero) | .82rem | 13.12 | 400 | — |
| Footer contact / sub | .78rem | 12.48 | 400 | 1.8 |
| Eyebrow (section) | .75rem | 12 | 600, `letter-spacing: .1em` | — |
| Badge | .72rem | 11.52 | 500 | — |

### 2.3 Radius

```css
:root {
  --radius-sm: 4px;
  --radius-md: 8px;   /* ช่องค้นหา, มุมล่างของ dropdown */
  --radius-lg: 14px;  /* card, hero, รูปภาพ */
}
/* ค่าพิเศษ: pill 20px (badge), 50% (วงกลม), 10px (กล่องโลโก้ footer) */
```

### 2.4 Shadow

```css
:root {
  --shadow-sm: 0 1px 3px rgba(26,111,168,.08), 0 1px 2px rgba(0,0,0,.04);
  --shadow-md: 0 4px 16px rgba(26,111,168,.10), 0 2px 6px rgba(0,0,0,.04);
  --shadow-lg: 0 10px 40px rgba(26,111,168,.14);
}
/* แถบ nav: 0 2px 8px rgba(26,111,168,.2) */
/* Focus ring: 0 0 0 3px rgba(26,111,168,.12) */
```

### 2.5 ขนาดโครงสร้าง / spacing

```css
:root {
  --header-h:  64px;
  --nav-h:     52px;
  --sidebar-w: 248px;
}
/* Container: max-width 1280px; margin-inline auto
   Padding หน้าหลัก: 32px 24px 48px → มือถือ (≤768px) 20px 16px 40px */
```

Spacing scale ที่ใช้ (px): 2 · 4 · 6 · 8 · 10 · 12 · 14 · 16 · 20 · 24 · 32 · 40 · 48
(ค่าที่ใช้บ่อยสุด: 8, 6, 10)

### 2.6 Motion

| ใช้กับ | ค่า |
|---|---|
| hover สี / พื้น / เงา | `.2s` (บางจุด `.12s`–`.15s`) |
| ลูกศร chevron หมุน | `transform .25s` |
| Drawer มือถือ | `transform .3s cubic-bezier(.4,0,.2,1)` |
| Dropdown เปิด | `fade-drop .18s ease` (opacity 0→1, translateY −6px→0) |
| Card hover | ยก `translateY(-2px)` + เปลี่ยนเป็น `--shadow-md` |
| Scroll | `scroll-behavior: smooth` |

### 2.7 Breakpoints (desktop-first, ใช้ `max-width`)

```css
/* lg */ @media (max-width: 900px) { … }  /* แท็บเล็ต */
/* md */ @media (max-width: 768px) { … }  /* มือถือ — จุดหลัก: padding มือถือ, input 16px */
/* sm */ @media (max-width: 600px) { … }  /* มือถือจอกลาง */
/* xs */ @media (max-width: 480px) { … }  /* มือถือจอเล็ก */
```

การรวมจากค่าเดิม 8 ค่า:

| ค่าใหม่ | ค่าเดิมที่รวมเข้ามา |
|---|---|
| 900px | 900 |
| 768px | 768, 720, 700 |
| 600px | 600, 580 |
| 480px | 480, 400 |

เมื่อรวมแล้ว rule ที่เคยอยู่คนละ breakpoint แต่ตกอยู่กลุ่มเดียวกัน ให้ย้ายไปไว้ใน media query เดียวกัน และตรวจว่าลำดับ cascade ยังถูกต้อง (rule ของ breakpoint ที่เล็กกว่าต้องอยู่หลัง)

---

## 3. Accessibility baseline
- Focus: ใช้ ring แบบเดียวกับช่องค้นหา `border-color: var(--clr-primary); box-shadow: 0 0 0 3px rgba(26,111,168,.12);` กับทุก interactive element ผ่าน `:focus-visible`
- Contrast ข้อความ ≥ 4.5:1 — ตรวจเป็นพิเศษ: ข้อความบน footer ที่ `.58` และ `--clr-text-light` บนพื้นขาว
- Touch target ≥ 44×44px
- `<html lang="th">`
- `@media (prefers-reduced-motion: reduce)` ปิด transition / animation และ `scroll-behavior: smooth`
- Heading ลำดับถูกต้อง: H1 เดียวต่อหน้า, section ใช้ H2
