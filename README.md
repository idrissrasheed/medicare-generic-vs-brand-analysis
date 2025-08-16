<div align="center">

![Medicare Banner](https://img.shields.io/badge/Medicare-Part%20D%20Analysis-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Healthcare](https://img.shields.io/badge/Healthcare-Cost%20Optimization-00A86B?style=for-the-badge&logo=health&logoColor=white)
![Savings](https://img.shields.io/badge/Potential%20Savings-$99.9B-FFD700?style=for-the-badge&logo=dollar-sign&logoColor=black)

</div>

---

## Medicare Part D: Generic vs Brand Savings Analysis

SQL-driven analysis of Medicare prescription data revealing $99.9B+ potential savings through generic adoption.

### Overview
- Goal: Quantify savings from brand-to-generic drug switching; generate Power BI datasets.
- Stack: Python, SQL (SQLite), pandas, CMS API integration.

### Dataset
- Source: [CMS Medicare Part D API](https://data.cms.gov/data-api/v1/dataset/7e0b4365-fd63-4a29-8f5e-e0ac9f66a81b/data)
- Included: 14,309 drug records (2019-2023), real-time data fetch.

### Methods
- Cleaning: drug type classification, numeric conversion, null handling.
- Analysis: brand vs generic cost comparisons, savings calculations via SQL CTEs.
- Export: 6 Power BI-ready CSV datasets with relationships.

### Results (highlights)
- Total potential savings: $99.9B annually.
- Generic adoption could save 66% on average per claim.
- 322 drug pairs available in both brand/generic forms.
- Top opportunity: Metformin HCL ($48.8B potential savings).
- See generated CSV outputs for full insights.

### How to Run
1) Install: `pip install pandas requests numpy`
2) Execute: `python generate_medicare_datasets.py`
3) Import CSVs to Power BI following setup guide.

### Structure
- `generate_medicare_datasets.py`: main analysis pipeline
- `docs/`: Power BI setup guide
- Output: `powerbi_*.csv` files for dashboard creation

### Next
- Automated refresh pipeline; trend forecasting.

### License
MIT

---

<div align="center">

![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=flat&logo=python&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-Ready-F2C811?style=flat&logo=powerbi&logoColor=black)
![SQL](https://img.shields.io/badge/SQL-Analysis-336791?style=flat&logo=postgresql&logoColor=white)
![CMS](https://img.shields.io/badge/Data%20Source-CMS%20API-1F425F?style=flat&logo=government&logoColor=white)

</div>
