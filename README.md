# Football_Table_Medallion_Databricks
End-to-end data pipeline for ingesting, transforming, and visualizing Latin American football league standings using TheSportsDB API, Azure Data Factory, Databricks (Delta Lake, Medallion Architecture), and Power BI. Includes automated testing and email alerts for data quality.

Pipeline Breakdown

1. Ingestion (Bronze Layer)**
- DataFactory extracts standings from [TheSportsDB API](https://www.thesportsdb.com/)
- JSON files are landed in Azure Blob Storage
- Databricks streaming reads JSON and converts to Delta format in Bronze

2. Normalization (Silver Layer)**
- Cleansing and schema enforcement
- Dimension tables: `dim_teams`, `dim_leagues`
- Fact table: `fact_standings` with metrics (points, matches, goals)

3. Data Modeling (Gold Layer)**
- Joins across dimensions and fact
- Selection of relevant fields only
- Final output: `gold.dashboard_standings` for reporting

4. Power BI Integration**
- Direct connection using Databricks connector (Key & Secret)
- Dashboard shows team rankings, league comparison, and metrics

5. Testing & Automation**
- Databricks jobs run the full pipeline weekly (`cron`)
- Separate testing notebook validates schema, nulls, and integrity
- Email alerts sent automatically if any inconsistency is found

Dashboard Features
- Team and league rankings
- Points, goal difference, and matches played
- Filters by country, league, and season
- Visual branding with team badges

---

Scheduling

The full pipeline runs **daily**, ensuring data freshness and low operational overhead. The workflow is managed entirely inside **Databricks Jobs**, with dependencies and alerts configured.

---
Data Quality Monitoring
If data quality checks fail (e.g., nulls in critical fields, row mismatch), an **automated email** is sent using Python + SMTP or Azure Logic Apps to notify the team.

---
