# Architecture — AI Work Log Assistant (Telegram + n8n + Google Sheets)

## Overview
This automation lets a user log work hours by sending natural language messages in Telegram.
The system extracts structured data, asks for confirmation, temporarily stores the entry, and only writes to Google Sheets once the user confirms.

## Core Components
- **Telegram Trigger**: receives incoming messages
- **AI Agent**: classifies intent and extracts work-log fields
- **Structured Output Parser**: enforces deterministic JSON
- **Data Table (pending store)**: stores entries awaiting confirmation
- **Google Sheets**: final destination for confirmed work logs

## Data Model (AI Output)
The AI Agent returns structured JSON with:
- `intent`: time_entry | confirm_yes | confirm_no | smalltalk | unknown
- `date`: DD-MM-YYYY
- `time_in`: HH:MM (24-hour) or empty
- `time_out`: HH:MM (24-hour) or empty
- `break_hours`: decimal hours
- `worked_hours`: NET worked hours
- `description`, `project`
- `reply`: used only for smalltalk/unknown redirection

## Flow Summary

### 1) User sends a message in Telegram
Examples:
- “Trabajé de 8am a 4pm con 1 hora break”
- “I worked 8 hours and took a 1 hour break”

### 2) AI parses and classifies
- If it’s a time entry → extract times, break, worked_hours
- If it’s confirmation → confirm_yes / confirm_no
- If it’s smalltalk → friendly response + redirect
- If it’s out-of-scope → redirect to the work-log purpose

### 3) If `time_entry` → Confirmation request
The workflow sends a message summarizing extracted fields and asks the user to reply “yes/sí” to save or “no” to cancel.

### 4) Pending entry storage
Before saving to Sheets, the system stores the pending record in an n8n **Data Table** (example name: `carlos_pending`).
This enables multi-step conversations without losing context.

Typical fields stored:
- `key` (e.g. `pending_<chatId>`)
- `chatId`
- `payload` (stringified JSON from AI)
- `createdAt`

### 5) If `confirm_yes` → Save
- Fetch pending record by key
- Parse payload JSON
- Format fields for Google Sheets
- Compute:
  - Total = worked_hours × hourly_rate
- Append row in Google Sheet
- Delete pending record
- Send success message

### 6) If `confirm_no` → Cancel
- Delete pending record
- Send cancellation message

## Key Design Principles
- **Human-in-the-loop**: confirmation before writing to database
- **Deterministic output**: schema-enforced AI JSON
- **State management**: pending store prevents mistakes
- **Language-aware UX**: responses in Spanish or English based on user input
- **Scope control**: out-of-scope questions are redirected back to the core function
