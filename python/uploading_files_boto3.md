# Using boto3 to upload files

This is the easiest way to use boto3 session to upload files

```python
import boto3

# S3 Session
s3_session = boto3.Session(
    aws_access_key_id=cfig.AWS_ACCESS_KEY,
    aws_secret_access_key=cfig.AWS_SECRET_ACCESS_KEY,
)
s3 = s3_session.resource("s3")

try:
    s3.Object(
        f"{cfig.AWS_S3_BUCKET}", # Bucket Name
        f"some_directory/{file}", # Directory of where you want to upload file
    ).upload_file(f"uploads/{file}") # Where you're getting file from to upload

except ClientError as e:
    log.error(e)
```