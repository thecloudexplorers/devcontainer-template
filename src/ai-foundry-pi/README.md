# Pi authentication with Azure Foundry

This folder contains a project-local Pi setup that authenticates to Azure AI Foundry using your Azure CLI login (`az login`).

## How it works

- `.pi/agent/models.json` defines provider `azure-foundry-user`.
- The provider `apiKey` is a command that gets a Microsoft Entra access token from Azure CLI:

- `.pi/extensions/azure-foundry-responses.ts` registers the same provider through Pi's `openai-responses` API (required for Foundry deployments that use `/openai/v1/responses`).

## Prerequisites

1. Azure CLI installed (`az`)
2. Pi CLI installed (`pi`)
3. Access to an Azure AI Foundry project and deployed model

## Configure

Update placeholders in `.pi/agent/models.json`:

- `baseUrl` → `https://<your-foundry-project>.services.ai.azure.com/openai/v1`
- model `id` and `name` → your deployed model (example: `gpt-5.3-codex`)

If needed, update `.pi/extensions/azure-foundry-responses.ts` to match the same `baseUrl` and model.

## Authenticate

Sign in with Azure CLI:

```bash
az login
az account set --subscription <your-subscription-id>
```

(Optional) verify token acquisition:

```bash
az account get-access-token --resource https://cognitiveservices.azure.com --query accessToken -o tsv | wc -c
```

## Run Pi with Azure Foundry

```bash
pi --provider azure-foundry-user --model <your-model-id>
```

Example:

```bash
pi --provider azure-foundry-user --model gpt-5.3-codex
```

## Verify end-to-end

```bash
PI_DISABLE_UPDATE_CHECK=1 \
pi --provider azure-foundry-user --model <your-model-id> \
  -p "Reply with pong only."
```

Expected response: `pong`

## Troubleshooting

- **401/403**: run `az login` again and ensure your account has access to the Foundry project/model.
- **Wrong endpoint**: check `baseUrl` includes `/openai/v1`.
- **Unsupported operation**: ensure Pi uses the `openai-responses` provider registration from `.pi/extensions/azure-foundry-responses.ts`.
- **Extension not loaded**: run Pi once with explicit extension path:

```bash
pi -e ./.pi/extensions/azure-foundry-responses.ts \
  --provider azure-foundry-user \
  --model <your-model-id>
```
