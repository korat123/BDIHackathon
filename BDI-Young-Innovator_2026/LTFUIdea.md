# Research Findings — BDI Hackathon 2026 (Health Track)

**ทีม:** กอ, แบงค์, จีน, ข้าวฟาง, เฟิม — KMUTT  
**วันที่:** 31 พ.ค. 2569  
**ไอเดียที่โฟกัส:** Idea 2 (DynaGlycemia Trajectory) + Idea 7 (Patient Rescue Radar)

---

## 1. Dataset Overview

### 1.1 Diabetes EMR (Sampled)
| รายการ | ค่า |
|--------|-----|
| จำนวนผู้ป่วย (sample) | 100 คน (จาก ~70,000 คนจริง) |
| จำนวน columns | 4,714 |
| Period range | P = -61 ถึง P = 68 (130 periods รวม) |
| 1 period = | ~60 วัน |
| Diabetes type | Type 1 ~2%, Type 2 ~72%, Unknown ~26% |
| วิธีวินิจฉัย (`identify_by`) | icd10, lab, medication |

### 1.2 Hypertension EMR (Sampled)
| รายการ | ค่า |
|--------|-----|
| จำนวนผู้ป่วย (sample) | 100 คน (จาก ~150,000 คนจริง) |
| จำนวน columns | 4,190 |
| Period range | P = -63 ถึง P = 68 (132 periods รวม) |
| **HbA1c columns** | **130 cols — มีเหมือน DM เพราะผู้ป่วย HT หลายคนมี DM ร่วม** |
| **FPG columns** | **มีเหมือนกัน — feature pipeline เดียวกันใช้ได้กับทั้ง DM และ HT** |

### 1.3 Column Groups (DM dataset)
| Group | จำนวน columns | ความหมาย |
|-------|--------------|----------|
| demographics | 7 | age, sex, dm_onset, type1/2/gdm, identify_by |
| vitalsign | 1,157 | sbp, dbp, bmi, hr, temp, o2sat × 130 periods |
| lab | 2,607 | hba1c, fpg, c_peptide, lipids × periods |
| comorbidity | 243 | co_ckd, co_cvd, co_stroke × periods |
| medication | 700 | ชนิดยา × periods |

---

## 2. Missingness Analysis

### 2.1 สรุป Missing Rate ต่อ Group
| Column Group | DM Missing Rate | HT Missing Rate | ความหมาย |
|-------------|----------------|----------------|----------|
| demographics | ~0% | ~0% | บันทึกครบตั้งแต่แรก |
| vitalsign | ~65–75% | ~65–75% | วัดเฉพาะตอนมาพบแพทย์ |
| lab | ~85–90% | ~85–90% | สั่งเฉพาะกรณี → sparse มาก |
| comorbidity | ~90–95% | ~90–95% | บันทึกเฉพาะกรณีสงสัย |
| medication | ~75–85% | ~75–85% | สั่งเป็น episode |

> **ข้อสรุปหลัก:** Missingness ≈ 80–90% ในข้อมูล lab และ comorbidity ไม่ใช่ data quality issue  
> มันคือ **clinical workflow** — แพทย์สั่งตรวจเฉพาะเมื่อจำเป็น = missing แปลว่า "ไม่ได้ตรวจ ไม่ได้บันทึก"

### 2.2 Patient-Level Missingness Pattern
- ผู้ป่วย sample 100 คน: `max_consecutive_gap` เฉลี่ย 58.68 periods (~9.5 ปี)
- `visit_frequency` เฉลี่ย 0.22 → มาแค่ 22% ของ 130 periods ที่เป็นไปได้
- ผู้ป่วยที่มี `last_observed_period = -99` = ไม่มีข้อมูลแม้แต่ period เดียว

### 2.3 ความสำคัญของ Missingness สำหรับ Idea 7
Pattern ของ missingness บอก "story" ของผู้ป่วย:
- **มีข้อมูล SBP ต่อเนื่อง** → ผู้ป่วยมาพบแพทย์สม่ำเสมอ = Active
- **มีข้อมูลช่วงต้น แล้วหาย** → ผู้ป่วย dropout = LTFU candidate
- **ไม่มีข้อมูล lab เลย** → ไม่ได้รับการตรวจติดตาม = ความเสี่ยงสูง

---

## 3. LTFU Definition & Threshold Analysis

### 3.1 นิยาม LTFU ที่เลือก
> **ผู้ป่วยที่มี ≥ 3 consecutive periods ที่ไม่มีข้อมูล vitalsign_sbp**  
> = ไม่มาโรงพยาบาลต่อเนื่อง ≥ 6 เดือน

### 3.2 Sensitivity Analysis
| Threshold | LTFU Count (sample 100) | LTFU % | หมายเหตุ |
|-----------|------------------------|--------|----------|
| ≥ 2 periods (~4 เดือน) | 100 | 100% | ทุกคนมี gap ≥ 4 เดือน |
| ≥ 3 periods (~6 เดือน) | 100 | 100% | **ค่า default** |
| ≥ 4 periods (~8 เดือน) | 100 | 100% | — |
| ≥ 5 periods (~10 เดือน) | 100 | 100% | — |

> **ปัญหาที่พบใน sample 100 คน:** ทุกคนมี max gap ≥ 3 → label เป็น LTFU = 100%  
> **สาเหตุ:** sample ถูก bias ต่อผู้ป่วยที่มีข้อมูลแบบ sparse เป็นพิเศษ  
> **แนวทางแก้ไข onsite:** ใช้ dataset เต็ม 70K/150K คน → จะมี distribution หลากหลายกว่า

---

## 4. Feature Engineering Results

### 4.1 Features 12 ตัวที่สร้างได้
| Feature | วิธีสร้าง | Median (DM sample) | ความหมาย |
|---------|----------|-------------------|----------|
| `visit_frequency` | notna().sum() / n_periods | 0.21 | สัดส่วน periods ที่มาจริง |
| `max_consecutive_gap` | นับ consecutive NaN ยาวสุด | 61 periods | ช่วงหายนานที่สุด (~10 ปี) |
| `last_observed_period` | period สุดท้ายที่มีข้อมูล | P=33 | "หยุดมาที่ period ไหน" |
| `sbp_at_last_visit` | SBP ณ visit สุดท้าย | 135.5 mmHg | BP ก่อนหายไป (Stage 1 HT) |
| `sbp_slope` | np.polyfit(periods, sbp, 1)[0] | +0.04 | trend BP ขึ้น/ลง ต่อ period |
| `lab_sparsity_ratio` | HbA1c.isnull().mean() | 0.89 | ตรวจ lab น้อยแค่ไหน |
| `hba1c_slope` | polyfit เหมือน sbp_slope | ~0.00 | trend น้ำตาลขึ้น/ลง |
| `hba1c_at_baseline` | ค่า HbA1c ที่ P=0 | 6.8% | ระดับน้ำตาลตอนวินิจฉัย |
| `med_in_last_period` | มียาในช่วงท้ายไหม | 1 (50/50) | รับยาต่อเนื่องหรือเปล่า |
| `comorbidity_count` | co_cols.notna().sum() | 10 | จำนวนโรคร่วมที่บันทึก |
| `age` | ข้อมูล demographics | 65 ปี | อายุผู้ป่วย |
| `identify_by_enc` | category encoding | 0 | วิธีที่วินิจฉัยโรค |

### 4.2 Key Findings จาก Feature Statistics
- **`sbp_at_last_visit` median = 135.5 mmHg** → Stage 1 Hypertension ตาม AHA guideline
- **`hba1c_at_baseline` mean = 7.49%** → เกินเกณฑ์ควบคุม (ADA target < 7%) → late detection
- **`lab_sparsity_ratio` mean = 0.86** → ตรวจ HbA1c แค่ 14% ของ periods → ติดตามน้อยมาก
- **`sbp_slope` mean = +0.25** → ความดันมี trend ขึ้นเล็กน้อยต่อ period โดยเฉลี่ย

---

## 5. Clinical Interpretations

### 5.1 ผู้ป่วยในกลุ่มตัวอย่างนี้ (DM 100 คน)
- อายุเฉลี่ย 65.7 ปี (range 41–98 ปี)
- HbA1c ≥ 6.5% ตอนวินิจฉัย = มีโรคเบาหวานแน่นอน; mean = 7.49% = ค่อนข้าง poorly controlled
- SBP ตอน last visit = 135.5 mmHg = อยู่ในช่วง Stage 1 Hypertension (ควบคู่โรค)
- Visit frequency 22% = ทุกๆ 5 periods มา 1 ครั้ง → ต่ำกว่า guideline (ควรมาทุก 1–3 months)

### 5.2 ความเสี่ยงที่ซ่อนอยู่
ผู้ป่วยเบาหวาน + ความดัน + ติดตามน้อย = combination ที่อันตราย:
- Uncontrolled DM → kidney damage (CKD) → dialysis
- Uncontrolled HTN → stroke, MI
- LTFU ≥ 6 เดือน → rebound ของ HbA1c และ BP → acute event

---

## 6. Limitations & Ethical Considerations

| ประเด็น | รายละเอียด | วิธีจัดการ |
|---------|-----------|-----------|
| Sample bias | 100 คน อาจไม่ represent population จริง | ใช้ full dataset onsite |
| Label quality | Missing ≠ LTFU — อาจย้ายโรงพยาบาล | Verify ก่อนโทรหาผู้ป่วย |
| Class imbalance | ใน sample 100 คน LTFU = 100% | ใช้ scale_pos_weight + SMOTE |
| PDPA | ข้อมูลผู้ป่วยเป็น sensitive data ระดับสูง | output เป็น risk score เท่านั้น |
| Re-identification | แม้ anonymize แล้ว ยังมี risk | ไม่ expose raw data ในระบบ |

---

## 7. Next Steps สำหรับ Onsite (2 วัน)

> **อัปเดต 3 มิ.ย. 2569:** Pipeline ขยายรองรับ **DM + HT combined** แล้ว  
> ยืนยันแล้วว่า HT dataset มี HbA1c (130 cols) และ FPG เหมือน DM → feature pipeline เดิมใช้ได้ทันที  
> Notebook `idea7_ltfu_eda.ipynb` ทดสอบบน combined 200 คน (DM 100 + HT 100) — รันผ่านทุก step

### วันที่ 1 — Data Pipeline (เริ่มจาก notebook ที่เตรียมไว้แล้ว)
- [ ] โหลด full dataset (DM 70K + HT 150K = 220K+ คน)
- [ ] รัน `idea7_ltfu_eda.ipynb` ตั้งแต่ต้น — pipeline จะ combine DM+HT อัตโนมัติ
- [ ] ตรวจสอบ LTFU threshold สำหรับ full data (threshold 3 จะทำงานปกติ ไม่ต้อง auto-adjust)
- [ ] ดู class balance จริงใน 220K คน และ tune `scale_pos_weight` ตามสัดส่วนจริง

### วันที่ 2 — Model & Presentation
- [ ] Train XGBoost บน full 220K คน (Section 6 รันได้เลย)
- [ ] Cross-validate (Stratified 5-Fold) → target AUROC ≥ 0.80
- [ ] SHAP analysis → ตีความ top features แยกตาม DM / HT
- [ ] สร้าง output: risk score table + patient risk card สำหรับ Case Manager
- [ ] เตรียม pitch: "3 เหตุผลหลักที่ผู้ป่วยคนนี้เสี่ยง" ภาษาคลินิก

---

## 8. References & Resources

- Dataset: [BDI Hackathon 2026 Sampled Dataset](https://github.com/anonymaew/bdi-hackathon-2026-sampled-dataset)
- EDA Notebook: `idea7_ltfu_eda.ipynb`
- Proposal EDA: `healthcare_datathon_proposal_eda.ipynb`
- Ideas Evaluation: `datathon_ideas_evaluation.md`
- ADA Glycemic Standards: HbA1c target < 7% for most adults
- AHA Blood Pressure: Normal < 120/80, Stage 1: 130–139/80–89, Stage 2: ≥140/90
