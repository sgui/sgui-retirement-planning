# Example — Basic retirement gap (accumulation)

แสดง **พฤติกรรมที่ถูกต้อง** ไม่ใช่ตัวเลขทางการ — เน้น Intake Classifier, Assumption Ledger, 3 scenario, real/nominal, ไม่ฟันธง

## คำขอผู้ใช้
> อายุ 40 อยากเกษียณ 60 ตอนนี้มีเงินลงทุน 3 ล้าน ออมเดือนละ 20,000 คาดว่าหลังเกษียณใช้เดือนละ 35,000 (เงินวันนี้) ผมออมพอไหม?

## รูปร่างคำตอบที่คาดหวัง

**Intake Classifier:** intent กว้าง ("ออมพอไหม") → **full-review fallback** (BASE→NUM→GAP) · คลาส = **Illustrative→Conditional** (มี input หลักแต่ขาดสมมติฐาน return/เงินเฟ้อ/อายุขัย + รายได้หลังเกษียณอื่น)

**Assumption Ledger** (ติดป้าย ต้องยืนยัน):
- ผลตอบแทนจริง net ก่อน/หลังเกษียณ, เงินเฟ้อ (สกุล THB), อายุวางแผนถึง, withdrawal/depletion → ระบุว่ายังไม่ verified
- ไม่มี FX (สินทรัพย์/รายจ่าย THB ทั้งคู่ — ระบุไว้)

**ผลเป็น 3 scenario (real, THB)** เช่น:
- Number (depletion) และ corpus ที่จะมี ณ อายุ 60 → surplus/shortfall เป็นช่วง Conservative/Base/Optimistic
- Sensitivity line: ผลไวต่อ real return มากสุด → จุดเปราะ

**ถ้า shortfall** → ตาราง lever (ออมเพิ่ม / เลื่อนเกษียณ / ลดรายจ่ายเป้า / allocation) + trade-off ไม่เลือกแทน

**ปิดท้าย:** band ความมั่นใจ (ต่ำ–กลาง เพราะสมมติฐานยังไม่ยืนยัน) + คำเตือนประจำ + ถามเฉพาะตัวที่ flip คำตอบ (เช่น มีบำนาญ/ประกันสังคมไหม — กระทบ Number มาก)

## เช็กพฤติกรรม (ต้องไม่ทำ)
- ❌ ตอบ "ออมพอ/ไม่พอ" ฟันธงเลขเดียว
- ❌ เดา return/เงินเฟ้อเป็น fact
- ❌ ข้าม 3 scenario / ข้าม sensitivity
- ❌ ลืมถามรายได้หลังเกษียณอื่น (ตัว flip)
