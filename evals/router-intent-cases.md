# Smoke evals — Router intent

เช็กว่า routing + fallback ทำงานตามสัญญา (รันด้วยมือ/สายตาก่อน ship)

| # | input | คาดหวัง routing | ผ่านเมื่อ |
|---|---|---|---|
| R1 | "ผมออมพอไหม" (กว้าง) | **full-review fallback** BASE→NUM→GAP→… | ไม่ route แคบไป GAP อย่างเดียว |
| R2 | "เกษียณได้หรือยัง" (กว้าง) | full-review fallback | เห็นภาพครบ ไม่ตอบครึ่งเดียว |
| R3 | "ควรวางแผนยังไง" (กว้าง) | full-review fallback | — |
| R4 | "ช่วยคำนวณ Number ให้หน่อย" (แคบ) | route NUM | ไม่ลาก chain เต็มโดยไม่จำเป็น |
| R5 | "อายุ 58 จะถอนเงินยังไง" | **DRAW-first** | DRAW มาก่อน ALLOC + พูด sequence risk |
| R6 | "RMF กับ SSF ต่างกันยังไง" | VEH + เตือนยืนยันกฎล่าสุด | ไม่ฝังวงเงิน/กฎเป็น fact |
| R7 | "เกษียณ 52 บำนาญเริ่ม 60" | GAP + **bridge บังคับ** | โมเดล bridge ไม่ลืม |

## Fail conditions
- กว้างแต่ route แคบ (under-deliver)
- near-retirement แต่ไม่ DRAW-first / ไม่พูด sequence risk
- vehicle question แต่ฝังกฎภาษีเป็น fact
- เกษียณก่อนรายได้เริ่มแต่ไม่มี bridge
