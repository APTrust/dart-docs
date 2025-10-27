# Uploads

You can specify one or more upload targets on the uploads page. Simply check the box beside each target you want to upload to.

The list of available targets comes from the [Storage Service](../settings/storage_services.md) settings in your local DART installation. You can add as many storage services as you like. To appear in the list of upload targets, a storage service must include a protocol, URL, login, and password, and it must have the _Allows Upload_ set to _Yes_. If some of your storage services do not appear in the list, check to see they meet all of these criteria.

__Note:__ DART 2 sometimes struggled to upload bags over 400 GB. DART 3's uploader is more robust. If you do encounter problems uploading large bags, we recommend using DART to create bags and using a third-party S3 client (e.g., <a href="https://cyberduck.io/" target="_blank">Cyberduck</a>, <a href="https://s3browser.com/" target="_blank">S3 Browser</a>) to upload them.

![Job upload](../../img/jobs/upload.png)
