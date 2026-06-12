# HANDOFF — โปรเจกต์ PK Lab (ภาพรวมทั้งหมด)

อัปเดตล่าสุด: รวมจาก Handoff เซสชันก่อน (สร้างแท็บ zero/first-order) + เซสชันปัจจุบัน (ปรับเป็นโครงหลายไฟล์ + เริ่มย้ายเข้า Claude Code)
ใช้คู่กับ `CLAUDE.md` (ฉบับสั้นสำหรับ Claude Code อ่านทุกเซสชัน) — ไฟล์นี้เก็บประวัติ + อ้างอิงฉบับเต็ม

---

## 1. บริบทโปรเจกต์
- เว็บสอนเภสัชจลนศาสตร์ (PK) ภาษาไทย สำหรับนักศึกษาเภสัชศาสตร์ · HTML + vanilla JS + custom SVG ไม่มีไลบรารีภายนอก
- เป้าหมายคู่ขนาน: สอน + งานวิจัยในชั้นเรียนเพื่อตีพิมพ์ (within-subject pre–post, แบบสอบถาม TAM, ยื่น EC, วารสารเป้าหมาย AJPE / Pharmacy Education)
- กติกาประจำ: ค่ายาจริงพร้อมอ้างอิง Vancouver + ลิงก์; เกรดอยู่ใน Moodle; ภาษาไทยเชิงวิชาการ; อธิบายโค้ดทีละขั้น; ทำทีละเครื่องมือ; ไม่ regenerate เนื้อหาที่ verify แล้ว

---

## 2. สถาปัตยกรรม — การเปลี่ยนแปลงสำคัญในเซสชันนี้
**เดิม (เซสชันก่อน):** ไฟล์เดียว `pk-lab.html` — แท็บสร้างอัตโนมัติจาก `<section class="lab" data-tab="...">` ตามลำดับใน DOM

**ปัจจุบัน (เซสชันนี้):** ย้ายเป็น **โครงหลายไฟล์ในโฟลเดอร์ `pk-lab-site/`**
```
pk-lab-site/
├─ index.html        ← เปลือกแถบแท็บ (ลิสต์ TABS + โหลดแต่ละไฟล์ผ่าน <iframe> แบบ lazy)
├─ pk-lab.html       ← หัวข้อ PK ที่ยังไม่ได้แยก (ทยอยแยกออก)
├─ order-nl5.html    ← แท็บ zero/first-order · linear/nonlinear (รายละเอียดข้อ 5)
├─ vd.html           ← แท็บ Vd (แยกออกมาแล้วในเซสชันนี้)
├─ absorption.html   ← (กำลังแยก) การดูดซึม/ชีวประสิทธิผล
├─ firstpass.html    ← (กำลังแยก) first-pass — มีแผนทำให้ง่ายลง (ข้อ 4)
├─ clearance.html    ← (กำลังแยก) clearance
├─ halflife.html     ← (กำลังแยก) t½
├─ dosing.html       ← (กำลังแยก) การให้ยาซ้ำ
└─ template.html     ← หน้าเปล่าแม่แบบ (แท็บ "หัวข้อใหม่ (ตัวอย่าง)")
```
- **เหตุผลที่ย้าย:** ไฟล์เดียวใหญ่เกินไป (เพิ่ม/แก้แท็บในแชตติดเพดาน) · โครงหลายไฟล์แก้ทีละหัวข้อได้ ไม่กระทบกัน เหมาะกับ Claude Code
- เนื้อหาหัวข้ออยู่ในไฟล์ของหัวข้อนั้น; index.html จัดการแค่ลำดับ/ชื่อ/การมีอยู่ของแท็บ
- หมายเหตุ: เปิดต้องผ่าน http (local server หรือ deploy) — file:// จะบล็อก iframe

---

## 3. สถานะแท็บปัจจุบัน
ลำดับ/ชื่อแท็บกำหนดที่ลิสต์ `TABS` ใน index.html (ผู้สอนตั้งชื่อแท็บแรกว่า "PK Lab ครั้งที่ 1 2569")
- เสร็จ/ทำงานแล้ว: PK Lab (pk-lab.html, ลบ Vd ออกแล้ว) · zero/first-order (order-nl5.html) · **Vd (vd.html) — แยกครบ ทดสอบผ่าน** · template (หน้าเปล่า)
- กำลังทยอยแยกจาก pk-lab.html: การดูดซึม/F, first-pass, clearance, t½, การให้ยาซ้ำ
- ปลายทาง: เมื่อแยกครบ pk-lab.html จะว่าง → ลบแท็บทิ้งหรือทำเป็นหน้าแนะนำ/สารบัญ
- เคยขอจัดลำดับ: ย้าย CL และ t½ มาก่อนการดูดซึม (จัดที่ลิสต์ TABS)

---

## 4. แผนปรับแท็บ First-pass ให้ง่ายลง (รออนุมัติ/ดำเนินการ)
ผู้สอนต้องการลดเนื้อหาให้เหมาะกับ lab 2 ชม. (เพราะมีวิชา Basic PK เทอม 2) — ให้นิสิตทราบแค่ **first-pass เกิดที่ใด + จากปัจจัย/เอนไซม์ใด** แต่ **ยังคงมี widget**
- คงภาพ diagram เส้นทางยา: ช่องทางเดินอาหาร → ผนังลำไส้เล็ก → ตับ → ระบบไหลเวียนโลหิต (แสดงตำแหน่งที่ยาเสียไป)
- ป้ายปัจจัย/เอนไซม์แต่ละด่าน: ช่องทางเดินอาหาร (เอนไซม์/normal flora), ผนังลำไส้เล็ก (CYP3A4), ตับ (เอนไซม์ที่ตับ)
- เปลี่ยนสไลเดอร์ Fa/Fg/Fh → **toggle เปิด/ปิด** first-pass แต่ละด่าน
- แสดงผล **เชิงคุณภาพ** "ยาเข้าสู่ระบบไหลเวียนโลหิต: มาก/ปานกลาง/น้อย" แทนตัวเลข F% ละเอียด
- คงปุ่มยาตัวอย่าง Propranolol (เสียที่ตับ) / Midazolam (เสียที่ผนังลำไส้ + ตับ)
- เอาออก: สมการ F = Fa×Fg×Fh และตัวเลข F% ละเอียด (ส่วน quantitative เก็บไว้เทอม 2)
- **Convention สัญลักษณ์ที่ตกลง:** ใช้ **F = Fa × Fg × Fh** (F ใหญ่ทั้งสาม subscript เล็ก a/g/h) — แก้จาก "fa" เดิม

---

## 5. แท็บ zero/first-order · linear/nonlinear (order-nl5.html)
ระดับ concept ล้วน — **ไม่แสดงสมการ Michaelis–Menten / ไม่มี Vmax, Km บนจอ** · id ภายใน prefix `nl-`

**เครื่องมือ 1 — รูปการลดลงของระดับยา (หลัง IV bolus)**
- สลับ first-order / zero-order และสลับแกน ปกติ / semi-log
- first-order: exponential → semi-log เป็นเส้นตรง (ลดสัดส่วนคงที่ %/เวลา, t½ คงที่)
- zero-order: เส้นตรงบนสเกลปกติ (ลดปริมาณคงที่ mg/ชม., t½ ไม่คงที่)
- ค่ากราฟ: C0 = 20 mg/L, first-order k = 0.35/ชม., zero-order K0 = 2.2 mg/L/ชม., ช่วง 12 ชม.

**เครื่องมือ 2 — ขนาดยา/วัน เทียบ Css ที่ steady state**
- เลือกยา: phenytoin (nonlinear) vs levetiracetam (linear) + สไลเดอร์ขนาดยา
- phenytoin: Css = R·Km/(Vmax − R); Vmax 490 mg/วัน, Km 6 mg/L → เส้นโค้งงอขึ้น; เส้นประเทา = เส้นเทียบ "ถ้าเป็นเชิงเส้น"; แถบช่วงอ้างอิง 10–20; ขนาดยา 50–470 mg/วัน
- levetiracetam: Css = R/CL (CL ≈ 91 L/วัน) → เส้นตรง; แถบ 12–46; ขนาดยา 250–3500 mg/วัน
- ประเด็นสอน: linear → ขนาดยา 2 เท่า Css 2 เท่า; phenytoin (nonlinear) ใกล้จุดอิ่มตัว เพิ่มขนาดยานิดเดียว Css เพิ่มมากกว่าสัดส่วน เสี่ยงเกินช่วงการรักษา
- ethanol = ตัวอย่าง zero-order แท้ (กล่าวในข้อความ)

**แบบทบทวนตัวเอง (6 ข้อ)** — รูปแบบ `{q, kind, options, answer, why}`
1. รูปการลดลงของ first-order → "ลดลงเป็นสัดส่วน (%) เท่ากันทุกหน่วยเวลา"
2. กราฟใดเป็นเส้นตรงบน semi-log → first-order
3. สาเหตุของ zero-order → ระบบกำจัด (เอนไซม์) อิ่มตัว
4. ทำไม phenytoin เป็น nonlinear → เมแทบอลิซึมอิ่มตัวในช่วงการรักษา
5. phenytoin ใกล้จุดอิ่มตัว เพิ่มขนาดยาเล็กน้อย → **"Css เพิ่มขึ้นมากกว่าสัดส่วนโดยตรง"** (ถ้อยคำตามที่ผู้สอนกำหนด)
6. ยา linear (levetiracetam) ขนาดยา 2 เท่า → "เพิ่มเป็น ~2 เท่า"

---

## 6. ค่ายา + เอกสารอ้างอิง Vancouver (ตรวจสอบแล้วทุกฉบับ)
> ⚠️ การแก้สำคัญ: PMID 2714918 ผู้แต่งคือ **el-Sayed YM, Islam SI** (ไม่ใช่ "Bachmann KA" ที่เคยพิมพ์พลาด)

1. el-Sayed YM, Islam SI. Phenytoin Michaelis-Menten pharmacokinetics in Saudi patients. Int J Clin Pharmacol Ther Toxicol. 1989;27(4):173–178. PMID: 2714918. *(ผู้ใหญ่ Vmax 6.91 mg/kg/วัน, Km 6.44 mg/L)*
2. Ismail R, Rahman AFA. Michaelis-Menten pharmacokinetics of phenytoin in adult Malaysian patients. J Clin Pharm Ther. 1990;15(6):411–417. doi:10.1111/j.1365-2710.1990.tb00405.x. PMID: 2089048. *(Vmax 8.45 mg/kg/วัน, Km 6.72 mg/L)*
3. Radtke RA. Pharmacokinetics of levetiracetam. Epilepsia. 2001;42(Suppl 4):24–27. doi:10.1046/j.1528-1157.2001.0420s4024.x. PMID: 11564121.
4. Keppra (levetiracetam) [prescribing information]. Smyrna (GA): UCB, Inc.; 2009.
5. Patsalos PN, Berry DJ, Bourgeois BFD, et al. Antiepileptic drugs—best practice guidelines for therapeutic drug monitoring: a position paper by the subcommission on therapeutic drug monitoring, ILAE Commission on Therapeutic Strategies. Epilepsia. 2008;49(7):1239–1276. doi:10.1111/j.1528-1167.2008.01561.x.
6. Grasela TH, et al. Clin Pharmacokinet. 1983;8(4):355–364. *(อ้างอิงเสริมค่า 70 kg: Vmax ≈ 415 mg/วัน, Km ≈ 5.7 mg/L)*

**ค่า default ในวิดเจ็ต (ผู้ใหญ่ ~70 kg):** phenytoin Vmax ≈ 490 mg/วัน (≈7 mg/kg/วัน), Km ≈ 6 mg/L (อยู่ในช่วง 6.4–6.7) · levetiracetam CL ≈ 0.9 mL/min/kg (linear), ช่วงอ้างอิง TDM ~12–46 mg/L (ไม่มีช่วงการรักษานิยามตายตัว — ใช้คำว่า "ช่วงอ้างอิง")

**สำหรับแท็บ first-pass (ค่าจริงไว้เป็นข้อมูลประกอบ ถ้าต้องการ):** Propranolol F≈25%, E_h≈0.74–0.79 · Midazolam intestinal E_g≈0.43, E_h≈0.44 (อ้างอิง FDA Inderal PI; Weiss et al. 1978 PMID 656285; Thummel et al. 1996 PMID 8646820; Paine et al. 1996 PMID 8689807)

---

## 7. Convention & ข้อจำกัดการสอน
- **F = Fa × Fg × Fh** (F ใหญ่ทั้งสาม, subscript เล็ก a/g/h) — Fa ดูดซึมเข้า enterocyte, Fg รอด first-pass ผนังลำไส้, Fh รอด first-pass ตับ
- zero/first-order: concept ล้วน ไม่แสดง MM equation / Vmax / Km
- first-pass: เน้น "เกิดที่ใด + ปัจจัยใด" คง widget แบบง่าย (toggle + เชิงคุณภาพ)
- lab พื้นฐาน ~2 ชม. — เลี่ยงเนื้อหาซ้ำกับวิชา Basic PK เทอม 2

---

## 8. งานที่ค้าง / ขั้นถัดไป
**ส่วนเว็บ:**
1. แยกหัวข้อที่เหลือจาก pk-lab.html ให้ครบ (absorption, first-pass, clearance, t½, dosing) → ทดสอบทีละอัน
2. ทำแท็บ first-pass ให้ง่ายลงตามแผนข้อ 4
3. จัดลำดับแท็บให้ตรงตามที่ต้องการ (เช่น CL, t½ ก่อนการดูดซึม)
4. จัดการ pk-lab.html ที่จะว่างเมื่อแยกครบ (ลบทิ้ง/ทำหน้าแนะนำ)
5. Deploy ขึ้น GitHub Pages (repo Spakawadee/pk-lab)

**ส่วนงานวิจัย (กลับมาทำหลังเว็บเสร็จ):**
- ข้อสอบ pre/post (5–8 MCQ), แบบสอบถามการรับรู้แบบ TAM (Likert)
- เอกสารยื่น EC (within-subject pre–post design)
- เตรียมตีพิมพ์ AJPE / Pharmacy Education

---

## 9. หมายเหตุ/ข้อควรระวัง
- กราฟ phenytoin เคยมี "หักมุม" ที่ขอบบน → แก้แล้ว (interpolate จุดแตะ ymax, จบเส้นพอดี, operating point แสดงเฉพาะเมื่ออยู่ในกราฟ) — **อย่าให้กลับมา**
- ตรวจไวยากรณ์ทุก `<script>` โดย**ตัดคอมเมนต์ HTML ออกก่อน** (มี `<script>` ในคอมเมนต์ใกล้ต้นไฟล์ จะ false positive)
- ทดสอบทุกครั้งผ่าน local server (`python3 -m http.server 8000`) ไม่ใช่ double-click
- (ประวัติ) เซสชันแชตก่อนหน้าเจออาการคำสั่งยาวถูกแทรกคำแปลกปลอม — ไม่เกี่ยวกับ Claude Code โดยตรง
