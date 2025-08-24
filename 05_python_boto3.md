# 5) Python & Boto3 Automation

**Q132. Boto3 session vs client vs resource?**

**Answer (Natural):**  
Session holds config/credentials; client is low-level service API; resource is higher-level OO wrapper. Use client for full coverage and pagination; resource for ergonomics.


👉 **Example:** s3 = boto3.client('s3'); s3.list_buckets()


---

**Q133. Safe pagination & retries?**

**Answer (Natural):**  
Use paginators (`get_paginator`) and `botocore.config.Config` with retry mode `standard` or `adaptive`. Avoid manual while-loops with markers.


👉 **Example:** Paginator for `list_objects_v2` across big buckets.


---

**Q134. Handling credentials securely?**

**Answer (Natural):**  
Prefer IAM roles (instance profile/Lambda execution role). Avoid hardcoding keys; if needed, use AWS CLI profiles and env vars.


👉 **Example:** AssumeRole with STS and short-lived tokens.


---

**Q135. Exception handling patterns?**

**Answer (Natural):**  
Catch `botocore.exceptions.ClientError`, inspect `e.response['Error']['Code']`, implement retries for throttling (`ThrottlingException`) and backoff.


👉 **Example:** Retry on `RequestLimitExceeded` with jitter.


---

**Q136. Example: Upload folder to S3 with retries?**

**Answer (Natural):**  
Walk the directory, multipart upload large files, set content-type, and server-side encryption. Use threads with a bounded pool.


👉 **Example:** See script skeleton below.


---

```python
import boto3, os, mimetypes
from concurrent.futures import ThreadPoolExecutor
from botocore.config import Config

s3 = boto3.client('s3', config=Config(retries={'max_attempts': 10, 'mode': 'adaptive'}))

def put_file(bucket, key, path):
    extra = {'ContentType': mimetypes.guess_type(path)[0] or 'application/octet-stream',
             'ServerSideEncryption': 'AES256'}
    s3.upload_file(path, bucket, key, ExtraArgs=extra)

bucket = 'my-bucket'
root = '/path/to/folder'
with ThreadPoolExecutor(max_workers=8) as ex:
    for rootdir,_,files in os.walk(root):
        for f in files:
            p = os.path.join(rootdir,f)
            k = os.path.relpath(p, root)
            ex.submit(put_file, bucket, k, p)
```

---
