# Repository Guidelines
####
## Project Structure & Module Organization

`workflows/` contains the deployed n8n workflow JSON files and is the main delivery surface. `custom-nodes/n8n-nodes-custom/` contains custom n8n nodes and credentials, including `nodes/` and `credentials/`. `claudeToOpenAIProxy/` holds the Node.js proxy that translates Claude-style requests to Azure OpenAI. `scripts/` contains local build, run, export, and utility helpers. `devops/` contains Terraform configuration, deployment helpers, and operational documentation such as `TRACEABILITY_MATRIX.md`.

## Build, Test, and Development Commands

Run `./scripts/build_local.sh` to build the Docker image as `n8n-automation:local`. Run `./scripts/run_local.sh` to start the local container; override `ENV_FILE=.local.env` or `HOST_PORT=9090` when needed. Use `./scripts/build_and_push_acr.sh` to publish images to ACR. For infrastructure changes, run `cd devops && ./deploy.sh sandbox plan` before any apply, then `./deploy.sh <env> apply` for the target environment.

For the proxy, use `cd claudeToOpenAIProxy && npm start`. Existing shell checks include `./basic-test.sh` and `./test-claude-setup.sh`.

## Coding Style & Naming Conventions

Shell scripts use `bash`, `set -euo pipefail`, and uppercase environment-variable names. Keep JSON workflow files formatted consistently with existing exports and preserve descriptive names such as `SaMD 12 _ Test _ OQ Test Automation.json`. JavaScript in custom nodes and the proxy uses CommonJS-style n8n node files or ESM where already established; match the surrounding module style instead of mixing conventions.

## Testing Guidelines

There is no unified automated test suite yet, so validate changes at the narrowest level possible. For workflow edits, re-import or execute the affected workflow in local n8n. For proxy changes, run the provided shell checks in `claudeToOpenAIProxy/`. For deployment changes, treat `./deploy.sh <env> plan` as the minimum verification step and review the Terraform diff before apply.

## Commit & Pull Request Guidelines

Recent history favors short, imperative commit messages such as `Fix intake router and URS Quality Scan` or `Add Dummy workflows to deploy and start the intake router on the server`. Follow that pattern: state what changed and why, avoid vague one-word messages, and group related workflow or infrastructure edits together. Pull requests should summarize affected areas, deployment impact, required environment changes, and include screenshots only when UI behavior in n8n is relevant.

## Security & Configuration Tips

Never commit populated `.env`, `.local.env`, or secrets copied from Azure, Entra ID, or Azure DevOps. Keep `ENVIRONMENT.md` aligned with any new variables. When editing workflows that touch credentials, prefer environment-backed configuration and document new flags or required secrets in the relevant Markdown file.
