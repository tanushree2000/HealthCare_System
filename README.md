
```markdown
# 🏥 Healthcare Enrollment ETL Workflow – Informatica PowerCenter

This project demonstrates a complete **ETL workflow** for healthcare enrollment using Informatica PowerCenter. It ingests group, subgroup, and subscriber data from flat files, performs transformation and validation, loads into Oracle staging tables, and finally generates personalized XML welcome letters.

---

## 📁 Folder Structure

```

/Healthcare\_Enrollment\_ETL/
├── Data/
│   ├── Group\_Comma\_2239842.csv
│   ├── Group\_Fixed\_2239842.txt
│   ├── Subgrp\_Tab\_2239842.txt
│   ├── Subscriber\_Comma\_2239842.csv
├── Workflow\_Mappings/
│   ├── WRKF\_GROUP\_2239842.XML
│   ├── SUBGRP\_WRKF\_2239842.XML
│   ├── WRKFL\_SUBSCRIBER\_2239842.XML
│   ├── WRKFL\_LETTER\_2239842.XML
├── Output/
│   ├── LETTER\_2239842.XML
│   ├── W\_Welcome\_letter.XML
├── Reference/
│   ├── group\_details.xml
│   ├── group\_details.1.xml / .2 / .3
│   ├── Group\_Details.xsd
├── Errors/
│   ├── subgrp\_join\_errors\_2239842\_.txt
├── Session\_Logs/
│   ├── GROUP\_log.txt
│   ├── SUBGRP\_log.txt
│   ├── SUBSCRIBER\_log.txt
│   ├── welcome\_letter\_log.txt
├── Screenshots/
│   ├── Architecture Overview of Data Flow Process.png
│   ├── Star Schema Model for OptiRetail.png
│   ├── Project\_monitor.PNG
│   ├── Project\_wf Monitor.PNG
│   ├── subgrp\_wfmonitor.PNG
└── README.md

````

---

## 📌 Objective

To build a reliable data pipeline that:
- Loads raw healthcare data from multiple flat file sources
- Validates subgroup and subscriber associations
- Stores clean data in Oracle staging tables
- Outputs XML-based welcome letters for valid subscribers

---

## 🔁 ETL Process Flow

### Step-by-step flow of data from source to output:

![Data Flow Architecture](./Screenshots/Architecture%20Overview%20of%20Data%20Flow%20Process.png)

1. **Group Load**  
   Source: Fixed-width + comma file → `GROUP_2239842` Oracle table

2. **Subgroup Load**  
   Source: Tab-delimited file  
   Lookups: Validated against `GROUP_ID`  
   Rejections: Logged in `subgrp_join_errors_2239842_.txt`

3. **Subscriber Load**  
   Source: Comma-delimited  
   Lookups: Against `(GRP_ID, SUBGRP_ID)` pair  
   Invalid records: Skipped

4. **Letter Generation**  
   Source: Oracle staging tables  
   Output: Structured XML per subscriber → `LETTER_2239842.XML`

---

## 🧩 Star Schema

Your backend design follows a dimensional model structure:

![Star Schema](./Screenshots/Star%20Schema%20Model%20for%20OptiRetail.png)

- **Fact Table**: Subscriber Enrollment  
- **Dimensions**: Group, Subgroup, Time

---

## ⚙️ Workflows and Mappings

### 1. `WRKFL_GROUP_2239842`
- Loads data from `Group_Fixed_2239842.txt` and `Group_Comma_2239842.csv`
- Populates `GROUP_2239842` table in Oracle
- Includes transformation with `LPAD(GROUP_ID, 8, '0')` to standardize IDs

### 2. `SUBGRP_WRKF_2239842`
- Reads `Subgrp_Tab_2239842.txt`
- Validates `GROUP_ID` using lookup on group table
- Inserts valid rows into `SUBGRP_39842`
- Logs unmatched `GROUP_ID`s in `subgrp_join_errors_2239842_.txt`

### 3. `WRKFL_SUBSCRIBER_2239842`
- Processes `Subscriber_Comma_2239842.csv`
- Ensures valid `(GRP_ID, SUBGRP_ID)` using joins
- Pushes clean data to `SUBSCRIBER_2239842` table

### 4. `WRKFL_LETTER_2239842`
- Performs final 3-way join between Group, Subgroup, and Subscriber
- Generates subscriber-level XML welcome letters
- Output saved as `LETTER_2239842.XML` and `W_Welcome_letter.XML`

---

## 🔍 Session Log Snapshots

All sessions completed **successfully**. Execution time: under 5 seconds per job.

| Workflow        | Mapping    | Status     | Time (HH:MM:SS)         |
|-----------------|------------|------------|--------------------------|
| WRKFL_GROUP     | GROUP      | ✅ Succeeded | 15:21:17 – 15:21:19     |
| SUBGRP_WRKF     | SUBGRP     | ✅ Succeeded | 15:21:30 – 15:21:33     |
| WRKFL_SUBSCRIBER| SUBSCRIBER | ✅ Succeeded | 15:21:46 – 15:21:48     |

📸 Screens:
- ![Workflow Monitor](./Screenshots/Project_wf%20Monitor.PNG)
- ![Subgroup Monitor](./Screenshots/subgrp_wfmonitor.PNG)

---

## 🧠 Notes on Warnings

Some harmless parse warnings observed:
- `LPAD(GRP_ID, 8, 0)` and `LPAD(SUBGRP_ID, 4, 0)` auto-cast to string
- Didn’t affect final outputs

**Fix (optional)**:
```sql
TO_CHAR(LPAD(TO_CHAR(GRP_ID), 8, '0'))
````

---

## 🛠 Tools Used

* **Informatica PowerCenter v10.5.2**
* **Oracle 12c**
* Flat files (.csv, .txt)
* XML / XSD validation
* Windows OS

---

## 👩‍💻 Author

**Tanushree Poojary**
MS in Information Management
University of Illinois Urbana-Champaign
📫 [tp25@illinois.edu](mailto:tp25@illinois.edu)
🔗 [LinkedIn](https://www.linkedin.com/in/tanushreep25/)

---

## 📁 License

For academic and demo purposes only.

```

---

Paste this directly into your `README.md` on GitHub. Make sure to place your PNG images in a `/Screenshots/` folder and name them exactly as referenced. Let me know if you want a live demo GIF or a badge section.
```
