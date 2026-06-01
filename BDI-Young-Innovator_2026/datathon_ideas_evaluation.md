# Datathon Ideas Evaluation — BDI Hackathon 2026 (Health Track)

**ทีม:** กอ, แบงค์, จีน, ข้าวฟาง, เฟิม — KMUTT  
**เกณฑ์การให้คะแนน:** คะแนนเต็ม 130 (Phase 2: 100 + EMR Track: 30)

---

## สรุปคะแนนทั้ง 9 ไอเดีย

| # | ชื่อไอเดีย | คะแนน | สถานะ |
|---|-----------|-------|-------|
| 1 | SmartWean AI | 122/130 | — |
| **2** | **DynaGlycemia Trajectory** | **124/130** | **✅ เลือกแล้ว** |
| 3 | Multimodal Care Sync | 109/130 | — |
| 4 | Hidden Epidemic Predictor | 118/130 | — |
| 5 | Polypharmacy Synergy Optimizer | 114/130 | — |
| 6 | Cardiorenal Domino Point | 123/130 | — |
| **7** | **Patient Rescue Radar** | **126/130** | **✅ เลือกแล้ว (Champion)** |
| 8 | Daily Wellness Guard | 90/130 | ❌ ไม่แนะนำ |
| 9 | PreciseDose AI | 120/130 | — |

> Idea 8 ถูก flag: dataset ไม่รองรับ (ไม่มี daily behavioral logs ใน EMR)

---

## ไอเดียที่เลือก: รายละเอียดเต็ม

---

### ✅ Idea 7: Patient Rescue Radar (126/130) — Champion Idea

#### ปัญหา
ผู้ป่วย NCD (เบาหวาน/ความดัน) ไม่รู้สึกเจ็บปวดในช่วงแรก → หยุดมาพบแพทย์ (Lost-to-Follow-Up: LTFU)  
→ ความดัน/น้ำตาลกลับสูงกะทันหัน → หลอดเลือดแข็งตัว → **stroke/MI เฉียบพลัน**  
นี่คือสาเหตุหลักที่ผู้ป่วยต้องเข้า ICU แบบฉุกเฉิน ซึ่งป้องกันได้

#### แนวทางแก้ไข
1. วิเคราะห์ pattern การขาดนัดและความผิดปกติของ missing data ใน EMR
2. Feature engineering จาก:
   - `visit_frequency` — สัดส่วน periods ที่มาจริง
   - `max_consecutive_gap` — ช่วงหายนานที่สุด
   - slope ของ BP/glucose — trend deterioration ก่อน dropout
   - `lab_sparsity_ratio` — ความถี่ตรวจ lab (ต่ำ = ไม่มา)
   - `med_in_last_period` — รับยาต่อเนื่องหรือเปล่า
3. XGBoost ทำนายโอกาส LTFU → ranked list ของผู้ป่วยเสี่ยงสูง
4. ส่งรายชื่อให้ Case Manager / รพ.สต. ติดต่อเชิงรุกก่อนเกิดวิกฤต

#### ข้อมูลที่ใช้
- `vitalsign_sbp_<P>`, `vitalsign_dbp_<P>` — pattern การมาพบแพทย์
- `lab_hba1c_<P>`, `lab_fpg_<P>` — lab sparsity
- `med_*_<P>` — medication continuity
- `age`, `sex`, `dm_onset` — demographics

#### จุดเด่นที่กรรมการ highlight
- **ใช้ sparsity เป็น feature หลัก** — ข้อมูลที่หายไปคือ signal สำคัญที่สุด
- Operational impact สูงสุด — เปลี่ยน AI จาก clinical analysis เป็น **preventive healthcare operations**
- Feasibility ใน 2 วัน — feature engineering + XGBoost ทำได้ทันที
- กรรมการประเมินว่าเป็น Champion Idea อันดับ 1

---

### ✅ Idea 2: DynaGlycemia Trajectory (124/130)

#### ปัญหา
ผู้ป่วยเบาหวาน+ความดัน ทำลายหลอดเลือดอย่างเงียบๆ ทีละน้อย  
HbA1c แบบ snapshot จุดเดียวไม่สามารถพยากรณ์การเสื่อมของไต (CKD/ESRD) ก่อนถึงจุดที่ย้อนกลับไม่ได้  
ค่าฟอกไตดึงงบประมาณบัตรทองปีละหลายหมื่นล้านบาท

#### แนวทางแก้ไข
1. แปลง wide-format 4,000+ คอลัมน์เป็น **longitudinal curves**
2. วิเคราะห์ **trajectory ของ HbA1c และ BP** ข้ามเวลา (ไม่ใช่แค่ snapshot)
3. **Survival XGBoost / Cox Regression** ทำนาย eGFR decline ภายใน 2 ปี
4. แจ้งเตือนแพทย์ปรับยาก่อนถึงจุดที่ไตเสื่อมถาวร

#### ข้อมูลที่ใช้
- `vitalsign_sbp_<P>`, `vitalsign_dbp_<P>`, `vitalsign_bmi_<P>`
- `lab_hba1c_<P>`, `lab_fpg_<P>`
- `co_ckd_<P>` — outcome variable

#### จุดเด่น
- Feasibility สูงมาก — pipeline เสร็จได้ใน 2 วัน
- Academic rigor ดี — ใช้ functional data analysis + survival model
- ผลกระทบสูง — ลดภาระ dialysis ระดับชาติ

---

## ไอเดียที่ไม่เลือก: สรุปสั้น

### Idea 1: SmartWean AI (122/130)
**ปัญหา:** ผู้ป่วย ICU บน ventilator นานเกินจำเป็น — ค่าใช้จ่ายสูง ความเสี่ยง pneumonia เพิ่ม  
**แนวทาง:** วิเคราะห์ waveform 25Hz จาก ventilator → ทำนาย readiness ถอด tube  
**เหตุที่ไม่เลือก:** ต้องการ domain expertise ด้าน ICU/respiratory สูง, feature engineering ซับซ้อนกว่า

### Idea 3: Multimodal Care Sync (109/130)
**ปัญหา:** ผู้ป่วย NCD มีหลายโรคร่วม แต่แพทย์แต่ละสาขาดูแยกกัน  
**แนวทาง:** รวม signal จากทุก dataset → สร้าง unified patient profile  
**เหตุที่ไม่เลือก:** complexity สูง, feasibility ใน 2 วันต่ำ

### Idea 4: Hidden Epidemic Predictor (118/130)
**ปัญหา:** ผู้ป่วยที่ยังไม่ถูกวินิจฉัยแต่มีความเสี่ยงสูง (pre-DM, pre-HTN)  
**แนวทาง:** ใช้ vitalsign + lab baseline ทำนายผู้ที่จะพัฒนาเป็นโรค  
**เหตุที่ไม่เลือก:** dataset เป็น diagnosed patients อยู่แล้ว, label ยาก

### Idea 5: Polypharmacy Synergy Optimizer (114/130)
**ปัญหา:** ผู้ป่วยสูงอายุกินยาหลายชนิด — drug interaction เป็นสาเหตุ adverse event  
**แนวทาง:** วิเคราะห์ medication columns → flag combination ที่อันตราย  
**เหตุที่ไม่เลือก:** ต้องการ pharmacology database ภายนอก, data ที่มีอาจไม่ครอบคลุม

### Idea 6: Cardiorenal Domino Point (123/130)
**ปัญหา:** หัวใจและไตเสื่อมพร้อมกันแบบ cascade — ตรวจจับช้าเกินไป  
**แนวทาง:** ติดตาม biomarker ของทั้งสองระบบพร้อมกัน  
**เหตุที่ไม่เลือก:** Idea 2 ครอบคลุม CKD aspect แล้ว, เกทับกัน

### Idea 8: Daily Wellness Guard (90/130) ❌
**ปัญหา:** ต้องการ daily behavioral logging  
**เหตุที่ไม่เลือก:** **dataset ไม่รองรับ** — ไม่มี daily behavioral data ใน EMR

### Idea 9: PreciseDose AI (120/130)
**ปัญหา:** ผู้ป่วยได้รับยาขนาดไม่เหมาะสม → under/over-treatment  
**แนวทาง:** ใช้ response ของ BP/glucose ต่อยา → recommend dose adjustment  
**เหตุที่ไม่เลือก:** ต้องการ causal inference methodology ที่ซับซ้อน

---

## เกณฑ์การให้คะแนนของการแข่งขัน

### Phase 2 (100 คะแนน)
| เกณฑ์ | คะแนน | รายละเอียด |
|-------|-------|-----------|
| Feasibility & Tech Plan | 30 | สร้าง prototype ได้ใน 2 วัน onsite |
| Problem & Data Usage | 25 | ใช้ dataset ได้ตรงจุด, data cleaning ถูกต้อง |
| Impact & Value | 20 | แก้ปัญหาสุขภาพไทยจริง, scale ได้ |
| Creativity & Novelty | 15 | มุมมองใหม่, Deep Tech ชัดเจน |
| Team Readiness | 10 | แบ่งหน้าที่ชัด, พัฒนา parallel ได้ |

### EMR Track เพิ่มเติม (30 คะแนน)
| เกณฑ์ | คะแนน |
|-------|-------|
| Medical Data Processing (pipeline, จัดการ sparsity ถูกต้อง) | 10 |
| Health Data Ethics (PDPA, ไม่มี bias, privacy) | 10 |
| Clinical Value (ยืด lifespan, ช่วยแพทย์ตัดสินใจ) | 10 |
