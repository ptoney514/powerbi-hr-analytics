# Trio Quarterly 2025 - HR Analytics Report

A comprehensive Power BI analytics solution for tracking quarterly workforce metrics at Creighton University.

## Overview

This Power BI Project (.pbip) provides executive-level insights and detailed analysis of HR data including headcount, employee demographics, turnover rates, compensation changes, and recruitment pipeline metrics.

## Key Features

### Analytics Coverage
- **Headcount Tracking**: Monitor benefit-eligible and non-benefit-eligible employees
- **Faculty & Staff Analysis**: Separate tracking for faculty vs staff populations
- **Turnover Analytics**: Historical termination data and turnover rate calculations
- **Compensation Insights**: Salary increases and additional payment analysis
- **Recruitment Pipeline**: Open positions and time-to-fill metrics
- **Demographics**: Age, gender, and ethnicity breakdowns
- **Time Intelligence**: Year-over-year and quarter-over-quarter comparisons

### Data Model

**18 Tables** organized in a star schema:
- **Active Employee Tables**: Current employee details, job assignments, and organizational hierarchy
- **Termination Tables**: Historical separation data since 2017
- **Compensation Tables**: Salary increases and additional pay extracts
- **Recruitment Table**: Open positions from Taleo ATS
- **Date Tables**: Fiscal and calendar date dimensions

**Key Relationships**:
- Bidirectional filtering between Employee ↔ Job Details ↔ Organizations
- Date-based relationships for time-series analysis
- Many-to-many support where needed

### Report Pages

1. **Quarterly Overview** - Executive dashboard with key metrics
2. **Quarterly Overview by Department** - Department-level breakdown
3. **Headcount and Terms** - Employee counts and terminations
4. **Turnover** - Turnover rate analysis with trends
5. **Salary Increases** - Compensation change summary
6. **Salary Increase Decomposition** - Detailed compensation analysis
7. **Additional Payments** - Supplemental pay overview
8. **Additional Payments Decomposition** - Detailed supplemental pay breakdown
9. **Recruiting** - Open positions and requisition pipeline
10-15. Various detailed analysis and backup pages

## Data Sources

The report connects to Excel files hosted on SharePoint:
- Active employee snapshots (quarterly extracts)
- Historical termination records
- Compensation change data
- Recruiting/requisition data from Taleo

**Data Refresh**: Manual or scheduled refresh via Power BI Service

## Technology Stack

- **Format**: Power BI Project (.pbip) - text-based, version-control friendly
- **Model Language**: TMDL (Tabular Model Definition Language)
- **Query Language**: Power Query (M)
- **Calculation Language**: DAX (Data Analysis Expressions)
- **Compatibility Level**: 1567

## Getting Started

### Prerequisites
- Power BI Desktop (latest version recommended)
- Access to Creighton University SharePoint data sources
- Appropriate permissions for HR data

### Opening the Report

1. Clone this repository
2. Open `Trio Quarterly 2025 v1.pbip` in Power BI Desktop
3. The application will reconstruct the report from TMDL/JSON definitions
4. Refresh data to load latest from SharePoint sources

### Making Changes

**Editing DAX Measures**:
- Most measures are in `Trio Quarterly 2025 v1.SemanticModel/definition/tables/All Measures.tmdl`
- Individual table measures are in their respective `.tmdl` files

**Editing Power Query**:
- Source queries are in the `partition` section of each table's `.tmdl` file
- Use Power BI Desktop's Power Query Editor for complex transformations

**Modifying Relationships**:
- All relationships are in `Trio Quarterly 2025 v1.SemanticModel/definition/relationships.tmdl`

**Creating Report Pages**:
- Use Power BI Desktop's report canvas
- Pages are auto-saved to `Trio Quarterly 2025 v1.Report/definition/pages/`

## Version Control

This project uses the `.pbip` format for better version control:
- Text-based files (TMDL, JSON) instead of binary `.pbix`
- Enables meaningful diff comparisons
- Supports collaborative development
- Easier code review process

**Excluded Files** (see `.gitignore`):
- Local user settings
- Power BI cache files

## Key Metrics

### Naming Conventions
- `(BE)` = Benefit Eligible employees
- `(NBE)` = Not Benefit Eligible employees
- `(Terms)` = Termination-related metrics

### Core Measures
- **Headcount**: Current active employee counts by type
- **Variance**: Period-over-period changes (count and percentage)
- **Turnover**: Separation counts and rates
- **Visual Indicators**: Arrows (↑/↓) showing positive/negative trends

### Time Periods
- Current Quarter
- Previous Quarter (QoQ comparison)
- Prior Year (YoY comparison)
- Historical trends since 2017

## Employee Classification

Employees are categorized by:
- **Benefit Eligibility**: Benefit Eligible vs Not Benefit Eligible
- **Person Type**: Faculty vs Staff
- **Assignment Eligibility**: Resident, Student Worker, Federal Work Study, etc.

## Project Structure

```
Trio Report/
├── CLAUDE.md                                 # Developer guidance
├── README.md                                 # This file
├── .gitignore                                # Git exclusions
├── Trio Quarterly 2025 v1.pbip              # Project root file
├── Trio Quarterly 2025 v1.Report/           # Report layer
│   ├── definition/
│   │   ├── pages/                           # 15 report pages
│   │   ├── bookmarks/                       # Saved bookmarks
│   │   ├── report.json                      # Report settings
│   │   └── version.json
│   └── StaticResources/                     # Themes, images
└── Trio Quarterly 2025 v1.SemanticModel/    # Data model layer
    ├── definition/
    │   ├── tables/                          # 18 table definitions
    │   ├── model.tmdl                       # Model settings
    │   ├── relationships.tmdl               # Table relationships
    │   └── database.tmdl                    # Compatibility settings
    └── diagramLayout.json                   # Model diagram
```

## Documentation

For detailed technical documentation and development guidelines, see [CLAUDE.md](CLAUDE.md).

## Support

For questions or issues with this report:
1. Check the CLAUDE.md file for technical details
2. Review Power BI documentation for general Power BI questions
3. Contact the HR Analytics team for data or business logic questions

---

**Last Updated**: October 2025
**Report Version**: v1
**Data Period**: Quarterly (FY2025)
