This repository showcases Agentic AI development in Power BI.

# 🔍 What is Agentic Development?

Agentic development is a new paradigm where developers shift from writing code or using UI applications to guiding intelligent agents. The developer defines the what - the requirements, rules, and intent — and the AI agent handles the how — generating specs, implementing the model, running validations, fixing issues, and iterating toward a working solution. 

In Power BI, this is made possible by open file formats with [Power BI Project file format](https://learn.microsoft.com/power-bi/developer/projects/projects-overview) and the [TMDL language](https://learn.microsoft.com/analysis-services/tmdl/tmdl-overview), which provide structure and linting AI agents can use to reason, check, and improve their own work. 

# ⚙️ How does it work?

1. You describe what you need – your requirements and the rules your model should follow.
2. An AI agent turns that into a development spec – readable, reviewable, and adjustable.
3. You say "Go" and the agent starts implementing the spec.
4. It autonomously runs its own best practice checks, fixes issues, and iterates until it gets it right.
5. You open the Power BI application and review the result, just like you would with a teammate’s pull request.

# 🧪 Try it yourself

1. Clone this Repo into your laptop
2. Install [Visual Studio Code](https://code.visualstudio.com/)
3. Install [GitHub Copilot extension](https://docs.github.com/en/copilot/responsible-use-of-github-copilot-features/responsible-use-of-github-copilot-chat-in-your-ide?tool=vscode)
4. Open Github Copilot Chat in [Agent mode](https://code.visualstudio.com/blogs/2025/02/24/introducing-copilot-agent-mode)
5. Attach the [requirements-01.md](.requirements/requirements-01.md) and prompt the following:
```plaintext
Use the attached requirement document and elaborate a development spec that I will review and give you for implementation.
```
> 💡 **Note:** I got best results using `CLaude Sonnet 4` model
6. Review the generated development spec, then prompt Copilot to proceed with implementing the semantic model.

