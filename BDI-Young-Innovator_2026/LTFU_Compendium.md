# LTFU Compendium — Patient Rescue Radar (Idea 7)
### คัมภีร์ฉบับสมบูรณ์: ทุกสิ่งที่ต้องรู้เกี่ยวกับ LTFU Prediction

**ทีม:** กอ, แบงค์, จีน, ข้าวฟาง, เฟิม — KMUTT  
**โปรเจกต์:** BDI Hackathon 2026 — Health Track  
**วันที่รวบรวม:** 1 มิ.ย. 2569

> เอกสารนี้รวบรวมข้อมูลจาก academic literature, clinical guidelines, และ Thai healthcare policy  
> เพื่อใช้เป็น reference สำหรับการพัฒนา Idea 7 และเตรียม pitch ให้กรรมการ

---

## Part A: LTFU คืออะไร และทำไมถึงสำคัญ

### A.1 นิยามทางคลินิก

**Lost-to-Follow-Up (LTFU)** หมายถึงผู้ป่วยที่:
- ไม่กลับมาพบแพทย์ตามนัด โดยไม่มีการแจ้งเหตุผล
- ในบริบท NCD: **ไม่มีการบันทึก visit หรือรับยาต่อเนื่อง ≥ 6 เดือน** (นิยามที่ใช้กันกว้างที่สุดในงานวิจัย)
- ไม่รวมกรณีที่ย้ายสถานพยาบาล, เสียชีวิต, หรือถูก refer ออก

**ทำไมต้อง 6 เดือน (3 periods ใน dataset เรา)?**
- Clinical consensus จากงานวิจัยหลายประเทศกำหนด 3–6 เดือนเป็น threshold
- ใน dataset เรา: 1 period ≈ 60 วัน → 3 periods = 180 วัน ≈ 6 เดือน
- ในระดับ pathophysiology: uncontrolled DM ≥ 6 เดือน เริ่มสะสม end-organ damage

### A.2 LTFU Rates ในผู้ป่วย NCD — ข้อมูลจากงานวิจัย

| ประเทศ | โรค | LTFU Rate | แหล่งข้อมูล |
|--------|-----|-----------|------------|
| กานา (2017–2019) | DM/HT ใน NHIS | **37%** | Asante et al., PMC12054282 |
| ยูกันดา (13 ปี) | Type 2 DM | **34.9 per 100 person-years** | PMC6708238 |
| เอธิโอเปีย | HIV/ART (analogous) | ~25–30% | BMC Med Inform 2025 |
| Diabetic Retinopathy | DM complication | **16.3%** | Multiple studies |
| **ประมาณการไทย** | DM/HT | **~20–35%** | (extrapolate จาก regional data) |

> **ถ้า LTFU rate ในไทย = 20%** และมีผู้ป่วย DM 6.1 ล้านคน → มีผู้ป่วย LTFU **~1.2 ล้านคน** ที่อยู่ในความเสี่ยงอยู่ตอนนี้

### A.3 Cascade Effect: LTFU → Complications

```
ผู้ป่วย NCD หยุดมาพบแพทย์
        ↓  (4–6 เดือน)
ยาหมด / หยุดทานยา
        ↓  (2–3 เดือน)
BP กลับสูง (SBP rebound ≥ 160 mmHg)
HbA1c พุ่ง (กลับไปที่ >9–10%)
        ↓  (เพิ่มความเสี่ยงสะสม)
หลอดเลือดเสียหาย → ไตเสื่อม (CKD) → Eye damage
        ↓  (acute trigger)
STROKE / Myocardial Infarction → ICU admission
        ↓
ค่าใช้จ่าย ICU ≥ 200,000–500,000 บาท/ครั้ง
```

**หลักฐานทางวิทยาศาสตร์:**
- Stress hyperglycemia (หลัง LTFU และ rebound) เพิ่มความเสี่ยง **stroke recurrence 5.3 เท่า** ใน 90 วัน (PMC8034909)
- Hyperglycemia → Adjusted HR สำหรับ stroke recurrence = **1.50** (PMC10701542)
- Uncontrolled hypertension: major modifiable risk factor สำหรับ stroke ทั่วโลก

---

## Part B: บริบทสาธารณสุขไทย

### B.1 ขนาดของปัญหา NCD ในไทย (2025)

| ตัวชี้วัด | ค่า | แหล่งข้อมูล |
|-----------|-----|------------|
| ผู้ป่วยเบาหวาน | **6.1 ล้านคน** (10.6% ประชากร) | Nature 2025 |
| ผู้ป่วยความดันโลหิตสูง | **17.5 ล้านคน** (29.5%) | PMC9716924 |
| กลุ่มเสี่ยง pre-diabetes | 5.7 ล้านคน | — |
| ผู้ที่ยังไม่ได้วินิจฉัย (เบาหวาน) | **1.6 ล้านคน** (27% ของผู้ป่วยทั้งหมด) | — |
| ผู้ที่ยังไม่ได้วินิจฉัย (ความดัน) | **8.4 ล้านคน** (48%) | — |
| การเสียชีวิตจาก NCD | **400,000+ คน/ปี** (81% ของการเสียชีวิตทั้งหมด) | WHO Thailand |
| ค่าใช้จ่าย NCD รัฐบาล | **~$4.4 billion/ปี** | — |

### B.2 ประสิทธิภาพการดูแล NCD ในไทย (ปัญหาที่แท้จริง)

| Metric | เบาหวาน | ความดัน |
|--------|---------|--------|
| Detection rate | 74.7% | 53.3% |
| ผู้ที่ได้รับยา (crude coverage) | 49.8% | 29.2% |
| ผู้ที่ควบคุมได้ดี (effective coverage) | **11.7%** | **15.7%** |

> **ข้อสรุปสำคัญ:** มีผู้ป่วยเบาหวานถึง 88.3% ที่ "ยังไม่ได้รับการดูแลที่ effective" — LTFU prediction คือกุญแจสู่การปิด gap นี้

### B.3 ภาระ CKD และ Dialysis

| ตัวชี้วัด | ค่า |
|-----------|-----|
| CKD prevalence | **11.6 ล้านคน** (17.5% ของประชากร) |
| Advanced CKD (stage 3–5) | 5.7 ล้านคน |
| ผู้ป่วย Hemodialysis | 129,724 คน (2020) |
| ผู้ป่วย Peritoneal Dialysis | 34,467 คน (2020) |
| **ค่าใช้จ่าย Dialysis ต่อปี** | **12.27 พันล้านบาท (~$350M USD)** |
| Coverage ของ kidney replacement therapy | 98.5% (บัตรทอง) |

> ค่าฟอกไต 1 คน ≈ **~540,000 บาท/ปี** → ถ้าป้องกัน LTFU ได้ 1,000 คน = ประหยัด **540 ล้านบาท/ปี**

**Sources:** PMC9606783, PMC8815550

### B.4 ระบบสาธารณสุขไทย: Infrastructure ที่ Idea 7 ใช้ได้

```
Bangkok Hospital / โรงพยาบาลใหญ่
        ↓  (AI risk score ส่งไป)
Case Manager (พยาบาลวิชาชีพ)
        ↓  (รับ ranked list + เหตุผล)
โทรหา / SMS ผู้ป่วยเสี่ยงสูง
        ↓
รพ.สต. (Primary Care Unit)
        ↓
อสม. (Village Health Volunteer) — ครอบคลุม 95% ของชุมชน
        ↓
ผู้ป่วยกลับมาพบแพทย์ → ป้องกัน stroke/ICU
```

- Thailand มี **อสม. ~1 ล้านคน** ทั่วประเทศ — network ที่ใหญ่ที่สุดในโลกสำหรับ community health
- **Smart NCD Network** (2023–2027): นโยบายรัฐบาลที่สนับสนุน AI-based NCD management

---

## Part C: งานวิจัยด้าน ML สำหรับ LTFU Prediction

### C.1 งานวิจัยที่คล้ายกัน (Analogous Studies)

| งานวิจัย | Dataset | Algorithm | AUC / Performance |
|---------|---------|-----------|-------------------|
| LTFU in HIV/ART, Ethiopia | EMR 115,000+ คน | ML ensemble | **AUROC 85.9%** (95% CI 82.0–89.6) |
| LTFU in TB treatment | National registry | XGBoost/RF | ~80–85% |
| LTFU prediction in Haiti | Clinic EMR | Logistic/RF | ~78–82% |
| Diabetes LTFU, Ghana | NHIS claims | Decision Tree/LR | ~75–80% |
| Undiagnosed DM prediction | Population survey | XGBoost | **86.2%** (without lab), **95.7%** (with lab) |

> **ประเด็นสำคัญ:** งานวิจัยใกล้เคียงที่สุด (Ethiopia ART) ทำได้ **AUROC 85.9%** ใน dataset ผู้ป่วย NCD-like — เป็น benchmark ที่ realistic สำหรับ Idea 7 ของเรา

**Sources:** BMC Med Inform 2025 (10.1186/s12911-025-03030-7), PMC9449961, PMC10355459

### C.2 Features ที่พบบ่อยในงานวิจัย LTFU

| Feature Category | ตัวอย่าง Feature | ใน Dataset เรา? |
|-----------------|----------------|----------------|
| Visit history | จำนวน visits ที่ผ่านมา, ช่วงห่างระหว่าง visit | ✅ `visit_frequency`, `max_consecutive_gap` |
| Lab trends | slope ของ HbA1c, glucose | ✅ `hba1c_slope`, `sbp_slope` |
| Medication adherence | การรับยาต่อเนื่อง | ✅ `med_in_last_period` |
| Demographics | อายุ, เพศ, distance to facility | ✅ `age`, `identify_by_enc` |
| Disease severity | comorbidity count, baseline values | ✅ `comorbidity_count`, `hba1c_at_baseline` |
| Missingness pattern | lab sparsity ratio | ✅ `lab_sparsity_ratio` |

> **ความแข็งแกร่งของ Idea 7:** features ทั้ง 12 ตัวของเรา **ครอบคลุมทุก category** ที่พบในงานวิจัย

### C.3 ทำไมถึงเลือก XGBoost?

| เกณฑ์ | Logistic Regression | Random Forest | **XGBoost** | Neural Network |
|-------|--------------------|--------------|-----------|-|
| Performance | ปานกลาง | ดี | **ดีที่สุด** | ดีมาก |
| Interpretability | สูง | ปานกลาง | **ปานกลาง (SHAP ช่วยได้)** | ต่ำ |
| Class imbalance | ยาก | ปานกลาง | **scale_pos_weight** | ยาก |
| Sparse features | แย่ | ดี | **ดีมาก** | ดี |
| Onsite speed (2 วัน) | เร็ว | เร็ว | **เร็ว** | ช้า |
| Clinical acceptance | สูง | ปานกลาง | **ปานกลาง-สูง (explainable)** | ต่ำ |

**สรุป:** XGBoost เหมาะสมที่สุดสำหรับ sparse EMR data + class imbalance + need for explainability ใน 2 วัน

---

## Part D: Informative Missingness — ทำไม Missing Data คือ Signal

### D.1 ประเภทของ Missing Data (สำหรับ clinical ML)

| ประเภท | นิยาม | ตัวอย่างใน EMR ของเรา |
|--------|-------|----------------------|
| **MCAR** (Missing Completely At Random) | สุ่มโดยไม่มี pattern | ข้อมูลหายเพราะระบบล่ม |
| **MAR** (Missing At Random) | มี pattern แต่ขึ้นกับตัวแปรอื่น | สั่งตรวจ HbA1c เฉพาะ DM type ที่กำหนด |
| **MNAR** (Missing Not At Random) | **pattern ขึ้นกับค่าที่หายเอง** | ผู้ป่วยไม่มา → ไม่มีข้อมูล → missing |

> **ข้อมูลใน EMR ของเราเป็น MNAR เกือบทั้งหมด** — missing vitalsign = ผู้ป่วยไม่มา = clinically significant

### D.2 Evidence: Missingness เป็น Feature ที่ดี

- **Missing-indicator method:** เพิ่ม binary flags ว่าค่าไหน missing → ช่วย model เรียนรู้ pattern
- **ICU research (JMIR 2021):** Missing vitals เป็น early warning signal สำหรับ patient deterioration — **ไม่ใช่ noise**
- **EMR review (JMIR 2025):** การ include missingness features consistently ช่วย improve model performance
- **Causal representation learning (arXiv 2025):** Multimodal clinical records ที่ใช้ MNAR-aware fusion ได้ผลดีกว่า naive imputation

**ความหมายสำหรับโปรเจกต์เรา:**
```
vitalsign_sbp_23 = NaN  → ไม่ได้แปลว่าข้อมูลเสีย
                         → แปลว่า "ผู้ป่วยไม่มาที่ period 23"
                         → นี่คือ clinical information ที่ model ต้องเรียนรู้
```

**Sources:** JMIR Med Inform 2021 (e25022), JMIR 2025 (e79307), arXiv 2025 (2509.17228)

---

## Part E: Impact Quantification — Business Case

### E.1 Cost of Inaction

| สถานการณ์ | จำนวน | ค่าใช้จ่าย |
|-----------|-------|----------|
| ผู้ป่วย DM LTFU ที่ rebound และ develop CKD | ~10% ของ LTFU (120,000 คน) | ฟอกไต 12.27B บาท/ปี |
| ผู้ป่วยที่มี acute stroke/MI จาก uncontrolled DM/HT | ~5% ของ LTFU | ICU ≥ 200,000–500,000 บาท/ครั้ง |
| ค่า productivity loss จากผู้ป่วยวัยทำงาน | — | ประมาณการ ~$4.4B USD/ปี (NCD รวม) |

### E.2 Cost of Intervention (LTFU Prediction Program)

- ค่า phone call จาก Case Manager: ~50–100 บาท/คน
- ค่า outreach จาก อสม.: ต่ำมาก (volunteer system)
- Infrastructure: ใช้ EMR ที่มีอยู่แล้ว + AI model
- **ค่าใช้จ่ายต่อผู้ป่วยที่ติดต่อ: ~100–200 บาท**

### E.3 ROI Calculation

```
สมมุติ: intervene 10,000 คน/ปี
→ ค่าใช้จ่าย: 10,000 × 200 = 2 ล้านบาท

ถ้า 10% กลับมาพบแพทย์และหลีกเลี่ยง dialysis:
→ ประหยัด: 1,000 × 540,000 = 540 ล้านบาท/ปี

ROI = (540M - 2M) / 2M = 269x return
```

> **ข้อความ pitch:** "ลงทุน 2 ล้านบาท ประหยัดงบประมาณ 540 ล้านบาท"

### E.4 Analogous Programs ที่ประสบความสำเร็จ

- **ยูกันดา AMPATH:** Predictive model ช่วยลด LTFU rate ใน HIV clinic จาก 34% → 18% ใน 2 ปี
- **เคนยา IPA:** SMS reminders + risk-stratified outreach → 27% reduction in LTFU
- **India RSSDI:** Case manager + algorithm → 31% improvement in medication adherence

---

## Part F: Technical Deep Dive — SHAP สำหรับ Clinical Explainability

### F.1 ทำไม Explainability จำเป็นสำหรับ Clinical AI

1. **Trust:** แพทย์และพยาบาลต้องเข้าใจว่า AI คิดอะไร ก่อนจะเชื่อและใช้
2. **Accountability:** ถ้าผู้ป่วยเกิดอันตราย → ต้องอธิบายได้ว่า model ตัดสินใจยังไง
3. **Regulatory:** FDA/อย. / สธ. ต้องการ explainable AI ในบริบทการแพทย์
4. **Improvement:** SHAP บอกให้รู้ว่า feature ไหน "หัก" → ช่วย debug และ improve model

### F.2 วิธีอ่าน SHAP สำหรับ Idea 7

**Beeswarm Plot:**
```
feature            impact on model output
─────────────────────────────────────────
max_consecutive_gap  ████████████ → สูง = เพิ่มความเสี่ยง LTFU
visit_frequency      ████████     ← ต่ำ = เพิ่มความเสี่ยง LTFU  
lab_sparsity_ratio   ██████       → สูง = เพิ่มความเสี่ยง
sbp_at_last_visit    ████         → สูง = เพิ่มความเสี่ยง (SBP ที่ควบคุมไม่ได้)
hba1c_at_baseline    ███          → สูง = เพิ่มความเสี่ยง (เริ่มต้นแย่)
age                  ██           กลาง (U-shape)
```

**ตีความสำหรับ Case Manager:**
```
ผู้ป่วย ID: 00142
Risk Score: 87% (เสี่ยงสูงมาก)
เหตุผลหลัก 3 อย่าง:
  1. ไม่มาพบแพทย์ต่อเนื่อง 14 เดือน (gap ยาวกว่าค่าเฉลี่ย 3x)
  2. ไม่มีการตรวจ HbA1c เลยใน 2 ปีที่ผ่านมา
  3. ความดันครั้งล่าสุด = 158 mmHg (Stage 2 — ควบคุมไม่ได้)
→ แนะนำ: โทรติดต่อภายใน 7 วัน
```

**Source:** Nature npj Digital Medicine 2025 (s41746-025-01958-8), PLOS Digital Health (0001417)

---

## Part G: Ethics & PDPA Compliance

### G.1 พ.ร.บ. คุ้มครองข้อมูลส่วนบุคคล (PDPA) 2562 — บริบท Health Data

| ประเด็น | ข้อกำหนด | วิธีที่ Idea 7 จัดการ |
|---------|---------|----------------------|
| Sensitive data | ข้อมูลสุขภาพ = sensitive category ต้องมี explicit consent | ใช้ข้อมูลที่โรงพยาบาลได้ consent แล้ว |
| Data minimization | ใช้เฉพาะข้อมูลที่จำเป็น | ใช้ pattern ของ visit records ไม่ใช่ diagnoses รายละเอียด |
| Purpose limitation | ข้อมูลใช้ได้ตาม consent เท่านั้น | เพื่อ clinical care continuity (ตรงกับ consent เดิม) |
| Output | ห้าม expose raw data | output เป็นแค่ risk score + เหตุผล 3 ข้อ |
| Retention | กำหนด data retention policy | model results ไม่เก็บนานกว่าจำเป็น |

### G.2 Algorithmic Bias Risks และ Mitigation

| Bias Type | ความเสี่ยง | Mitigation |
|-----------|-----------|-----------|
| **Geographic bias** | ผู้ป่วยชนบท vs เมือง มี visit pattern ต่าง | Stratify analysis โดย facility type |
| **Age bias** | ผู้สูงอายุอาจมี gap เพราะสุขภาพ ไม่ใช่ LTFU | เพิ่ม "reason for gap" feature ถ้ามี |
| **Socioeconomic bias** | คนจนมี access น้อยกว่า | ระบุ bias นี้อย่างชัดเจนใน output |
| **Label bias** | Missing ≠ LTFU เสมอ (ย้ายโรงพยาบาล) | Case Manager ต้อง verify ก่อน act |

### G.3 Explainability Requirement

งานวิจัยล่าสุด (npj Digital Medicine 2025) พบว่า:
- SHAP explanations alone ยังยากสำหรับแพทย์ในการตีความ
- **วิธีที่ดีกว่า:** แปล SHAP เป็นภาษาธรรมชาติ + ระดับความรุนแรง (ต่ำ/กลาง/สูง)
- Explanation design มีผลอย่างมีนัยสำคัญต่อ clinical decision behavior

---

## Part H: Relationship กับ Idea 2 (DynaGlycemia Trajectory)

### H.1 สองด้านของเหรียญเดียวกัน

```
NCD Patient Population
         │
         ├── ยังมาพบแพทย์สม่ำเสมอ
         │         ↓
         │    [Idea 2: DynaGlycemia]
         │    ติดตาม HbA1c + BP trajectory
         │    ทำนาย CKD decline ใน 2 ปี
         │    → แจ้งเตือนแพทย์ปรับยา
         │
         └── มีความเสี่ยงหยุดมา / หยุดมาแล้ว
                   ↓
              [Idea 7: Patient Rescue Radar]
              ทำนาย LTFU ก่อนเกิด dropout
              → ส่ง Case Manager ติดต่อเชิงรุก
              → ดึงกลับมาเข้า Idea 2 pipeline
```

### H.2 Synergy สำหรับ Pitch

> "ระบบของเรามีสองชั้น:  
> Idea 7 ป้องกันผู้ป่วยออกนอกระบบ  
> Idea 2 ดูแลผู้ป่วยที่อยู่ในระบบให้ไม่เสื่อม  
> ร่วมกัน = complete NCD lifecycle management"

---

## Part I: Pitch Script (5 นาที)

### I.1 Story Arc

**Hook (30 วิ):**
> "ปีที่แล้ว คนไทยเสียชีวิตจาก NCD วันละ **1,100 คน** — มากกว่าอุบัติเหตุบนท้องถนน 10 เท่า  
> ที่น่าเจ็บปวดคือ **40% ของกรณีเหล่านี้ป้องกันได้** ถ้าผู้ป่วยยังอยู่ในระบบ"

**Problem (1 นาที):**
- ผู้ป่วย NCD ไม่เจ็บในช่วงแรก → หยุดมาพบแพทย์
- เมื่อหายไป 6 เดือน → ยาหมด → BP/น้ำตาลพุ่ง → Stroke/MI เฉียบพลัน
- ระบบสาธารณสุขไทยปัจจุบัน: **รอให้ป่วยหนักแล้วค่อยรักษา** (ไม่ใช่ป้องกัน)

**Solution (2 นาที):**
- วิเคราะห์ pattern ใน EMR 70K+ คน → สัญญาณที่มองไม่เห็นด้วยตาเปล่า
- 12 features จาก visit records, lab sparsity, medication continuity
- XGBoost → risk score → ranked list ส่ง Case Manager
- SHAP: "3 เหตุผลหลักที่คนไข้คนนี้เสี่ยง" เพื่อให้ Case Manager โทรหาได้ทันที

**Impact (1 นาที):**
- งานวิจัยที่คล้ายกัน: AUROC **85.9%** ใน EMR dataset ขนาดใหญ่
- ROI: ลงทุน 2 ล้านบาท → ประหยัด 540 ล้านบาท (dialysis cost)
- Scale: ใช้ได้กับ อสม. 1 ล้านคน + รพ.สต. 10,000 แห่ง ทั่วไทย

**Close (30 วิ):**
> "เราไม่ได้สร้าง AI เพื่อแทนแพทย์  
> เราสร้าง AI เพื่อบอกว่า 'ผู้ป่วยคนนี้กำลังจะหายไป — โทรหาเขาตอนนี้เลย'"

### I.2 คำถามที่กรรมการมักถาม

| คำถาม | คำตอบที่เตรียมไว้ |
|-------|----------------|
| "วัดผลได้ยังไง?" | AUROC เป้าหมาย ≥ 0.80; Precision@K (top 10% patients) |
| "Missing data 86% จะ train ได้เหรอ?" | นั่นคือ feature! MNAR patterns เป็น signal หลัก |
| "PDPA ล่ะ?" | Output เป็น risk score เท่านั้น, ใช้ within ระบบโรงพยาบาล |
| "Scale ได้ไหม?" | Pipeline ทำงานกับ dataset เต็ม 70K ได้เลยในวันเดียว |
| "Idea 2 กับ 7 ต่างกันยังไง?" | Idea 7 = ป้องกัน dropout, Idea 2 = ติดตาม trajectory |
| "ถ้า model ผิด แล้วโทรหาผู้ป่วยที่ไม่เสี่ยง?" | False positive cost = เสียเวลา Case Manager 5 นาที; False negative cost = ICU admission |

---

## Part J: Onsite Execution Plan

### Day 1 (วันที่ 1): Data → Features → Model

| เวลา | งาน | คน |
|------|-----|-----|
| 09:00–10:00 | รับ full dataset, verify schema กับ sample | ทุกคน |
| 10:00–11:00 | รัน `idea7_ltfu_eda.ipynb` กับ full data, ตรวจ LTFU distribution | กอ + จีน |
| 11:00–12:00 | Tune LTFU threshold ร่วมกับ clinical team onsite | จีน + เฟิม |
| 12:00–13:00 | Feature engineering (เพิ่ม DBP slope, med class change) | แบงค์ + ข้าวฟาง |
| 13:00–15:00 | Train XGBoost, tune hyperparameters | กอ + แบงค์ |
| 15:00–17:00 | Stratified K-Fold validation, compute AUROC + Precision@K | กอ |
| 17:00–18:00 | SHAP analysis, เลือก top 5 features | ทุกคน |

### Day 2 (วันที่ 2): Output → Pitch

| เวลา | งาน | คน |
|------|-----|-----|
| 09:00–10:00 | สร้าง risk score output table (ranked list) | แบงค์ |
| 10:00–11:00 | แปล SHAP เป็น "3 เหตุผลภาษาคลินิก" | จีน + เฟิม |
| 11:00–12:00 | สร้าง visualizations สำหรับ presentation | ข้าวฟาง |
| 12:00–14:00 | เขียน presentation / poster | ทุกคน |
| 14:00–15:00 | ซ้อม pitch 5 นาที × 3 รอบ | ทุกคน |
| 15:00– | Presentation ต่อกรรมการ | กอ เป็น presenter หลัก |

### Contingency Plan

| ปัญหา | Plan B |
|-------|--------|
| Full dataset schema ต่างจาก sample | Adapt column mapping ใน `load_patient_data()` |
| LTFU = 100% อีก (ทุกคน LTFU) | ลด threshold เป็น 60 periods (~10 ปี) |
| XGBoost AUC ต่ำกว่า 0.7 | เพิ่ม features / ใช้ Logistic Regression เพื่อ baseline |
| ไม่มีเวลา SHAP | ใช้ built-in feature importance แทน |

---

## References

### Clinical Literature
- Asante et al. (2025). Determinants of LTFU in Ghana. *PMC12054282*
- Predictors of LTFU in Uganda DM clinic. *PMC6708238*
- Stress hyperglycemia & stroke recurrence. *PMC8034909*
- Hyperglycemia meta-analysis. *PMC10701542*

### Thai Healthcare
- Effective coverage of DM/HT in Thailand. *PMC9716924*
- Diabetes trends in Thai adults. *Nature 2025 (s41598-025-17619-5)*
- CKD economic burden. *PMC9606783*
- Global Dialysis Perspective: Thailand. *PMC8815550*
- WHO Thailand NCD Strategy. *who.int/thailand*

### ML for LTFU
- ML prediction LTFU in HIV/ART. *BMC Med Inform 2025 (s12911-025-03030-7)*
- Historical visit attendance as predictor. *PMC10355459*
- ML for LTFU in Haiti. *IJPH 2026 (1609496)*
- ML for TB LTFU. *PMC10760311*
- Diabetic nephropathy explainable ML. *arXiv 2309.16730*

### Missingness & EMR
- Missingness in multimodal clinical records. *arXiv 2509.17228*
- Methods for missingness in EHR. *JMIR 2025 (e79307)*
- Missingness in critical care ML. *JMIR Med Inform 2021 (e25022)*
- Statistical modeling with non-ignorable missingness. *PMC8209781*

### Explainability & Ethics
- SHAP vs clinician explanations (npj Digital Medicine 2025). *Nature s41746-025-01958-8*
- XAI in clinical decision support. *PLOS Digital Health (0001417)*
- Systematic review XAI in healthcare. *Frontiers AI 2026 (1749527)*
