# Telegram Invoice Manager (n8n)

Send an invoice photo or PDF to a Telegram bot. AI reads it, files it in Google Drive, logs the details to Google Sheets, and replies with a summary.

**13 nodes** - built with [n8n](https://n8n.io). Telegram Bot API, Google Gemini, OpenRouter, Google Drive, Google Sheets

## How it works

1. **Invoice Uploaded** - Telegram trigger fires on every message.
2. **Has File** - checks the message has a PDF/photo; if not, the bot asks the user to send one.
3. **Download Invoice** - fetches the file from Telegram.
4. **Save To Drive** - uploads the original file to Google Drive.
5. **Read Invoice** - an AI agent (Gemini, with OpenRouter as fallback) extracts vendor, customer, invoice number, dates, currency, subtotal, tax, total, line items, payment details and category into structured JSON.
6. **Combine File And Data -> Build Row -> Log Invoice** - merges the Drive link with the extracted data and appends a row to Google Sheets.
7. **Send Summary** - replies in Telegram with the extracted details and the Drive link.

## Credentials you need

- Telegram Bot API (create a bot with @BotFather)
- Google Drive OAuth2
- Google Sheets OAuth2
- Google Gemini (PaLM) API
- OpenRouter API (fallback model)

## Setup

1. In n8n, choose **Workflows -> Import from File** and select `workflow.json`.
2. Create/assign the credentials listed above on each node that asks for one.
3. Create a Google Sheet with a header row: `Vendor, Customer, Invoice_number, Invoice_date, due_date, currency, Subtotal, tax, total, line_items, payment_details, Category, File_name, Drive_link, Date_added`.
4. In the **Log Invoice** node, replace `YOUR_GOOGLE_SHEET_ID` with your sheet's ID (the long string in its URL).
5. Test with a manual run, then activate the workflow.

## Notes

- This export has been sanitized: credential IDs, chat IDs, sheet IDs, phone number IDs and webhook IDs were removed or replaced with `YOUR_...` placeholders.
- Never commit real API keys, tokens or customer data to this repo.

## License

MIT
