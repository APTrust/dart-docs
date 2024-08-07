# Welcome to DART

DART is the Digital Archivist's Resource Tool. It provides both a GUI and a command-line interface for packaging files and uploading them to remote repositories.

## Download

{! shared/downloads.md !}

After installation, check out the [Getting Started page](./users/getting_started.md)

## Supported Operations

The current release of DART 2.0.22 supports the following features:

* Creating BagIt bags that conform to defined BagIt profiles
* Validating BagIt bags according to defined BagIt profiles
* Uploading bags and other files to remote S3 and SFTP endpoints
* Creating and modifying BagIt profiles through a visual point-and-click editor
* Defining repeatable Workflows for bagging and uploading files
* Running multiple simultaneous bagging and upload jobs
* Read-only integration with the APTrust's REST API to display the status of ingested materials and pending work items
* A command-line tool to enable scriptable bagging and upload operations
* BagIt Profile import and export
* BagIt Profile customization throug a visual editor
* Settings import and export
* Workflow export

To start using DART, see our [Getting Started](users/getting_started.md) page.

!!! note
    We recently released DART Runner to run workflows on servers that have no graphical environment.
    For more info, see [DART Runner](users/dart_runner.md).

## Plugin Architecture

Most of DART's features are implemented in plugins, which enable developers to add new features without having to understand all of DART's internals. DART is an open source project of the Academic Preservation Trust, which encourages developers to contribute new plugins to extend the tool's functionality.

DART 2.0 supports the following types of plugins:

1. Format Readers - These allow DART to read files packaged in various formats, such as tar, zip, rar, parchive, OCFL, etc. Currently supported in formats in version 2.0:

    1. directory/file system
    1. tar

1. Format Writers - These allow DART to write files in various formats, such as tar, zip, rar, parchive, OCFL, etc. Currently supported in formats in version 2.0:

    1. directory/file system
    1. tar

1. Network Clients - These allow DART to send and retrieve files across a network. DART 2.0 supports the following protocols:

    1. S3
    1. SFTP

1. Repository Clients - These allow DART to interact with remote repositories. Currently supported:

    1. APTrust

Writing DART plugins requires a working knowledge of JavaScript and HTML. If you're interested in developing DART plugins, see our [Developers](developers/index.md) page and our [full API documentation](https://aptrust.github.io/dart/).

## DART User Group

APTrust hosts a [DART User Group](https://aptrust.org/resources/user-groups/dart-user-group/) for the entire digital preservation community. This group will primarily be a [mailing list](https://groups.google.com/a/aptrust.org/g/dart-users), where users can share experiences, ask questions, and support one another. Depending on the level of interest and engagement, we may expand this initiative to include regular virtual meetings and more structured activities in the future.

## Useful Links

[Getting Started with DART](users/getting_started.md)

[Developing DART Plugins](developers/index.md)

[DART API documentation](https://aptrust.github.io/dart/)

[DART source code on GitHub](https://github.com/aptrust/dart/)

[Video: Jobs and Workflows](https://www.youtube.com/watch?v=_ga9XfuyO-I)

[Video: BagIt Profiles in DART](https://www.youtube.com/watch?v=qnFFlRhlfCA)

[Video: Settings Import and Export](https://www.youtube.com/watch?v=GcJptP4h4IM)

## Credits

Brace yourselves gentlemen. According to the gas chromatograph, the secret ingredient is... Love!?

<img src="./img/about/frink.png" height="300"/>

Who's been screwing with this thing?
