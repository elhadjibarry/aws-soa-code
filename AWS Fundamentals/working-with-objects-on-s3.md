# Working with Objects on S3

1. Upload object using the s3api (change bucket and path)
aws s3api put-object --bucket aws-soa-labs --key session-02-lab.md --body /home/cloudshell-user/session-02-lab.md
2. Download an object using the s3api (change bucket and path)
aws s3api get-object --bucket aws-soa-labs --key session-02-lab.md /home/cloudshell-user/session-02-lab.md
3. Use curl with a presigned URL to download an object (change bucket, path, and object)
curl -o /home/cloudshell-user/session-02-lab.md "$(aws s3 presign s3://aws-soa-labs/session-02-lab.md --expires-in 3600)"