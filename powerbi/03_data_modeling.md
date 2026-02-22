🧩 Power BI Data Modeling Documentation
1. Overview

This project follows a database-first analytics approach, where all data cleansing, transformations, and business logic are implemented in SQL Server.
Power BI is used exclusively for visualization and presentation.
All KPIs and analytical metrics originate from validated SQL views to ensure consistency, performance, and auditability.

2. Data Sources Used

Each Power BI report page connects directly to SQL Server analytical views (not CSV files).

| Report Page / Dashboard  | SQL Server View             |
| ------------------------ | --------------------------- |
| Executive Overview       | `bank.KPI_Executive_Overview` |
| Risk & Default Analysis  | `	bank.Risk_Default_Analysis` |
| Portfolio Distribution   | `bank.Portfolio_Distribution` |
| Loan Default Trends      | `bank.Loan_Default_Trends`    |
| Drill-down / Exploration | `bank.Loan_Risk_Master`       |

SQL Server is the single source of truth. Power BI consumes curated analytical views rather than raw tables.

3. Data Granularity

Executive Overview: Single-row, fully aggregated KPI-level dataset

Risk & Default Analysis: Aggregated by risk segment and credit score band

Portfolio Distribution: Aggregated by loan type, loan amount band, and loan term band

Loan Default Trends: Aggregated by issue year and month

Loan Risk Master: Loan-level (one row per loan)

Each dataset is pre-shaped in SQL to match the exact analytical grain required by the report.

4. Modeling Approach

One SQL analytical view per report page

No transformations performed in Power Query

No business KPIs calculated in Power BI using DAX

Power BI is used purely for:

Visualization

Filtering

Slicing

Drill-down interactions

All business logic and metric definitions are enforced at the database layer.

5. Rationale for Design Choices

SQL Server as the single source of truth

Prevents KPI mismatches across Power BI pages

Eliminates duplicated logic between SQL and DAX

Improves governance and auditability

Aligns with enterprise BI architecture where BI tools are consumers of curated semantic layers

This design reduces semantic drift and makes metric definitions traceable and reviewable.

6. Handling Relationships

Because each SQL view already represents a complete analytical dataset at the required level of aggregation, no relationships or star schema modeling are required within Power BI.

This avoids:

Incorrect aggregations

Double counting

Complex relationship management

Performance degradation caused by large model joins

Power BI acts as a thin semantic layer on top of pre-modeled SQL views.

7. Power BI Connectivity Considerations

Power BI connects directly to SQL Server using:

Import mode (for performance and portability)

or DirectQuery (if real-time access is required)

The project architecture supports enterprise-grade refresh strategies using:

Scheduled refresh (Power BI Service)

SQL Server as the governed data layer

Unlike Tableau Public, Power BI supports native database connectivity and scheduled refresh pipelines.

8. Limitations & Trade-offs

Business logic changes require SQL updates rather than quick DAX edits

Requires stronger SQL engineering discipline

Less flexibility for ad-hoc metric creation in Power BI

Performance depends on SQL view optimization

This trade-off is intentional to prioritize:

Metric consistency

Governance

Production-like architecture
