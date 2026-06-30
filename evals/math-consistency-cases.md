# Smoke evals — Math consistency

เช็ก correctness ของการคำนวณ + guardrail ตัวเลข (P0)

| # | เช็ก | ผ่านเมื่อ |
|---|---|---|
| M1 | อัตราต่อเดือน | ใช้ `i=(1+r)^(1/12)−1` ไม่ใช่ `r/12` (เว้นระบุ nominal APR) |
| M2 | pre vs post return | r (ก่อน) กับ rr (หลัง) ตั้งแยก ไม่สมมติเท่ากันเอง |
| M3 | net return | ผลตอบแทนเป็น net หลัง fee/ภาษี หรือเตือนชัดว่าเป็น gross |
| M4 | real/nominal | ทุกตัวเลขติดป้าย real หรือ nominal ไม่ปนกัน |
| M5 | FX | ไม่เอา return สกุลหนึ่งหักเงินเฟ้ออีกสกุลโดยไม่แปลง/ติดป้าย; liability โมเดลในสกุลที่ใช้จ่าย |
| M6 | หักรายได้อื่น | Number หักบำนาญ/ประกันสังคมก่อน ไม่พองเกินจริง |
| M7 | bridge | เกษียณก่อนรายได้เริ่ม → มี Bridge_need คำนวณแยก |
| M8 | 3 scenario | ทุก projection เป็นช่วง 3 เส้น + sensitivity line ไม่ใช่เลขเดียว |
| M9 | Assumption Ledger | output ที่มี `[คำนวณ]` แนบ ledger (ค่า/สกุล/real-nominal/verified) |
| M10 | PMT total vs additional | แยก `PMT_total` กับ `PMT_additional` ไม่ปนกัน |
| M11 | gross direction | ระบุว่า gross ทำให้ประเมิน corpus **สูงเกินจริง** (ไม่ใช่ต่ำ) |
| M12 | bridge time-base | post-bridge need discount กลับวันเกษียณ `×(1+rr)^(−b)` ก่อนรวม |
| M13 | ไม่นับเงินเฟ้อซ้ำ | โหมด real ไม่ปรับเงินเฟ้อเป็น dial แยก (เว้น nominal/หมวดเฉพาะ) |
| M14 | surplus guard | `max(0,…)` — surplus รายงานเป็น surplus ไม่แสดง PMT additional ติดลบ |
| M15 | multi-segment bridge | รายได้เริ่มหลายอายุ → แบ่ง segment คิด net W แยก ไม่ใช่ `b` เดียว |

## Fail = critical
M1, M4, M5, M7, M8 ผิด = ผลใช้ไม่ได้ (ตรง critical-fail ใน SKILL.md)
