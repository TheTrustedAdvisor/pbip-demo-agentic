This repository showcases Agentic AI development in Power BI using London Taxi revenue data.

# 🔍 What is Agentic Development?

Agentic development is a new paradigm where developers shift from writing code or using UI applications to guiding intelligent agents. The developer defines the what - the requirements, rules, and intent - and the AI agent handles the how - generating specs, implementing the model, running validations, fixing issues, and iterating toward a working solution. 

In Power BI, this is made possible by open file formats with [Power BI Project file format](https://learn.microsoft.com/power-bi/developer/projects/projects-overview) and the [TMDL language](https://learn.microsoft.com/analysis-services/tmdl/tmdl-overview), which provide structure and linting AI agents can use to reason, check, and improve their own work. 

# 🚖 London Taxi Revenue Analytics

This demonstration uses London Taxi trip data from a Microsoft Fabric Data Warehouse to showcase how AI agents can build comprehensive business intelligence solutions. The scenario includes:

- **Revenue Analysis**: Track total revenue from fares and tips across different payment methods
- **Location Intelligence**: Identify top-performing pickup locations and boroughs
- **Time-based Insights**: Analyze revenue trends, growth patterns, and seasonal variations
- **Fleet Optimization**: Support operational decisions through data-driven insights

# ⚙️ How does it work?

1. You describe what you need – your [requirements](.requirements/requirements-01.md) and the [rules](.resources/kb-pbip.md) the agent should follow.
2. An AI agent turns that into a development spec – readable, reviewable, and adjustable.
3. You say "Go" and the agent starts implementing the spec autonomously.
4. It runs its own best practice checks, fixes issues, and iterates until it gets it right.
5. You open the Power BI application and review the result, just like you would with a teammate’s pull request.

# 🧪 Try it yourself

1. Clone this Repo into your laptop
2. Install [Visual Studio Code](https://code.visualstudio.com/)
3. Install [GitHub Copilot extension](https://docs.github.com/en/copilot/responsible-use-of-github-copilot-features/responsible-use-of-github-copilot-chat-in-your-ide?tool=vscode)
4. Open Github Copilot Chat in [Agent mode](https://code.visualstudio.com/blogs/2025/02/24/introducing-copilot-agent-mode)
5. Attach the [requirements-01.md](.requirements/requirements-01.md) and run the following prompt:

    > Please review the attached requirements and draft a comprehensive development specification in Markdown format for my review. Focus on London Taxi revenue analytics with semantic modeling and interactive visualizations.

6. Review the generated development spec, then prompt Copilot to proceed with implementing the semantic model and Power BI reports.

## 📊 London Taxi Revenue Analytics Scenario

This demonstration showcases a comprehensive business intelligence solution using **real-world London Taxi trip data** stored in a **Microsoft Fabric Data Warehouse**. The scenario demonstrates how AI agents can automatically create:

### 🎯 **Business Intelligence Requirements**
- **Revenue Analysis**: Track total revenue from fares and tips across payment methods and vendors
- **Growth Analytics**: Year-over-year and month-over-month revenue trend analysis with time intelligence
- **Location Intelligence**: Identify top-performing pickup locations and boroughs for fleet optimization
- **Payment Insights**: Analyze revenue distribution across different payment methods

### 📈 **Power BI Visualizations**
- **Executive Dashboard**: KPI cards with trend indicators and conditional formatting
- **Time Series Analysis**: Interactive line charts with drill-down capabilities
- **Location Rankings**: Horizontal bar charts showing top performing areas
- **Payment Distribution**: Donut charts with percentage breakdowns
- **Interactive Exploration**: Comprehensive slicers and cross-filtering

### 🏗️ **Technical Implementation**
- **Microsoft Fabric Data Warehouse** integration with parametrized connections
- **TMDL semantic modeling** following Power BI best practices
- **DAX measures** for advanced analytics and time intelligence
- **PBIP file format** enabling version control and CI/CD workflows

> 📌 **Notes** 
> - The [kb-pbip.md](.resources/kb-pbip.md) file includes important knowledge base that AI will use to better understand PBIP file structure and TMDL.
> - Best results are achieved using advanced AI models like `Claude Sonnet 4` which requires [GitHub Copilot Pro subscription](https://github.com/features/copilot/plans?cft=copilot_li.features_copilot)

