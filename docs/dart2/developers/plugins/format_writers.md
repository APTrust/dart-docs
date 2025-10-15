# Format Writers

Format writers write file formats like tar, zip, OCFL, etc. Format writers provide methods for writing contents into a directory or serialized file. The initial release of DART 2.0 includes two format writers: a [FileSystemWriter](https://aptrust.github.io/dart/FileSystemWriter.html) and a [TarWriter](https://aptrust.github.io/dart/TarWriter.html). Developers can use these two examples as references for how to write a format writer.

Format writers may be synchronous or asynchronous under the hood. Certain types of writers, such as the built-in [TarWriter](https://aptrust.github.io/dart/TarWriter.html) MUST be synchronous internally because the tar format requires files to be written one at a time, in order. To assist with this, the [BaseWriter](https://aptrust.github.io/dart/BaseWriter.html) implements a queue that executes requests sequentially, in the order they were received, with each write request beginning only after the last write has completed.

## API and Events

Format writers should extend the DART [BaseWriter](https://aptrust.github.io/dart/BaseWriter.html) and provide a description() method that returns meaningful [PluginDescription](https://aptrust.github.io/dart/global.html#PluginDefinition) information.

Format writers must implement the following methods:

* A __constructor__ that takes a single parameter, which is the path to the file or directory that the reader will write.

* A static __definition__ method that takes no parameters and returns a description of the plugin (as described in [The Base Plugin](index.md/#the-base-plugin)).

* An __add__ method that takes two parameters: a [BagItFile](https://aptrust.github.io/dart/BagItFile.html) and an options list of cryptographic hash algorithm names (such as 'md5', 'sha256', etc.). This method must emit a `fileAdded` event each time it writes a file into the target directory (or tar archive, zip archive, etc). Note that a [BagItFile](https://aptrust.github.io/dart/BagItFile.html) is a simple object with properties to denote the source path from which the file is copied, the destination path to which it should be copied, and some basic stats information that includes the file size. In the process of copying the file, the Format Writer should calculate the requested checksums and store them in the `checksums` hash of the BagItFile.

Format writers should fire the __fileAdded__ event each time a file has been written. See the __add__ method of [FileSystemWriter](https://aptrust.github.io/dart/FileSystemWriter.html) or [TarWriter](https://aptrust.github.io/dart/TarWriter.html) to see when and how this occurs. The data passed by the __fileAdded__ event includes the [BagItFile](https://aptrust.github.io/dart/BagItFile.html) that was just added and the percent complete of the overall write operation (i.e. number of bytes written divided by number of total bytes to be written).

Format writers won't need to emit the __error__ event on their own, unless some special circumstance warrents it. The [BaseWriter](https://aptrust.github.io/dart/BaseWriter.html) emits this event, passing back a string showing the name of the file being written and the error message.

!!! Note
    The BaseWriter always emits the __finish__ event immediately after emitting the __error__ event. This is done on the assumption that there is no sense in continuing to write a failed, incomplete, or corrupt output file.

Format writers don't need to emit the __finish__ event. The BaseWriter takes care of that internally. This event does not pass any parameters.

!!! Note
    After emission of the __finish__ event, you should be able to check the __filesWritten__ property to get the number of files written. That number is updated by the BaseWriter's __onFileWritten__ method. If you override that method, be sure your override calls __super()__.

## Format Writer Queue Functions

DART's [BaseWriter](https://aptrust.github.io/dart/BaseWriter.html) uses a one-at-a-time queue to force sequential synchronous writes. The queue is an instance of [async.queue](https://caolan.github.io/async/v3/docs.html#queue), and it requires a function to be run on each item. The __writeIntoArchive()__ functions in [FileSystemWriter source](https://github.com/APTrust/dart/blob/master/plugins/formats/write/file_system_writer.js) and [TarWriter source](https://github.com/APTrust/dart/blob/master/plugins/formats/write/tar_writer.js) provide examples on how to write a function for the queue.

Notice that in both format writers, the __add()__ method sets up a hash containing data and functions, then passes that hash into the queue by calling `this._queue.push(data)`. The queue eventually hands that data structure off to __writeIntoArchive()__, which does some piping internally to calculate multiple checksums in a single write.
