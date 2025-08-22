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

▶️End-to-End demo: [LinkedIn post & video](https://www.linkedin.com/feed/update/urn:li:activity:7347917906363518977/?commentUrn=urn%3Ali%3Acomment%3A(ugcPost%3A7347917694953754625%2C7348260275357204482)&dashCommentUrn=urn%3Ali%3Afsd_comment%3A(7348260275357204482%2Curn%3Ali%3AugcPost%3A7347917694953754625))

See the branch [ai-agent](https://github.com/RuiRomano/pbip-demo-agentic/tree/ai-agent) for an example of what was built by Github Copilot.

> 📌 **Notes** 
> - The [kb-pbip.md](.resources/kb-pbip.md) file includes important knowledge base that AI will use to better understand PBIP file structure and TMDL.
> - I got best results using `Claude Sonnet 4` model which requires [GitHub Copilot Pro subscription](https://github.com/features/copilot/plans?cft=copilot_li.features_copilot)

