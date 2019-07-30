# Network Clients

Network Clients allow DART to send and receive data using defined network protocols such as S3, FTP, etc. These clients should be able to do the following:

* Use the information in a [StorageService](https://aptrust.github.io/dart/StorageService.html) object to login to or authenticate with the remote service. StorageService objects include a URL, login name, password, and other information required for authentication.

* Upload files or data to the remote service (if the service supports upload).

* Download files or data from the remote service (if the service supports download).

* List files or objects in the remote service (if the service supports list operations).

Note that at the time of DART's 2.0 release, DART supports only S3 uploads.

## API and Events

Network clients must implement the following:

* A __constructor__ that takes a [StorageService](https://aptrust.github.io/dart/StorageService.html) object as its sole parameter.

* An __upload__ method that takes two parameters, __localPath__ and __remotePath__. This method uploads the file at localPath to the destination remotePath.

* A __download__ method that takes two parameters, __localPath__ and __remotePath__. This method downloads the file from remotePath to localPath.

* A __list__ method whose parameters and return values are yet to be determined.

!!! Warning
    The API for network client plugins is still in flux. We may add optional parameters to each of the methods above, and we may further define some as-yet undefined behaviors.