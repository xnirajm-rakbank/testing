#testing

```json
{
  "blobUrl": "@{items('For_each')?['data']?['url']}",
  "eventTime": "@{items('For_each')?['eventTime']}",
  "contentType": "@{items('For_each')?['data']?['contentType']}",
  "contentLength": "@{items('For_each')?['data']?['contentLength']}"
}

```
