# Changelog — sgui-retirement-planning

## v1.1 — 2026-06-30
Batch fix หลัง dual-review (Claude self-review + GPT cross-check) — reconcile จุดตรงกัน + รับจุดที่ GPT จับเพิ่ม P0 ก่อน polish

**P0 — correctness/consistency**
- **Bridge period:** เพิ่มสูตร bridge (projection.md สูตร 6) + income-start schedule (SKILL Intake) + Bridge bucket บังคับ (allocation-vehicles) + ลำดับถอนช่วง bridge (decumulation) + critical-fail เมื่อไม่โมเดล
- **FX/currency:** เข้า Assumption Ledger (คอลัมน์สกุลเงิน) + Operating Rule 6 + critical-fail (ปน USD return กับ THB inflation) + แถว RSK
- **Monthly rate:** แก้ `i=r/12` → effective `i=(1+r)^(1/12)−1`
- **pre/post return แยก** (r vs rr) + **net return** (หลัง fee/ภาษี) เป็น Operating Rule 7 + scenario table แยกก่อน/หลัง
- **Intake gate → classifier:** No-calc / Illustrative / Conditional / Plan-ready + รายการ "missing data ที่ flip คำตอบ" (best-effort ไม่บล็อก)
- **Refusal/redirect templates** ใน SKILL.md · **full-review fallback** สำหรับ intent กว้าง · **LICENSE (MIT)**

**P1**
- Cashflow Priority เป็น first-class ใน BASE · 5-bucket layer (reusable) · Action Plan 30d/3–6mo/annual ใน DEC · Response Modes · README · worked examples (basic gap, early-retirement bridge) · smoke evals (router, math)

**จุดที่ไม่รับจาก GPT (พร้อมเหตุผล)**
- "references path bug" = false positive (โครงจริงถูก; เห็นจาก output ที่ flatten ตอน present) — v1.1 zip คง nesting
- "ลบคำว่า Claude" — คงไว้เพื่อ house consistency (เป็น Claude Skill; stock-analysis ก็ใช้) เปลี่ยน neutral ได้ถ้าต้องการ

**Pre-ship corrections (รอบ review สุดท้าย)**
- PMT แยก `PMT_total` (ทั้งสตรีม) กับ `PMT_additional` (ส่วนที่ต้องเพิ่มจากที่ออมอยู่)
- bridge: ระบุชัดต้อง discount post-bridge need กลับมาที่วันเกษียณ `×(1+rr)^(−b)` ก่อนรวม
- gross return: แก้ทิศ — gross ทำให้ประเมิน corpus **สูงเกินจริง** (เดิมเขียนผิดเป็นต่ำเกินจริง)
- scenario: กันนับเงินเฟ้อซ้ำในโหมด real (เงินเฟ้ออยู่ใน real return แล้ว) — แยก dial ได้เฉพาะ nominal หรือเงินเฟ้อเฉพาะหมวด (ค่ารักษา)
- frontmatter description สั้นลง · เพิ่ม VERSION.md (1.1.0) · ลบ loose files นอก package (single source = zip)
- `max(0,…)` กัน PMT ติดลบ + รายงาน surplus แทน · bridge หลาย start age = แบ่งหลาย segment (net W แยกต่อช่วง) · sync eval M14–M15

## v1 — 2026-06-29
- โครงเริ่มต้น: single skill + router + guardrail core + Intake Gate (table contract)
- 8 โมดูล: BASE / NUM / GAP / ALLOC / VEH / DRAW / RSK / DEC ครอบคลุมทั้ง lifecycle (สะสม + ถอน)
- Guardrail เฉพาะแผนเกษียณ: ไม่เดา input · **Assumption Ledger** + สมมติฐานติดป้าย · **3-scenario discipline** · แยก **real/nominal** · capacity vs tolerance · ไม่ฝังกฎภาษี/วงเงินที่อาจล้าสมัยเป็น fact
- ป้ายข้อมูล 4 ระดับ: `[ข้อเท็จจริง]` / `[คำนวณ]` / `[วิเคราะห์]` / `[คาดเดา]`
- DRAW บังคับพูดถึง **sequence-of-returns** เสมอ (ข้าม = critical-fail)
- VEH แบบ Thai-first + global-friendly (RMF/SSF/Thai ESG/PVD/ประกันสังคม/กบข/กอช/ประกันบำนาญ เชิงโครงสร้าง + ชี้แหล่งทางการให้ยืนยันล่าสุด)
- progressive disclosure (SKILL.md + references), ผูก sgui-output-style, disclaimer ประจำ (ชี้ CFP)

### Design decisions (locked)
- ขอบเขต VEH: **Thai-first + global-friendly**
- Lifecycle: **เต็ม (accumulation + decumulation)** — DRAW เป็นโมดูลเสริมเรียกเมื่อ near/in retirement
- การคำนวณ: **compute projection + 3 scenario** (ไม่ใช่ framing-only)

### Deferred / backlog (ยังไม่ทำใน v1)
1. ตัวอย่าง worked example เต็ม 1 เคส (BASE→DEC) ใน references/ เพื่อ anchor พฤติกรรม
2. evals/ สำหรับ regression (เช่น เคสที่ต้อง refuse การฟันธง, เคส real/nominal trap, เคสขาด input)
3. ทบทวนช่วงค่าสมมติฐานตัวอย่างใน projection.md ไม่ให้ถูกอ่านเป็น "ตัวเลขทางการ"
