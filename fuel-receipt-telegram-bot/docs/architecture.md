# Architecture

## Overview

This system is a **transaction-safe conversational automation** built in n8n.

- Stateless inbound message handling
- Stateful confirmation via Data Table (`fuel_pending`)
- Safe commit to Google Sheets only after approval
- Optional manual correction path (single-message form)

## Core components

- **Telegram Trigger**: entrypoint for messages
- **AI Agent (files)**: extract structured fields from receipt image
- **Drive Upload**: store the receipt image and return share link
- **Data Table**: store pending payload for multi-step confirmation
- **AI Agent (text)**: classify user intent + language
- **Manual Form Parser**: deterministic parsing (no extra agent)
- **Google Sheets Append**: write final record

## Transaction pattern

1. Extract → validate → build pending payload
2. Send confirmation message
3. On approve:
   - read pending
   - append to Sheets
   - delete pending
4. On cancel:
   - delete pending
5. On manual:
   - set pending.status = MANUAL_ENTRY
   - request correction form
   - parse form
   - update pending and return to confirmation

## Why this design

- Prevents accidental writes
- Makes failures recoverable
- Keeps AI in the interpretation layer, not the execution layer
- Deterministic parsing for critical fields
