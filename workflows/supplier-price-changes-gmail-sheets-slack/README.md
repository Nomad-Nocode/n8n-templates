# Track supplier price changes from Gmail CSV to Google Sheets and Slack

An n8n workflow that watches a Gmail inbox for a supplier's CSV price list, compares it with the prices stored in a Google Sheet, writes the new prices back and posts one Slack message with every change.

## Who it's for

Small shops, resellers and agencies that receive price lists by email and update their own prices by hand.

## What it does

1. Gmail Trigger checks every hour for unread emails with a CSV attachment. Add the supplier's address to the Sender filter.
2. The CSV is read with Extract from File. The first row is the header.
3. "Set import settings" names the CSV columns (SKU, name, price) and the minimum change in percent. "Normalize CSV rows" cleans each row. Spaces in numbers and decimal commas are handled (`1 250,50` becomes `1250.5`). Rows without a SKU or a numeric price are dropped.
4. The current prices are read from a Google Sheet and joined to the rows by SKU.
5. Each row gets a status: `new`, `increase`, `decrease` or `same`. Rows with no change, or a change below the minimum percent, are dropped.
6. Changed rows are written back to the sheet (matched by `sku`) and listed in one Slack message.
7. The email is marked as read so it is not processed again.

## Requirements

- An n8n instance (cloud or self-hosted).
- Gmail, Google Sheets and Slack accounts.
- A sheet with the header row `sku`, `name`, `price`, `updated_at`.

## Setup

1. Import `workflow.json` into n8n.
2. Connect Gmail in the trigger and in "Mark email as read".
3. Connect Google Sheets in both Google Sheets nodes and select the same sheet. `sample-prices-sheet.csv` shows the layout.
4. In the Gmail Trigger filters, add the supplier's address to Sender.
5. In "Set import settings", enter the column names used in the CSV and the minimum change in percent.
6. Connect Slack and choose a channel in "Post price changes to Slack".
7. Activate the workflow and send yourself a test email with `sample-price-list.csv`.

## How to customize

- Set `minChangePercent` to 2 to ignore price moves smaller than 2 percent.
- Replace Slack with Telegram, Gmail or Microsoft Teams. The message text is built in "Build Slack message".
- Change the delimiter in "Read CSV attachment" for semicolon-separated files.
- Add more columns to the sheet mapping, for example a supplier name or a currency.

## Limitations

- Only the first attachment of each email is read (`attachment_0`).
- One price column is compared. Multi-currency or tiered price lists need extra mapping.
- A new SKU is reported as `new` and added to the sheet. SKUs missing from the new file are left untouched.
- Slack messages list at most 40 changes. All changes are still written to the sheet.
