# testing

{
  "type": "array",
  "items": {
    "type": "object",
    "properties": {
      "id": { "type": "string" },
      "eventType": { "type": "string" },
      "subject": { "type": "string" },
      "eventTime": { "type": "string" },
      "data": {
        "type": "object",
        "properties": {
          "api": { "type": "string" },
          "clientRequestId": { "type": "string" },
          "requestId": { "type": "string" },
          "url": { "type": "string" },
          "contentType": { "type": "string" },
          "contentLength": { "type": "integer" }
        },
        "required": ["url"]
      }
    },
    "required": ["id", "eventType", "subject", "eventTime", "data"]
  }
}
