# Format Writers

Format writers write file formats like tar, zip, OCFL, etc. Format writers provide methods for writing contents into a directory or serialized file. The initial release of DART 2.0 includes two format writers: a [FileSystemWriter](https://aptrust.github.io/dart/FileSystemWriter.html) and a [TarWriter](https://aptrust.github.io/dart/TarWriter.html). Developers can use these two examples as references for how to write a format writer.

Format writers may be synchronous or asynchronous under the hood. Certain types of writers, such as the built-in [TarWriter](https://aptrust.github.io/dart/TarWriter.html) MUST be synchronous internally because the tar format requires files to be written one at a time, in order. To assist with this, the [BaseWriter](https://aptrust.github.io/dart/BaseWriter.html) implements a queue that executes requests sequentially, in the order they were received, with each write request beginning only after the last write has completed.

## API and Events

Format writers should extend the DART [BaseWriter](https://aptrust.github.io/dart/BaseWriter.html) and provide a description() method that returns meaningful [PluginDescription](https://aptrust.github.io/dart/global.html#PluginDefinition) information.

Format writers must implement the following methods:

* A __constructor__ that takes a single parameter, which is the path to the file or directory that the reader will write.

* A static __definition__ method that takes no parameters and returns a description of the plugin (as described in [The Base Plugin](index.md/#the-base-plugin)).

* An __add__ method that takes two parameters: a [BagItFile](https://aptrust.github.io/dart/BagItFile.html) and an options list of cryptographic hash algorithm names (such as 'md5', 'sha256', etc.). This method must emit a `fileAdded` event each time it writes a file into the target directory (or tar archive, zip archive, etc).

The __fileAdded__ event ...  TODO: WRITE ME

The __error__ event ...  TODO: WRITE ME

The __finish__ event ...  TODO: WRITE ME
