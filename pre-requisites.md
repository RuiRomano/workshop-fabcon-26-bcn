# FabCon Barcelona 2026: Power BI Meets Agentic AI Prerequisites

If you have access before the workshop, complete these prerequisites one or two days in advance because new software versions might be available. Otherwise, you can complete them on the day of the workshop.

## Technical knowledge

Participants should have practical Power BI experience. No prior knowledge of agentic development, GitHub Copilot, or MCP servers is required.

Participants should be able to:

- Use Power BI Desktop to connect to data and build a report
- Understand core Power BI semantic model concepts such as tables, relationships, measures, and DAX
- Navigate the Power BI interface and common authoring workflows

## Laptop

- A Windows laptop, ideally with administrator permissions to install software
- A modern web browser, such as Microsoft Edge or Google Chrome

## Software

> [!NOTE]
> **Recommended:** Follow [Prerequisites auto setup](#prerequisites-auto-setup) to install everything with a script. Alternatively, use the links below to install each application manually.

- [GitHub Copilot CLI](https://github.com/features/copilot/cli/)
- [GitHub Copilot App](https://github.com/features/ai/github-app)
- [Power BI Desktop (August 2026 or later release)](https://pbi.onl/download)
- [Visual Studio Code](https://code.visualstudio.com/download)
- [Git for Windows](https://gitforwindows.org/)
- [Node.js and npm](https://nodejs.org/en/download/)
- [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest&pivots=winget)

### Prerequisites auto setup

1. Download the [resources/install-prerequisites.ps1](resources/install-prerequisites.ps1) powershell script to your computer. You can also create a file named `install-prerequisites.ps1` and paste the script into it.
2. Open a terminal (`Win + X` > **Terminal**`) in the folder that contains the script, and run:

   ![open terminal](resources/img/open-terminal-from-folder.png)

   ```powershell
   powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\install-prerequisites.ps1
   ```

   If you use PowerShell 7, run the script with `pwsh.exe` instead:

   ```powershell
   pwsh.exe -NoProfile -File .\install-prerequisites.ps1
   ```

> [!TIP]
> You might see installation errors for software that is already installed. You can ignore these errors if you have confirmed that the required software is available on your computer and up to date.

3. Review the installation details and final status summary in the console. The script attempts every installation, even if one package fails.

> [!NOTE]
> You might see installation errors for software that is already installed. You can ignore these errors if you have confirmed that the required software is available on your computer and up to date.


## Fabric account and tenant

We will provide a Fabric account with access to Fabric capacity on the day of the workshop.

If you want to use your own Fabric tenant, ensure that the following tenant settings are enabled:

- **Users can use the Power BI Model Context Protocol server endpoint**
- **Allow XMLA endpoints and Analyze in Excel with on-premises semantic models**
- **Enable Fabric App Items**

> [!IMPORTANT]
> The workshop Fabric account will be valid on the day of the workshop and deleted a few days later.

## GitHub account and GitHub Copilot license

We will provide a GitHub Copilot license for the workshop. You can use your own license if you prefer.

> [!WARNING]
> Joining a workshop organization can affect an existing GitHub Copilot license on your account.
>
> - If you have a **GitHub Copilot Individual** subscription, your individual license will be cancelled and refunded after being added to the organization. Consider creating and using a separate GitHub account for workshop participation if you want to avoid impacting your current setup.
> - If you already have a **GitHub Copilot Business** or **GitHub Copilot Enterprise** license, you can choose which enterprise account should receive your Copilot charges in your Copilot settings: https://github.com/settings/copilot

To request a GitHub Copilot license for the workshop:

1. Open a browser and authenticate with a **personal GitHub account**. Enterprise Managed User accounts won't work. If you don't have a personal account, [sign up for GitHub](https://github.com/signup).
2. Open the [GitHub Copilot self-signup](https://forms.cloud.microsoft/r/AaJCQCKwAZ) and select **Sign in with GitHub**.

   ![gh-license-self-sign-up](resources/img/gh-license-self-sign-up.png)

3. Follow the instructions, then select **Request organization invitation**.
4. Accept the organization invitation from your email or the self-signup page.
   
   ![join organization](resources/img/gh-license-join-organization.png)
5. After joining the organization, open [Copilot features](https://github.com/settings/copilot/features) and verify that 10,000 AI credits are available.

   ![copilot-ai-credits-page](resources/img/copilot-ai-credits-page.png)

--- 

If the steps above don't work, submit the [GitHub Copilot license request form](https://forms.cloud.microsoft/r/AaJCQCKwAZ) with the username of your personal GitHub account.

> [!IMPORTANT]
> The GitHub Copilot license will only be valid on the day of the workshop. Your access will be removed a few days later.

### Enable the GitHub Copilot license

1. Join the workshop GitHub organization.
2. Close all **Visual Studio Code** windows.
3. Open **Visual Studio Code**.
4. Open **GitHub Copilot Chat** (`Ctrl+Alt+I`).
5. Select **Sign in** in the VS Code status bar and use the GitHub account you associated with the workshop GitHub organization.

   ![vscode-github-copilot-signin](resources/img/vscode-github-copilot-signin.png)

> [!IMPORTANT]
> You might already be signed in with another account. Sign out, then sign in with the account that you used to join the workshop GitHub organization.
>
> ![gh-account-sign-out](resources/img/gh-account-sign-out.png)

6. Click the Copilot icon in the VS Code status bar (bottom of the window).
   
   ![vscode-github-copilot-credits](resources/img/vscode-github-copilot-credits.png)

7. Confirm that you can select a reasoning model provided for the workshop.

   ![vscode-github-copilot-models](resources/img/vscode-github-copilot-models.png)

> [!NOTE]
> You might need to sign out, sign in again, and restart Visual Studio Code before the AI credits take effect.