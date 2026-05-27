![Cover Image](/cover_image.png)

> Enterprise-grade operational analytics platform built using Power BI for FMCG-3PL Quality Assurance operations

<p style="color: orange; font-size: 16px; font-style: italic">Disclaimer..❗ </br>
This project is a portfolio representation of a real-world enterprise analytics initiative. Certain implementation details and data elements have been anonymized for confidentiality purposes.</p>

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
# 📟 Dashboard Preview

![Dashboard Screenshot](/PBI_dashboard.png)

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

## 👨‍💻 Author
**Thilina Perera | Data with TP**
```
📌 Data Science/ Data Analytics D-Technosavant
📌 Machine Learning/ Deep Learning, LLM/LMM, NLP, and Data Engineering Inquisitive
``` 
[![LinkedIn](https://img.shields.io/badge/LinkedIn-%230077B5.svg?logo=linkedin&logoColor=white)](https://linkedin.com/in/thilina-perera-148aa934/)  [![TikTok](https://img.shields.io/badge/TikTok-%23000000.svg?logo=TikTok&logoColor=white)](https://tiktok.com/@data_with_tp) [![YouTube](https://img.shields.io/badge/YouTube-%23FF0000.svg?logo=YouTube&logoColor=white)](https://youtube.com/@Data_with_TP) [![email](https://img.shields.io/badge/Email-D14836?logo=gmail&logoColor=white)](mailto:kgttpereraqatar2022@gmail.com) 

## 📝 License 
    This project is licensed under the MIT License.
    Free to use and extend.