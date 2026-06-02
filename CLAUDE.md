# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## ภาพรวมโปรเจกต์

**BDI Hackathon 2026 (KKU)** — การแข่งขัน data science ด้านสุขภาพ จัดโดย Bangkok Hospital / National Phenome Institute

ทีมเราเลือก **Health Track** เท่านั้น โดยใช้ชุดข้อมูล EMR ผู้ป่วยเบาหวาน, ความดัน และสัญญาณคลื่นเครื่องช่วยหายใจจาก ICU เพื่อพัฒนาโซลูชัน AI ด้านการแพทย์

**ไอเดียที่ทีมเลือกโฟกัส:**
- **Idea 2: DynaGlycemia Trajectory** — ทำนายการเสื่อมของไต (CKD) จาก trajectory ของ HbA1c และความดันโลหิต
- **Idea 7: Patient Rescue Radar** — ทำนายผู้ป่วย NCD ที่มีความเสี่ยงหยุดมาพบแพทย์ (LTFU)

---

## ทีม

| ชื่อเล่น | สถาบัน | หลักสูตร |
|---------|--------|---------|
| กอ (เรา) | KMUTT วิศวกรรมคอมพิวเตอร์ ปี 1 | หลักสูตรนานาชาติ |
| แบงค์ | KMUTT วิศวกรรมคอมพิวเตอร์ ปี 1 | หลักสูตรนานาชาติ |
| จีน | KMUTT วิศวกรรมคอมพิวเตอร์ ปี 1 | วิทยาศาสตร์ข้อมูลสุขภาพ |
| ข้าวฟาง | KMUTT วิศวกรรมคอมพิวเตอร์ ปี 1 | วิทยาศาสตร์ข้อมูลสุขภาพ |
| เฟิม | KMUTT วิศวกรรมคอมพิวเตอร์ ปี 1 | วิทยาศาสตร์ข้อมูลสุขภาพ |

> แบงค์, จีน, ข้าวฟาง, เฟิม จบจากโรงเรียนวิทยาศาสตร์จุฬาภรณราชวิทยาลัย และมีประสบการณ์แข่ง hackathon มาก่อน

**หน้าที่ใน Proposal:**
- **กอ** — Technical & AI Architect: เขียน `สถาปัตยกรรมและเทคโนโลยีที่ใช้` + ทำ Slide Presentation (เพื่อนอัดคลิป)
- **จีน** — WORKFLOW diagram (`BDI-Young-Innovator_2026/Workflow Diagram(จีน).png`)
- **ข้าวฟาง** — Product & Innovation Strategist
- **ขฟ (แบงค์?)** — Business & Domain Lead (proposal(ขฟ).pdf)

---

## โครงสร้าง Repository

**GitHub remote:** `https://github.com/korat123/BDIHackathon`

```
BDIHackathon/
├── BDI-Young-Innovator_2026/                       # งาน EDA + research ของทีม (regular files, ไม่ใช่ submodule)
│   ├── idea7_ltfu_eda.ipynb                        # โน้ตบุ๊ค LTFU prediction หลัก (Idea 7)
│   ├── healthcare_datathon_proposal_eda.ipynb      # โน้ตบุ๊ค EDA หลัก + ไอเดีย 6 แนวทาง
│   ├── datathon_ideas_evaluation.md                # ไอเดียทั้งหมด 9 ข้อ + คะแนนจาก AI กรรมการ
│   ├── LTFU_Compendium.md                          # คัมภีร์ Idea 7: clinical research, ML lit, citations ครบ
│   ├── LTFUIdea.md                                 # EDA findings สำหรับ idea7_ltfu_eda.ipynb
│   ├── Propdetailed.md                             # ร่าง proposal ของแต่ละ role (technical section = กอ)
│   ├── Workflow Diagram(จีน).png                   # System workflow diagram (จีนทำ)
│   ├── Product & Innovation Strategist (สายนวัตกรรมและจุดขาย).pdf  # ข้าวฟางทำ
│   ├── proposal(ขฟ).pdf                            # Business & Domain section
│   └── requirements.txt
├── CompInfo/                                       # ไฟล์ทรัพยากรการแข่งขัน (slides, template, คู่มือ)
└── Sampled Dataset/
    └── bdi-hackathon-2026-sampled-dataset/         # git submodule → anonymaew/bdi-hackathon-2026-sampled-dataset (Git LFS)
        ├── diabetes/        # XLSX, ตัวอย่าง 100 จาก 70K คน
        ├── hypertension/    # XLSX, ตัวอย่าง 100 จาก 150K คน
        └── ventilator/      # CSV/JSON, 3 คนไข้ × ~30 เหตุการณ์
```

> `BDI-Young-Innovator_2026/` คืองาน data exploration ของทีม ไม่ใช่ production code — อ่านโน้ตบุ๊คและ `datathon_ideas_evaluation.md` ก่อนเริ่มพัฒนา

`.gitignore` ใน `BDI-Young-Innovator_2026/` ตั้งใจไม่ commit โฟลเดอร์ `diabetes/`, `hypertension/`, `ventilator/` — ให้ copy ข้อมูลมาไว้ในเครื่องตัวเอง แต่ห้าม commit

**Clone command สำหรับเพื่อนทีม:**
```bash
git clone --recurse-submodules https://github.com/korat123/BDIHackathon.git
# ถ้า clone ไปแล้วแต่ลืม --recurse-submodules:
git submodule update --init
```

---

## ติดตั้ง Environment

```bash
cd BDI-Young-Innovator_2026
pip install -r requirements.txt   # pandas, numpy, matplotlib, seaborn, openpyxl

jupyter notebook healthcare_datathon_proposal_eda.ipynb
```

---

## โครงสร้างข้อมูล Health Track

### EMR ผู้ป่วยเบาหวาน & ความดัน (Longitudinal)

ข้อมูลทั้งสองชุดใช้ **Period (P)** เป็น index โดยนับจากวันวินิจฉัย:
- `P = -1` : 30–90 วันก่อนวินิจฉัย
- `P = 0` : 30 วันรอบวันวินิจฉัย
- `P ≥ 1` : ทุก 60 วันหลังวินิจฉัย

รูปแบบไฟล์: wide-format **4,000+ คอลัมน์** (features × periods ต่อคนไข้)

ฟิลด์สำคัญ:
- Vitalsigns: `vitalsign_sbp_<P>`, `vitalsign_dbp_<P>`, `vitalsign_bmi_<P>` (บันทึกทุก visit — completeness สูง)
- Labs: `lab_hba1c_<P>`, `lab_fpg_<P>`, `lab_c_peptide_<P>`, lipids (ordered ทุก 3–6 เดือน — sparse)
- Comorbidities: `co_ckd_<P>`, `co_cvd_<P>` (ordered เฉพาะกรณีสงสัย — sparse มาก)
- Medications, Diagnosis metadata

ข้อสังเกตสำคัญจาก EDA:
- คนไข้หลายรายมี HbA1c >8% ณ วันวินิจฉัย → สัญญาณ late detection
- SBP ที่ diagnosis มักอยู่ 140–160 mmHg (Stage 1–2 hypertension)
- **Missingness ≈ 80–90% ใน lab ไม่ใช่ noise** — สะท้อน clinical decision-making และพฤติกรรมผู้ป่วย → ใช้เป็น feature ได้และสำคัญมากสำหรับทั้ง 2 ไอเดียที่เลือก

Diabetes subtypes: Type 1 ~2K, Type 2 ~50K, Unknown ~20K (จาก 70K ทั้งหมด)

### คลื่นเครื่องช่วยหายใจ ICU (Ventilator Waveform)

- สัญญาณ 3 แกน: Flow (l/min), Pressure (cmH2O), Volume (ml)
- Sampling rate: **25 Hz** ต่อเนื่อง 24 ชั่วโมง/วัน นาน 2–3 สัปดาห์/คนไข้
- โครงสร้างไฟล์: แต่ละ waveform event เป็น CSV แยก; demographics + diagnosis อยู่ใน JSON

---

## ไอเดียทั้งหมดของทีม (9 ข้อ) และคะแนนประเมิน

ประเมินโดย AI กรรมการ คะแนนเต็ม 130 (Phase 2: 100 + EMR Track: 30)

| # | ชื่อไอเดีย | คะแนน | สถานะ |
|---|-----------|-------|-------|
| 1 | SmartWean AI | 122/130 | — |
| **2** | **DynaGlycemia Trajectory** | **124/130** | **เลือกแล้ว** |
| 3 | Multimodal Care Sync | 109/130 | — |
| 4 | Hidden Epidemic Predictor | 118/130 | — |
| 5 | Polypharmacy Synergy Optimizer | 114/130 | — |
| 6 | Cardiorenal Domino Point | 123/130 | — |
| **7** | **Patient Rescue Radar** | **126/130** | **เลือกแล้ว** |
| 8 | Daily Wellness Guard | 90/130 | ไม่แนะนำ* |
| 9 | PreciseDose AI | 120/130 | — |

> *Idea 8 ถูก flag ว่า dataset ไม่รองรับ (ไม่มี daily behavioral logs ใน EMR)

---

## ไอเดียที่ทีมเลือกโฟกัส

### Idea 2: DynaGlycemia Trajectory (คะแนน 124/130)

**ปัญหา:**
ผู้ป่วยเบาหวาน+ความดัน ทำลายหลอดเลือดอย่างเงียบๆ ทีละน้อย HbA1c แบบ snapshot จุดเดียวไม่สามารถพยากรณ์การเสื่อมของไต (CKD/ESRD) ก่อนถึงจุดที่ย้อนกลับไม่ได้ ค่าฟอกไตดึงงบประมาณบัตรทองปีละหลายหมื่นล้านบาท

**แนวทางแก้ไข:**
แปลง wide-format 4,000+ คอลัมน์เป็น longitudinal curves → วิเคราะห์ trajectory ของ HbA1c และ BP → Survival XGBoost / Cox Regression ทำนาย eGFR decline ภายใน 2 ปี → แจ้งเตือนแพทย์ปรับยาก่อนถึงจุดที่ไตเสื่อมถาวร

**ข้อมูลที่ใช้:** `vitalsign_sbp_<P>`, `vitalsign_dbp_<P>`, `vitalsign_bmi_<P>`, `lab_hba1c_<P>`, `lab_fpg_<P>`, `co_ckd_<P>`

**จุดเด่นสำหรับ hackathon:**
- Feasibility สูงมาก — pipeline เสร็จได้ใน 2 วัน
- Academic rigor ดี — ใช้ functional data analysis + survival model
- ผลกระทบสูง — ลดภาระ dialysis ระดับชาติ

---

### Idea 7: Patient Rescue Radar (คะแนน 126/130 — อันดับ 1)

**ปัญหา:**
ผู้ป่วย NCD (เบาหวาน/ความดัน) ไม่รู้สึกเจ็บปวดในช่วงแรก → หยุดมาพบแพทย์ (Lost-to-Follow-Up: LTFU) → ความดัน/น้ำตาลกลับสูงกะทันหัน → หลอดเลือดแข็งตัว → **stroke/MI เฉียบพลัน** นี่คือสาเหตุหลักที่ผู้ป่วยต้องเข้า ICU แบบฉุกเฉิน ซึ่งป้องกันได้

**แนวทางแก้ไข:**
วิเคราะห์ pattern การขาดนัดและความผิดปกติของ missing data ใน EMR → Feature engineering จาก visit frequency, slope ของ BP/glucose, sparsity signature → XGBoost ทำนายโอกาส LTFU → ส่งรายชื่อผู้ป่วยเสี่ยงสูงให้ Case Manager / รพ.สต. ติดต่อเชิงรุกก่อนเกิดวิกฤต

**ข้อมูลที่ใช้:** pattern การหายไปของ visit records, slope ของ BP/glucose relative to baseline, demographics (age, sex), ความถี่ในการรับยา

**จุดเด่นสำหรับ hackathon:**
- **ใช้ sparsity เป็น feature หลัก** — ข้อมูลที่หายไปคือ signal สำคัญที่สุด ไม่ใช่ noise
- Feasibility ใน 2 วัน — feature engineering + XGBoost ทำได้ทันที
- Operational impact สูงสุด — เปลี่ยน AI จาก clinical analysis เป็น preventive healthcare operations
- กรรมการประเมินว่าเป็น **Champion Idea อันดับ 1**

**Dataset scope (implemented):** DM (70,000+ คน) + HT (150,000+ คน) = **220,000+ คนรวม**
- HT dataset มี HbA1c (130 cols) และ FPG เหมือน DM → ใช้ feature pipeline เดียวกันได้ทันที
- Pipeline: `df_combined = pd.concat([df_dm_features, df_ht_features])` พร้อม `disease_type` feature (0=DM, 1=HT)
- Risk stratification: **High Risk ≥ 70** → Active Intervention (Case Manager + อสม.) / **Low Risk < 70** → Routine Care
- Verified ด้วย sample 200 คน (DM 100 + HT 100) ใน `idea7_ltfu_eda.ipynb`

---

## เกณฑ์การให้คะแนนของการแข่งขัน

เข้าใจเกณฑ์นี้เพื่อทิศทางการพัฒนา:

**Phase 2 (100 คะแนน):**
| เกณฑ์ | คะแนน | รายละเอียด |
|-------|-------|-----------|
| Feasibility & Tech Plan | 30 | สร้าง prototype ได้ใน 2 วัน onsite |
| Problem & Data Usage | 25 | ใช้ dataset ได้ตรงจุด, data cleaning ถูกต้อง |
| Impact & Value | 20 | แก้ปัญหาสุขภาพไทยจริง, scale ได้ |
| Creativity & Novelty | 15 | มุมมองใหม่, Deep Tech ชัดเจน |
| Team Readiness | 10 | แบ่งหน้าที่ชัด, พัฒนา parallel ได้ |

**EMR Track เพิ่มเติม (30 คะแนน):**
| เกณฑ์ | คะแนน |
|-------|-------|
| Medical Data Processing (pipeline, จัดการ sparsity ถูกต้อง) | 10 |
| Health Data Ethics (PDPA, ไม่มี bias, privacy) | 10 |
| Clinical Value (ยืด lifespan, ช่วยแพทย์ตัดสินใจ) | 10 |

---

## หมายเหตุสำคัญ

- ชื่อคอลัมน์ในไฟล์ข้อมูลอาจเป็น **ภาษาไทย (UTF-8)** — อ่าน data dictionary (.xlsx) ในแต่ละโฟลเดอร์ก่อนเสมอ
- `Sampled Dataset` sub-repo ใช้ **Git LFS** — ต้องรัน `git lfs pull` หลัง clone
- มี `.git` สอง level (root, `Sampled Dataset/.../`) — `BDI-Young-Innovator_2026/` เป็น regular files แล้ว ไม่มี `.git` ของตัวเอง
- ชุดข้อมูลที่มีอยู่เป็น **ตัวอย่างเท่านั้น** ไม่ใช่ชุดข้อมูลเต็ม ทีมที่ผ่านรอบ proposal จะได้ชุดข้อมูลเต็ม

---

## ⚠️ Git Warning: คำสั่งที่ห้ามใช้ใน repo นี้

**อย่าใช้ `git submodule deinit --force <path>` เด็ดขาด**

คำสั่งนี้จะ **ลบไฟล์ทั้งหมดในโฟลเดอร์นั้น รวมถึงไฟล์ที่ยังไม่ได้ commit** (untracked files) โดยไม่มีการเตือน และไม่สามารถ recover ได้จาก git (เพราะไม่เคย commit)

หากต้องการเปลี่ยน submodule เป็น regular files ให้ทำแบบนี้แทน:
```bash
# 1. ถอด submodule tracking ออกจาก index (ไม่แตะไฟล์)
git rm --cached <path>
# 2. ลบ .gitmodules entry ด้วยมือ
# 3. ลบ .git ข้างใน (ถ้ามี) ด้วยมือ
# 4. git add <path>/ เพื่อ track เป็น regular files
```

**Recovery command** (ถ้า deinit เกิดขึ้นแล้ว — กู้ได้เฉพาะ committed files เท่านั้น):
```bash
git --git-dir=".git/modules/<name>" show HEAD:<filename> > <destination>
```

---

## ไฟล์ข้อมูลจริงใน Sampled Dataset

ไฟล์ที่มีอยู่จริง (ตรวจสอบแล้ว):
- `diabetes/data_dictionary_diabetes_example.xlsx` — **xlsx เท่านั้น** (ไม่ใช่ .csv)
- `hypertension/data_dictionary_hypertension_example.xlsx` — **xlsx เท่านั้น** (ไม่ใช่ .csv)
- `ventilator/smart_icu_data/` — CSV waveform files + JSON demographics
- `ventilator/data_dictionary_smarticu.xlsx`

ไฟล์ที่ **ไม่มีอยู่จริง** (โน้ตบุ๊คเก่าอ้างผิด):
- `*_example_2.csv` — ไม่มี ข้อมูลผู้ป่วยอยู่ใน sheet ของ xlsx เดียวกัน
- ไฟล์ `.csv` ใดๆ ใน diabetes/ หรือ hypertension/

อ่านด้วย `pd.read_excel()` ไม่ใช่ `pd.read_csv()`

โครงสร้าง sheet ใน xlsx (ยืนยันแล้ว):
- `description` — data dictionary (อธิบาย columns)
- `data` — ข้อมูลผู้ป่วยจริง (100 rows × 4,714 cols สำหรับ DM; 4,190 cols สำหรับ HT)

`load_patient_data()` ใน idea7 เลือก sheet `data` ได้อัตโนมัติ (มี rows มากกว่า)

---

## Pattern: Path Resolution ใน Jupyter Notebook (VS Code)

VS Code ตั้ง CWD เป็น workspace root (`BDIHackathon/`) **ไม่ใช่** directory ของ notebook — ทำให้ relative path เช่น `".."` พาไปผิดที่

**Pattern ที่ถูกต้อง** (ใช้ใน notebook ทุกไฟล์):

```python
import os
try:
    # VS Code Jupyter ตั้งค่า __vsc_ipynb_file__ เป็น absolute path ของ notebook
    _NOTEBOOK_DIR = os.path.dirname(os.path.abspath(__vsc_ipynb_file__))
except NameError:
    # Jupyter Lab/Notebook classic: ต้อง launch จาก BDI-Young-Innovator_2026/
    _NOTEBOOK_DIR = os.getcwd()

_DATA_ROOT = os.path.join(_NOTEBOOK_DIR, "..", "Sampled Dataset",
                          "bdi-hackathon-2026-sampled-dataset")
```

Pattern นี้ใช้แล้วใน `idea7_ltfu_eda.ipynb` และ `healthcare_datathon_proposal_eda.ipynb`

---

## การรัน Notebook แบบ Headless (CLI)

`jupyter` ไม่อยู่ใน PATH บนเครื่องนี้ — ต้องรันผ่าน `python -m jupyter`:

```bash
# รัน notebook และ capture output ทั้งหมด
python -m jupyter nbconvert --to notebook --execute \
  --output <output_path.ipynb> \
  --ExecutePreprocessor.timeout=180 \
  <input_notebook.ipynb>
```

ใช้สำหรับ: verify notebook รันครบโดยไม่มี error, capture output ก่อน commit

---

## ข้อควรระวัง: แก้ไขไฟล์ .ipynb

ต้องใช้ **`NotebookEdit` tool** เท่านั้น — `Edit` tool จะ error ว่า "File is a Jupyter Notebook. Use the NotebookEdit to edit this file."

`NotebookEdit` ใช้ `cell_id` (ไม่ใช่ cell index) ในการระบุ cell ที่ต้องการแก้

---

## ข้อมูลพฤติกรรม Sample Dataset (100 คน)

**DM sample data quirk:** `max_consecutive_gap` min = 31 periods → clinical threshold = 3 ทำให้ LTFU = 100%

แก้ไขแล้วใน `idea7_ltfu_eda.ipynb` Section 3 ด้วย **auto-detect threshold**:
- ถ้า clinical threshold (3) ทำให้ทุกคนเป็น LTFU → ปรับเป็น `75th percentile + 1` อัตโนมัติ
- ผลลัพธ์ใน sample: threshold = 62, Active = 76%, LTFU = 24%
- Full dataset (70K คน): threshold 3 จะทำงานปกติ ไม่มี auto-adjust

---

## Pattern: แยก Logic ออกเป็น .py ก่อน แล้ว Import เข้า Notebook

VS Code Jupyter มีปัญหา output ไม่ตรงกัน — ไฟล์ `.py` จริงกับ cell ที่กำลังรันอยู่อาจแสดงผลคนละอย่าง เนื่องจาก notebook kernel cache code เวอร์ชันเก่าไว้

**Workflow ที่ถูกต้อง:**
1. เขียน logic ทั้งหมด (functions, classes, pipelines) ใน `.py` แยก เช่น `idea7_features.py`, `idea2_model.py`
2. `import` เข้า notebook เพื่อทดลองและแสดงผล
3. เมื่อแก้ไข `.py` ให้ restart kernel หรือใช้ `importlib.reload()` เพื่อโหลดโค้ดใหม่

```python
import importlib
import idea7_features
importlib.reload(idea7_features)
from idea7_features import load_patient_data, engineer_features
```

**สิ่งที่ต้องหลีกเลี่ยง:** อย่าเขียน logic หลักทั้งหมดลงใน notebook cell โดยตรง — แก้แล้ว output อาจไม่อัปเดตถ้าไม่ restart kernel
