# AI Work Log Assistant (Telegram + n8n + Google Sheets)

This folder contains an n8n workflow JSON and documentation for an AI-powered Telegram assistant that logs work hours into Google Sheets.

### Overview

This workflow is an AI-powered conversational assistant built in n8n that allows users to log work hours via Telegram. The assistant:

1. Understands natural language (Spanish or English)
2. Extracts structured work data
3. Asks for confirmation
4. Stores pending entries temporarily
5. Saves confirmed entries to Google Sheets
6. Handles cancellations, greetings, and off-topic messages

It combines:

- Conversational AI
- Structured data validation
- Conditional logic
- Temporary state management
- Spreadsheet automation

---

## High-Level Flow

### 1️⃣ Telegram Trigger

The workflow starts when a user sends a message to the Telegram bot.

The message contains:

- Chat ID
- User name
- Message text

---

### 2️⃣ Workflow Configuration (Preprocessing Layer)

This node standardizes incoming data by setting:

- `hourlyRate`
- `chatId`
- `userName`
- `messageText`

This creates a clean internal structure for the rest of the workflow.

---

### 3️⃣ AI Agent (Core Intelligence)

The AI Agent:

- Detects language (Spanish or English)
- Classifies intent:
    - `time_entry`
    - `confirm_yes`
    - `confirm_no`
    - `smalltalk`
    - `unknown`
- Extracts:
    - Date
    - Time in / Time out
    - Break duration
    - Worked hours
    - Description
    - Project

It returns a strictly structured JSON format validated by a **Structured Output Parser**.

This ensures:

- No malformed data
- Predictable automation behavior
- Clean integration with downstream nodes

---

### 4️⃣ Intent Routing (Decision Logic)

The workflow branches using multiple IF nodes:

### If → confirm_yes

- Fetch pending entry
- Parse stored payload
- Save to Google Sheets
- Delete pending row
- Send success message

### If → confirm_no

- Delete pending row
- Send cancellation message

### If → time_entry

- Format confirmation message
- Send message asking user to confirm
- Store pending data in Data Table

### smalltalk / unknown

- Respond conversationally
- No data saved

---

### 5️⃣ Temporary State Management (Data Table)

A Data Table (`carlos_pending`) is used to store unconfirmed entries.

This enables:

- Multi-step conversational confirmation
- Safe cancellation
- No accidental saves
- Clean transactional behavior

Each entry is saved with:

- key (pending_chatId)
- chatId
- payload (JSON string)
- timestamp

---

### 6️⃣ Confirmation Flow

When a user confirms:

- System retrieves pending entry
- Parses JSON via Code node
- Formats fields for Sheets
- Calculates total (worked_hours × rate)
- Appends row to Google Sheet
- Deletes temporary record
- Sends confirmation message

---

### 7️⃣ Google Sheets Integration

The system appends rows with:

- Date
- Time In
- Time Out
- Lunch Break
- Rate
- Worked Hours
- Total

All values are calculated and structured before insertion.

---

## Key Design Principles Used

- Intent classification before logic
- Structured JSON enforcement
- Temporary storage for conversational state
- Explicit confirmation before committing data
- Clean error handling
- Language-aware responses
- Separation of parsing vs formatting layers
