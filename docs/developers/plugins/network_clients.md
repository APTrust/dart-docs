# Network Clients

Network Clients allow DART to send and receive data using defined network protocols such as S3, FTP, etc. These clients should be able to do the following:

* Use the information in a [StorageService](https://aptrust.github.io/dart/StorageService.html) object to login to or authenticate with the remote service. StorageService objects include a URL, login name, password, and other information required for authentication.

* Upload files or data to the remote service (if the service supports upload).

* Download files or data from the remote service (if the service supports download).

* List files or objects in the remote service (if the service supports list operations).

Note DART 2.0.14 supports only S3 and SFTP uploads.

## API and Events

Network clients must implement the following:

* A __constructor__ that takes a [StorageService](https://aptrust.github.io/dart/StorageService.html) object as its sole parameter.

* An __upload__ method that takes two parameters, __localPath__ and __remotePath__. This method uploads the file at localPath to the destination remotePath.

* A __download__ method that takes two parameters, __localPath__ and __remotePath__. This method downloads the file from remotePath to localPath.

* A __list__ method whose parameters and return values are yet to be determined.

!!! Warning
    The API for network client plugins is still in flux. We may add optional parameters to each of the methods above, and we may further define some as-yet undefined behaviors. We plan to flesh out the network client API as we add new clients.

Network clients emit the following events:

* __start__ - Indicates an upload or download has started.

* __status__ - Indicates the status of an upload or download operation.

* __warning__ - Issued when a non-fatal error occurs, such as a network connection interruption. The client should retry operations that failed due to non-fatal errors.

* __error__ - Issued when a fatal error occurs. Fatal errors include authentication failures, missing files, permission errors, and other problems you cannot reasonably expect to go away when you retry the operation.

* __finish__ - Emitted when the upload/download is complete.

## The Transfer Object

Network clients may internally use their own custom transfer object to keep track of the details of an upload or download operation. The structure of a custom transfer object is entirely up to the developer. The only requirement is that the object include a property called `result` of type [OperationResult](https://aptrust.github.io/dart/OperationResult.html).

DART doesn't care what network client's do under the hood, but it does care about the results of each operation. Those results should be stored in the [OperationResult](https://aptrust.github.io/dart/OperationResult.html) object, and that object should be made accessible to DART through the events listed above.

## Reference Implementation

The only current reference implementation for a network client plugin is the [S3Client](https://github.com/APTrust/dart/blob/master/plugins/network/s3_client.js). The [S3Client tests](https://github.com/APTrust/dart/blob/master/plugins/network/s3_client.test.js) will give you an idea of how the client is expected to behave.
