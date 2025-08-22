Thoroughly read and understand the PBIP knowledge base in the [kb-pbip](../.resources/kb-pbip.md) file.

# Business Requirements 💼

Create a new semantic model named 'TaxiRevenue' and ensure the following requirements are met:

| Requirement ID | Description | User Story | Expected Behavior |
|---------------|-------------|------------|-------------------|
| **TAXI-001** | Total Revenue Performance Overview | As a **Fleet Manager**, I want to see the total revenue across all payment types and vendors so that I can understand our overall revenue performance. | Create a combined [Total Revenue] measure that sums fare_amount + tip_amount from trips with proper relationship handling through shared dimensions. |
| **TAXI-002** | Revenue Growth Analysis | As **Executive Leadership**, I want to see year-over-year and month-over-month revenue growth percentages so that I can track business progress against targets. | Create time intelligence measures using SAMEPERIODLASTYEAR and PREVIOUSMONTH functions: [Revenue YoY Growth %] and [Revenue MoM Growth %] with proper Calendar table relationships. |
| **TAXI-003** | Top Performing Areas | As a **Operations Manager**, I want to identify the top 10 pickup locations by revenue so that I can optimize fleet deployment and focus on high-performing areas. | Create [Location Revenue Rank] measure using RANKX function and establish proper relationships between trips and locations for accurate location-level aggregations. |

# Visual Requirements 📊

Create interactive Power BI reports with the following visualizations to support the business requirements:

| Requirement ID | Description | User Story | Expected Behavior |
|---------------|-------------|------------|-------------------|
| **VISUAL-001** | Executive Revenue Dashboard | As an **Executive**, I want a high-level dashboard showing key revenue metrics at a glance so that I can quickly assess taxi business performance. | Create a dashboard page with KPI cards showing [Total Revenue], [Revenue YoY Growth %], and [Revenue MoM Growth %]. Include trend sparklines and conditional formatting for growth indicators. |
| **VISUAL-002** | Revenue Trend Analysis | As a **Fleet Manager**, I want to see revenue trends over time with the ability to drill down by month/quarter so that I can identify seasonal patterns and performance trends. | Create a line chart showing daily/monthly revenue trends with year-over-year comparison. Include date hierarchy for drill-down capabilities and reference lines for targets. |
| **VISUAL-003** | Location Performance Ranking | As an **Operations Manager**, I want to see top performing pickup locations in an easy-to-read format so that I can quickly identify high-revenue areas and optimize fleet deployment. | Create a horizontal bar chart showing top 10 pickup locations by revenue with conditional formatting. Include borough as a drill-through option. |
| **VISUAL-004** | Payment Method Performance | As a **Finance Manager**, I want to compare revenue by payment type so that I can understand customer payment preferences and optimize payment processing. | Create a donut chart showing revenue distribution by payment type (Cash, Credit Card, etc.) with percentage labels and drill-through to detailed payment analysis. |
| **VISUAL-005** | Interactive Revenue Explorer | As an **Analyst**, I want interactive filters and slicers so that I can explore data from different angles and time periods. | Add date slicer, borough slicer, and payment type filter. Ensure all visuals respond to slicer selections with cross-filtering. |

# Data source information 🛢️

- **Server**: Use parameter `Parameter_Server` (default: `if7l2k6jajhexaligopzadcdde-jqdtzkxghetune2a4f7mxsbodq.datawarehouse.fabric.microsoft.com`)
- **Database**: Use parameter `Parameter_Database` (default: `LondonTaxi`)
- **Connection Type**: Microsoft Fabric Data Warehouse
- **Authentication**: Use organizational account or service principal
- **Data Description**: London Taxi trip data including pickup/dropoff locations, fare amounts, tips, payment methods, and temporal information

**Important**: The server and database values should be implemented as semantic model parameters to allow flexibility for different environments (dev/test/prod).

If you cannot connect to the Fabric Data Warehouse above to get the schema, all information of the Datawarehouse can be found in the file `AzureSQLSchema.csv`. 

# Development rules 🧑‍💻

## Data Modeling Guidelines
- Analyze the datasource tables in `AzureSQLSchema.csv` and pick the tables that best answer the requirements. The main fact table is `trips` with dimensions `payment_types`, `vendors`, `locations`, and `calendar`.
- Use import storage mode for all tables (DirectLake mode can be considered for Fabric sources if performance is critical).
- Always prefer simple star-schema modeling. The trips table should be the central fact table with relationships to dimension tables.
- Create a date key relationship between Calendar and trips using pickup_datetime. Transform pickup_datetime to a date key in Power Query if needed.

## TMDL Implementation Rules
- Don't create a new Calendar table, make sure you reuse Calendar table in the TMDL file `.resources/tmdl/Calendar.tmdl` - you may rename columns but keep the structure exactly as-is.
- The created TMDL tables should have the necessary columns and measures to answer the taxi revenue requirements.
- All measures should be created in the trips fact table TMDL file, but do not duplicate measures - they must be unique within the model.
- Make sure you set descriptions on all created measures using business language relevant to taxi operations.

## Parameter Configuration
- The server and database name MUST be implemented as semantic model parameters using this exact naming:
  - `Parameter_Server` for the server name
  - `Parameter_Database` for the database name
- Use these parameters in all Power Query data source connections

## Technical Guidelines
- Don't try to test the semantic model data during development
- For Fabric Data Warehouse connections, use proper authentication and consider security best practices
- Follow the PBIP file structure exactly as documented in the knowledge base

# Report Development rules 📊

## Visual Implementation Guidelines
- Create a comprehensive Power BI report (not just an empty report) that implements all visual requirements (VISUAL-001 through VISUAL-005)
- Follow Power BI design best practices: consistent color schemes, proper spacing, clear titles
- Use appropriate visual types for each data story:
  - KPI cards for metrics with trend indicators
  - Line charts for time-series analysis
  - Horizontal bar charts for rankings (better for long location names)
  - Donut charts for categorical distributions
- Implement proper drill-through and cross-filtering between visuals

## Interactivity & User Experience
- Add meaningful tooltips and conditional formatting where applicable
- Ensure all visuals respond to slicer selections with cross-filtering
- Include proper error handling for empty data scenarios
- Design for mobile-responsive layouts where possible

## Page Layout & Design
- Create a single comprehensive dashboard page that tells the complete taxi revenue story
- Use F-pattern layout (top-left to bottom-right reading flow)
- Maintain visual hierarchy with clear titles and consistent spacing
- Apply professional color scheme suitable for business presentations

## Performance Considerations
- Optimize DAX measures for performance
- Consider visual rendering performance with large datasets
- Test cross-filtering behavior across all visuals

# Semantic Model Naming conventions 🏷️

- Use singular names for dimension tables (payment_type, vendor, location, calendar)
- Use plural names for fact tables (trips)
- All model object names should be lower case
- Don't use 'fact' or 'dim' in the table or column names. Prefer business friendly representation
- Don't use column names like 'location name' prefer 'location' instead
- Measure names should follow a consistent naming convention: 
  - [measure name] for base measure (e.g., [total revenue])
  - [measure name (ly)] for last year value of the base measure (e.g., [total revenue (ly)])
  - [measure name (ytd)] for ytd value for the base measure (e.g., [total revenue (ytd)])
  - Use taxi industry terminology where appropriate (e.g., [fare amount], [tip amount], [trip count])

# Validation & Quality Assurance 🔍

## Pre-Development Checklist
- [ ] Verify schema information in `AzureSQLSchema.csv` matches the actual data warehouse structure
- [ ] Confirm Calendar table template in `.resources/tmdl/Calendar.tmdl` is available and correct
- [ ] Validate server connectivity and authentication requirements
- [ ] Review PBIP knowledge base documentation for latest best practices

## Post-Development Validation
- [ ] Run Best Practice Analysis script `.bpa/bpa.ps1` to identify critical issues
- [ ] Verify all measures have proper descriptions and formatting
- [ ] Test parameter functionality for server/database switching
- [ ] Confirm all relationships are properly established
- [ ] Validate all visual requirements are fully implemented
- [ ] Test cross-filtering and interactivity across all visuals

## Success Criteria
- Semantic model successfully connects to Fabric Data Warehouse using parameters
- All business requirements (TAXI-001 through TAXI-003) are implemented with proper DAX measures
- All visual requirements (VISUAL-001 through VISUAL-005) are implemented in a single comprehensive dashboard
- Report provides meaningful insights for taxi fleet management and revenue optimization
- Solution follows all PBIP and TMDL best practices as documented in the knowledge base
