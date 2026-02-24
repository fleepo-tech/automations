# Sample Messages (User → Bot)

## Work log messages (should trigger `time_entry`)

### Spanish
- "Trabajé 8 horas y tuve break 1 hora"
- "Trabajé de 8am a 4pm y tuve 1 hora break"
- "Trabajé de 8am a 4pm y mi break fue de 1pm a 2pm"
- "Ayer trabajé de 9am a 6pm con 30 min de break"

### English
- "I worked 8 hours and took a break of 1 hour"
- "I worked from 8am to 4pm with a 1 hour break"
- "I worked from 8am to 4pm and my break was from 1pm to 2pm"
- "Yesterday I worked from 9am to 6pm with a 30 minute break"

Expected extraction:
- time_in/time_out should be returned if a time range exists (HH:MM, 24h)
- break_hours should be decimal (e.g., 0.5)
- worked_hours should be NET worked time:
  - If explicit "worked X hours" → worked_hours = X
  - If time range → worked_hours = (time_out - time_in) - break_hours

---

## Confirmations

### Confirm yes (should trigger `confirm_yes`)
- "sí"
- "si"
- "yes"
- "ok"
- "dale"
- "listo"
- "👍"

### Confirm no (should trigger `confirm_no`)
- "no"
- "cancelar"
- "cancela"
- "stop"
- "nope"

---

## Smalltalk / greetings (should trigger `smalltalk`)
Spanish:
- "hola"
- "gracias"
- "chao"

English:
- "hello"
- "thanks"
- "bye"

Expected behavior:
- Bot replies politely in the same language
- Bot redirects the user to send work hours

---

## Out-of-scope questions (should trigger `unknown`)
- "¿Cómo está el clima?"
- "What’s the USD price today?"
- "Tell me the news"

Expected behavior:
- Bot politely says it only logs work hours
- Provides a quick example of a valid hours message
