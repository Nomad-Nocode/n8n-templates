# n8n templates

Ready-to-import n8n workflows from [Nomad Nocode](https://nomadnocode.com), a studio that builds automation for e-commerce and small teams (n8n, Make, Supabase). MIT license: use, change and resell freely.

## Workflows

| Workflow | What it does | Needs |
|---|---|---|
| [Track supplier price changes from Gmail CSV to Google Sheets and Slack](workflows/supplier-price-changes-gmail-sheets-slack) | Reads a supplier's CSV price list from Gmail, compares it with a Google Sheet, updates the sheet and posts every price change to Slack. | Gmail, Google Sheets, Slack |

## How to use

1. Open the workflow folder and download `workflow.json`.
2. In n8n choose **Import from file**.
3. Connect your own credentials and follow the setup steps in the folder README.

No workflow in this repository contains credentials. If you find one, please open an issue.

## Need a workflow built or migrated?

See [nomadnocode.com](https://nomadnocode.com) for services and cases.
