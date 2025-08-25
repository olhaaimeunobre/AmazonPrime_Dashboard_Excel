[![Releases - Download the dashboard file](https://img.shields.io/badge/Releases-Download%20Excel%20Dashboard-brightgreen)](https://github.com/olhaaimeunobre/AmazonPrime_Dashboard_Excel/releases)

# Amazon Prime Churn Dashboard — Excel Workbook for Churn Analysis and Visualization

📊 📈 🧾

Short project name: Amazon Prime churn analysis and dashboard built in Excel.  
This repository hosts an Excel workbook that blends data preparation, cohort analysis, retention metrics and visual dashboards. The workbook lets analysts and business users explore churn drivers for a subscription product similar to Amazon Prime. The release file must be downloaded and executed from the Releases page linked above.

Table of contents
- About this project
- Where to get the workbook
- Quick start — download and run
- Required software and environment
- Files in the release and what to run
- Data model and structure
- Key metrics and definitions
- Analysis methods used
- Dashboard overview and screenshots
- How the Excel workbook is built (Power Query, Data Model, DAX, VBA)
- Reproducing sections manually
- Typical workflows and FAQs
- Troubleshooting pointers
- Contributing
- License
- Credits and references
- Contact

About this project
This project equips analysts with an interactive Excel dashboard to measure and explain subscriber churn. The workbook covers:
- Data import and shaping.
- Cohort and retention analysis.
- Churn drivers by segment and feature usage.
- Visualization: retention curves, funnel charts, heat maps, KPIs.
- Scenario testing and simple forecasting.

The content suits analysts who use Excel for business intelligence, product managers who track subscription health, and data teams that want a portable dashboard without full BI stack dependencies.

Where to get the workbook
Download the release package and run the workbook. The file lives in the Releases section. Go to:
https://github.com/olhaaimeunobre/AmazonPrime_Dashboard_Excel/releases

The release contains an Excel workbook that you must download and open locally. Follow the Quick start steps below for guidance.

Quick start — download and run
1. Visit the Releases page linked at the top or here:
   https://github.com/olhaaimeunobre/AmazonPrime_Dashboard_Excel/releases
2. Download the Excel file named in the release assets. The file includes everything needed: data model, queries, dashboards and macros.
3. Open the workbook in Microsoft Excel (see Requirements for version).
4. If Excel prompts to enable content, enable it to allow queries and macros to run.
5. Use the "Dashboard" sheet tab to explore KPIs and visuals.
6. Use the "Data" and "Model" tabs to inspect underlying tables and transformations.

Required software and environment
- Microsoft Excel for Windows or macOS. For full functionality use Excel 2016 or later with Power Query and Data Model support. Excel 365 works best.
- Optional: Power BI Desktop if you want to rebuild visuals there.
- No external server required. The workbook runs locally.
- If macros run, Windows Excel will support VBA. Mac builds of Excel may limit VBA behavior; test macros in your environment.

Files in the release and what to run
The release includes one primary Excel workbook and a README release note. The Excel file is packaged as the main asset. Download that file and open it. The workbook contains:
- Dashboard worksheet(s) with interactive charts.
- Data import queries (Power Query / Get & Transform).
- Data Model (Power Pivot) with relationships and calculated measures.
- VBA macros to refresh data and toggle view states.
- Example raw dataset in a sheet for offline use.

Because the release link points to a concrete release path, you must download the workbook file from the Releases page and open it in Excel. After opening the workbook, run the included macros if prompted. The workbook requires you to enable workbook connections and macros for a seamless experience.

Data model and structure
The workbook uses a normalized data model. The main tables:
- Subscriptions: one row per subscriber with subscription start date, end date (if churned), region, plan type, join channel.
- Events: time-stamped activity logs (streaming hours, purchases, feature usage).
- Billing: billing history, payment method, churn reason where available.
- Catalog & Features: feature flags and product attributes used for segmentation.
- Lookup tables: date dimension, geography, plan metadata.

Relationships
- Subscriptions[subscriber_id] -> Events[subscriber_id]
- Subscriptions[subscriber_id] -> Billing[subscriber_id]
- Events[date] -> DateDimension[date]

Date handling
A robust Date table drives time-based measures. The Date table includes fiscal weeks, months, cohort month, and retention period index. Cohort grouping uses subscription first active month.

Key metrics and definitions
Clear metric definitions are critical for consistent analysis. This workbook uses the following terms and measures:

- Active subscriber: subscriber with service activity or valid subscription on a reference date.
- Churn event: subscriber whose subscription terminates and does not renew within a grace period.
- Churn rate: number of churn events divided by the active population over a period.
- Retention rate: share of a cohort that remains active after N periods.
- Monthly Recurring Revenue (MRR): sum of recurring fees from active subscribers in a month.
- ARPU (Average Revenue Per User): MRR divided by active subscribers in the period.
- LTV (Customer Lifetime Value): simple estimate using ARPU times average lifetime or discounted cash flows for advanced modeling.
- Cohort period: period index from subscription start (month 0, month 1, ...).
- Survival rate: probability a subscriber remains active after t periods.
- Reactivation rate: share of churned subscribers who return within a window.

Analysis methods used
The workbook implements several standard churn analysis techniques:

Cohort analysis
- Cohorts are defined by subscription start month.
- Cohort tables show counts and retention rates across periods.
- Visuals include heat maps and retention curves.

Funnel analysis
- Converts user lifecycle events into funnel stages (signup -> activation -> paid -> retained).
- Measures conversion rates between stages and drop-offs.

Survival analysis (simple)
- Uses period-based retention to approximate survival curves.
- Allows estimation of median lifetime using retention decay.

Segmented analysis
- Segment by plan type, geography, acquisition channel, or feature usage.
- Compare churn and retention across segments with normalized cohorts.

Correlation and logistic models (descriptive)
- The workbook includes correlation matrices between usage metrics and churn.
- A simple logistic regression example uses Excel regression tools or add-in to highlight predictive signals.

Forecasting (scenario)
- Simple linear and exponential decay forecasts for cohorts.
- Scenario sliders in the dashboard allow changing assumed churn rates.

Dashboard overview and screenshots
The dashboard gives a high-level snapshot and drillable views. Key areas:
- Top KPIs: current active subscribers, monthly churn rate, MRR, ARPU, LTV estimate.
- Trend charts: active base over time, churn rate trend, MRR trend.
- Cohort heat map: shows retention percentage by cohort month.
- Retention curves: line charts for selected cohorts.
- Segment comparison: bar charts comparing churn by plan, region, or acquisition channel.
- Funnel widget: counts and conversion rates across lifecycle stages.
- Anomaly markers: highlight months with unusual churn spikes.

Example image links (illustrative)
- Hero dashboard mockup: https://images.unsplash.com/photo-1519389950473-47ba0277781c (visualize data)
- Excel icon: https://upload.wikimedia.org/wikipedia/commons/7/76/Microsoft_Office_Excel_%282018%E2%80%93present%29.svg

How the Excel workbook is built
This workbook uses native Excel features to keep the solution portable.

Power Query (Get & Transform)
- Power Query handles raw import and cleaning.
- Queries implement date parsing, event deduplication, and key lookups.
- Use query parameters to switch between example data and production extracts.

Data Model (Power Pivot)
- The model stores tables and relationships.
- Calculated measures use DAX for dynamic KPIs.
- The model allows fast aggregation across large tables.

DAX measures (examples)
- ActiveSubscribers = DISTINCTCOUNT(Subscriptions[subscriber_id]) filtered by period.
- ChurnCount = COUNTROWS(FILTER(Billing, Billing[churn_flag] = 1 && Billing[churn_date] <= PeriodEnd))
- ChurnRate = DIVIDE([ChurnCount], [ActiveSubscribersStartPeriod])
- MRR = SUM(Billing[recurring_amount]) filtered by active period

VBA macros
- Macro: RefreshAllModel — refreshes Power Query and recalculates the model.
- Macro: ExportSnapshot — exports a snapshot of KPIs to CSV for archiving.
- Macro: ToggleCohortLabels — show or hide cohort annotations.

PivotTables and charts
- The dashboard uses PivotTables fed by the Data Model.
- Pivot charts link to PivotTables for dynamic filtering.
- Slicers and timeline controls let users filter by date and segment.

Reproducing sections manually
Some teams prefer to rebuild parts of the workbook. The README includes step-by-step instructions for the most common tasks.

Rebuild a cohort table manually
1. Create a Date dimension with MonthStart, MonthIndex and CohortMonth.
2. In Subscriptions, compute CohortMonth = MONTH(SubscriptionStartDate) + YEAR(SubscriptionStartDate)*12.
3. In Events, compute ActiveMonth for each event row.
4. Use PivotTable with CohortMonth as rows and ActiveMonth offset as columns.
5. Calculate retention by dividing counts in each column by cohort size (column 0).

Compute churn and retention in raw Excel
- Use COUNTIFS with date ranges to count active and churned subscribers in a month.
- Churn rate = COUNTIFS(EndDate, "<=" & MonthEnd, EndDate, ">=" & MonthStart) / ActiveAtStart.
- Use structured tables and named ranges to simplify formulas.

Example formulas
- ActiveAtMonthStart: =COUNTIFS(Subscriptions[StartDate], "<=", MonthStart, Subscriptions[EndDate], ">=", MonthStart)
- MonthlyChurn: =COUNTIFS(Subscriptions[EndDate], ">=", MonthStart, Subscriptions[EndDate], "<=", MonthEnd)
- ChurnRate: =IF(ActiveAtMonthStart=0, 0, MonthlyChurn / ActiveAtMonthStart)

Visual best practices used in workbook
- Keep charts simple and labeled.
- Use color to convey direction: neutral palette, accent for change.
- Show cohort counts and percentages together.
- Use tooltips or comments to explain calculations.

Typical workflows and FAQs
Q: How do I refresh the dashboard after loading new data?
A: Use the Refresh All button in Data ribbon or run the RefreshAllModel macro. Power Query pulls the new rows and the model recalculates measures.

Q: Can I connect the workbook to a live database?
A: Yes. Update Power Query source settings to use an ODBC, SQL Server, or other supported connector. Maintain column names used by queries.

Q: Does the workbook work on Mac?
A: Most features work on modern Excel for Mac, but some VBA or Windows-only connectors may not behave the same. Test macros in your environment.

Q: How do I change the cohort granularity from months to weeks?
A: Update the Date table to include WeekStart and WeekIndex. Adjust cohort grouping logic in Power Query and update DAX measures to use WeekIndex.

Q: How do I add a new segment dimension?
A: Add the segment field to Subscriptions or Events tables, refresh queries, then use slicers or Pivot filters on the new field.

Q: Where can I find the release file?
A: The release file is available on the Releases page:
https://github.com/olhaaimeunobre/AmazonPrime_Dashboard_Excel/releases
Download the Excel asset and open it in Excel.

Troubleshooting pointers
- If Power Query fails on refresh, inspect the query steps and sample data types.
- If DAX measures return blank, verify relationships and filter contexts.
- If macros do not run, enable VBA and allow the workbook to run signed macros if policy requires.
- If charts do not update, refresh PivotTables and verify data source ranges.

Advanced sections — deeper detail on analyses
Cohort retention analysis
- Design cohorts by acquisition month to isolate cohort behavior.
- Use cohort size normalization to compare retention across cohorts that started with different volumes.
- Present both absolute counts and percentages for clarity.

Retention curve interpretation
- A steep early drop indicates onboarding friction.
- Gradual decay suggests long-term engagement issues.
- Compare retention curves across plans to prioritize product fixes.

Churn driver analysis
- Combine usage metrics (hours streamed, visits, purchases) with billing behavior.
- Use segmentation by payment method and geography to detect localized issues.
- Map churn spikes to product launches, pricing changes, or outages.

Predictive signal exploration
- Use correlation and simple regression to rank features by association with churn.
- Build a logistic model using the Excel Data Analysis Toolpak or export to a statistical tool.
- Convert predictive scores into risk bands and show them in the dashboard.

Actionable use cases
- Identify low-retention cohorts by acquisition channel and optimize marketing spend.
- Detect features correlated with retention and prioritize product improvements.
- Run retention experiments with control and exposed groups and visualize lift.

Reporting and distribution
- Save snapshots for executive reporting using ExportSnapshot macro.
- Publish dashboard extracts to SharePoint or internal portals as PDF or static images.
- For scheduled refresh, use Power Automate + OneDrive linking or an enterprise scheduling tool if available.

Contributing
This repository accepts contributions that improve clarity, add tests or refine the workbook. Typical contributions:
- Bug fixes to queries or formulas.
- New visualizations with clear user stories.
- Templates for other subscription businesses.

How to contribute
1. Fork the repository.
2. Make changes in a branch and verify that the workbook runs.
3. Create a pull request with a description of changes and screenshots.
4. Tag maintainers for review.

Guidelines for contributions
- Keep macros documented and scoped to named modules.
- Avoid hard-coded paths in Power Query; use parameters.
- Add comments to complex DAX measures.

License
The workbook and supporting files use the MIT License unless otherwise noted in repository. Check the LICENSE file for details.

Credits and references
- Data visualization and cohort methods adapted from standard analytics best practices.
- Power Query and DAX patterns based on community resources.
- Imagery: Unsplash public images used for illustration and presentation.

Useful references and learning links
- Microsoft Docs — Power Query and Power Pivot
- SQLBI — DAX patterns for time intelligence
- Churn analysis primer articles and cohort analysis tutorials

Badges and links
- Releases (download the workbook): [![Releases - Download the dashboard file](https://img.shields.io/badge/Releases-Download%20Excel%20Dashboard-brightgreen)](https://github.com/olhaaimeunobre/AmazonPrime_Dashboard_Excel/releases)
- Topics and tags: amazon, amazon-prime, churn, churn-analysis, dashboard, data-analysis, data-visualization, excel-dashboard

Screenshots and sample images
Below are sample visuals and a layout guide. Replace these with actual screenshots from the downloaded workbook for presentations.

Dashboard layout (visual guide)
- Top row: KPI tiles (Active, Churn %, MRR, ARPU, LTV)
- Left column: Date timeline and main filters (Region, Plan, Acquisition)
- Center: Trend charts (Active base, MRR)
- Right: Cohort heat map and retention curves
- Bottom: Funnel and segment comparison

Example chart placeholders
![Retention heat map placeholder](https://images.unsplash.com/photo-1519389950473-47ba0277781c)
![Excel dashboard placeholder](https://images.unsplash.com/photo-1498050108023-c5249f4df085)

Data security and privacy
Handle subscriber data according to your organization's privacy policy and applicable laws. When you load production data into the workbook, ensure the file storage and sharing meet security requirements.

Export and automation ideas
- Schedule a nightly export of KPIs to a CSV for archival.
- Use Power Automate or similar to push KPI snapshots to a Slack channel or email list.
- Convert critical pivot views to static CSV files for downstream systems.

Checklist for adoption
- Confirm Excel version supports Data Model and Power Query.
- Verify source data schema matches that used in queries.
- Test macros in the target environment.
- Validate calculated KPIs against known values before operational use.

Example deployment flow for a team
1. Update Power Query sources to point at secure staging database.
2. Refresh the workbook and validate KPI outputs against production reports.
3. Save a published copy to a shared drive with version control tags.
4. Schedule a daily refresh routine using a local automation runner or cloud connector.
5. Use exported snapshots for archival and further downstream automation.

Common pitfalls
- Mismatched date formats between source systems can break period grouping.
- Large raw event tables may slow Power Query; use query folding or pre-aggregate.
- Pivot cache stale state requires manual refresh in some Excel builds.

Appendix A — DAX snippets (examples)
ActiveSubscribers :=
CALCULATE(
    DISTINCTCOUNT(Subscriptions[subscriber_id]),
    FILTER(Date, Date[MonthStart] = SELECTEDVALUE(Date[MonthStart]))
)

MonthlyChurn :=
CALCULATE(
    COUNTROWS(FILTER(Subscriptions, Subscriptions[EndDate] >= MIN(Date[Date]) && Subscriptions[EndDate] <= MAX(Date[Date]) && Subscriptions[churn_flag] = 1 )),
    ALLSELECTED(Date)
)

CohortRetention :=
DIVIDE([RetainedCount], [CohortSize])

Appendix B — Power Query patterns
- Remove duplicates by subscriber_id and event timestamp to avoid double-counting.
- Use "Fill Down" for missing categorical attributes.
- Use "Group By" to compute monthly aggregates before loading to Data Model for performance.

Contact
For questions about the workbook, raise an issue in this repository or open a discussion. Provide sample data snippets and screenshots when you report an issue.

End of file