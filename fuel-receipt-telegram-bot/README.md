# Fuel Receipt Telegram Bot (n8n + Gemini)

Set up these credentials in n8n:

- Telegram API (bot token)
- Google Drive OAuth2
- Google Sheets OAuth2
- Google Gemini / PaLM API (as supported by your n8n node)

### 4) Create the Data Table

Create an n8n Data Table called `fuel_pending` with columns:

- `key` (string)
- `payload` (string)
- `created_at` (string)

The workflow uses a single key: `pending_fuel`.

### 5) Update IDs inside nodes

In your imported workflow, update:

- Drive folder ID (receipt storage folder)
- Sheets document + sheet name (destination)
- Any credential selectors

### 6) Activate workflow

- Set Telegram Trigger to listen for messages
- Activate workflow

## Usage

### Upload receipt

Send a receipt photo to the bot.

### Approve

Reply: `approve`, `yes`, `si`, `ok` (intent routing maps to approve).

### Cancel

Reply: `cancel`, `no guardar`, `discard`.

### Manual fix (single message)

Reply with any of:

- `fix`, `edit`, `corrige`, `mal`, `incorrecto`, `arregla`, `manual`

The bot will respond with a copy/paste form. Fill and send it as one message.

## Data captured

Saved to Google Sheets:

- timestamp_ingested
- receipt_date (DD/MM/YYYY)
- vendor
- total_amount
- currency
- gst_included
- gst_amount
- notes
- drive_file_url
- telegram_chat_id
- telegram_message_id

## Security

See [SECURITY.md](SECURITY.md).

## License

MIT — see [LICENSE](LICENSE).

## Contributing

PRs welcome. Please:

- keep prompts deterministic
- avoid storing secrets in workflow exports
- add test examples to `examples/sample-messages.md`
