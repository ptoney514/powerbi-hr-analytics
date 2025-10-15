# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Power BI Project (.pbip) for HR analytics at Creighton University. The report tracks quarterly workforce metrics including headcount, terminations, turnover rates, salary increases, and recruitment data for faculty and staff.

## Project Structure

This uses the Power BI Project format (.pbip), which is a text-based, version-control-friendly format that decomposes Power BI reports into individual files.

Key directories:
- `Trio Quarterly 2025 v1.Report/` - Report layer (pages, visuals, bookmarks, themes)
- `Trio Quarterly 2025 v1.SemanticModel/` - Data model layer (tables, relationships, measures)
- `Trio Quarterly 2025 v1.pbip` - Root project file that references both artifacts

## Data Model Architecture

### Model Language: TMDL (Tabular Model Definition Language)

The semantic model is defined using TMDL files in `Trio Quarterly 2025 v1.SemanticModel/definition/`:

- `model.tmdl` - Model-level settings and table references (defines query order)
- `relationships.tmdl` - All table relationships in one file
- `database.tmdl` - Database compatibility level settings
- `tables/*.tmdl` - Individual table definitions (one file per table)

### Data Model Pattern: Star Schema

**Fact Tables** (contain metrics and foreign keys):
- `(Active) Job Details` - Current employee job assignments (primary fact table)
- `Terms Since 2017` - Historical termination records
- `Salary Increases` - Compensation change events
- `Additional Pay Extract` - Supplemental payment transactions

**Dimension Tables** (contain descriptive attributes):
- `(Active) Employee Details` - Employee demographics (gender, ethnicity)
- `(Active) Orgs` - Organizational hierarchy and departments
- `Date (Report End)` - Primary date dimension with fiscal calendar
- `Employee` - Additional employee reference data
- `Positions (Taleo)` - Recruiting/open positions data

**Special Tables**:
- `All Measures` - Calculated table containing all DAX measures (best practice for organization)
- `Date Field Parameter` - Field parameter for dynamic axis selection

### Key Relationships

- Bidirectional cross-filtering between `(Active) Employee Details` ↔ `(Active) Job Details` ↔ `(Active) Orgs` to enable dynamic slicing
- All fact tables connect to `Date (Report End)` for time-based analysis
- Relationships defined by Report Period END DATE, Term Date, Salary Change Date, or DATE EARNED

### Data Sources

Tables use Power Query (M language) to load data from SharePoint-hosted Excel files. Source queries are embedded in the `partition` section of each table's .tmdl file.

Example pattern:
```m
Source = Excel.Workbook(Web.Contents("https://creightonuniv-my.sharepoint.com/..."), null, true)
```

## Key Metrics and Business Logic

### Employee Classification System

The model distinguishes employees by:
- **Benefit Eligibility**: "Benefit Eligible" vs "Not Benefit Eligible"
- **Person Type**: "FACULTY" vs "STAFF"
- **Assignment Eligibility**: Additional categories like "Resident", "Student Worker", "Federal Work Study"

### Core Measures (in All Measures.tmdl)

Naming convention: `(BE)` = Benefit Eligible, `(NBE)` = Not Benefit Eligible, `(Terms)` = Terminations

Key measure patterns:
- Current period counts: `(BE) Faculty`, `(BE) Staff`
- Prior period comparisons: `(BE) Faculty (Prev Qtr)`, `(BE) Faculty (Prev Year)`
- Variance calculations: `(BE) Faculty Var`, `(BE) Faculty Var %`
- Visual indicators: `(BE) Faculty Var % Arrow` - uses UNICHAR for up/down arrows

### Time Intelligence

Uses relative date calculations and DATEADD functions. The Date (Report End) table contains:
- Standard calendar attributes (Year, Quarter, Month)
- Fiscal calendar fields (Fis Qtr qq-yy)
- Relative period numbers for quarter-over-quarter comparisons

## Report Pages

The report contains 15 pages defined in `Trio Quarterly 2025 v1.Report/definition/pages/`:

Primary pages:
- `Quarterly Overview` (e628b7fdd49d73d6475d) - Main executive dashboard
- `Quarterly Overview by Department` (730ffa0ebcd8fb7fba64)
- `Headcount and Terms` - Combined employee count and termination analysis
- `Turnover` - Turnover rate calculations
- `Salary Increases` + `Salary Increase Decomposition` - Compensation analytics
- `Additional Payments` + `Additional Payments Decomposition` - Bonus/supplemental pay
- `Recruiting` - Open positions pipeline

Page order is defined in `pages/pages.json`.

## Working with This Project

### Opening in Power BI Desktop

Open the `.pbip` file in Power BI Desktop. The application will reconstruct the report and model from the TMDL and JSON definitions.

### Editing DAX Measures

DAX measures are stored in table `.tmdl` files. Most measures are in `tables/All Measures.tmdl`.

Multi-line DAX is wrapped in triple backticks:
```
measure 'Measure Name' = ```
    CALCULATE(
        expression
    )
    ```
```

Single-line DAX uses simple assignment:
```
measure 'Measure Name' = expression
```

### Editing Power Query (M)

Power Query transformations are in the `partition` section of each table's `.tmdl` file. The M code defines the data source connection and transformation steps.

### Modifying Relationships

All relationships are centralized in `definition/relationships.tmdl`. Each relationship has:
- Unique ID
- `fromColumn` and `toColumn` (format: 'TableName'.'ColumnName')
- Optional: `crossFilteringBehavior: bothDirections`
- Optional: `toCardinality: many` (for many-to-many)

### Version Control Notes

Files excluded from git (see .gitignore):
- `**/.pbi/localSettings.json` - User-specific local settings
- `**/.pbi/cache.abf` - Power BI cache file

When making changes in Power BI Desktop, save and close to ensure all TMDL/JSON files are updated.

## Data Refresh Considerations

This report pulls from SharePoint Excel files. Data is refreshed when:
1. Opening the report in Power BI Desktop and refreshing
2. Publishing to Power BI Service with scheduled refresh configured

The source files appear to be manually updated quarterly (based on naming: "Q1FY25").
