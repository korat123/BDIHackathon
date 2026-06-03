# Pitch Script — Patient Rescue Radar
### BDI Young Innovator Hackathon 2026 | KMUTT
**Video Pitch 3 นาที | ครอบคลุม Rubric 100 คะแนน**

---

## SECTION 1: HOOK & PROBLEM
*(เวลา: 0:00 – 0:50 | Rubric: Problem & Customer Insight 20 คะแนน)*

---

### Slide 1 — Hook (Cover)
**Title:** Patient Rescue Radar
**Tagline:** เปลี่ยนข้อมูลเป็นเรดาร์กู้ชีพ — พลิกวิกฤตผู้ป่วยหลุดระบบก่อนจะสายเกินแก้

---

### Slide 2 — ปัญหา: ขนาดของปัญหา

**Main Message:** ประเทศไทยมีผู้ป่วย NCD กว่า 23 ล้านคน แต่ดูแลได้จริงไม่ถึง 1 ใน 8

**Key Numbers (ใส่ใหญ่ในสไลด์):**
- **23 ล้านคน** — ผู้ป่วยเบาหวาน (6.1 ล้าน) + ความดันโลหิตสูง (17.5 ล้าน) ในไทย
- **11.7%** — ผู้ป่วยเบาหวานที่ควบคุมโรคได้จริงตามเป้า
- **15.7%** — ผู้ป่วยความดันโลหิตสูงที่ควบคุมโรคได้จริง
- **88.3%** ของผู้ป่วยเบาหวาน ยังไม่ได้รับการดูแลอย่างมีประสิทธิภาพ

**Supporting Text:**
> ตัวเลขเหล่านี้ไม่ใช่แค่ความล้มเหลวทางการแพทย์ แต่คือโศกนาฏกรรมที่เกิดขึ้นทุกวัน ผู้ป่วยที่ควรได้รับการรักษากลับหายออกไปจากระบบก่อนที่จะเกิดอะไรขึ้น

---

### Slide 3 — ปัญหา: Cascade Effect ที่น่ากลัว

**Main Message:** ผู้ป่วยไม่รู้ตัวว่ากำลังเดินสู่หายนะ

**Cascade Diagram ที่ควรแสดงในสไลด์:**
```
ผู้ป่วย NCD ไม่รู้สึกเจ็บปวดในระยะแรก
        ↓
หยุดมาพบแพทย์ (LTFU) — ขาดนัด 6 เดือน+
        ↓
ยาหมด / น้ำตาล-ความดัน rebound สูงกะทันหัน
        ↓
STROKE / หัวใจล้มเหลว / ไตวาย
        ↓
เข้า ICU ฉุกเฉิน — ค่าใช้จ่าย 200,000–500,000 บาท/ครั้ง
```

**Key Stats:**
- **20–37%** ของผู้ป่วย NCD ขาดนัดและหลุดจากระบบ (LTFU) ทุกปี
- ค่า **dialysis 540,000 บาท/คน/ปี** — บัตรทองแบกรับอยู่
- ค่าใช้จ่าย NCD ต่อเศรษฐกิจไทย **1.6 ล้านล้านบาท/ปี**

**Pain Point ที่ต้องชี้ให้ชัด:**
> ระบบปัจจุบันรู้ว่าผู้ป่วยหายไปก็ต่อเมื่อพวกเขาเข้า ICU แล้ว นั่นสายเกินไป

---

## SECTION 2: SOLUTION & VALUE PROPOSITION
*(เวลา: 0:50 – 1:40 | Rubric: Solution & Value Proposition 20 คะแนน)*

---

### Slide 4 — โซลูชัน: Patient Rescue Radar คืออะไร

**Main Message:** ระบบ AI ที่ไม่รอให้ผู้ป่วยหาย แต่รู้ล่วงหน้าว่าใครกำลังจะหาย

**Value Proposition (3 ประโยคสำคัญ):**

1. **ทำอะไร:** วิเคราะห์ข้อมูล EMR ของผู้ป่วย NCD 220,000+ คน แล้วส่ง risk score พร้อมเหตุผล 3 ข้อให้ Case Manager ก่อนผู้ป่วยจะหายออกจากระบบ

2. **ทำงานอย่างไร:** ใช้ "ช่องว่างในข้อมูล" (Missing Data) เป็น signal หลัก ไม่ใช่แค่ค่าตรวจ — ถ้าผู้ป่วยไม่มาพบแพทย์ ข้อมูล EMR จะว่าง ช่องว่างนั้นคือสัญญาณเตือน

3. **ใครใช้:** พยาบาล/Case Manager เห็น ranked list ผู้ป่วยเสี่ยงสูง และ อสม. ลงพื้นที่ติดตามได้ตรงจุด

---

### Slide 5 — นวัตกรรม: จุดที่ต่างจากระบบอื่น

**Main Message:** เราอาศัยข้อมูลที่ระบบอื่นทิ้งทุกวัน

---

**ทำไมระบบเดิมล้มเหลว — 3 ข้อเท็จจริง:**

1. **ไม่มี ML-based LTFU system สำหรับ NCD ในไทย** — ปัจจุบันใช้ manual scheduling และโทรหาผู้ป่วยทุกคนโดยไม่มี risk stratification
2. **Gap rule อย่างเดียวไม่พอ** — การ flag แบบง่ายๆ ว่า "ไม่มาเกิน 6 เดือน = LTFU" มี AUC เพียง ~0.65 หมายความว่าคาดผิดถึง 35% ของกรณี
3. **ไม่ scale** — Case Manager 1 คนดูแลผู้ป่วยได้แค่ ~200–300 คน/เดือน ถ้าต้องโทรหาทุกคนคือเป็นไปไม่ได้

---

**3 นวัตกรรมที่แท้จริง — พร้อมหลักฐานวิชาการ:**

**① MNAR as Feature — ข้อมูลที่หายไปคือข้อมูลที่สำคัญที่สุด**

ข้อมูล EMR ของเราหายไป 80–90% ในส่วน lab — ซึ่งไม่ใช่ random noise แต่คือ **MNAR (Missing Not At Random)**: ผู้ป่วยไม่มา → ไม่มีค่าบันทึก → ช่องว่างนั้นคือ signal

ระบบทั่วไปเห็น missing data → impute ด้วย mean/median → ทำลาย signal ที่สำคัญที่สุดทิ้ง

เราทำ: ใช้ `lab_sparsity_ratio`, `max_consecutive_gap`, `visit_frequency` เป็น **top features** ของโมเดล

> **หลักฐาน:** Sperrin et al. (JMIR Med Inform 2021) พบว่า missing vitals ใน ICU เป็น early warning signal ที่ improve AUC; Nijman et al. (JMIR 2025) review 50+ papers สรุปว่า "including missingness indicators **consistently improves** model performance"

**② SHAP → Clinical Language — Actionable ไม่ใช่แค่ Explainable**

ระบบ ML ทั่วไป: ให้ SHAP plot → แพทย์งงว่าต้องอ่านยังไง

เราแปล SHAP values → **"3 ประโยคภาษาคลินิก"** ที่ Case Manager อ่านแล้ว act ได้ทันทีโดยไม่ต้องตีความ

> **หลักฐาน:** npj Digital Medicine 2025 (doi:10.1038/s41746-025-01958-8) พบว่า SHAP plot ดิบๆ ยังยากสำหรับแพทย์ — วิธีที่ดีกว่าคือแปลเป็นภาษาธรรมชาติ + ระดับความรุนแรง ซึ่งเป็นสิ่งที่เราทำ

**③ Cost Asymmetry — ทำไม precision 50% ก็คุ้มค่า**

| ประเภทข้อผิดพลาด | ต้นทุน |
|-----------------|--------|
| False Positive (โทรหาคนที่ไม่ LTFU จริง) | เสียเวลา Case Manager **5 นาที** |
| False Negative (พลาดคน LTFU จริง) | ICU **200,000–500,000 บาท** |

→ Asymmetry นี้ทำให้ active monitoring คุ้มค่าแม้จะมี precision ต่ำ และ justify ROI 134× ของระบบ

---

**Performance Benchmark — เปรียบเทียบกับ baseline:**

| โมเดล | AUC คาดหวัง | หมายเหตุ |
|-------|------------|---------|
| Gap threshold (naive, ≥6 เดือน) | ~0.65 | วิธีที่ใช้อยู่ในปัจจุบัน |
| Logistic Regression | ~0.72–0.78 | clinical ML standard |
| **XGBoost + MNAR features (เรา)** | **≥ 0.80** | เป้าหมายตาม literature |

**Analogous proof ที่ใกล้เคียงที่สุด:**
> Azmeraw et al. (BMC Medical Informatics 2025) ทำนาย LTFU ใน HIV/ART **115,000+ คน** ด้วย XGBoost + visit pattern features ได้ AUROC **85.9%** (95% CI: 82.0–89.6)
> → เราใช้ approach เดียวกัน แต่กับ NCD dataset ของไทยที่ใหญ่กว่า (220K+ คน)

---

### Slide 6 — วิธีทำงาน (Workflow)

**Title:** วิธีทำงาน

**Flow สั้นๆ สำหรับสไลด์:**
```
EMR ผู้ป่วย 220,000+ คน
        ↓
สกัด 12 Features (visit pattern, lab sparsity, medication gap)
        ↓
XGBoost ทำนาย Risk Score 0–100
        ↓
SHAP แปลงเป็น 3 เหตุผลภาษาคลินิก
        ↓
High Risk ≥70: Case Manager โทรหา + อสม. ลงพื้นที่
Low Risk <70: ดูแลตามปกติ
```

**ตัวอย่าง Output ที่ Case Manager เห็น:**
```
ผู้ป่วย DM-00142 | ความเสี่ยง: 87% 🔴
(1) ไม่มาพบแพทย์ 14 เดือน
(2) ไม่มีผล HbA1c ใน 2 ปี
(3) ความดันครั้งล่าสุด 158 mmHg
→ แนะนำ: โทรติดต่อภายใน 7 วัน
```

---

## SECTION 3: PROJECT VIABILITY & IMPACT
*(เวลา: 1:40 – 2:20 | Rubric: Project Viability & Impact 20 คะแนน)*

---

### Slide 7 — ความเป็นไปได้ทางเทคนิค

**Main Message:** สร้าง Prototype ได้จริงใน 2 วัน Onsite — ไม่ใช่แค่ concept

**Technical Feasibility (ตอบคำถาม "ทำได้จริงไหม?"):**

- **XGBoost + Python** — train เร็ว บน dataset 220K คนได้ภายใน hackathon 2 วัน
- **220,000+ คน** — เบาหวาน 70,000 + ความดัน 150,000 คน (พร้อมใช้ onsite)
- **Prototype verified** — ทดสอบบน sample 200 คน ทุก step ทำงานได้แล้ว
- **On-premise deploy** — ข้อมูลผู้ป่วยไม่ออก cloud, PDPA compliant
- **Output ชัดเจน** — risk score CSV + SHAP plot + patient risk card สำหรับ Case Manager

**Technology Stack (ถ้าต้องการแสดงในสไลด์):**
Python | XGBoost | SHAP | scikit-learn | FastAPI | Streamlit

---

### Slide 8 — ผลกระทบ & ROI

**Main Message:** ลงทุน 2 ล้าน ประหยัดงบประมาณ 270–540 ล้านบาท/ปี

**Key Impact Numbers (ใส่ใหญ่ในสไลด์):**

| ตัวชี้วัด | ค่าที่คาดหวัง |
|-----------|-------------|
| ลด LTFU Rate | **15–20%** ภายใน 1 ปี |
| ลดภาระ Case Manager | **25%** ในการคัดกรอง |
| ROI | **134 เท่า** (conservative) |
| ประหยัด dialysis | **540,000 บาท/คน/ปี** |

**ROI Calculation (แสดงเหตุผล):**
```
ลงทุน: 2,000,000 บาท (intervention 10,000 คน/ปี)

ถ้าป้องกัน dialysis ได้ 5%:
500 คน × 540,000 บาท/ปี = 270,000,000 บาท

ROI = 270M / 2M = 134 เท่า
```

**กลุ่มที่ได้ประโยชน์:**
- ผู้ป่วย NCD 220,000+ คน — ได้รับการดูแลต่อเนื่อง ลดภาวะแทรกซ้อน
- Case Manager / พยาบาล — ลดภาระงานคัดกรอง 25%
- อสม. 1.04 ล้านคน — ลงพื้นที่ได้ตรงจุด
- สปสช. / ระบบสาธารณสุข — ลดต้นทุน dialysis และ ICU

---

## SECTION 4: TEAM CAPABILITY & CALL TO ACTION
*(เวลา: 2:20 – 3:00 | Rubric: Team Capability 10 + Delivery 30 คะแนน)*

---

### Slide 9 — ทีม

**Main Message:** ทีมที่มีทั้งสาย Data Science, Healthcare, Product และ Business

**Team Members:**

| ชื่อ | บทบาท | ความเชี่ยวชาญ |
|------|-------|--------------|
| กอ | Technical & AI Architect | XGBoost, SHAP, EMR data pipeline |
| จีน | System Design & Workflow | System architecture, data flow |
| ข้าวฟาง | Product & Innovation | Healthcare operations, UX |
| ขฟ | Business & Domain Lead | NCD clinical context, impact analysis |
| แบงค์/เฟิม | *(ตาม role จริง)* | |

**Why This Team:**
> ทีม KMUTT วิศวกรรมคอมพิวเตอร์ ปี 1 ที่ผสมผสานทักษะ data science, healthcare data, และ product design — พร้อม deliver prototype จริงใน 2 วัน

---

### Slide 10 — Roadmap

**Title:** แผนพัฒนา

**4 Phases:**
- **Phase 1 — Hackathon (2 วัน):** Prototype 220K คน | XGBoost + SHAP | Patient Risk Card demo
- **Phase 2 — Pilot:** ทดสอบกับโรงพยาบาลพันธมิตร | FastAPI + Streamlit dashboard
- **Phase 3 — Scale:** เชื่อม HIS โรงพยาบาล | On-premise deploy | PDPA compliant
- **Phase 4 — National:** ขยายผลผ่าน Smart NCD Network 2027

---

### Slide 11 — Call to Action (Closing)

**Title:** Patient Rescue Radar

**Big Closing Message:**
> "มีผู้ป่วย NCD กว่า 220,000 คนที่รอให้เราช่วยพวกเขากลับมารักษาก่อนสายเกินไป
> เรามีข้อมูล เรามีเทคโนโลยี และเรามีระบบสาธารณสุขที่พร้อม
> สิ่งที่ขาดคือเรดาร์ที่จะบอกว่าใครกำลังจะหายไป — นั่นคือสิ่งที่เราสร้าง"

**Tagline:**
> **เปลี่ยนข้อมูลเป็นเรดาร์กู้ชีพ | Patient Rescue Radar**

**Team:** กอ | จีน | ข้าวฟาง | ขฟ | KMUTT
**BDI Young Innovator Hackathon 2026**

---

## หมายเหตุสำหรับการ Present (Delivery Tips)

เพื่อให้ได้คะแนน **Pitching Delivery สูงสุด (30 คะแนน)**:

1. **Hook ใน 10 วินาทีแรก** — เปิดด้วยตัวเลข shock เช่น "88% ของผู้ป่วยเบาหวานในไทยยังไม่ได้รับการดูแลที่ได้ผล"

2. **ใช้ story ผู้ป่วยสมมติ** — "ลุงสมชาย อายุ 67 ปี เป็นเบาหวานมา 5 ปี แต่ไม่รู้ว่าตัวเองกำลังจะ stroke..."

3. **ตัวเลข ROI ต้องพูดดัง ๆ** — "134 เท่า — ลงทุน 2 ล้าน ประหยัด 270 ล้าน"

4. **Closing ต้องจำได้** — ทิ้งท้ายด้วย tagline เดียว พูดช้า ๆ ชัด ๆ

5. **ใน 3 นาที** — แต่ละสไลด์ใช้เวลาไม่เกิน 20-25 วินาที

---

## สรุปตาม Rubric

| Rubric | เนื้อหาที่ครอบ | Slide |
|--------|--------------|-------|
| Problem & Customer Insight (20pt) | 23M คน, 88.3% control gap, cascade effect, 1.6 ล้านล้าน | 2, 3 |
| Solution & Value Proposition (20pt) | MNAR insight, XGBoost+SHAP, Patient Risk Card example | 4, 5, 6 |
| Project Viability & Impact (20pt) | 2-day prototype, 220K dataset, ROI 134x, 15-20% LTFU reduction | 7, 8 |
| Team Capability (10pt) | 5 คน ทักษะครบ, บทบาทชัด, KMUTT | 9 |
| Pitching Delivery (30pt) | Hook, story, ตัวเลข, tagline closing | ทุก slide |
