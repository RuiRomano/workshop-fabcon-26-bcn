# Lab - Personalized Power BI Agents

⏱️ **Total duration:** 120 minutes

> [!IMPORTANT]
> This lab uses prompts with AI tools. Responses may vary from the examples shown, even when you use the same prompt. After each exercise, review the **Expected outcome** and compare it with your results before continuing.

## Overview

In this lab you learn how to use a personalized agentic Power BI development using [GitHub Copilot](https://github.com/copilot) and Power BI agentic tools.

The lab covers the two scenarios you meet in real projects:

- **Brownfield development.** You inherit an existing Power BI report, convert it to a PBIP project, track it with Git, and use AI agents to help you with development tasks.
- **Greenfield development.** You start from a Fabric Lakehouse, and let the agent plan and build a new semantic model and reports with you steering the implementation.

Both parts use the shared prerequisites and environment setup. After completing that setup, you can work through either part independently.

## What you will learn

- How to install and use the `powerbi-authoring` plugin across multiple harnesses: GitHub Copilot CLI, Visual Studio Code, and the GitHub Copilot app
- How PBIP and Git let you track, review, and revert AI-generated changes
- How AI context help guide agent behavior
- How to feed company context and team standards to an agent as versioned team shareable files
- How to modify an existing semantic model and report using AI and Power BI agentic skills and tools
- How to plan before implementing, and how to choose a model for each phase
- How to use the remote Power BI Authoring MCP server with no local setup
- How to run parallel subagents to scale with different implementation variations

## Lab structure

| Section                                                                                                                    | Learning goal                                       |
| -------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| [Prerequisites](#prerequisites)                                                                                            | Confirm tools, licenses, and access                 |
| [Prepare the environment](#prepare-the-environment)                                                                        | Install the plugin and sign in to the required CLIs |
| **[Part 1: Brownfield development](#part-1-brownfield-development)**                                                       | **Develop an existing Power BI project**            |
| [1.1 Save the report as a PBIP project and track it with Git](#11-save-the-report-as-a-pbip-project-and-track-it-with-git) | Create a reviewable baseline                        |
| [1.2 Prepare the codebase with agentic context](#12-prepare-the-codebase-with-agentic-context)                             | Add `AGENTS.md` and a local skill to the project    |
| [1.3 Generate documentation for the model and report](#13-generate-documentation-for-the-model-and-report)                 | Automate a task nobody enjoys                       |
| [1.4 Add measure descriptions using company context](#14-add-measure-descriptions-using-company-context)                   | Ground the agent in business language               |
| [1.5 Add currency conversion with a calculation group](#15-add-currency-conversion-with-a-calculation-group)               | Extend the semantic model                           |
| [1.6 Restyle the report pages](#16-restyle-the-report-pages)                                                               | Apply report-wide layout changes                    |
| **[Part 2: Greenfield development](#part-2-greenfield-development)**                                                       | **Build new Power BI artifacts in Fabric**          |
| [2.1 Prepare the Fabric Workspace](#21-prepare-the-fabric-workspace)                                                       | Create the greenfield data source                   |
| [2.2 Connect the GitHub Copilot app to the remote MCP server](#22-connect-the-github-copilot-app-to-the-remote-mcp-server) | Work without local setup                            |
| [2.3 Plan and build an end-to-end Power BI solution](#23-plan-and-build-an-end-to-end-power-bi-solution)                   | Build a model and two report variations             |

## Prerequisites

Before you begin, complete the [workshop prerequisites](../pre-requisites.md). That guide includes installation instructions and account setup.

This lab requires the following:

* GitHub Copilot CLI
* GitHub Copilot app
* Power BI Desktop
* Visual Studio Code
* Git for Windows
* Node.js and npm
* Azure CLI
* A GitHub Copilot license
* A Fabric account with access to Fabric capacity and permission to create a workspace

## Prepare the environment

✅ **Goal**: Install the Power BI authoring plugin once and make it available to every GitHub Copilot surface, then sign in to the accounts the agent needs.

### Clone or download the lab resources

Clone or download as zip the entire repository to your machine, for example under `C:\FabCon\repo`. The lab exercises refer to several resource files by their location in the repository, so downloading the complete repository is easier than downloading each file separately.
  
1. Go to the root of this repository and select **Download Zip**
   
	![clone-repository](../resources/img/clone-repository.png)
  
	If you downloaded the repository as a ZIP file, extract it.

	![cloned-repo](resources/img/cloned-repo.png)

	The resources for this lab are in `./lab2/resources`.

### Install the Power BI authoring plugin

1. Open a terminal (`Win + X` > **Terminal**`).
   
   ![open-terminal-windows](resources/img/open-terminal-windows.png)

2. Run the following commands:

	```powershell
	copilot plugin marketplace add microsoft/skills-for-fabric	
	```

	```powershell	
	copilot plugin install powerbi-authoring@fabric-collection
	```

> [!TIP]
> There are several ways to install skills and plugins. You can install them directly in Visual Studio Code, using [NPX Skills](https://github.com/vercel-labs/skills), [Agent Package Manager](https://microsoft.github.io/apm/) or simply copy them into your workspace or Copilot folder. Installing the plugin through GitHub Copilot CLI is a simple way to make its skills and MCP server available across GitHub Copilot CLI, Visual Studio Code, and the GitHub Copilot app without installing duplicate copies.
>
> Learn more in [`powerbi-authoring-plugin`](https://learn.microsoft.com/en-us/power-bi/developer/agentic/power-bi-agentic-overview#get-started) documentation page.

### Ensure Visual Studio Code is ready

1. Open **Visual Studio Code**.
2. Select [Open the AI features setting](vscode://settings/chat.disableAIFeatures) and ensure that **Disable AI Features** is cleared.
	- **Note:** If the link does not open from your Markdown viewer, open **Settings** in Visual Studio Code and search for `chat.disableAIFeatures`.
3. Open **GitHub Copilot Chat** (`CTRL+ALT+I`) and confirm that the chat view is accessible.
4. You may need to sign-in with your GitHub Copilot account.
	
	![vscode-github-copilot-signin](resources/img/vscode-github-copilot-signin.png)	

5. Open the chat settings and confirm the `powerbi-authoring` plugin is installed.
   
	![vscode-chat-plugin-installed](resources/img/vscode-chat-plugin-installed.png)	

### Ensure GitHub Copilot App is ready

1. Open the **GitHub Copilot App**
2. Sign-in with your GitHub account
   
	![gh-app-sign-in](resources/img/gh-app-sign-in.png)

	Make sure you are signed in with the GitHub account you plan to use at the workshop.

	![gh-app-signed-in](resources/img/gh-app-signed-in.png)

3. Check if the `powerbi-authoring` plugin is installed. Open **Customize** > **Plugins**.

	![gh-app-plugin-installed](resources/img/gh-app-plugin-installed.png)
    
### Sign in to Azure CLI

1. Open a terminal
2. Sign in with your Fabric account:

	```powershell
	az login
	```
3. Follow the browser prompts and return to the terminal when the sign-in completes.
3. Confirm that the correct account is active

	```powershell
	az account show
	```
	![az-account-show](resources/img/az-account-show.png)

> [!IMPORTANT]
> The Power BI report authoring tools use the Azure CLI token to reach Fabric. If the wrong account is active, later exercises fail with authorization errors.

---

## Part 1: Brownfield development

In this part you work with an existing Power BI report. You convert it to PBIP, place it under Git, and use an AI Agent inside Visual Studio Code to document and make changes to it. Power BI Desktop stays open so you can reload and inspect the agent's changes.

### 1.1 Save the report as a PBIP project and track it with Git

✅ **Goal**: Convert a provided PBIX file into a PBIP project and create a Git baseline so that every agent change is reviewable and reversible.

#### Steps

1. Open the workshop [`resources/sales.pbix`](resources/sales.pbix) file in **Power BI Desktop**.
2. Select **File** > **Save as** > **Browse this device**. In the **Save as type** option, select **Power BI project files (*.pbip)** and save the project to a local folder of you choice, for example `C:\FabCon\Lab2_Part1`.
	
	![pbid-save-pbip](resources/img/pbid-save-pbip.png)

3. Keep Power BI Desktop open. You reload changes from it later in the lab.
4. Open the project folder in **Visual Studio Code** by clicking the title bar and choosing **Open in Visual Studio Code**
   
	![pbid-open-vscode](resources/img/pbid-open-vscode.png)

   Confirm that the folder contains the `sales.SemanticModel` and `sales.Report` folders.

	![vscode-pbip](resources/img/vscode-pbip.png)

5. Click the **Source Control** (`CTRL+SHIFT+G`) tab and select **Initialize Repository**. 
6. Commit your changes with a message of your choice. For example: `Initial PBIP baseline`

	![vscode-init-git-pbip](resources/img/vscode-init-git-pbip.png)

> [!IMPORTANT]
> PBIP stores the semantic model as TMDL files and the report as PBIR files. Both are plain text, so Git can show you exactly what the agent changed. This is your safety net: review the diff after every prompt, keep what you want, and discard the rest with **Discard changes** in the Source Control view.    	

### 1.2 Prepare the codebase with agentic context

✅ **Goal**: Add versioned instructions and a local skill so agents understand how to work with this codebase before you enter the first prompt.

#### Steps

1. In the repository folder you cloned or downloaded in [Prepare the environment](#prepare-the-environment), for example `C:\FabCon\repo`, open `lab2/resources`. Copy the following items to the root of your PBIP project folder:
   
	- [resources/AGENTS.md](resources/AGENTS.md)
	- [resources/company-context.md](resources/company-context.md)
	- The [resources/.github](resources/.github) folder

2. Confirm that your folder looks like this:
	
	![vscode-pbip-folder](resources/img/vscode-pbip-folder.png)

> [!IMPORTANT]
> [`AGENTS.md`](https://agents.md/) is an important part of agentic development. It lets you define codebase-level rules, context, and constraints that agents need to understand and respect when working on the project. Because the file is stored with the codebase and read automatically, the same guidance applies consistently across chat sessions and team members.
>
> The `AGENTS.md` file in this workshop is a simple example. It reinforces that the agent always loads the appropriate Power BI authoring skills and directs it to use the Power BI Authoring MCP server when editing the semantic model. The agent can work with TMDL files directly, but using the MCP tools provides a more reliable authoring path less likely to break things.
>
> This workshop uses Microsoft-provided agent skills installed through the `powerbi-authoring` plugin. Skills give the agent context about processes and preferred ways of working. Teams can keep project-specific skills in source control to capture business practices and help developers produce consistent results. The [`powerbi-documentation` skill](resources/.github/skills/powerbi-documentation/SKILL.md) is an example of a repository-local skill that lives alongside the codebase. Skills can also be shared through private or public repositories and marketplaces.

3. Open **Source Control** (`CTRL+SHIFT+G`) and commit the new files.
   - **Tip:** You can use Copilot to generate analyze the changes and generate the commit message for you by clicking on **Generate commit message** in the top right corner of the textbox.

### 1.3 Generate documentation for the model and report

✅ **Goal**: Let the agent produce the documentation that usually never gets written, using the PBIP files as the source of truth.

Writing documentation from scratch and keeping it current both take time. AI can help you create a useful starting point, while the Power BI agentic tools, the MCP server, and the `powerbi-desktop` CLI can help keep it aligned with the model and report with minimal ongoing effort.

#### Steps

1. In **Visual Studio Code**, open **GitHub Copilot Chat** (`CTRL+ALT+I`).
2. Set the chat mode to **Agent** and in the model picker, select the reasoning model `GPT-6 Sol` and thinking effort `Medium`.	

	![vscode-copilot-chat](resources/img/vscode-copilot-chat-2.png)	

> [!IMPORTANT]
> **Choose the model and thinking effort based on the complexity of the task.**
> Example using GPT-6 models:
> * **Sol:** Best suited for more complex tasks that require deeper reasoning, planning, or validation.
> * **Luna:** Fast and cost-efficient for simple, high-volume tasks, but less suitable for complex Power BI development and reasoning.
>
> You can also adjust the **thinking effort** independently. Higher thinking effort can improve results on complex tasks, but can also increase cost.
>
> See [Models and pricing for GitHub Copilot](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing)
>
> **Rule of thumb:** Start with the least expensive model and thinking effort that can reliably complete the task, and scale up when the task requires more reasoning or validation.
>
> Highly recommend the following article from the Tabular Editor team: [Tabular Editor - Pick the right AI model](https://tabulareditor.com/blog/picking-the-ai-model-for-the-task)

3. Enter the following prompt:

	```text
	Document this Power BI Project code base.
	```

	**Expected outcome**

	- The agent reads `AGENTS.md` and loads the local `powerbi-documentation` skill. LLMs load skills on demand, and the instruction in `AGENTS.md` reinforces this requirement for certain tasks.
	- The agent follows the documentation structure and standards defined by the `powerbi-documentation` skill.
	- The agent loads semantic model and report skills from `powerbi-authoring` plugin. The skills include guidance on how to properly read and analyze semantic model and report metadata.		
	- The agent uses the Power BI report CLI tools to capture screenshots from the report open in Power BI Desktop.
	- A `docs/` folder is created with a catalog and one Markdown file for each semantic model and report in the codebase.
	- The generated documentation includes the model structure, measures, report flow, filters, and a screenshot of every report page.

> [!IMPORTANT]
> The short prompt works because `AGENTS.md` requires the agent to load the local `powerbi-documentation` skill. The skill defines how the team expects project documentation to be created, while the Power BI MCP server and report tools provide the model and report information needed to create it. `AGENTS.md` includes an important guidance to always prefer to use the MCP to edit the semantic model instead of direct TMDL file editing.

4. Notice that the agent asks for approval before each tool call. This is the default behavior of the VS Code agent harness. Review the tool and its parameters before approving it.

	![vscode-copilot-tool-permissions-1](resources/img/vscode-copilot-tool-permissions-1.png)	

	You can **Allow all** tools or switch the agent mode to **Autopilot** and **Allow all** permissions so it can run the required tools and continue until the task is complete without prompting you at every step.

	![vscode-copilot-chat-auto-pilot](resources/img/vscode-copilot-chat-auto-pilot.png)

	**Interactive vs. Autopilot**

	- **Interactive:** The agent pauses for your approval or input as it works. For example: `Agent → action → ask you → Agent → action → ask you`. You can inspect each tool call before it runs.
	- **Autopilot:** The agent keeps working through the task, including retries and validation, without stopping at each approval or question. For example: `Agent → action → action → error → fix → action → validate → done`.

	Both modes still need you to review the final documentation and changes. **Allow all** removes tool approval prompts, but is not the same as switching to Autopilot mode.

> [!IMPORTANT]
> **Autopilot** is an agent mode, not a permission level. It lets the agent work autonomously until the task is complete by auto-approving tools, retrying errors, and answering questions that would otherwise block progress. **Allow all** and **Autopilot** skip confirmation for potentially destructive actions, including file edits, terminal commands, and external tool calls. Use Autopilot or Allow All only in a trusted workspace and when you understand the security implications. For details, see [How Autopilot works](https://code.visualstudio.com/docs/agents/run/approvals#_how-autopilot-works).

5. Open the generated documentation Markdown files in `docs/` and preview them with **Ctrl+Shift+V**.
6. Open the **Source Control** (`CTRL+SHIFT+G`) and commit all changes.

#### Reflection

* How much time would you need to produce the same level of documentation for one of your own semantic models?
* Which documentation standards would your team add to or remove from the `powerbi-documentation` skill?
* Which parts of the generated documentation still need a human to verify?

### 1.4 Add measure descriptions using company context

✅ **Goal**: Add business-friendly descriptions to every measure, written in the language of Northwind Retail Group rather than generic BI text.

#### Steps

1. Start a **new chat session** in GitHub Copilot Chat and choose `GPT-6 Luna` model and thinking effort `Medium`.

> [!IMPORTANT]
> Using a cheaper model like `GPT-6 Luna` to generate descriptions for existing Power BI measures is a good fit because the task is well-scoped and repeatable. 

> [!TIP]
> Start a new session when moving to a different task. A clean session prevents decisions, assumptions, and tool results from the previous task from influencing the next one. You can also reuse an existing sessions to keep the session context. For example, you could reuse the documentation session to update the docs after making changes to the semantic models or reports.

2. Enter the following prompt:

	```text
	Add a description to every measure in the semantic model `Sales.SemanticModel\definition`.
	Use `company-context.md` for tone and business context so descriptions sound like they come from someone at Northwind Retail Group, not generic BI text.
	Keep each description to 1-2 sentences: what the measure calculates, and any business nuance from the context (e.g. net vs. gross, fiscal year, seasonality) where relevant.
	```

	**Expected outcome**

	- The agent loads the `semantic-model-authoring` skill.
	- The agent connects to the semantic model TMDL files through the Power BI Authoring MCP server instead of editing TMDL files by hand.
	- Every measure receives a concise description of one to two sentences.
	- The descriptions reflect the context file, for example revenue described as net sales, fiscal years labeled FY24 or FY25, and seasonal patterns called out where they are relevant.
	- The updated model is saved back to the PBIP folder.
	- No measure expressions, data types, or relationships are changed.

> [!TIP]
> There is little difference between `company-context.md` and the context contained in a skill. The company context could be packaged as a skill. This exercise keeps it as a regular file to show that you can also give an agent context by referring to a file directly in your prompt.

3. Switch to **Power BI Desktop** and select **Apply external changes** to load into **Power BI Desktop** the changes the agent did to the semantic model.
   
   ![pbi-desktop-reload-external-changes](resources/img/pbi-desktop-reload-external-changes.png)

> [!TIP]
> **Apply external changes** shipped with the August 2026 Power BI Desktop release. It detects and reloads PBIP files changed outside Power BI Desktop, whether those changes were made manually in Visual Studio Code or generated by AI agents and tools. Learn more in [Edit Power BI Desktop project files in Visual Studio Code](https://learn.microsoft.com/en-us/power-bi/developer/projects/projects-external-editing).

4. Select a measure in the model view and review the AI generated description infused with context from the [`company-context.md`](resources/company-context.md).
5. Open the **Source Control tab** in Visual Studio Code (`CTRL+SHIFT+G`) and review the Git diffs. Confirm that the changed lines are description properties only, and that no DAX expression was modified. In the end **Commit** the changes to the repo.
    
    ![vscode-copilot-change-tmdl-diff](resources/img/vscode-copilot-change-tmdl-diff.png)

> [!TIP]
> This is the main advantage of PBIP with Git. You see the exact change before you accept it.

#### Reflection

* Which descriptions would you keep as written, and which would you rewrite? Could you include context in [`AGENTS.md`](resources/AGENTS.md) or [`company-context.md`](resources/company-context.md) to make it better?
* What other team knowledge would be worth storing as a context file in the repository?
* Without Git, how would you determine exactly what the agent changed?
* How would you safely and quickly revert an agent change that produced the wrong result?

### 1.5 Add currency conversion with a calculation group

✅ **Goal**: Extend the semantic model with a new source table and a calculation group so sales can be analyzed in multiple currencies.

#### Steps

1. Start a **new chat session** and pick `GPT-6 Sol` model and thinking effort `Medium`.

2. Enter the following prompt:

	```text
	Add https://raw.githubusercontent.com/pbi-tools/sales-sample/refs/heads/data/RAW-CurrencyExchange.csv to the `sales.SemanticModel\definition` semantic model, then create a calculation group to convert and analyze sales in EUR, USD, and GBP.
	```

	**Expected outcome**

	- The agent loads the `semantic-model-authoring` skill.
	- The agent inspects the CSV file to determine its schema before creating anything.
	- The agent uses the Power BI Authoring MCP server to create the currency exchange table and the calculation group.
	- The calculation group contains calculation items for EUR, USD, and GBP.
	- The updated model is saved back to the PBIP folder.
	- The Git diff shows new TMDL files for the table and the calculation group, and no unrelated model changes.

3. Switch to **Power BI Desktop** and select **Apply external changes**.
4. Click **Refresh** to load the data from the remote CSV file.
5. Create a temporary visual with a sales measure, then apply the calculation group items to confirm that the converted values change as expected.
   
   ![pbi-desktop-calc-group-test](resources/img/pbi-desktop-calc-group-test.png)

7. Open **Source Control** (`CTRL+SHIFT+G`) and commit the changes.

#### Reflection

* Notice how the agent fetched a new remote data source, inspected its schema, and translated it into semantic model definitions.

### 1.6 Restyle the report pages

✅ **Goal**: Apply a consistent layout across every page of the report through the report authoring tools.

#### Steps

1. Open the "Sales" report page in **Power BI Desktop** and observe that visual positioning and size is not consistent.

    ![pbi-desktop-current-report](resources/img/pbi-desktop-current-report.png)

2. Select **Save** in **Power BI Desktop** to ensure there are no pending changes. The agent will modify the PBIR files and reload the report automatically, but Power BI Desktop blocks the reload if it has unsaved changes.
   
3. Start a **new chat session** and pick model `GPT-6 Sol` and thinking effort `Medium`.

4. Enter the following prompt to use AI to help you make changes to the report.

	```text
	Remove visual titles on all visuals of all pages in report `Sales.Report`.
    Ensure the grid layout of the report is consistent in terms of visual alignment and size:
    - Visual alignment between the grid rows
    - Spacing between visuals should be consistent
    - Visuals below the cards and slicers should have the same size on each grid row.
    Note: If CLI reports `hasUnsavedChanges=true` overwrite and continue with the reload for the screenshot validation.
	```

	**Expected outcome**

	- The agent loads the `powerbi-report-cli` skill.
	- The agent reads and modify the PBIR *.json files
	- The agent uses both the `powerbi-report-author` CLI to validate schema changes and preview the report with screenshots in **Power BI Desktop** for validation.

> [!IMPORTANT]
> The agent might refuse to reload the report if the Power BI Desktop CLI reports `unsavedChanges`. This usually means that Power BI Desktop contains changes that have not been saved to the PBIP files. Stopping prevents the agent from overwriting your work. In this exercise we know that agent is the only one modifying the report and because of that we state explicitly in the prompt that it can proceed despite the warning.

5. Switch to **Power BI Desktop** and confirm that titles are removed and the visuals are aligned.
   
	The report should look like the following. But not necessarily the same.

	![pbi-desktop-after-report](resources/img/pbi-desktop-after-report.png)

6. Open **Source Control** (`CTRL+SHIFT+G`) and commit the changes.

#### Reflection

* This example makes a simple change to two report pages, but the same approach can scale to larger reports or multiple reports.
* You could capture your team's layout and design standards as agentic context, then use agents to review reports against those standards autonomously.
* Power BI CLI tools let the agent validate the PBIR JSON changes and use screenshots from Power BI Desktop to confirm that the rendered report matches the intended result.

---

## Part 2: Greenfield development

In this part you start from nothing. You create a Fabric workspace, load a Lakehouse with a notebook, and then build a Direct Lake semantic model and two reports using the **GitHub Copilot app**.

There are no local files in this part. The **GitHub Copilot app** is a good fit for that: it is more approachable than Visual Studio Code or the CLI. Underneath it is the same GitHub Copilot orchestrator, the same skills, and the same MCP capabilities, so the experience stays consistent. Which surface you use is a matter of preference.

### 2.1 Prepare the Fabric Workspace

✅ **Goal**: Create an isolated Fabric workspace and load it with a Lakehouse containing the sample sales tables.

#### Steps

1. Go to [Power BI](https://app.powerbi.com) and sign in with the workshop account.
2. Select **Workspaces** > **New workspace**.
3. Name the workspace using this convention:

	```text
	FabCon-Agentic-Lab2-[YourInitials]
	```

4. Assign the workspace to the avaiable Fabric/Premium capacity and select **Apply**.
5. In the new workspace, select **New item** > **Notebook**.
6. Open [resources/notebook.py](resources/notebook.py) from the workshop repository and copy its contents.
7. Paste the code into the first cell of the notebook.
8. Run the notebook cell (`CTRL+ENTER`) and wait for it to finish.
   
	![fabric-notebook-lakehouse-create](resources/img/fabric-notebook-lakehouse-create.png)

9. Refresh the workspace and confirm that a Lakehouse named `Lakehouse_01` was created.
10. Open the Lakehouse and confirm that it contains the following tables:

	* `dimension_city`
	* `dimension_customer`
	* `dimension_date`
	* `dimension_employee`
	* `dimension_stock_item`
	* `fact_sale`

	![fabric-sample-lakehouse-tables](resources/img/fabric-sample-lakehouse-tables.png)


### 2.2 Connect the GitHub Copilot app to the remote MCP server

✅ **Goal**: Register the remote Power BI Authoring MCP server so the agent can work against Fabric semantic models with no local installation.

#### Steps

1. Open the **GitHub Copilot app** and sign in with your GitHub account.
2. Click on **Customize** > **MCP** > **Add server** > **Custom server** and configure the Power BI Authoring MCP server using the HTTP configuration.

	| Setting | Value                                                       |
	| ------- | ----------------------------------------------------------- |
	| Server  | `powerbi-authoring-remote`                                  |
	| URL     | `https://api.fabric.microsoft.com/v1/mcp/powerbi/authoring` |

	![gh-app-pbi-remote-mcp](resources/img/gh-app-pbi-remote-mcp.png)

3. Complete the authentication prompt with the workshop Fabric account.
4. Disable the local MCP from the `powerbi-authoring` plugin. 

	![gh-app-pbi-local-mcp-disabled](resources/img/gh-app-pbi-local-mcp-disabled.png)

> [!IMPORTANT]
> The Power BI Authoring MCP server is available in local and remote (hosted) versions. Use the local server with Power BI Desktop or PBIP files. Use the remote server with semantic models in Fabric because it requires no local installation. For details, see [Power BI Authoring MCP server](https://learn.microsoft.com/power-bi/developer/mcp/power-bi-authoring-mcp).
> 
> The `powerbi-authoring` plugin you installed previously ships with the local version to support both local development and remote. **You should avoid enabling both local and remote versions at the same time**. The agent then sees two overlapping tool sets, which makes routing ambiguous and consumes extra tokens on every request. Pick one: the hosted server when you work against semantic models in Fabric workspaces, and the local server when you work against Power BI Desktop or Power BI Project files on your machine.


### 2.3 Plan and build an end-to-end Power BI solution

✅ **Goal**: Use one prompt to plan and build a Direct Lake semantic model and two executive reports. The exercise demonstrates how skills, MCP tools, and parallel subagents can deliver a complete Power BI solution.

You first review the full implementation plan. After you approve it, the agent creates the semantic model and assigns each report variation to a separate subagent.

#### Steps

1. Create an empty folder on your laptop, for example `C:\FabCon\Lab2_Part2`.
2. Open the **GitHub Copilot app**
3. In **Projects**, select the **+** > **Open folder**, then open the folder you created.

	![gh-app-add-folder](resources/img/gh-app-add-folder.png)

> [!TIP]
> A working folder gives the agent a defined project boundary. It can discover project instructions, use folder-specific MCP settings, and keep related sessions together. Opening a folder does not automatically add every file to the context. The agent reads files as needed.

4. Start a **New session** under the working folder. Set the session mode to **Plan** and select the `GPT-6 Sol` model and effort `Medium`.
	
	![gh-app-new-session](resources/img/gh-app-new-session.png)

> [!TIP]
> **Plan** mode lets Copilot inspect the available context and propose an implementation approach before creating anything. You can correct assumptions and agree on the complete solution before implementation begins. For details, see [Use the GitHub Copilot plan agent](https://learn.microsoft.com/en-us/visualstudio/ide/copilot-plan-agent?view=visualstudio).

5. Add [resources/team-rules.md](resources/team-rules.md) and [resources/contoso-design-standards.png](resources/contoso-design-standards.png) to the session context.	

	![gh-app-add-context-file](resources/img/gh-app-add-context-file.png)

	**Note:** You can also copy these files into the working folder (`C:\FabCon\Lab2_Part2`) and simply refer their name in the prompt.

6. Enter and run the following prompt, replacing the [YOUR_WORKSPACE_NAME] with the name of your workspace:

	```text
	Create a Direct Lake semantic model named **Sales Model** using lakehouse **Lakehouse_01** in workspace **[YOUR_WORKSPACE_NAME]**.

	Include these tables:

	- `dimension_city`
	- `dimension_customer`
	- `dimension_date`
	- `dimension_employee`
	- `dimension_stock_item`
	- `fact_sale`

	Follow the development rules in `team-rules.md`.

	Then create two separate, single-page executive Power BI reports using `contoso-design-standards.png` design system:

	- **Style 1:** KPI cards across the top; trends and category breakdowns below.
	- **Style 2:** KPI cards on the left; trends and category breakdowns on the right.

	Assign one subagent to each report style as nested sessions. Save both reports separately in the workspace for my review in the workspace.

	Do not modify the semantic model after creation. Use the available fields and measures, applying best judgment to select the most relevant executive insights.

	IMPORTANT:
		When calling `powerbi-report-author preview` with `--host service` and the preview renders an empty/blank report. Stop and apply the following patch:

		The bundled powerbi-client drops `reportDefinition`, idempotently patch the installed CLI `dist\cli.js` and `dist\preview\index.js` before `service.createReport(...)` by wrapping `Create.prototype.create` and copying `this.config.reportDefinition` to `this.createConfig.reportDefinition`.
	```

	**Expected planning outcome**

	- The agent loads the semantic model and report authoring skills required for the task.
	- The agent uses [`team-rules.md`](resources/team-rules.md) for semantic model standards and [`contoso-design-standards.png`](resources/contoso-design-standards.png) for report design guidance.
	- The agent produces a plan for the complete solution without creating any Fabric items.
	- The plan creates the semantic model before the reports and prevents report work from changing the completed model.
	- The plan assigns one isolated subagent to each report style so both variations can be built in parallel.	

7. Review the plan. Confirm that it follows the team rules, uses the requested tables, creates both report styles, and includes validation for the model and reports.	

	![gh-app-plan-review](resources/img/gh-app-plan-review.png)

8. Adjust the plan if needed, then approve it to start the implementation.

	**Expected implementation outcome**

	- The agent discovers the Fabric workspace, lakehouse, and required metadata.
	- The agent uses the Power BI Authoring MCP server to create the `Sales Model` Direct Lake semantic model over the selected lakehouse tables.
	- The model follows `team-rules.md`, including friendly table names, explicit measures, hidden base columns, relationships, and the `About` table.
	- After the model is complete, the agent starts two subagents in isolated background sessions, one for each report style.
	- Each subagent uses the completed semantic model without modifying it and saves a separate single-page report in the workspace.
		
		![gh-app-sub-agents-running](resources/img/gh-app-sub-agents-running.png)

> [!TIP]
> Subagents run in separate, isolated contexts. Each subagent can focus on one report style without mixing its work with the parent agent or the other subagent. Learn more in [Agents and Subagents](https://awesome-copilot.github.com/learning-hub/agents-and-subagents/).

9. Review the session transcripts.

	Notice how the parent agent first creates the model in the Fabric Lakehouse using the Power BI Authoring MCP server:

	![gh-app-create-direct-lake](resources/img/gh-app-create-direct-lake.png)

	After creating the model, the parent agent starts two subagents, one for each report style. It gives each subagent the model context and instructs it to author only its assigned report without changing the semantic model.

	![gh-app-nested-sessions](resources/img/gh-app-nested-sessions.png)	

	Select a subagent to inspect the prompt it received from the parent agent and review the work it completed.

	![gh-app-sub-agent-session-prompt](resources/img/gh-app-sub-agent-session-prompt.png)

	Each subagent uses the Power BI report authoring skill to create and preview its report before deploying it to the workspace.

	![gh-app-nested-sessions-rp-preview](resources/img/gh-app-nested-sessions-rp-preview.png)

10. Open `Sales Model` in the Fabric workspace. Confirm that the tables, relationships, hidden base columns, explicit measures, and `About` table follow the team rules.

	![fabric-created-semantic-model](resources/img/fabric-created-semantic-model.png)

11. Open both reports in the Fabric portal. Confirm that each report has one page, follows the assigned layout, and uses the provided design standards. Compare the two variations.
    
	| Style 1 | Style 2 |
	| --- | --- |
	| ![Report style 1](resources/img/fabric-created-report-style-1.png) | ![Report style 2](resources/img/fabric-created-report-style-2.png) |

12. Select the session name at the top of the window to review the total spend and token usage for the parent session and its subagents.
	
	![gh-app-session-context](resources/img/gh-app-session-context.png)

	You can also click on **View session insights** for a more detailed timeline.

	![gh-app-session-context-insights](resources/img/gh-app-session-context-insights.png)


#### Reflection

* How did the time and cost of using the agent compare with completing the entire task yourself?
* What additional instructions would you add to the prompt, `team-rules.md`, or another context file to help the agent meet your development quality standards?
* For real projects, point agents to a development workspace and use [Fabric Git integration](https://learn.microsoft.com/en-us/fabric/cicd/git-integration/intro-to-git-integration) and [Fabric CI/CD](https://learn.microsoft.com/en-us/fabric/cicd/cicd-overview) to promote reviewed changes. Do not point agents directly at a production workspace.

## ✅ Wrap-up

You've now learned how to:

* Use Git to review, version, and revert changes made by AI agents
* Guide agents with shared instructions, skills, and project context stored alongside your code
* Combine skills that describe how to work with MCP tools that perform and validate the work
* Separate planning from implementation so you can review an approach before the agent makes changes
* Choose an AI model based on the reasoning, cost, and execution needs of each task
* Use separate sessions to keep unrelated tasks from influencing each other
* Use subagents with isolated contexts to explore independent approaches in parallel
* Work in development environments and promote reviewed changes instead of pointing agents at production
* Apply the same agentic workflow across GitHub Copilot CLI, Visual Studio Code, and the GitHub Copilot app

## Useful links

* [Power BI Desktop projects (PBIP)](https://learn.microsoft.com/power-bi/developer/projects/projects-overview)
* [Power BI MCP servers](https://learn.microsoft.com/en-us/power-bi/developer/mcp/mcp-servers-overview)
* [Skills for Fabric GitHub repo](https://github.com/microsoft/skills-for-fabric)
* [Direct Lake overview](https://learn.microsoft.com/fabric/fundamentals/direct-lake-overview)
* [Model Context Protocol](https://modelcontextprotocol.io/)
* [Agent Plugins spec](https://github.com/agentplugins/agent-plugins-spec)
* [Agent Skills spec](https://agentskills.io/specification)
* [VS Code agent harnesses](https://code.visualstudio.com/docs/agents/run/agent-harnesses)
* [Tabular Editor - Get Started with Agentic Development](https://tabulareditor.com/blog/how-to-get-started-with-agentic-development-for-business-intelligence)
* [Tabular Editor - Pick the right AI model](https://tabulareditor.com/blog/picking-the-ai-model-for-the-task)
* [Tabular Editor - LLMs for data professionals](https://tabulareditor.com/blog/practical-introduction-to-llms-for-data-professionals)
* [Git Will Finally Make Sense After This](https://www.youtube.com/watch?si=h_hAniLBVfO05X7A&v=Ala6PHlYjmw&feature=youtu.be)
* [Introduction to Git in Visual Studio Code](https://code.visualstudio.com/docs/sourcecontrol/overview)
* [Git cheat-sheet](https://git-scm.com/cheat-sheet)