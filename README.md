# Manufacturing-Analytics-Project
Welcome to Manufacturing Analytics — a hands-on, end-to-end analytics project that turns factory floor noise into clear, actionable business insights. This README gives a polished, step-by-step tour of the project files (what’s inside, what we did, and how you can re-run or extend it). Think of this as the story of the data: from messy logs to dashboards that ask better questions.

Project status: ✅ Completed | Dashboards: Tableau + Power BI + Excel | Data: Production exports (Nov 2015 sample).

Authors : Manufacturing Analytics.

📁 Files in this repo (what to expect)

PROD DATA.txt — raw production export (rows of production events: doc dates, machines, employees, produced/rejected qty, costs, etc.). 

Manufacturing Data.xlsx & Excel project data.xlsx — cleaned & transformed Excel workbooks used for pivot analysis, intermediate cleansing and summary KPIs.

Dataset_SQL.sql — SQL scripts used to calculate key KPIs (manufactured qty, rejected qty, wastage percentage, machine/employee breakdowns, daily averages). Use these directly against a dataset_csv table. 

Tableau_project_file.twbx — interactive Tableau dashboard showing department/machine/employee visualizations and KPI charts.

🧭 Step-by-step workflow (what we did — reproducible & clear)
1) Ingest & explore (where curiosity starts)

Load the raw production export (PROD DATA.txt) into your tool of choice (Excel, Python, R or a database). The file includes timestamps, Machine Code, Emp Name, Produced Qty, Rejected Qty, Today Manufactured Qty, Processed Qty, cost fields, work centre and operation info. 

PROD DATA

Quick checks: row count, column list, null counts, data types and obvious outliers (negative quantities, zeros in denominators).

2) Data cleaning & transformation (the detective work)

Standardize date/time fields (Doc Date, SO Delivery Date, WO Date) to ISO format (YYYY-MM-DD) and create FiscalDate or DocDate keys for joins.

Normalize categorical fields: trim whitespace, consistent capitalization for Department Name, Operation Name, Machine Code, Emp Name.

Convert numeric fields (Produced/Rejected/Processed/TotalQty/TotalValue) to numeric types; coerce and log coercion issues for manual review.

Create derived fields:

Wastage = Rejected Qty / Processed Qty (handle Processed Qty = 0 safely).

Yield = (Produced Qty - Rejected Qty) / Produced Qty (or equivalent business definition).

DateKey, Week, Month, Shift for time-series aggregations.

Save cleaned outputs as cleaned_manufacturing_data.xlsx or push into a database table dataset_csv.

(These cleaning/transform steps were applied before performing KPI queries and creating dashboards; the raw text file provides the source rows.) 

PROD DATA

3) SQL KPIs (repeatable, auditable calculations)

Use Dataset_SQL.sql to compute the core KPIs — the script contains ready-to-run queries such as:

Total manufactured qty: SUM(Today Manufactured Qty)

Total rejected qty: SUM(Rejected Qty)

Total processed qty: SUM(processed qty)

Wastage %: SUM(rejected Qty) / SUM(processed Qty)

Employee-wise & machine-wise rejected counts and averages, daily average production, department comparisons. 

Dataset_SQL

Tip: Run the SQL against a MySQL or compatible DB after importing the cleaned CSV into data.dataset_csv. If your column names contain spaces, use backticks (the SQL script already notes this). 

Dataset_SQL

4) Excel analysis & quick visuals

Use Excel project data.xlsx to create pivot tables for quick breakdowns (department vs rejected/produced, machine vs rejection counts, employee vs avg rejected).

Build a “dashboard” sheet in Excel showing top KPIs, small multiples and conditional formatting (heatmaps for high rejection machines).

Export pivot summaries as CSVs for consumption by Tableau/Power BI.

5) Tableau dashboard (story-first visualization)

Source: cleaned dataset (or exported Excel pivot).

Dashboards implemented: KPI tiles (Total Manufactured, Processed, Rejected, Wastage%), machine-level rejection chart, employee rejection leaderboard, department manufactured vs rejected comparison, operation-level rejection distribution. These charts and the logic are also described in the final presentation. 

Final project ppt

Interactive features: date range filters, department and machine slicers, hover tooltips with per-row drill-down.

6) Power BI report (business-ready storytelling)

The .pbix contains:

KPI cards and trend charts (daily average production, rejection rate trend),

Cross-filtering visualizations (select a machine to see associated employees/operations),

Measures implemented in DAX for dynamic calculations (e.g., rolling averages, % change month-over-month).

Power BI is ideal for scheduled refresh and embedding in Power BI Service for the business owners.

7) Findings & actions (from the final presentation)

Key insights captured in the presentation: high-wastage flags, machines with unusually high rejections, employees/operations that may need training or maintenance actions, and departmental variance in yield. Suggested actions include preventive maintenance scheduling, targeted operator training and process standardization.

powerBi_project.pbix — Power BI report with similar interactive visuals and slicers (date, department, operation).

Final project ppt .pptx — presentation summarizing insights, KPI definitions and recommended actions. 

Final project ppt

Note: some files are raw exports (TXT/CSV) while others are dashboards and presentation files ready to open in their respective tools.
