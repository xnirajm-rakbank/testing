# testing
```python
import json
import os
from urllib.parse import urlparse

import azure.functions as func
from azure.storage.blob import BlobClient
from azure.identity import DefaultAzureCredential

app = func.FunctionApp()

@app.function_name(name="ProcessBlobHttp")
@app.route(route="process-blob", methods=["POST"], auth_level=func.AuthLevel.FUNCTION)
def process_blob_http(req: func.HttpRequest) -> func.HttpResponse:
    try:
        body = req.get_json()
    except Exception:
        return func.HttpResponse("Body must be JSON", status_code=400)

    blob_url = body.get("blobUrl")
    if not blob_url:
        return func.HttpResponse("Missing 'blobUrl' in body", status_code=400)

    # Option A: Use Managed Identity (recommended)
    # - Enable System-assigned identity on your Function App
    # - Assign role: Storage Blob Data Reader to the storage (container or account)
    # - Then:
    cred = DefaultAzureCredential(exclude_interactive_browser_credential=True)
    blob_client = BlobClient.from_blob_url(blob_url=blob_url, credential=cred)

    # Option B: Use a connection string to the data storage (if you prefer secrets)
    # conn_str = os.environ.get("DATA_STORAGE_CONNECTION_STRING")
    # if conn_str:
    #     parsed = urlparse(blob_url)
    #     parts = parsed.path.lstrip('/').split('/', 1)
    #     container = parts[0]
    #     blob_name = parts[1] if len(parts) > 1 else ''
    #     blob_client = BlobClient.from_connection_string(conn_str, container, blob_name)

    try:
        stream = blob_client.download_blob(max_concurrency=4)
        data = stream.readall()
        # TODO: Your processing here
        processed_len = len(data)
        return func.HttpResponse(
            json.dumps({"status": "ok", "blob": blob_url, "bytesRead": processed_len}),
            status_code=200,
            mimetype="application/json"
        )
    except Exception as e:
        # Helpful error to see RBAC/auth issues
        return func.HttpResponse(f"Error reading blob: {e}", status_code=500)

```
