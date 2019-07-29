# Format Readers

Format readers read file formats like tar, zip, OCFL, etc. Format readers provide methods for listing the contents of a directory or serialized file, and for reading individual files from the directory/file.

The initial release of DART 2.0 includes two format readers: a [FileSystemReader](https://aptrust.github.io/dart/FileSystemReader.html) and a [TarReader](https://aptrust.github.io/dart/TarReader.html). Developers can use these two examples as references for how to write a format reader.

## API and Events

Format readers must extend the DART base [Plugin](https://aptrust.github.io/dart/Plugin.html) and providing a description() method that returns meaningful [PluginDescription](https://aptrust.github.io/dart/global.html#PluginDefinition) information.

Format readers must implement the following methods:

* A __constructor__ that takes a single parameter, which is the path to the file that the reader will read. (If the reader reads from a directory, such as an OCFL object, the path should point to a directory.)

* A static __definition__ method that takes no parameters and returns a description of the plugin (as described in [The Base Plugin](index.md/#the-base-plugin).

* A __read__ method that takes no parameters and emits events `entry`, `error`, and `end` (described below).

* A __list__ method that takes no parameters and emits events `entry`, `error`, and `end` (described below).

* Property __fileCount__, a number that starts at zero and is incremented by one each time __read__ or __list__ emits an entry event for a file.

* Property __dirCount__, a number that starts at zero and is incremented by one each time __read__ or __list__ emits an entry event for a directory.

* Property __byteCount__, a number that starts at zero and is incremented by the number of bytes in a file each time __read__ or __list__ emits an entry event for a file.

The __error__ event passes a JavaScript error object to registered listeners and causes DART to stop reading.

The __end__ event passes nothing to registered listeners and tells DART that the reader has finished with all entries.

The __entry__ event passes an object containing the relative path of the current file entry, a lightweight [FileStat](https://aptrust.github.io/dart/FileStat.html) object, and in the case a of the __read__ method, an open [Readable](https://nodejs.org/api/stream.html#stream_class_stream_readable) stream so DART can read the contents of the file.

!!! Note
    DART uses its own simplified [FileStat](https://aptrust.github.io/dart/FileStat.html) object instead of Node's built-in [Stats](https://nodejs.org/api/fs.html#fs_class_fs_stats) object because format readers will often have to read from serialized formats that don't include all of the information you'd find in a Stats object. For example, tar and zip headers may exclude information about inode, blocksize, etc.  The [FileStat](https://aptrust.github.io/dart/FileStat.html) object simply captures the essentials that are common across most formats.

!!! Tip
    In cases where a FormatReader's __read__ method cannot return a readable stream, it can return a [DummyReader](https://aptrust.github.io/dart/DummyReader.html). This is necessary in cases where the reader encounters a directory entry that cannot be read like a regular file. DART will still try to read from the readable stream in the __entry__ event, and a DummyReader will allow DART to read without error. For an example of how to do this, look for DummyReader in the __read__ method of the [FileSystemReader](https://github.com/APTrust/dart/blob/master/plugins/formats/read/file_system_reader.js) source code.

## Format Reader Examples

The best way to understand how to write a FormatReader plugin is to review the API documentation, source code, and tests for the following:

* [FileSystemReader](https://aptrust.github.io/dart/FileSystemReader.html), which provides a working example.

* The [FileSystemReader tests](https://github.com/APTrust/dart/blob/master/plugins/formats/read/file_system_reader.test.js), which show how the reader is expected to behave.

* [TarReader](https://aptrust.github.io/dart/TarReader.html).

* The [TarReader tests](https://github.com/APTrust/dart/blob/master/plugins/formats/read/tar_reader.test.js), which show how the TarReader is expected to behave.

* The [FileStat](https://aptrust.github.io/dart/FileStat.html) documentation, which shows the structure of the FileStat object.
