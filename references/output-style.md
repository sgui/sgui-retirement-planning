# Output Style — Summary Card Spec

ใช้ไฟล์นี้เฉพาะ **เมื่อผู้ใช้ขอผลแบบ artifact** (เช่น "ทำเป็นการ์ดสรุป", "ขอ HTML", "เก็บไว้เทียบ") — ไม่ต้องทำ HTML ถ้าผู้ใช้แค่คุยในแชต

## หลักการ: inherit ก่อน ไม่เขียนซ้ำ

**สไตล์ภาพทั้งหมดยึดตาม `sgui-output-style` เป็น source of truth เดียว** — อย่า hardcode สี/ฟอนต์/spacing ซ้ำในไฟล์นี้ ให้ดึงจากที่นั่น:

- Dark theme เป็นค่าเริ่มต้น
- **Noto Sans Thai** เป็นฟอนต์หลัก (โหลดตามวิธีของ sgui-output-style)
- **CSS variables บังคับ** — สี/ระยะ/ขนาดทุกค่าอ้างผ่านตัวแปร ไม่ใส่ค่าดิบ
- Self-contained HTML ไฟล์เดียว ไม่พึ่ง asset ภายนอกที่ไม่จำเป็น
- ไม่ใช้ browser storage (localStorage/sessionStorage)

ไฟล์นี้เพิ่มแค่ **โครงการ์ดเฉพาะงานวางแผนเกษียณ** ทับบนฐานนั้น

## Semantic tokens ที่ต้องมี (นิยามที่ `:root` ถ้า base ยังไม่มี)

| แนวคิดใน skill | ตัวแปร | ใช้สื่อ |
|---|---|---|
| `[ข้อเท็จจริง]` | `--label-fact` | กลาง/info — ข้อมูลที่ผู้ใช้ให้ |
| `[คำนวณ]` | `--label-calc` | accent — ผลจาก math + สมมติฐาน |
| `[วิเคราะห์]` | `--label-analysis` | accent รอง — การอนุมาน |
| `[คาดเดา]` | `--label-speculation` | muted/จาง — ความแน่นอนต่ำ (เช่นสมมติฐานผลตอบแทน) |
| Shortfall / ธงเสี่ยง | `--signal-danger` | ต้องระวัง |
| Surplus / on track | `--signal-positive` | เป็นบวก |
| ความมั่นใจ สูง / กลาง / ต่ำ | `--conf-high` / `--conf-mid` / `--conf-low` | positive / warning / danger |
| Missing data / ยังไม่ยืนยัน | `--signal-muted` | เทาจาง |

## Card anatomy (โครงบังคับ)

เรียงลำดับนี้เสมอ:

1. **Header** — หัวข้อแผน + **ID badge ของโมดูล** (BASE/NUM/GAP/ALLOC/VEH/DRAW/RSK/DEC) + วันที่ + **ป้าย real/nominal**
2. **Confidence badge** — สูง/กลาง/ต่ำ ใช้สีตาม `--conf-*` + เหตุผล 1 บรรทัด
3. **Assumption Ledger** (ถ้ามี `[คำนวณ]`) — ตารางสมมติฐาน + ป้าย "ยืนยันแล้ว / ต้องยืนยัน"
4. **Body** — เนื้อตาม output contract ของโมดูล ทุกประโยคที่ติดป้ายครอบด้วย label chip สีตามตารางบน
5. **Scenario block** (ถ้ามี projection) — แสดง **3 scenario เป็นช่วง** (Conservative/Base/Optimistic) + บรรทัด sensitivity ("ตัวแปรไหนเปราะสุด")
6. **Callouts** (ถ้ามี) — กล่อง shortfall/ธงเสี่ยง (`--signal-danger`) และกล่อง "ข้อมูล/สมมติฐานที่ขาด" (`--signal-muted`) แยกชัด
7. **Traceability line** (ถ้ามาจาก chain) — เช่น `GAP-shortfall → DEC-trade-off` แสดงเป็น mono/เล็ก
8. **Disclaimer footer** — **บังคับทุกใบ** ห้ามตัดออก

## ตัวอย่างโครง (สเกเลตัน — ค่าจริงมาจากตัวแปร)

```html
<!-- ฐานสไตล์/ฟอนต์/ตัวแปร: ตาม sgui-output-style -->
<article class="rp-card">
  <header class="rp-head">
    <span class="rp-title">แผนเกษียณ — อายุเป้าหมาย 60</span>
    <span class="rp-badge rp-badge--module">GAP</span>
    <span class="rp-badge rp-badge--unit">real (เงินวันนี้)</span>
    <time class="rp-date">2569-06-29</time>
  </header>

  <div class="rp-conf rp-conf--mid">ความมั่นใจ: กลาง — input ครบ แต่สมมติฐานผลตอบแทนยังไม่ยืนยัน</div>

  <table class="rp-ledger"><!-- Assumption Ledger: return/เงินเฟ้อ/อายุขัย + ยืนยันหรือยัง --></table>

  <section class="rp-body">
    <p><span class="chip chip--fact">ข้อเท็จจริง</span> เงินก้อนปัจจุบัน … เงินออม/เดือน …</p>
    <p><span class="chip chip--calc">คำนวณ</span> เส้นทางปัจจุบันจะมี … vs Number …</p>
  </section>

  <section class="rp-scenarios"><!-- 3 scenario เป็นช่วง + sensitivity line --></section>

  <aside class="rp-callout rp-callout--danger">
    <b>Shortfall</b> ~30% ใน Base · ~45% ใน Conservative
  </aside>
  <aside class="rp-callout rp-callout--muted">
    <b>ยังไม่ยืนยัน</b> สมมติฐานผลตอบแทน → ผลไวต่อค่านี้มากสุด
  </aside>

  <p class="rp-trace">GAP-shortfall → DEC-trade-off</p>

  <footer class="rp-disclaimer">
    เพื่อการศึกษา/ช่วยจัดระบบคิด ไม่ใช่คำแนะนำการเงิน/ภาษีรายบุคคล ตัวเลขมาจากสมมติฐานที่ปรับได้ ควรยืนยันกฎภาษี/ผลิตภัณฑ์กับแหล่งทางการ และพิจารณาปรึกษานักวางแผนมีใบอนุญาต
  </footer>
</article>
```

## กฎเฉพาะของการ์ดวางแผนเกษียณ

- **Disclaimer footer ห้ามหายไม่ว่ากรณีใด** — เป็นส่วนที่ทำให้ "เครื่องมือช่วยคิด ไม่ใช่คำแนะนำ" เป็น load-bearing เชิงภาพ
- **ป้าย real/nominal ต้องอยู่บนหัวการ์ดเสมอ** — กันผู้ใช้อ่านตัวเลขผิดหน่วย
- **projection ต้องโชว์ 3 scenario** ในการ์ด — ห้ามแสดงเลขเดี่ยวให้ดูแม่นเกินจริง
- **อย่าใช้สีเขียว/แดงสื่อ "ทำเลย/อย่าทำ"** — สีสื่อ surplus/shortfall, ความมั่นใจ, ประเภทข้อมูล เท่านั้น ไม่ใช่คำสั่ง
- ถ้า confidence ต่ำ ให้ทั้งใบดู "เบาลง" (เช่น body opacity ตาม `--conf-low`) เพื่อไม่ให้ผู้ใช้เผลอเชื่อผลที่สมมติฐานยังไม่ยืนยัน
- ตัวเลขทุกตัวต้องมาจาก body ที่ผ่าน Operating Rules + Assumption Ledger แล้ว — การ์ดเป็นชั้นนำเสนอ ไม่ใช่ที่สร้างตัวเลขใหม่
