GitHub Actions, workflows are made up of multiple layers (or building blocks) that define how automation runs. Think of it as a hierarchy:

Workflow at the top (your .yml pipeline definition).
Jobs (like build, test, deploy).
Steps (individual tasks inside jobs).
Actions (reusable automation units).
Runner (the machine executing everything).

We can use **needs:** for job dependencies

save **secrets in GitHub** under Settings → Secrets and variables → Actions, then reference them in workflows using ${{ secrets.NAME }}.

**Best Practices**
Never hardcode secrets in workflow files.
Use environment-level secrets for production vs staging separation.
Rotate secrets regularly.
Combine with GitHub Environments for approval workflows.
For advanced setups, consider GitHub Actions OIDC + AWS IAM roles (avoids long-lived AWS keys).

🏗️ Layers in GitHub Actions
**Workflow** (Top Layer)
Defined in a .yml file under .github/workflows/.
Represents the entire automation pipeline (e.g., CI/CD pipeline).
Example: ci.yml for continuous integration.

**Jobs** (Inside Workflow)
A workflow can have one or more jobs.
Each job runs in its own runner (VM or container).
Jobs can run in parallel or sequentially (using needs:).
Example: build, test, deploy.

**Steps** (Inside Jobs)
A job is broken down into steps.
Steps run sequentially within a job.
Each step can run a shell command or use an action.
Example: run: npm install, run: npm test.

**Actions** (Reusable Units)
Prebuilt or custom automation tasks.
Can be published by GitHub or the community.
Example: actions/checkout@v3 (to clone repo), actions/setup-node@v4 (to set up Node.js).

**Runners** (Execution Environment)
The machine where jobs run.
GitHub provides hosted runners (Ubuntu, Windows, macOS) or you can use self-hosted runners.

⚖️ Hierarchy Overview
Layer	Description	Example
Workflow	Entire automation pipeline	ci.yml
Job	A unit of work (runs on a runner)	build, test, deploy
Step	Individual task inside a job	run: npm install
Action	Reusable automation component	actions/checkout@v3
Runner	Machine executing the job	Ubuntu runner


🚀 Example Workflow File
yaml
name: CI Pipeline

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
👉 In short: Workflow → Jobs → Steps → Actions → Runner is the layered structure of GitHub Actions.
