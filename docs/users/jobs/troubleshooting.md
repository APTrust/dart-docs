# Troubleshooting Jobs

Note: APTrust will be adding to this section as issues arise.

## Uploading

### SFTP

#### Bad path: parent not exist

This can occur if the bucket name in your Storage Service record does not match
the name of the upload folder on the sftp server. For example, if uploads should
go into a folder on the server called `uploads`, but your StorageService has the
bucket name `upload`, you'll see this error.

See [Storage Services](../settings/storage_services.md) for details on how to
change Storage Service settings.

#### Upload failed due to unknown error.

This can occur when your local machine's SSH hosts file does not have the
public key of the sftp server you are connecting to.

You can fix this by manually connecting to the server with

```
local:~ yoozer$ ssh server.example.com
The authenticity of host 'server.example.com (::1)' can't be established.
ED25519 key fingerprint is SHA256:3WxPp9MParT+tsW/LpcNWR1m4c126aWIR98LVyEgfcw.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'localhost' (ED25519) to the list of known hosts.
yoozer@server.example.com's password:
```

You can hit Control-C after the last prompt, since your ssh hosts file now
has the key.
