# sgui-retirement-planning

Claude Skill ช่วย **จัดระบบความคิดเรื่องแผนเงินเกษียณ** สำหรับบุคคลทั่วไป — เครื่องมือ decision-support ไม่ใช่คำแนะนำการเงิน/ลงทุน/ภาษีรายบุคคล

Thai-first (มี global toggle) · ครอบคลุมทั้ง lifecycle (สะสม + ถอน) · ทุก projection ออกเป็น 3 scenario แยก real/nominal และระบุสกุลเงิน

## ใช้เมื่อ
- "ต้องมีเงินเท่าไรถึงเกษียณได้ / ตอนนี้ออมพอไหม / เกษียณอายุ X ได้ไหม"
- "ควรเคลียร์หนี้ก่อนหรือลงทุนก่อน" · "จัดพอร์ตเกษียณยังไง"
- "ควรใส่ RMF/SSF เท่าไร" (ชี้โครงสร้าง + ให้ยืนยันกฎล่าสุด)
- "เกษียณแล้วถอนยังไงไม่ให้หมดก่อน / เงินจะพอถึงกี่ขวบ" · FIRE / เกษียณก่อนรับบำนาญ (bridge)

## ขอบเขต & ความปลอดภัย
ให้กรอบการวางแผนเชิงการศึกษา ไม่ออกคำสั่งซื้อขาย ไม่ฟันธงตัวเลขเฉพาะบุคคล ไม่การันตีว่าเกษียณได้/ผลตอบแทน
กฎภาษี/วงเงิน/ผลิตภัณฑ์เป็นเรื่องเฉพาะเขตอำนาจและเปลี่ยนได้ — ผู้ใช้ต้องยืนยันกับแหล่งทางการ (สรรพากร/ประกันสังคม/กบข/กอช/บลจ.) และพิจารณาปรึกษานักวางแผนการเงินมีใบอนุญาต (เช่น CFP)

## โครงสร้าง
- `SKILL.md` — core: guardrail, Intake Classifier, routing + full-review fallback, refusal/redirect, response modes
- `references/` — modules (BASE/RSK), projection (NUM/GAP + bridge), allocation-vehicles (ALLOC + bucket layer + VEH), decumulation (DRAW), decision (DEC + action plan), output-style
- `examples/` — worked examples (basic gap, early-retirement bridge)
- `evals/` — smoke tests (router intent, math consistency)
- `CHANGELOG.md` · `VERSION.md` (ปัจจุบัน: **1.1.0**)

## ติดตั้ง
Settings → Customize → Skills (ใช้ได้ทั้ง chat / Cowork / add-ins)

## License
MIT (ดู `LICENSE`) — เพื่อการศึกษา ไม่ใช่คำแนะนำทางการเงินที่อยู่ภายใต้การกำกับ
