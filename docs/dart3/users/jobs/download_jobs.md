# Download Jobs

This feature enables you to download files from any S3 buckets to which you
have list and read access. Follow these steps to download files:

1. From the top menu, choose __Jobs > Download from S3__.
2. Choose an S3 service from the __Download From__ list.
3. Choose a bucket from the bucket list.
4. Click on the name of a key/object in files list to download it.

Note that Glacier files cannot be downloaded because AWS requires objects to be moved from Glacier to S3 before they are downloaded. DART is not currently able to move files from Glacier to S3.

![Download Job](../../img/jobs/download_job.png)
