# Medicare Part D: Generic vs Brand Drug Analysis

![Medicare Part D Analysis](https://img.shields.io/badge/Medicare-Part%20D%20Analysis-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Python](https://img.shields.io/badge/python-v3.8+-blue.svg?style=for-the-badge&logo=python&logoColor=white)
![Power BI](https://img.shields.io/badge/Power_BI-Ready-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![SQL](https://img.shields.io/badge/SQL-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)


# Medicare Part D - Generic vs Brand Name Savings Analysis

**A comprehensive analysis of potential cost savings through increased generic drug adoption in Medicare Part D**

---

## 📊 Executive Summary

This analysis examines Medicare Part D spending data to identify opportunities for cost savings through increased generic drug adoption. Using 2019-2023 CMS data, we've identified **$99.87 billion in potential annual savings** across **322 drug products** that have both brand and generic versions available.

### Key Findings
- **Total Potential Savings**: $99.87 billion annually
- **Drugs with Opportunities**: 322 of 14,309 total drugs (2.2%)
- **Brand Price Premium**: 417% higher cost than generic equivalents
- **Critical Impact Drugs**: 9 drugs account for $88+ billion in savings potential

---

## 🎯 Business Objectives

1. **Identify high-impact savings opportunities** in Medicare Part D drug spending
2. **Quantify potential cost reductions** through generic substitution
3. **Prioritize drugs** based on savings potential and beneficiary impact
4. **Create actionable insights** for policy makers and healthcare administrators
5. **Generate Power BI-ready datasets** for executive dashboards

---

## 📁 Project Structure

```
medicare-analysis/
├── notebooks/
│   └── medicare_analysis.ipynb          # Main analysis notebook
├── data/
│   ├── raw/                            # CMS API data (auto-fetched)
│   └── processed/                      # Cleaned and analyzed datasets
├── outputs/
│   ├── powerbi_main_drug_facts.csv     # Complete drug database
│   ├── powerbi_savings_opportunities.csv # 322 savings opportunities
│   ├── powerbi_top_opportunities.csv    # Top 50 with impact categories
│   ├── powerbi_generic_vs_brand_summary.csv # Market overview
│   └── powerbi_manufacturer_analysis.csv # Manufacturer breakdown
├── visuals/
│   └── dashboard_mockup.png            # Recommended dashboard layout
└── README.md                           # This file
```

---

## 🔧 Technical Implementation

### Data Source
- **API**: CMS Medicare Part D Spending by Drug Dataset
- **Endpoint**: `https://data.cms.gov/data-api/v1/dataset/7e0b4365-fd63-4a29-8f5e-e0ac9f66a81b/data`
- **Records**: 14,309 drug products with 5 years of spending data (2019-2023)
- **Update Frequency**: Annual (CMS releases new data each year)

### Analysis Methodology

#### 1. Data Extraction & Cleaning
```python
# Fetch all Medicare Part D data using pagination
df = fetch_all_medicare_data(batch_size=5000, target_records=14309)

# Clean and standardize drug names
clean_df['drug_type'] = clean_df.apply(
    lambda row: 'Generic' if row['brand_name'] == row['generic_name'] else 'Brand', 
    axis=1
)
```

#### 2. Savings Calculation Logic
```sql
-- Identify drugs with both brand and generic versions
WITH drug_comparison AS (
    SELECT 
        generic_name,
        MAX(CASE WHEN drug_type = 'Brand' THEN avg_spending_per_claim_2023 END) as brand_cost,
        MAX(CASE WHEN drug_type = 'Generic' THEN avg_spending_per_claim_2023 END) as generic_cost,
        MAX(CASE WHEN drug_type = 'Brand' THEN total_claims_2023 END) as brand_claims
    FROM medicare_drugs
    GROUP BY generic_name
    HAVING COUNT(DISTINCT drug_type) = 2
)
-- Calculate potential savings if all brand claims used generic pricing
SELECT 
    generic_name,
    brand_claims * (brand_cost - generic_cost) as total_potential_savings
FROM drug_comparison
WHERE brand_cost > generic_cost
```

#### 3. Impact Categorization
```python
# Categorize drugs by savings potential
impact_categories = {
    'Critical Impact': savings >= 1_000_000_000,    # ≥$1B
    'High Impact': savings >= 400_000_000,          # ≥$400M  
    'Medium Impact': savings >= 50_000_000,         # ≥$50M
    'Low Impact': savings < 50_000_000              # <$50M
}
```

---

## 📈 Key Analytics & Insights

### Top 10 Savings Opportunities

| Rank | Generic Name | Potential Savings | Market Share Generic | Impact Category |
|------|-------------|------------------|---------------------|-----------------|
| 1 | METFORMIN HCL | $48.84B | 72.4% | Critical |
| 2 | BUPROPION HCL | $25.64B | 9.8% | Critical |
| 3 | VENLAFAXINE HCL | $7.00B | 12.2% | Critical |
| 4 | NIFEDIPINE | $2.27B | 0.4% | Critical |
| 5 | INSULIN LISPRO | $1.98B | 13.2% | Critical |

### Market Analysis
- **Generic Drug Adoption**: 33.7% of drug products, but only 9.4% of spending
- **Brand Premium**: Average 417% higher cost than generic equivalents
- **Opportunity Concentration**: 9 Critical Impact drugs represent 88% of total savings potential

### Beneficiary Impact
- **Total Beneficiaries**: 50+ million Medicare Part D enrollees
- **Claims Affected**: 46+ million brand name claims annually
- **Average Savings per Claim**: $2,167 potential reduction

---

## 💻 Requirements & Setup

### Python Dependencies
```bash
pip install pandas requests numpy sqlite3 warnings datetime
```

### Environment Setup
```python
# No API keys required - CMS data is public
# Notebook automatically handles data fetching and processing
# Runtime: ~5-10 minutes for full analysis
```

### Running the Analysis
1. **Open Jupyter Notebook**: `jupyter notebook medicare_analysis.ipynb`
2. **Run All Cells**: Executes complete pipeline from data fetch to output generation
3. **Outputs Generated**: 5 CSV files ready for Power BI import

---

## 📊 Power BI Integration

### Dashboard Recommendations

#### Header KPIs
```
Annual Potential Savings: $99.87B
Coverage: 322 of 14,309 drugs (2.2%)  
Brand Premium: 417% vs Generic
Critical Impact: 9 drugs = $88B+ savings
```

#### Key Visualizations
1. **Top Opportunities Bar Chart**: Horizontal bars showing savings potential with generic market share overlay
2. **Opportunity Matrix**: Bubble chart (X: Generic %, Y: Savings, Size: Beneficiaries)
3. **Generic Adoption Progress**: Progress bars showing 33.7% drugs vs 9.4% spending
4. **Impact Category Breakdown**: Cards showing distribution across Critical/High/Medium/Low impact

#### Data Sources for Power BI
- `powerbi_main_drug_facts.csv` - Complete drug database (14,309 records)
- `powerbi_savings_opportunities.csv` - All opportunities (322 records)  
- `powerbi_top_opportunities.csv` - Top 50 with impact categories
- `powerbi_generic_vs_brand_summary.csv` - Market overview (2 records)
- `powerbi_manufacturer_analysis.csv` - Manufacturer breakdown (597 records)

---

## 🎯 Business Impact & Recommendations

### Immediate Actions (Critical Impact Drugs)
1. **BUPROPION HCL**: Only 9.8% generic adoption, $25.6B savings potential
2. **VENLAFAXINE HCL**: Only 12.2% generic adoption, $7.0B savings potential  
3. **NIFEDIPINE**: Only 0.4% generic adoption, $2.3B savings potential

### Policy Implications
- **Generic Substitution Programs**: Focus on drugs with <25% generic market share
- **Formulary Design**: Incentivize generic adoption for high-impact drugs
- **Provider Education**: Target prescribers of Critical Impact medications
- **Beneficiary Outreach**: Cost-sharing incentives for generic alternatives

### ROI Projections
- **10% increase in generic adoption**: ~$10B annual savings
- **25% increase in generic adoption**: ~$25B annual savings
- **50% increase in generic adoption**: ~$50B annual savings

---

## 📝 Data Dictionary

### Key Fields
- **`brand_name`**: Commercial/brand name of the drug product
- **`generic_name`**: Generic/chemical name of the drug
- **`drug_type`**: Classification (Brand/Generic) based on name comparison
- **`total_spending_2023`**: Total Medicare spending for the drug in 2023
- **`total_claims_2023`**: Number of prescription claims filled
- **`total_beneficiaries_2023`**: Number of unique Medicare beneficiaries
- **`avg_spending_per_claim_2023`**: Average cost per prescription claim
- **`savings_per_claim`**: Potential savings per claim if generic substituted
- **`total_potential_savings`**: Total annual savings if all brand claims used generic pricing
- **`market_share_generic`**: Percentage of claims that are generic vs total
- **`impact_category`**: Classification (Critical/High/Medium/Low) based on savings potential

---

## 🔄 Maintenance & Updates

### Data Refresh Schedule
- **Annual**: When CMS releases new Medicare Part D data (typically Q3)
- **Validation**: Cross-check with CMS published statistics
- **Monitoring**: Track changes in drug approvals and market dynamics

### Code Maintenance
- **Dependencies**: Update pandas/requests as needed
- **API Monitoring**: CMS occasionally updates endpoint structure
- **Performance**: Optimize for larger datasets as Medicare grows

---

---

## 📄 License & Disclaimer

This analysis uses publicly available CMS data and is intended for educational and policy research purposes. Savings calculations are theoretical and do not account for clinical appropriateness, manufacturer rebates, or implementation costs. Always consult healthcare professionals for medical decisions.

**Data Source**: Centers for Medicare & Medicaid Services (CMS)  
**Last Updated**: August 2025  
**Version**: 1.0
