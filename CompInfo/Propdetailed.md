Proposal
1. Business & Domain Lead (สายวิเคราะห์ปัญหาและผลกระทบ)
หัวข้อที่ต้องรับผิดชอบ:
- กลุ่มเป้าหมายที่ได้ประโยชน์
- โจทย์ปัญหา
- ผลกระทบที่คาดหวัง
- การประเมินตนเอง (เกณฑ์ที่ต้องโฟกัส): ผลกระทบต่อสังคม

2. Technical & AI Architect (สายเทคโนโลยีและข้อมูล)
หัวข้อที่ต้องรับผิดชอบ:
- สถาปัตยกรรมและเทคโนโลยีที่ใช้
    - เทคโนโลยีหลัก / อัลกอริทึม / โมเดล
        - ระบุโมเดล AI/ML อัลกอริทึม Pipeline ข้อมูล หรือวิธีวิเคราะห์ที่ใช้
         เช่น CNN สำหรับภาพทางการแพทย์ XGBoost สำหรับข้อมูล EMR แบบตาราง NLP สำหรับเรื่องร้องเรียน GATK สำหรับ Variant Calling…
    - แพลตฟอร์มและการ Deploy
        - Web app / Mobile / API / Dashboard? บน Cloud หรือ On-premise?
          เช่น FastAPI backend, React dashboard, Docker บน AWS EC2, Secure data enclave สำหรับ PHI…

- แผนภาพการทำงาน (WORKFLOW)
- การประเมินตนเอง (เกณฑ์ที่ต้องโฟกัส): ความเป็นไปได้เชิงเทคนิคและการสร้าง Prototype ใน 1 วัน และความเข้าใจโจทย์/ข้อมูล

3. Product & Innovation Strategist (สายนวัตกรรมและจุดขาย)
หัวข้อที่ต้องรับผิดชอบ:
- คำขวัญ / แนวคิดหลัก
- แนวทางการแก้ปัญหา
- นวัตกรรมและจุดเด่น

4. คนรวบรวม Docx จากทุกคน มาทำเป็น proposal ฉบับสมบูรณ์เพื่อตรวจเช็ค และ ทำส่ง

---

## ร่างเนื้อหา: หัวข้อ "สถาปัตยกรรมและเทคโนโลยีที่ใช้"
### Idea 7: Patient Rescue Radar — LTFU Prediction

---

### เทคโนโลยีหลัก / อัลกอริทึม / โมเดล

**Data Pipeline**
- อ่านข้อมูล EMR รูปแบบ wide-format XLSX (4,190+ columns) ด้วย `pd.read_excel()` จาก sheet `data`
- โครงสร้างข้อมูล: Period Index `P = -1` (ก่อนวินิจฉัย) → `P = 0` (วินิจฉัย) → `P ≥ 1` (ทุก 60 วัน)
- ฐานข้อมูลผู้ป่วยเบาหวาน 70,000+ คน แบ่งเป็น 2 กลุ่ม: Active vs LTFU

**Feature Engineering (12 Features)**

| Feature | ความหมาย | แหล่งข้อมูล |
|---------|---------|-----------|
| `visit_frequency` | สัดส่วน periods ที่มีบันทึก vitalsign (= มาจริง) | `vitalsign_sbp_<P>` |
| `max_consecutive_gap` | ช่วงขาดนัดต่อเนื่องยาวสุด (periods) | `vitalsign_sbp_<P>` |
| `lab_sparsity_ratio` | สัดส่วน HbA1c periods ที่ missing | `lab_hba1c_<P>` |
| `sbp_slope` | แนวโน้มความดันโลหิตข้ามเวลา | `vitalsign_sbp_<P>` |
| `hba1c_slope` | แนวโน้มน้ำตาลในเลือดข้ามเวลา | `lab_hba1c_<P>` |
| `sbp_at_last_visit` | ค่าความดัน ณ visit ล่าสุด | `vitalsign_sbp_<P>` |
| `hba1c_at_baseline` | HbA1c ณ วันวินิจฉัย | `lab_hba1c_0` |
| `last_observed_period` | Period ล่าสุดที่มีบันทึกข้อมูล | `vitalsign_sbp_<P>` |
| `med_in_last_period` | รับยาต่อเนื่องในช่วงล่าสุดหรือไม่ | `med_*_<P>` |
| `comorbidity_count` | จำนวนโรคร่วมที่บันทึก | `co_*_<P>` |
| `age` | อายุผู้ป่วย | demographics |
| `identify_by_enc` | วิธีวินิจฉัย (lab vs medication-based) | demographics |

**กลยุทธ์จัดการ Missing Data (MNAR)**
- ข้อมูลที่หายไปใน EMR คือ **Missing Not At Random** — ผู้ป่วยไม่มา → ไม่มีบันทึก → missing = signal
- ใช้ **Missing-Indicator Method**: ไม่ impute แต่เพิ่ม binary flag column ระบุตำแหน่งที่ missing
- `visit_frequency` และ `lab_sparsity_ratio` encode ความถี่การขาดนัดเป็น feature โดยตรง
- XGBoost รองรับ missing values natively — ไม่ต้องแปลงค่าเพิ่มเติม

**นิยาม LTFU (Label)**
- LTFU = `max_consecutive_gap ≥ 3 periods` (≈ 6 เดือนต่อเนื่อง ไม่มีบันทึก visit)
- อ้างอิง: Asante et al. 2025, Kalyesubula et al. 2019 (clinical consensus)

**โมเดล: XGBoost Classifier**
- `scale_pos_weight = n_active / n_ltfu` จัดการ class imbalance (LTFU มักน้อยกว่า Active)
- Hyperparameter tuning ด้วย 5-fold stratified cross-validation
- Baseline เปรียบเทียบ: Logistic Regression
- เป้าหมาย AUROC ≥ 0.80 (อ้างอิงจาก analogous LTFU prediction studies)

**Explainability: SHAP**
- คำนวณ SHAP values → ระบุ top 3 features ที่ขับเคลื่อน risk สำหรับคนไข้แต่ละราย
- แปลงเป็นภาษาคลินิกสำหรับ Case Manager เช่น:
  - "ไม่มาพบแพทย์ต่อเนื่อง 14 เดือน"
  - "ไม่มีผล HbA1c ใน 2 ปีที่ผ่านมา"
  - "ความดันครั้งล่าสุด 158 mmHg (Stage 2)"

**Libraries ที่ใช้**
```
pandas, numpy, scikit-learn, xgboost, shap, matplotlib, seaborn, openpyxl
```

---

### แพลตฟอร์มและการ Deploy

**Hackathon Prototype (2 วัน on-site)**

| องค์ประกอบ | รายละเอียด |
|-----------|-----------|
| Environment | Python 3.x + Jupyter Notebook / VS Code |
| ข้อมูล | local XLSX — 100-row sample ระหว่างพัฒนา / 70K คนเมื่อผ่านรอบ |
| Output | Risk score CSV + SHAP summary plot + Patient Risk Card (text) |

**Production Vision**

```
EMR / HIS (โรงพยาบาล)
        ↓  batch pull ทุกคืน
FastAPI Backend
  → รัน feature pipeline + XGBoost inference + SHAP
        ↓
Streamlit Dashboard (Case Manager)
  → Ranked list ผู้ป่วยเสี่ยงสูง + Risk Card (score + 3 เหตุผล)
        ↓
โทร / SMS ผู้ป่วย หรือส่ง อสม. เยี่ยมบ้าน
```

| องค์ประกอบ | เทคโนโลยี | เหตุผล |
|-----------|----------|-------|
| Backend API | FastAPI (Python) | เร็ว, async, รองรับ batch inference |
| Dashboard | Streamlit | พัฒนาเร็ว, ใช้งานง่ายสำหรับ clinical staff |
| Deployment | On-premise ที่โรงพยาบาล | ข้อมูลผู้ป่วยไม่ออกนอกระบบ (PDPA) |
| EMR Integration | Internal REST API | เชื่อมต่อกับ HIS ที่มีอยู่โดยไม่ต้องเปลี่ยนระบบ |
| Privacy | Output = risk score + 3 เหตุผล เท่านั้น | ไม่ expose raw medical records |