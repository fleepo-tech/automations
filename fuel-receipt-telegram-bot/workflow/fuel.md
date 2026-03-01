[Fuel.json](https://github.com/user-attachments/files/25657432/Fuel.json)
{
  "name": "My workflow 2",
  "nodes": [
    {
      "parameters": {
        "updates": [
          "message"
        ],
        "additionalFields": {}
      },
      "type": "n8n-nodes-base.telegramTrigger",
      "typeVersion": 1.2,
      "position": [
        -944,
        524
      ],
      "id": "ded6b57d-33a9-4fb5-b019-031d4d54f311",
      "name": "Telegram Trigger",
      "webhookId": "66f72d6e-b443-4c5f-89d4-0fbe6fa0277c",
      "credentials": {
        "telegramApi": {
          "id": "2MP2EllErfqqfJPy",
          "name": "fuelobot"
        }
      }
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "105cfa97-9434-4e99-bef7-071a6dcacff7",
              "name": "chatId",
              "value": "={{ $json.message?.chat?.id || $json.callback_query?.message?.chat?.id }}",
              "type": "number"
            },
            {
              "id": "3638ae47-0671-48c3-9b23-39a42320a6db",
              "name": "messageId",
              "value": "={{ $json.message?.message_id || $json.callback_query?.message?.message_id }}",
              "type": "number"
            },
            {
              "id": "7a12962a-5084-4b2e-b18b-c48f6bdee19a",
              "name": "text",
              "value": "=={{ $json.message?.text || $json.callback_query?.data || \"\" }}",
              "type": "string"
            },
            {
              "id": "e22aab0b-288b-4d84-bdd1-219697aa612f",
              "name": "hasPhoto",
              "value": "={{ !!$json.message?.photo?.length }}",
              "type": "boolean"
            },
            {
              "id": "a68f6407-5fce-4b4f-a3fb-5e387683bf0c",
              "name": "hasDocument",
              "value": "={{ !!$json.message?.document }}",
              "type": "boolean"
            },
            {
              "id": "d1cbc704-ad74-4347-8ae0-3a5d30be57e8",
              "name": "photoFileId",
              "value": "={{ $json.message?.photo ? $json.message.photo[$json.message.photo.length-1].file_id : \"\" }}",
              "type": "string"
            },
            {
              "id": "ca4d320a-b2f9-472c-985a-47ba604427e5",
              "name": "documentFileId",
              "value": "={{ $json.message?.document?.file_id || \"\" }}",
              "type": "string"
            },
            {
              "id": "73b8de19-e104-437c-9e00-09d181c6a90d",
              "name": "isCallback",
              "value": "={{ !!$json.callback_query }}",
              "type": "boolean"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        -720,
        524
      ],
      "id": "c52906a1-da51-48a2-9061-b4b635904f25",
      "name": "Edit Fields"
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 3
          },
          "conditions": [
            {
              "id": "fec60d44-f6eb-4460-b620-c028a3a03f84",
              "leftValue": "={{ $json.hasPhoto }}",
              "rightValue": "true",
              "operator": {
                "type": "boolean",
                "operation": "true",
                "singleValue": true
              }
            },
            {
              "id": "9b72ff27-0005-4bd3-8ff4-9c71968c3f65",
              "leftValue": "={{ $json.hasDocument }}",
              "rightValue": "true",
              "operator": {
                "type": "boolean",
                "operation": "true",
                "singleValue": true
              }
            }
          ],
          "combinator": "or"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.3,
      "position": [
        -496,
        524
      ],
      "id": "2fc3bd38-638b-40eb-88b2-0546aa501bbd",
      "name": "IF - Is receipt upload?"
    },
    {
      "parameters": {
        "resource": "file",
        "fileId": "={{ $json.photoFileId }}",
        "additionalFields": {}
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        -272,
        12
      ],
      "id": "7d704254-4784-4768-a0a6-80b77a6d99de",
      "name": "Get a file",
      "webhookId": "8c721380-a368-4310-92c0-d2802b72337e",
      "credentials": {
        "telegramApi": {
          "id": "2MP2EllErfqqfJPy",
          "name": "fuelobot"
        }
      }
    },
    {
      "parameters": {
        "name": "={{ \"fuel_\" + $now.toFormat(\"yyyyMMdd_HHmmss\") + \".jpg\" }}",
        "driveId": {
          "__rl": true,
          "value": "My Drive",
          "mode": "list",
          "cachedResultName": "My Drive",
          "cachedResultUrl": "https://drive.google.com/drive/my-drive"
        },
        "folderId": {
          "__rl": true,
          "value": "1fM-I0W7pn2NZCDyj2qhek-64PxFZQ343",
          "mode": "list",
          "cachedResultName": "Fuel Receipts (Uber)",
          "cachedResultUrl": "https://drive.google.com/drive/folders/1fM-I0W7pn2NZCDyj2qhek-64PxFZQ343"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleDrive",
      "typeVersion": 3,
      "position": [
        304,
        -112
      ],
      "id": "6336df56-d708-4087-af43-0cb507924e69",
      "name": "Upload file",
      "credentials": {
        "googleDriveOAuth2Api": {
          "id": "GLg1Y5F85V7mDROq",
          "name": "Google Drive account"
        }
      }
    },
    {
      "parameters": {
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatGoogleGemini",
      "typeVersion": 1,
      "position": [
        24,
        360
      ],
      "id": "380bcd87-54b4-4075-a0f0-c4a2f1f01802",
      "name": "Google Gemini Chat Model",
      "credentials": {
        "googlePalmApi": {
          "id": "WgtvdxYIF5UGXmxL",
          "name": "Google Gemini(PaLM) Api account"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "// 1) Parse AI Agent JSON string from $json.output\nconst parsed = JSON.parse($json.output);\n\n// 2) Normalize receipt_date to DD/MM/YYYY and create receipt_date_iso\nfunction toIsoFromAny(s) {\n  if (!s) return \"\";\n  const str = String(s).trim();\n\n  // YYYY-MM-DD\n  let m = str.match(/^(\\d{4})-(\\d{2})-(\\d{2})$/);\n  if (m) return `${m[1]}-${m[2]}-${m[3]}`;\n\n  // DD/MM/YYYY or DD-MM-YYYY\n  m = str.match(/^(\\d{2})[\\/-](\\d{2})[\\/-](\\d{4})$/);\n  if (m) return `${m[3]}-${m[2]}-${m[1]}`;\n\n  return \"\";\n}\n\nfunction toDmyFromIso(iso) {\n  if (!iso) return \"\";\n  const m = iso.match(/^(\\d{4})-(\\d{2})-(\\d{2})$/);\n  if (!m) return \"\";\n  return `${m[3]}/${m[2]}/${m[1]}`;\n}\n\nconst iso = toIsoFromAny(parsed.receipt_date);\nif (iso) {\n  parsed.receipt_date_iso = iso;\n  parsed.receipt_date = toDmyFromIso(iso);\n}\n\n// 3) Normalize notes null -> \"\"\nif (parsed.notes === null || parsed.notes === undefined) parsed.notes = \"\";\n\n// 4) Merge parsed fields into main json\nreturn [{ json: { ...$json, ...parsed } }];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        304,
        136
      ],
      "id": "6bb9d7d2-e3de-4240-ab25-c5e2cc0fea98",
      "name": "Code in JavaScript"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "d9be787d-1aeb-4015-b7e6-74a011362037",
              "name": "confirmText",
              "value": "={{ \n\"✅ I captured this fuel expense:\\n\\n\" +\n\"Date: \" +$('Merge').item.json.receipt_date + \"\\n\" +\n\"Vendor: \" +$('Merge').item.json.vendor+ \"\\n\" +\n\"Total: \" + $('Merge').item.json.currency + \" \" + $('Merge').item.json.total_amount + \"\\n\" +\n\"GST included: \" + ($('Merge').item.json.gst_included ? \"YES\" : \"NO\") + \"\\n\" +\n\"GST amount: \" + $('Merge').item.json.gst_amount + \"\\n\" +\n\"Liters: \" + $('Merge').item.json.liters + \"\\n\\n\" +\n\"Reply with:\\napprove\\ncancel\"\n}}",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        1040,
        12
      ],
      "id": "7debdf26-9046-43ba-84f5-853b2d266315",
      "name": "Edit Fields1"
    },
    {
      "parameters": {
        "chatId": "={{ $('Telegram Trigger').item.json.message.chat.id }}",
        "text": "={{ $json.confirmText }}",
        "additionalFields": {
          "appendAttribution": false
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        1328,
        12
      ],
      "id": "7a3e0f15-5856-4d82-af54-ac6c6c3bfd32",
      "name": "Send a text message",
      "webhookId": "5c447839-9f57-4eb2-8132-f15587fff990",
      "credentials": {
        "telegramApi": {
          "id": "2MP2EllErfqqfJPy",
          "name": "fuelobot"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "const raw = String($json.output || \"\").trim();\n\n// find JSON object boundaries\nconst start = raw.indexOf(\"{\");\nconst end = raw.lastIndexOf(\"}\");\n\nif (start === -1 || end === -1 || end <= start) {\n  throw new Error(\"No JSON object found in AI output: \" + raw);\n}\n\nconst jsonStr = raw.slice(start, end + 1);\nconst parsed = JSON.parse(jsonStr);\n\nreturn [{ json: { ...$json, ...parsed } }];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        1328,
        1156
      ],
      "id": "457fd1be-ba1c-4a90-bd6a-8c8f1471c38f",
      "name": "Code in JavaScript1"
    },
    {
      "parameters": {
        "resource": "table",
        "operation": "create",
        "tableName": "fuel_pending",
        "columns": {
          "column": [
            {
              "name": "key"
            },
            {
              "name": "payload"
            },
            {
              "name": "created_at"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.dataTable",
      "typeVersion": 1.1,
      "position": [
        -944,
        -336
      ],
      "id": "583d2c6b-8fb4-4677-b742-4d3a35685def",
      "name": "Create a data table"
    },
    {
      "parameters": {
        "operation": "upsert",
        "dataTableId": {
          "__rl": true,
          "value": "61o63rJme621Gytr",
          "mode": "list",
          "cachedResultName": "fuel_pending",
          "cachedResultUrl": "/projects/JGj39VtKBIK4QfYh/datatables/61o63rJme621Gytr"
        },
        "filters": {
          "conditions": [
            {
              "keyName": "key",
              "keyValue": "pending_fuel"
            }
          ]
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "key": "pending_fuel",
            "payload": "={{ JSON.stringify({\n  receipt_date: $json.receipt_date,\n  receipt_date_iso: $json.receipt_date_iso,\n  vendor: $json.vendor,\n  total_amount: $json.total_amount,\n  currency: $json.currency,\n  gst_included: $json.gst_included,\n  gst_amount: $json.gst_amount,\n  liters: $json.liters,\n  notes: $json.notes,\n\n  drive_file_id: $json.id,\n  drive_file_url: $json.webViewLink,\n\n  telegram_chat_id: $json.chatId,\n  telegram_message_id: $json.messageId,\n\nstatus: \"AWAITING_CONFIRMATION\",\nmanual_step: \"\",\nlanguage: \"\"\n}) }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "key",
              "displayName": "key",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "readOnly": false,
              "removed": false
            },
            {
              "id": "payload",
              "displayName": "payload",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "readOnly": false,
              "removed": false
            },
            {
              "id": "created_at",
              "displayName": "created_at",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "readOnly": false,
              "removed": false
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.dataTable",
      "typeVersion": 1.1,
      "position": [
        752,
        12
      ],
      "id": "e8d74146-5773-4b49-8dcc-577a7b6625e1",
      "name": "Upsert row(s)"
    },
    {
      "parameters": {
        "options": {}
      },
      "type": "@n8n/n8n-nodes-langchain.lmChatGoogleGemini",
      "typeVersion": 1,
      "position": [
        1048,
        1380
      ],
      "id": "1d382580-cefc-43bc-85fa-9efe36fb0707",
      "name": "Google Gemini Chat Model1",
      "credentials": {
        "googlePalmApi": {
          "id": "WgtvdxYIF5UGXmxL",
          "name": "Google Gemini(PaLM) Api account"
        }
      }
    },
    {
      "parameters": {
        "operation": "get",
        "dataTableId": {
          "__rl": true,
          "value": "61o63rJme621Gytr",
          "mode": "list",
          "cachedResultName": "fuel_pending",
          "cachedResultUrl": "/projects/JGj39VtKBIK4QfYh/datatables/61o63rJme621Gytr"
        },
        "filters": {
          "conditions": [
            {
              "keyName": "key",
              "keyValue": "pending_fuel"
            }
          ]
        }
      },
      "type": "n8n-nodes-base.dataTable",
      "typeVersion": 1.1,
      "position": [
        1776,
        676
      ],
      "id": "7cc3a129-0cc4-42f1-a27a-b53af4daf29b",
      "name": "Get row(s)"
    },
    {
      "parameters": {
        "jsCode": "const p = JSON.parse($json.payload);\nreturn [{ json: { ...$json, ...p } }];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        2000,
        676
      ],
      "id": "0d5caf90-b8d3-4f21-add8-ece1db6383d4",
      "name": "Code in JavaScript2"
    },
    {
      "parameters": {
        "operation": "append",
        "documentId": {
          "__rl": true,
          "value": "1eH8JaKNCayAcAGM23kQauC8gkNwFiaUjLoF_xk8i4rw",
          "mode": "list",
          "cachedResultName": "Fuel Expenses",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1eH8JaKNCayAcAGM23kQauC8gkNwFiaUjLoF_xk8i4rw/edit?usp=drivesdk"
        },
        "sheetName": {
          "__rl": true,
          "value": "gid=0",
          "mode": "list",
          "cachedResultName": "Expenses",
          "cachedResultUrl": "https://docs.google.com/spreadsheets/d/1eH8JaKNCayAcAGM23kQauC8gkNwFiaUjLoF_xk8i4rw/edit#gid=0"
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "gst_included": "={{ $json.gst_included }}",
            "gst_amount": "={{ $json.gst_amount }}",
            "notes": "={{ $json.notes }}",
            "telegram_chat_id": "={{ $('Telegram Trigger').item.json.message.chat.id }}",
            "telegram_message_id": "={{ $('Telegram Trigger').item.json.message.message_id }}",
            "currency": "={{ $json.currency }}",
            "total_amount": "={{ $json.total_amount }}",
            "vendor": "={{ $json.vendor }}",
            "receipt_date": "={{ $json.receipt_date }}",
            "timestamp_ingested": "={{ $json.createdAt }}",
            "drive_file_url": "={{ $json.drive_file_url }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "timestamp_ingested",
              "displayName": "timestamp_ingested",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "receipt_date",
              "displayName": "receipt_date",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "vendor",
              "displayName": "vendor",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "total_amount",
              "displayName": "total_amount",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "currency",
              "displayName": "currency",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "gst_included",
              "displayName": "gst_included",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "gst_amount",
              "displayName": "gst_amount",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "notes",
              "displayName": "notes",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "drive_file_url",
              "displayName": "drive_file_url",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "telegram_chat_id",
              "displayName": "telegram_chat_id",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            },
            {
              "id": "telegram_message_id",
              "displayName": "telegram_message_id",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "canBeUsedToMatch": true
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.googleSheets",
      "typeVersion": 4.7,
      "position": [
        2224,
        676
      ],
      "id": "7e11b4c4-d7b4-4e66-8bc2-a59d25492863",
      "name": "Append row in sheet",
      "credentials": {
        "googleSheetsOAuth2Api": {
          "id": "zq1f1ULU75jNT75j",
          "name": "Google Sheets account"
        }
      }
    },
    {
      "parameters": {
        "chatId": "={{ $('Telegram Trigger').item.json.message.chat.id }}",
        "text": "={{ $json.language === \"es\" ? \"Guardado ✅ Tu gasto quedó registrado.\" : \"Saved ✅ Your expense was recorded.\" }}",
        "additionalFields": {
          "appendAttribution": false
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        2448,
        676
      ],
      "id": "ebca42aa-035f-4082-b52e-97313838c21d",
      "name": "Send a text message1",
      "webhookId": "5d6d7c30-ec7b-4d96-8f3e-69c88b88b249",
      "credentials": {
        "telegramApi": {
          "id": "2MP2EllErfqqfJPy",
          "name": "fuelobot"
        }
      }
    },
    {
      "parameters": {
        "mode": "combine",
        "combineBy": "combineAll",
        "options": {}
      },
      "type": "n8n-nodes-base.merge",
      "typeVersion": 3.2,
      "position": [
        528,
        12
      ],
      "id": "dd2568d9-34e8-451d-afff-af9832b7ac7b",
      "name": "Merge"
    },
    {
      "parameters": {
        "operation": "deleteRows",
        "dataTableId": {
          "__rl": true,
          "value": "61o63rJme621Gytr",
          "mode": "list",
          "cachedResultName": "fuel_pending",
          "cachedResultUrl": "/projects/JGj39VtKBIK4QfYh/datatables/61o63rJme621Gytr"
        },
        "filters": {
          "conditions": [
            {
              "keyName": "key",
              "keyValue": "pending_fuel"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.dataTable",
      "typeVersion": 1.1,
      "position": [
        1776,
        868
      ],
      "id": "010b8ef6-f072-4ecd-9a55-d7820218c404",
      "name": "Delete row(s)"
    },
    {
      "parameters": {
        "operation": "get",
        "dataTableId": {
          "__rl": true,
          "value": "61o63rJme621Gytr",
          "mode": "list",
          "cachedResultName": "fuel_pending",
          "cachedResultUrl": "/projects/JGj39VtKBIK4QfYh/datatables/61o63rJme621Gytr"
        },
        "filters": {
          "conditions": [
            {
              "keyName": "key",
              "keyValue": "pending_fuel"
            }
          ]
        }
      },
      "type": "n8n-nodes-base.dataTable",
      "typeVersion": 1.1,
      "position": [
        16,
        844
      ],
      "id": "c113cc64-fa4a-4203-9cd5-8dbe761991b3",
      "name": "Get row(s)1",
      "alwaysOutputData": true
    },
    {
      "parameters": {
        "jsCode": "// Preserve user text\nconst text = $json.user_text ?? $json.text ?? \"\";\n\n// If no pending row was found, payload will be undefined\nif (!$json.payload) {\n  return [{ json: { ...$json, text, pending: null } }];\n}\n\nconst raw = String($json.payload).trim();\nconst start = raw.indexOf(\"{\");\nconst end = raw.lastIndexOf(\"}\");\n\nif (start === -1 || end === -1 || end <= start) {\n  return [{ json: { ...$json, text, pending: null } }];\n}\n\nconst pending = JSON.parse(raw.slice(start, end + 1));\nreturn [{ json: { ...$json, text, pending } }];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        528,
        772
      ],
      "id": "c0a0f6b9-78b0-4baa-a995-87cfbd34d1af",
      "name": "Code in JavaScript3"
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 3
          },
          "conditions": [
            {
              "id": "83bb888f-2417-4f87-93cf-caff9db8806b",
              "leftValue": "={{ $json.pending?.status }}",
              "rightValue": "MANUAL_ENTRY",
              "operator": {
                "type": "string",
                "operation": "equals"
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.3,
      "position": [
        752,
        772
      ],
      "id": "0401dc00-5a65-4313-8555-a1a5361f7a1d",
      "name": "If1"
    },
    {
      "parameters": {
        "mode": "combine",
        "combineBy": "combineByPosition",
        "options": {}
      },
      "type": "n8n-nodes-base.merge",
      "typeVersion": 3.2,
      "position": [
        304,
        772
      ],
      "id": "42e90451-d291-496a-86cd-8996757a6d91",
      "name": "Merge1"
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "Return ONLY JSON. Do NOT write the word \"json\". Do NOT use code fences.\n\nOutput must be a single JSON object with exactly these keys:\nreceipt_date, vendor, total_amount, currency, gst_included, gst_amount, liters, notes\n\nIf you are unsure about a value, use:\n- \"\" for receipt_date/vendor/notes\n- 0 for total_amount/gst_amount/liters\n- false for gst_included",
        "options": {
          "passthroughBinaryImages": true
        }
      },
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 3.1,
      "position": [
        -48,
        136
      ],
      "id": "47ad8da5-f762-4fa9-854f-bf8f30420715",
      "name": "AI Agent (files)"
    },
    {
      "parameters": {
        "promptType": "define",
        "text": "={{ $json.text }}",
        "options": {
          "systemMessage": "You are a Telegram bookkeeping assistant for logging fuel receipts.\n\nYou will classify the user's message into one intent and detect language.\n\nReturn ONLY valid JSON with exactly these keys:\nlanguage, intent, reply\n\nlanguage: \"en\" or \"es\"\nintent: \"approve\" | \"cancel\" | \"manual\" | \"greeting\" | \"goodbye\" | \"out_of_scope\"\nIMPORTANT:\n- If the user says: \"mal\", \"incorrecto\", \"wrong\", \"incorrect\", \"fix\", \"arregla\", \"corrige\", \"corregir\", \"edit\", \"editar\"\n  then intent MUST be \"manual\" (they want to correct fields), NOT \"cancel\".\n- intent \"cancel\" is only when the user clearly wants to discard: \"cancelar\", \"rechazar\", \"no\", \"no guardar\", \"olvida\", \"stop\", \"discard\".\n\nRules:\n- Approve examples:\n  EN: yes, yep, ok, okay, approve, confirm, correct, sounds good, good\n  ES: si, sí, dale, listo, ok, está bien, correcto, perfecto, bueno\n- Cancel examples:\n  EN: no, nope, cancel, reject, stop, wrong, incorrect, bad\n  ES: no, cancelar, rechazar, está mal, incorrecto, mal, no gracias\n- Greeting examples:\n  EN: hi, hello, hey\n  ES: hola, buenas, hey\n- Goodbye examples:\n  EN: bye, goodbye, thanks, thank you\n  ES: chao, adiós, gracias, nos vemos\n\n- If the user asks anything outside fuel receipts, use intent \"out_of_scope\".\n- If the user says something like \"manual\", \"enter manually\", \"hagámoslo manual\", \"no sé\", or if they seem confused, use intent \"manual\".\n\nReply requirements:\n- reply must be in the SAME language as the user.\n- greeting reply: greet and tell them to send a receipt photo.\n- goodbye reply: say goodbye politely.\n- out_of_scope reply: explain you can only log fuel receipts, tell them to send a receipt photo.\n- manual reply: say you’ll collect the fields manually and ask for the date (DD/MM/YYYY).\n- approve/cancel reply can be short confirmation.\n\nReturn JSON only. No markdown. No extra text."
        }
      },
      "type": "@n8n/n8n-nodes-langchain.agent",
      "typeVersion": 3.1,
      "position": [
        976,
        1156
      ],
      "id": "494e6347-dae2-47ea-af91-f7805bb69599",
      "name": "AI Agent (text)"
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "86f3bdd7-72ab-4fbb-8888-cfd9ebf55fd3",
              "name": "user_text",
              "value": "={{ $json.text }}",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        -272,
        772
      ],
      "id": "ce94c83d-a500-4b3e-904e-5974651d14ea",
      "name": "Edit Fields2"
    },
    {
      "parameters": {
        "chatId": "={{ $('Telegram Trigger').item.json.message.chat.id }}",
        "text": "={{ $json.reply }}",
        "additionalFields": {
          "appendAttribution": false
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        1776,
        1252
      ],
      "id": "39979228-4246-46d5-bafe-06a356dde396",
      "name": "Send a text message2",
      "webhookId": "5d6d7c30-ec7b-4d96-8f3e-69c88b88b249",
      "credentials": {
        "telegramApi": {
          "id": "2MP2EllErfqqfJPy",
          "name": "fuelobot"
        }
      }
    },
    {
      "parameters": {
        "rules": {
          "values": [
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "leftValue": "={{ $json.intent }}",
                    "rightValue": "approve",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    },
                    "id": "fd7205b9-2d00-4374-a0aa-80598a463748"
                  }
                ],
                "combinator": "and"
              }
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "id": "af550afc-968d-457c-b722-51d00cec0c16",
                    "leftValue": "={{ $json.intent }}",
                    "rightValue": "cancel",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              }
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "id": "cb6c0bb1-797c-467d-9499-a0251b40bca2",
                    "leftValue": "={{ $json.intent }}",
                    "rightValue": "manual",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              }
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "id": "854e033f-3021-4294-8d87-23c0236a54d5",
                    "leftValue": "={{ $json.intent }}",
                    "rightValue": "greeting",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              }
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "id": "906a6271-196e-443b-a6f8-c9741e0fb5a2",
                    "leftValue": "={{ $json.intent }}",
                    "rightValue": "goodbye",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              }
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "id": "1782100e-aaa7-4cdf-b23d-f59909ec2b6a",
                    "leftValue": "={{ $json.intent }}",
                    "rightValue": "out_of_scope",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              }
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.switch",
      "typeVersion": 3.4,
      "position": [
        1552,
        1092
      ],
      "id": "26532f3e-d98b-41a3-9638-c017fa546da0",
      "name": "Switch"
    },
    {
      "parameters": {
        "chatId": "={{ $('Telegram Trigger').item.json.message.chat.id }}",
        "text": "={{ $json.reply }}",
        "additionalFields": {
          "appendAttribution": false
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        1776,
        1444
      ],
      "id": "f0a4a252-e024-4433-8362-3984c29d9a2e",
      "name": "Send a text message3",
      "webhookId": "5d6d7c30-ec7b-4d96-8f3e-69c88b88b249",
      "credentials": {
        "telegramApi": {
          "id": "2MP2EllErfqqfJPy",
          "name": "fuelobot"
        }
      }
    },
    {
      "parameters": {
        "chatId": "={{ $('Telegram Trigger').item.json.message.chat.id }}",
        "text": "={{ $json.reply }}",
        "additionalFields": {
          "appendAttribution": false
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        1776,
        1636
      ],
      "id": "911c6aba-a734-4fc7-8c23-446775b6fd45",
      "name": "Send a text message4",
      "webhookId": "5d6d7c30-ec7b-4d96-8f3e-69c88b88b249",
      "credentials": {
        "telegramApi": {
          "id": "2MP2EllErfqqfJPy",
          "name": "fuelobot"
        }
      }
    },
    {
      "parameters": {
        "rules": {
          "values": [
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "leftValue": "={{ $json.pending.manual_step }}",
                    "rightValue": "DATE",
                    "operator": {
                      "type": "string",
                      "operation": "equals"
                    },
                    "id": "29bac046-dc8e-48e3-9439-f647039f5a76"
                  }
                ],
                "combinator": "and"
              }
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "id": "d7345c4a-4dff-4e8d-b8b4-b96f15f58288",
                    "leftValue": "={{ $json.pending.manual_step }}",
                    "rightValue": "TOTAL",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              }
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "id": "df99ba4d-ff1e-4631-9a9a-199c0be79f35",
                    "leftValue": "={{ $json.pending.manual_step }}",
                    "rightValue": "GST_INCLUDED",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              }
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "id": "ad6972af-2f3f-4442-b5e1-a54fa83dd742",
                    "leftValue": "={{ $json.pending.manual_step }}",
                    "rightValue": "GST_AMOUNT",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              }
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "id": "c872c2c6-1f16-4b79-85d6-87694cfa7501",
                    "leftValue": "={{ $json.pending.manual_step }}",
                    "rightValue": "LITERS",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              }
            },
            {
              "conditions": {
                "options": {
                  "caseSensitive": true,
                  "leftValue": "",
                  "typeValidation": "strict",
                  "version": 3
                },
                "conditions": [
                  {
                    "id": "67a75edd-86f3-4090-a84d-7ce928b1fc14",
                    "leftValue": "={{ $json.pending.manual_step }}",
                    "rightValue": "FORM",
                    "operator": {
                      "type": "string",
                      "operation": "equals",
                      "name": "filter.operator.equals"
                    }
                  }
                ],
                "combinator": "and"
              }
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.switch",
      "typeVersion": 3.4,
      "position": [
        1040,
        324
      ],
      "id": "28e8f85b-bf25-4ed5-9f6a-d34b799d8f2e",
      "name": "Switch1"
    },
    {
      "parameters": {
        "operation": "get",
        "dataTableId": {
          "__rl": true,
          "value": "61o63rJme621Gytr",
          "mode": "list",
          "cachedResultName": "fuel_pending",
          "cachedResultUrl": "/projects/JGj39VtKBIK4QfYh/datatables/61o63rJme621Gytr"
        },
        "filters": {
          "conditions": [
            {
              "keyName": "key",
              "keyValue": "pending_fuel"
            }
          ]
        }
      },
      "type": "n8n-nodes-base.dataTable",
      "typeVersion": 1.1,
      "position": [
        1776,
        1060
      ],
      "id": "56eaa22b-2e44-42ea-9453-c9bb098d18ce",
      "name": "Get row(s)2",
      "alwaysOutputData": true
    },
    {
      "parameters": {
        "jsCode": "// If there's no pending row, payload will be missing (empty item)\nconst hasPending = !!$json.payload;\n\nreturn [{\n  json: {\n    ...$json,\n    hasPending,\n  }\n}];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        2000,
        1060
      ],
      "id": "027b9f93-d76c-49ff-b7ee-370bac7cd921",
      "name": "Code in JavaScript4"
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 3
          },
          "conditions": [
            {
              "id": "5ec37106-3200-476b-a2a9-05d640b7c37e",
              "leftValue": "={{ $json.hasPending }}",
              "rightValue": "",
              "operator": {
                "type": "boolean",
                "operation": "true",
                "singleValue": true
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.3,
      "position": [
        2224,
        1060
      ],
      "id": "7753db1f-f049-41da-a2f8-74099ef3b10d",
      "name": "If"
    },
    {
      "parameters": {
        "chatId": "={{ $('Telegram Trigger').item.json.message.chat.id }}",
        "text": "={{ $json.language === \"es\"\n  ? \"No tengo ningún recibo pendiente. Envíame una foto del recibo primero.\"\n  : \"I don’t have any pending receipt. Please send a receipt photo first.\"\n}}",
        "additionalFields": {
          "appendAttribution": false
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        2448,
        1156
      ],
      "id": "eab6ce8c-9743-402f-b30f-4b93fcb8c40a",
      "name": "Send a text message5",
      "webhookId": "bb9e176e-0643-4280-a4ee-5ca98a398873",
      "credentials": {
        "telegramApi": {
          "id": "2MP2EllErfqqfJPy",
          "name": "fuelobot"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "// Requires: $json.payload (pending row) + $json.language from router\nconst p = JSON.parse($json.payload);\n\n// start one-message manual correction\np.status = \"MANUAL_ENTRY\";\np.manual_step = \"FORM\";\np.language = $json.language || p.language || \"\";\n\nreturn [{ json: { ...$json, pending: p } }];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        2448,
        964
      ],
      "id": "17638678-67e9-46aa-bfc0-ccf6e942cebf",
      "name": "Code in JavaScript5"
    },
    {
      "parameters": {
        "operation": "upsert",
        "dataTableId": {
          "__rl": true,
          "value": "61o63rJme621Gytr",
          "mode": "list",
          "cachedResultName": "fuel_pending",
          "cachedResultUrl": "/projects/JGj39VtKBIK4QfYh/datatables/61o63rJme621Gytr"
        },
        "filters": {
          "conditions": [
            {
              "keyName": "key",
              "keyValue": "pending_fuel"
            }
          ]
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "key": "pending_fuel",
            "payload": "={{ JSON.stringify($json.pending) }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "key",
              "displayName": "key",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "readOnly": false,
              "removed": false
            },
            {
              "id": "payload",
              "displayName": "payload",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "readOnly": false,
              "removed": false
            },
            {
              "id": "created_at",
              "displayName": "created_at",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "readOnly": false,
              "removed": false
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.dataTable",
      "typeVersion": 1.1,
      "position": [
        2672,
        964
      ],
      "id": "180cf0c5-59f4-41b0-b83f-6d31499b9962",
      "name": "Upsert row(s)1"
    },
    {
      "parameters": {
        "chatId": "={{ $('Telegram Trigger').item.json.message.chat.id }}",
        "text": "={{ ($json.language === \"es\" || $json.pending?.language === \"es\")\n? \n\"Ok. Corrijámoslo en un solo mensaje. Copia y pega esto y rellena:\\n\\n\" +\n\"DATE (DD/MM/YYYY): \\n\" +\n\"TOTAL: \\n\" +\n\"GST INCLUDED (si/no): \\n\" +\n\"GST AMOUNT: \\n\" +\n\"LITERS (o skip): \\n\" +\n\"VENDOR (opcional): \\n\\n\" +\n\"Ejemplo:\\nDATE: 16/01/2026\\nTOTAL: 37.87\\nGST INCLUDED: si\\nGST AMOUNT: 3.44\\nLITERS: 24.29\\nVENDOR: 7-ELEVEN\\n\\n\" +\n\"Envíalo en un solo mensaje.\"\n:\n\"Ok. Let’s correct it in one message. Copy/paste and fill:\\n\\n\" +\n\"DATE (DD/MM/YYYY): \\n\" +\n\"TOTAL: \\n\" +\n\"GST INCLUDED (yes/no): \\n\" +\n\"GST AMOUNT: \\n\" +\n\"LITERS (or skip): \\n\" +\n\"VENDOR (optional): \\n\\n\" +\n\"Example:\\nDATE: 16/01/2026\\nTOTAL: 37.87\\nGST INCLUDED: yes\\nGST AMOUNT: 3.44\\nLITERS: 24.29\\nVENDOR: 7-ELEVEN\\n\\n\" +\n\"Send it as a single message.\"\n}}",
        "additionalFields": {
          "appendAttribution": false
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        2896,
        964
      ],
      "id": "9f9428ef-b5a8-466f-b8c9-fbe6c54542a6",
      "name": "Send a text message6",
      "webhookId": "1e03f101-2692-4fc7-b99a-c0d18d35cd28",
      "credentials": {
        "telegramApi": {
          "id": "2MP2EllErfqqfJPy",
          "name": "fuelobot"
        }
      }
    },
    {
      "parameters": {
        "jsCode": "// Manual form parser for one-message correction\n// Expects lines like:\n// DATE: 16/01/2026\n// TOTAL: 37.87\n// GST INCLUDED: si/no (or yes/no)\n// GST AMOUNT: 3.44\n// LITERS: 24.29 (or skip)\n// VENDOR: 7-ELEVEN (optional)\n\nconst p = $json.pending;\nlet msg = String($json.text ?? $json.user_text ?? \"\").trim();\n// Telegram/n8n sometimes gives leading \"=\" (like spreadsheet formula style). Remove it.\nmsg = msg.replace(/^\\s*=+/, \"\");\n\nif (!msg) {\n  return [{\n    json: {\n      ...$json,\n      form_error: \"NO_TEXT\",\n      debug_fields: Object.keys($json),\n    }\n  }];\n}\n\nfunction getField(label) {\n  const re = new RegExp(`^\\\\s*${label}[^:]*:\\\\s*(.*)\\\\s*$`, \"im\");\n  const m = msg.match(re);\n  return m ? m[1].trim() : \"\";\n}\n\nconst dateStr = getField(\"DATE\");\nif (!dateStr) {\n  return [{ json: { ...$json, form_error: \"DATE\", debug_msg: msg } }];\n}\nconst totalStr = getField(\"TOTAL\");\nconst gstIncStr = getField(\"GST INCLUDED\");\nconst gstAmtStr = getField(\"GST AMOUNT\");\nconst litersStr = getField(\"LITERS\");\nconst vendorStr = getField(\"VENDOR\");\n\n// Validate date DD/MM/YYYY\nconst dm = dateStr.match(/^(\\d{2})\\/(\\d{2})\\/(\\d{4})$/);\nif (!dm) return [{ json: { ...$json, form_error: \"DATE\" } }];\n\n// Validate total\nconst total = Number(totalStr.replace(\",\", \".\"));\nif (!Number.isFinite(total) || total <= 0) return [{ json: { ...$json, form_error: \"TOTAL\" } }];\n\n// GST included\nlet gstIncluded = null;\nif (gstIncStr) {\n  const t = gstIncStr.toLowerCase();\n  if ([\"yes\",\"y\",\"si\",\"sí\"].some(w => t.includes(w))) gstIncluded = true;\n  if ([\"no\",\"n\"].some(w => t.includes(w))) gstIncluded = false;\n}\nif (gstIncluded === null) return [{ json: { ...$json, form_error: \"GST_INCLUDED\" } }];\n\n// GST amount\nlet gstAmount = 0;\nif (gstIncluded) {\n  gstAmount = Number((gstAmtStr || \"\").replace(\",\", \".\"));\n  if (!Number.isFinite(gstAmount) || gstAmount < 0) return [{ json: { ...$json, form_error: \"GST_AMOUNT\" } }];\n}\n\n// liters\n// LITERS (optional)\nlet liters = 0;\n\n// normalize: remove invisible chars, keep only digits, dot, comma, minus\nconst rawLiters = String(litersStr ?? \"\")\n  .replace(/[\\u200B-\\u200D\\uFEFF]/g, \"\")   // zero-width chars\n  .replace(/\\u00A0/g, \" \")                // non-breaking space\n  .trim()\n  .toLowerCase();\n\nif (rawLiters === \"\" || rawLiters === \"skip\") {\n  liters = 0;\n} else {\n  // keep only number-friendly chars\n  const numeric = rawLiters.replace(/[^\\d,.\\-]/g, \"\").trim();\n\n  if (numeric === \"\") {\n    liters = 0; // treat “weird text” as blank\n  } else {\n    const l = Number(numeric.replace(\",\", \".\"));\n    if (!Number.isFinite(l) || l < 0) {\n      return [{ json: { ...$json, form_error: \"LITERS\", debug_liters: litersStr } }];\n    }\n    liters = l;\n  }\n}\n\n// Update pending\np.receipt_date = dateStr;\np.receipt_date_iso = `${dm[3]}-${dm[2]}-${dm[1]}`;\np.total_amount = total;\np.gst_included = gstIncluded;\np.gst_amount = gstAmount;\np.liters = liters;\nif (vendorStr) p.vendor = vendorStr;\n\n// Exit manual mode back to confirmation\np.status = \"AWAITING_CONFIRMATION\";\np.manual_step = \"\";\n\nreturn [{ json: { ...$json, pending: p } }];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        1328,
        388
      ],
      "id": "9bd39297-0774-436b-b89f-64b825b81b85",
      "name": "Code in JavaScript6"
    },
    {
      "parameters": {
        "conditions": {
          "options": {
            "caseSensitive": true,
            "leftValue": "",
            "typeValidation": "strict",
            "version": 3
          },
          "conditions": [
            {
              "id": "a6572290-b290-4051-a7ec-b0304c55a571",
              "leftValue": "={{ $json.form_error }}",
              "rightValue": "",
              "operator": {
                "type": "string",
                "operation": "exists",
                "singleValue": true
              }
            }
          ],
          "combinator": "and"
        },
        "options": {}
      },
      "type": "n8n-nodes-base.if",
      "typeVersion": 2.3,
      "position": [
        1552,
        388
      ],
      "id": "37ea71ea-ffed-49a6-a448-78a0d1284e86",
      "name": "If3"
    },
    {
      "parameters": {
        "chatId": "={{ $('Telegram Trigger').item.json.message.chat.id }}",
        "text": "={{ ($json.pending.language === \"es\")  ? (\"Falta o está mal el campo: \" + $json.form_error + \". Reenvía el formulario completo corrigiendo ese campo.\")  : (\"Missing/invalid field: \" + $json.form_error + \". Please resend the full form correcting that field.\")}}",
        "additionalFields": {
          "appendAttribution": false
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        1776,
        292
      ],
      "id": "72a7c3c7-6454-47d6-b53e-9038939baff5",
      "name": "Send a text message7",
      "webhookId": "ef468f17-d6bc-43f8-bd09-9d3f16db71a1",
      "credentials": {
        "telegramApi": {
          "id": "2MP2EllErfqqfJPy",
          "name": "fuelobot"
        }
      }
    },
    {
      "parameters": {
        "operation": "upsert",
        "dataTableId": {
          "__rl": true,
          "value": "61o63rJme621Gytr",
          "mode": "list",
          "cachedResultName": "fuel_pending",
          "cachedResultUrl": "/projects/JGj39VtKBIK4QfYh/datatables/61o63rJme621Gytr"
        },
        "filters": {
          "conditions": [
            {
              "keyName": "key",
              "keyValue": "pending_fuel"
            }
          ]
        },
        "columns": {
          "mappingMode": "defineBelow",
          "value": {
            "key": "pending_fuel",
            "payload": "={{ JSON.stringify($json.pending) }}"
          },
          "matchingColumns": [],
          "schema": [
            {
              "id": "key",
              "displayName": "key",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "readOnly": false,
              "removed": false
            },
            {
              "id": "payload",
              "displayName": "payload",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "readOnly": false,
              "removed": false
            },
            {
              "id": "created_at",
              "displayName": "created_at",
              "required": false,
              "defaultMatch": false,
              "display": true,
              "type": "string",
              "readOnly": false,
              "removed": false
            }
          ],
          "attemptToConvertTypes": false,
          "convertFieldsToString": false
        },
        "options": {}
      },
      "type": "n8n-nodes-base.dataTable",
      "typeVersion": 1.1,
      "position": [
        1776,
        484
      ],
      "id": "460723dc-3bdb-41d3-b8b0-a79e7d342c47",
      "name": "Upsert row(s)2"
    },
    {
      "parameters": {
        "chatId": "={{ $('Telegram Trigger').item.json.message.chat.id }}",
        "text": "={{ $json.confirmText }}",
        "additionalFields": {
          "appendAttribution": false
        }
      },
      "type": "n8n-nodes-base.telegram",
      "typeVersion": 1.2,
      "position": [
        2448,
        484
      ],
      "id": "a750bcb5-3e99-4ad8-8e4e-896712d427ca",
      "name": "Send a text message8",
      "webhookId": "5d6d7c30-ec7b-4d96-8f3e-69c88b88b249",
      "credentials": {
        "telegramApi": {
          "id": "2MP2EllErfqqfJPy",
          "name": "fuelobot"
        }
      }
    },
    {
      "parameters": {
        "assignments": {
          "assignments": [
            {
              "id": "eee4c8a2-658b-4c8a-ac6c-53dd1eefba58",
              "name": "=confirmText",
              "value": "={{ \n\"✅ I captured this fuel expense:\\n\\n\" +\n\"Date: \" + $('Code in JavaScript6').item.json.pending.receipt_date+ \"\\n\" +\n\"Vendor: \" + $json.pending.vendor + \"\\n\" +\n\"Total: \" + $json.pending.currency + \" \" + $json.pending.total_amount + \"\\n\" +\n\"GST included: \" + ($json.pending.gst_included ? \"YES\" : \"NO\") + \"\\n\" +\n\"GST amount: \" + $json.pending.gst_amount + \"\\n\" +\n\"Liters: \" + $json.pending.liters + \"\\n\\n\" +\n\"Drive link: \" + ($json.pending.drive_file_url || \"\") + \"\\n\\n\" +\n\"Reply with:\\napprove\\ncancel\"\n}}",
              "type": "string"
            }
          ]
        },
        "options": {}
      },
      "type": "n8n-nodes-base.set",
      "typeVersion": 3.4,
      "position": [
        2224,
        484
      ],
      "id": "08e2bb3b-f49b-4516-b23a-f74f4965a621",
      "name": "Edit Fields3"
    },
    {
      "parameters": {
        "jsCode": "const raw = String($json.payload || \"\").trim();\nconst start = raw.indexOf(\"{\");\nconst end = raw.lastIndexOf(\"}\");\n\nif (start === -1 || end === -1 || end <= start) {\n  throw new Error(\"Upsert payload is not JSON: \" + raw);\n}\n\nconst pending = JSON.parse(raw.slice(start, end + 1));\nreturn [{ json: { ...$json, pending } }];"
      },
      "type": "n8n-nodes-base.code",
      "typeVersion": 2,
      "position": [
        2000,
        484
      ],
      "id": "7783c2a3-2df0-4be7-a81b-46dee63bc604",
      "name": "Code in JavaScript7"
    }
  ],
  "pinData": {},
  "connections": {
    "Telegram Trigger": {
      "main": [
        [
          {
            "node": "Edit Fields",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Edit Fields": {
      "main": [
        [
          {
            "node": "IF - Is receipt upload?",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "IF - Is receipt upload?": {
      "main": [
        [
          {
            "node": "Get a file",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Edit Fields2",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Get a file": {
      "main": [
        [
          {
            "node": "Upload file",
            "type": "main",
            "index": 0
          },
          {
            "node": "AI Agent (files)",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Upload file": {
      "main": [
        [
          {
            "node": "Merge",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Google Gemini Chat Model": {
      "ai_languageModel": [
        [
          {
            "node": "AI Agent (files)",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Code in JavaScript": {
      "main": [
        [
          {
            "node": "Merge",
            "type": "main",
            "index": 1
          }
        ]
      ]
    },
    "Edit Fields1": {
      "main": [
        [
          {
            "node": "Send a text message",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Upsert row(s)": {
      "main": [
        [
          {
            "node": "Edit Fields1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Google Gemini Chat Model1": {
      "ai_languageModel": [
        [
          {
            "node": "AI Agent (text)",
            "type": "ai_languageModel",
            "index": 0
          }
        ]
      ]
    },
    "Code in JavaScript1": {
      "main": [
        [
          {
            "node": "Switch",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Get row(s)": {
      "main": [
        [
          {
            "node": "Code in JavaScript2",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Code in JavaScript2": {
      "main": [
        [
          {
            "node": "Append row in sheet",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Append row in sheet": {
      "main": [
        [
          {
            "node": "Send a text message1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Merge": {
      "main": [
        [
          {
            "node": "Upsert row(s)",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Get row(s)1": {
      "main": [
        [
          {
            "node": "Merge1",
            "type": "main",
            "index": 1
          }
        ]
      ]
    },
    "Code in JavaScript3": {
      "main": [
        [
          {
            "node": "If1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "If1": {
      "main": [
        [
          {
            "node": "Switch1",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "AI Agent (text)",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Merge1": {
      "main": [
        [
          {
            "node": "Code in JavaScript3",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "AI Agent (files)": {
      "main": [
        [
          {
            "node": "Code in JavaScript",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "AI Agent (text)": {
      "main": [
        [
          {
            "node": "Code in JavaScript1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Edit Fields2": {
      "main": [
        [
          {
            "node": "Get row(s)1",
            "type": "main",
            "index": 0
          },
          {
            "node": "Merge1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Switch": {
      "main": [
        [
          {
            "node": "Get row(s)",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Delete row(s)",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Get row(s)2",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Send a text message2",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Send a text message3",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Send a text message4",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Get row(s)2": {
      "main": [
        [
          {
            "node": "Code in JavaScript4",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Code in JavaScript4": {
      "main": [
        [
          {
            "node": "If",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "If": {
      "main": [
        [
          {
            "node": "Code in JavaScript5",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Send a text message5",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Code in JavaScript5": {
      "main": [
        [
          {
            "node": "Upsert row(s)1",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Upsert row(s)1": {
      "main": [
        [
          {
            "node": "Send a text message6",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Switch1": {
      "main": [
        [],
        [],
        [],
        [],
        [],
        [
          {
            "node": "Code in JavaScript6",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Code in JavaScript6": {
      "main": [
        [
          {
            "node": "If3",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "If3": {
      "main": [
        [
          {
            "node": "Send a text message7",
            "type": "main",
            "index": 0
          }
        ],
        [
          {
            "node": "Upsert row(s)2",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Send a text message7": {
      "main": [
        []
      ]
    },
    "Upsert row(s)2": {
      "main": [
        [
          {
            "node": "Code in JavaScript7",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Edit Fields3": {
      "main": [
        [
          {
            "node": "Send a text message8",
            "type": "main",
            "index": 0
          }
        ]
      ]
    },
    "Code in JavaScript7": {
      "main": [
        [
          {
            "node": "Edit Fields3",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  },
  "active": false,
  "settings": {
    "executionOrder": "v1",
    "availableInMCP": false
  },
  "versionId": "d4c5c95a-893b-46e2-a613-cb8f20cccb2b",
  "meta": {
    "templateCredsSetupCompleted": true,
    "instanceId": "48d6009038edad90eccd2178bb14c74124af2c721b1b721da5c7fab4889037cf"
  },
  "id": "bp6EpoEm3mOHO8QLMV8gZ",
  "tags": []
}
