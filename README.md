![Cover Image](/cover_Image.png)

> Enterprise-grade operational analytics platform built using Power BI for FMCG-3PL Quality Assurance operations

<p style="color: red; font-size: 16px; font-style: italic">Disclaimer..❗ </br>
This project is a portfolio representation of a real-world enterprise analytics initiative. Certain implementation details and data elements have been anonymized for confidentiality purposes.</p>

---

# 🚀 Release Evolution

<p style="color: orange; font-size: 16px; font-style: italic; font-weight: bold">QCD Analytics v1.0 — Initial Enterprise Deployment.</p>

---

# 📃 Project Overview & Business Objectives

QCD Analytics is a real-world operational BI transformation initiative designed to modernize quality assurance and compliance reporting workflows within FMCG warehouse operations. The solution evolved from fragmented Excel-based reporting into a modular, scalable, service-deployed Power BI analytics platform supporting:



    👉 Centralize operational QA reporting
    👉 Reduce manual reporting effort
    👉 Improve visibility into quality KPIs/ SLAs
    👉 Support operational decision-making
    👉 Standardize reporting architecture
    👉 Enable scalable historical analytics

---
# 📂 Folder Structure

```bash
DE_Projects_QCD_Analytics_Enterprise_PBI_Digital_Transformation/
│
├── Governance/
│   ├── Approval Documents/
│   ├── Change log.xlsx
│   ├── Risk register.xlsx
│   └── Scope.xlsx
│
├── Source_Data/           
│   └── 15x Separate folders with continuously growing excel files/
│
├── Design/
│   ├── Data_Model_Diagrams/
│   └── RLS_Design.xlsx
│
├── PowerBI/
│   ├── Dev/
│   │   ├── QCD_Analytics_Dev_V1.0.pbix
│   │   ├── QCD_Analytics_Dev_V2.0.pbix
│   │   └── QCD_Analytics_Dev_V3.0.pbix
│   │
│   └── Prod/
│       └── QCD_Analytics_Prod.pbix
│
├── Validation/
│   ├── Signoff_Records/
│   ├── Data_Validation.xlsx
│   └── Performance_Tests.xlsx
│
├── Project Documentation/
│   ├── Team training/
│   ├── Data_Dictionary.xlsx
│   ├── Measure_Definitions.xlsx
│   ├── Refresh_ and_Operations_Guide.xlsx
│   └── Project_Report.docx
│
├── Documentation/
│   ├── Project Launch & Team training/
│   ├── Data_Dictionary.xlsx
│   ├── Measure_Definitions.xlsx
│   ├── Refresh_ and_Operations_Guide.xlsx
│   └── Project_Report.docx
│
├── Deployment
│    ├── Refresh_Logs/
│    ├── Publish_Checklist.xlsx
│    └── Post_Deployment_Issues.xlsx
│
├── README.MD
├── .gitignore
└── LICENSE.MD
```
---

# 🧰 Tech-Stack

| Category | Technologies |
|---|---|
| Data Sources | ![Excel](https://img.shields.io/badge/Excel-Spreadsheet-217346.svg?logo=microsoftexcel&logoColor=white) + ![Folder-based ingestion](https://img.shields.io/badge/Folder-Ingestion-0078D4.svg?logo=microsoft&logoColor=white) |
| BI Platform | ![Power BI](https://img.shields.io/badge/PBI_Desktop-Analytics-F2C811.svg?logo=powerbi&logoColor=black) |
| Data Transformation | ![Power Query](https://img.shields.io/badge/Power_Query_(M)-ETL-E94E1C.svg?logo=powerbi&logoColor=white) |
| Data Governance | ![Parameterized source management](https://img.shields.io/badge/Parameterized_Folder_Path-Dynamic_Source-E94E1C.svg?logo=powerbi&logoColor=white) |
| ETL Design | ![Stage → Model architecture](https://img.shields.io/badge/Stage_Layer-Input-4A90E2.svg?logo=diagramsdotnet&logoColor=white) + ![Folder-based ingestion]( https://img.shields.io/badge/Model_Layer-Design-FF6B6B.svg?logo=diagramsdotnet&logoColor=white) |
| Data Quality | ![Data Quality](https://img.shields.io/badge/Data_Quality-Validation_&_Audit-4CAF50.svg?logo=checkmarx&logoColor=white) |
| Sementic Modeling | ![DAX](https://img.shields.io/badge/DAX-Measures-F2C811.svg?logo=powerbi&logoColor=black) |
| UI Design | ![Adobe Photoshop](https://img.shields.io/badge/Adobe_Photoshop-Graphical_Layer-31A8FF.svg?logo=adobephotoshop&logoColor=white) |
| Deployment | ![Power BI Service](https://img.shields.io/badge/PBI_Service-Cloud-F2C811.svg?logo=powerbi&logoColor=black) |
| Connectivity | ![On-Premises Data Gateway](https://img.shields.io/badge/On_Premises_Data_Gateway-Bridge-0078D4.svg?logo=microsoft&logoColor=white) |
| Report Automation | ![Refresh Strategy](https://img.shields.io/badge/Refresh_Strategy-Scheduled_Automation-2196F3.svg?logo=clock&logoColor=white) |

---

# 🏗️ Solution Architecture

![Architecture Diagram](/architecture.png)
    
---

### Key Engineering Highlights

- Modular Stage → Model Power Query architecture
- Parameterized folder-based ingestion workflows
- Archive / Current partition strategy for scalable refresh optimization
- Reusable ETL transformation patterns
- Automated scheduled refresh through On-Premises Gateway
- Data quality validation and ETL audit logic
- Centralized operational reporting architecture
- Power BI Service deployment and workspace distribution
- Governance-oriented transformation design
- Transition roadmap toward SQL/Data Warehouse architecture

---

# 📊 Archive / Current Partition Strategy

![Arch-Cur Partition](/arch_curr_strategy.png)

Large operational datasets were separated into:

```yaml
(i.e.)
Daily_Inspections/
├── Archive/ # Historical Data
└── Current/ # Most recent data
```

This approach improved:
- Refresh efficiency
- Transformation scalability
- Historical data handling
- Operational maintainability

---
# 🎛️  Power Query Engineering Highlights

The ETL layer was engineered using reusable Power Query design patterns focused on maintainability, governance, and operational scalability. Implemented transformation logic included:

- Schema enforcement
- Defensive date & numeric conversions
- Missing value normalization
- Metadata preservation
- Parameterized source management
- Controlled column reduction
- ETL audit timestamping
- Data quality validation flags
- Reusable Combine Files pipelines
- Analytical unpivot transformations

---
# 🧩 Data Model

![Data Model](/data_model.png)

---
# 📟 Dashboard Preview

![Dashboard Screenshot](/pb_dashboard.png)

>>> 🔐 Data Privacy & Anonymization: No sensitive organizational data is included.

---

# 📊 Reporting Domains

The solution supports analytics across:
- Warehouse operations monitoring
- Vendor compliance
- Customer satisfaction analysis
- Audit tracking
- Temperature monitoring
- Operational QA KPIs & SLAs
- Inspection and hygiene monitoring

---

# 🔗 Gateway Architecture

![Gateway Architecture](/gateway.png)

Implemented:
- Power BI Service deployment
- On-Premises Gateway configuration
- Scheduled refresh automation
- Gateway authentication handling
- Workspace/App distribution strategy

---

# 🧠 Technical Challenges

A major production challenge involved:

    - Gateway authentication behavior
    - UNC path access handling
    - Windows access authorization
    - Scheduled refresh infrastructure configuration

The issue was successfully resolved through gateway credential reconfiguration and infrastructure validation, enabling automated scheduled refresh functionality within Power BI Service.

---

# 🏁 Operational Outcomes

The implemented solution successfully achieved:

- Automated scheduled refresh workflows
- Centralized operational reporting
- Scalable folder-based ingestion architecture
- Modular ETL transformation pipelines
- Reduced manual reporting dependency
- Improved operational KPI visibility
- Production-ready dashboard deployment

---

# 🛸 Future Roadmap

![Future Roadmap](/roadmap.png)

Planned future enhancements include:

    - Incremental Refresh
    - Deployment Pipelines
    - SQL-based ingestion layer
    - Medallion architecture migration
    - Data Warehouse transition
    - Python-assisted ETL automation
    - Fabric / Lakehouse exploration
    - CI/CD-oriented BI deployment workflows

---

# 🚀 Release Evolution

<p style="color: orange; font-size: 16px; font-style: italic; font-weight: bold">QCD Analytics v1.1 — Platform Stabilization & Operational Maturity.</p>

---
![QCDA v1.1: Stabilization & Maturity](/v1.1.png)

---
Following the initial deployment, the platform progressed from a reporting solution into a production-ready operational analytics platform.

---

### ⚡ Incremental Refresh Implementation

Designed, deployed, and validated Incremental Refresh architecture using:

* RangeStart / RangeEnd parameter framework
* Historical retention strategy
* Rolling refresh windows
* Partition-based processing

| Refresh Type        | Approx. Duration |
| ------------------- | ---------------- |
| Initial Refresh     | ~85 minutes      |
| Incremental Refresh | ~33–45 minutes   |

**Outcome:** Reduced refresh workload, improved scalability, and optimized historical data processing.

---

### 🔄 Multi-Environment Development Lifecycle

Established a structured promotion workflow:

```text
DEV → TEST → PROD
```

Environment Structure:

* **DEV** → Personal development environment (Personal Laptop to avoid IT policies, authorization constraints and potential security risks)
* **TEST** → Dedicated validation workspace
* **PROD** → Operational Quality Control workspace

This provides a lightweight deployment pipeline model within Power BI Pro licensing constraints.

---

### 🧹 Technical Debt Remediation

Completed architecture cleanup initiatives:

* Sample File architecture redesign
* Explicit Archive-folder schema sourcing
* Legacy connection removal
* Obsolete source cleanup
* Standardized data source inventory

**Outcome:** Improved maintainability, reduced migration risk, and strengthened long-term platform governance.

---

### 🏗️ Platform Capability Snapshot

Current platform capabilities include:

    ✅ Parameterized source architecture
    ✅ Archive / Current ingestion strategy
    ✅ Automated cloud refresh
    ✅ Incremental refresh
    ✅ Multi-environment promotion workflow
    ✅ Technical debt remediation
    ✅ Production Power BI App deployment

---

## 🔭 Next Planned Milestones

### Version 1.2 — Oracle Application Platform

* Oracle Database Free, ORDS & Oracle APEX
* Operational data capture applications
* Inspection and workflow management
* Digital process transformation

### Version 2.0 — Oracle Data Platform Foundation

* Bronze and Gold schema architecture
* Business rules and reporting views
* Centralized operational data platform
* Scalable reporting foundation

### Version 2.1 — Power BI Oracle Integration

* Oracle connectivity
* Dataset migration from file-based architecture
* Simplified Power Query transformations
* Data validation and reconciliation

### Version 3.0 — Enterprise Analytics Evolution

* Star schema preparation
* Data warehouse foundations
* Analytics engineering workflows
* Advanced operational analytics

---

# 🚀 Release Evolution

<p style="color: orange; font-size: 16px; font-style: italic; font-weight: bold">QCD Analytics v1.2 — Oracle Application Platform Prototype</p>

---
![QCDA v1.2](/oracle_incorporation.png)
---


## Click here to see the full Oracle DB and Apex Setup guide 👇
>## [Oracle DB & Apex Setup Guide](/documentation/Oracle%20db_&_apex_setup_guide.MD)

# 🧰 Tech-Stack

| Categoty                | Technology                       |
| ------------------------ | ------------------------------------ |
| Data Capture             | ![Oracle APEX](https://img.shields.io/badge/APEX-Forms-C74634.svg?logo=oracle&logoColor=white)                  |
| Data Storage             | ![Oracle Database](https://img.shields.io/badge/Oracle-DB-F80000.svg?logo=oracle&logoColor=white)                      |
| Data Cleansing           | ![Silver Views](https://img.shields.io/badge/Silver-Data_Cleaning-8C8C8C.svg?logo=oracle&logoColor=white)                         |
| Business Logic           | ![Gold Views](https://img.shields.io/badge/Gold-Business_Logic-FFD700.svg?logo=oracle&logoColor=white)                           |
| KPI Engine               | ![Oracle SQL / PL-SQL](https://img.shields.io/badge/SQL/_PL_SQL-Dev-F80000.svg?logo=oracle&logoColor=white)                  |
| Analytics                | ![Power BI](https://img.shields.io/badge/Power_BI-Analytics_&_Dashboards-F2C811.svg?logo=powerbi&logoColor=black)                             |
| Deployment Model         | ![Docker](https://img.shields.io/badge/Docker-Development_Environment-2496ED.svg?logo=docker&logoColor=white) |
| Source Control           | ![GitHub Release](https://img.shields.io/badge/Release-Release_Management-181717.svg?logo=github&logoColor=white)            |



# 📂 Folder Structure - Modified
```bash
DE_Projects_QCD_Analytics_Enterprise_PBI_Digital_Transformation/
│
.
.

├── Oracle/
│   │
│   ├── Database/
│   │   ├── Bronze_DDL/
│   │   ├── Silver_views/
│   │   ├── Gold_Views/
│   │   ├── Functions/
│   │   ├── Triggers/
│   │   └── Procedures/
│   │
│   ├── APEX/
│   │   ├── Applications/
│   │   ├── Exports/
│   │   ├── Screenshots/
│   │   └── Form_Designs/
│   │
│   └── Architecture/
│       ├── ERD/
│       ├── Data_Flow/
│       └── Environment_Setup/
│
├── PowerBI/
│
├── Design/
│   ├── Architecture
│   ├── Validation
│   ├── Data_Model_Diagrams/
│   ├── Source_To_Table_Mapping/
│   ├── APEX_Form_Mapping/
│   └── RLS_Design.xlsx
│
├── Validation/
│   ├── Data_Validation/
│   ├── Parallel_Run_Testing/
│   ├── Signoff_Records/
│   └── Performance_Tests/
│
├── Documentation/
│
├── Release notes/
│   └── v1.0.0-prototype.md
│
├── README.md
├── LICENSE.md
└── .gitignore
```
---

## Platform Transformation Milestone

QCD Analytics has now progressed beyond Power BI-only reporting and entered the Oracle Application Platform phase.

This release establishes the foundation for operational data capture, business-rule automation, and centralized analytics using Oracle Database and Oracle APEX.

---

## Oracle Application Platform

### Platform Setup

    ✅ Oracle Database Free (26ai Release 23.26) environment established
    ✅ Oracle APEX (24.1) development environment configured
    ✅ Oracle SQL Developer integration 
    ✅ Oracle SQL Developer Extension for VSCode
    ✅ GitHub-based development lifecycle initiated

---

## Operational Application Prototype dDevelopment
    Source file: Customer Complaint Log Excel -> Apex Form

```
✅ Customer Complaint Log operational data model designed
✅ Oracle Bronze layer implemented
✅ Lookup table framework implemented
✅ Lookup maintenance forms developed
✅ Customer Complaint APEX data entry application developed
✅ Interactive Grid maintenance functionality implemented
```
---

## Data Migration & Validation

### Historical Data Integration
```
✅ Historical complaint records successfully migrated
✅ Oracle import validation completed
✅ Data quality validation completed
✅ End-to-end record reconciliation completed
```
---

## Oracle Analytics Architecture

### Bronze → Silver → Gold Framework
```
✅ Bronze layer: For raw operational data capture
✅ Silver layer: For data cleansing and standardization
✅ Gold layer: For business logic and KPI calculations
✅ Reusable Oracle view architecture established
```
---

## Data Governance & Auditability

### Platform Controls
```
✅ Audit tracking framework implemented
✅ Created By / Created Date -> Data insertion tracking
✅ Updated By / Updated Date -> Data updation tracking
✅ Controlled lookup management
✅ Standardized data entry architecture
```
---

## Business Logic Migration

### Excel-to-Oracle Conversion
```
✅ Operational business rules migrated from Excel formulas to Oracle SQL
✅ KPI calculations centralized within Oracle
✅ Status determination logic centralized within Oracle
✅ Reporting-ready Gold Views established
```
---

## Current Status
```
✅ End-to-end Oracle operational workflow completed
✅ Customer Complaint module operational
✅ Bronze → Silver → Gold architecture validated
✅ Ready for stakeholder review and enterprise deployment discussion
```
---

# Next Planned Milestones

## Version 2.0 — Oracle Data Platform Foundation
- Establish dedicated corporate Oracle development environment
- Standardize Bronze, Silver and Gold architecture
- Expand operational form portfolio
- Create reusable enterprise data model patterns

---

## Version 2.1 — Power BI Oracle Integration
- Connect Power BI directly to Oracle Gold Views
- Reduce file-based transformation dependency
- Simplify Power Query architecture
- Centralize KPI calculations within Oracle

## Version 3.0 — Enterprise Analytics Evolution
- Star Schema preparation
- Data Warehouse foundations
- Analytics engineering practices
- Enterprise reporting framework

---

## 👨‍💻 Author/ BI Developer
**Thilina Perera | Data with TP**
```
📌 Data Science/ Data Analytics D-Technosavant
📌 Machine Learning/ Deep Learning, LLM/LMM, NLP, and Data Engineering Inquisitive
``` 
[![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?logo=linkedin&logoColor=white)](https://linkedin.com/in/thilina-perera-148aa934/)  [![TikTok](https://img.shields.io/badge/TikTok-%23000000.svg?logo=TikTok&logoColor=white)](https://tiktok.com/@data_with_tp) [![YouTube](https://img.shields.io/badge/YouTube-%23FF0000.svg?logo=YouTube&logoColor=white)](https://youtube.com/@Data_with_TP) [![email](https://img.shields.io/badge/Email-D14836?logo=gmail&logoColor=white)](mailto:kgttpereraqatar2022@gmail.com) 

## 📝 License 
    This project is licensed under the MIT License.
    Free to use and extend.