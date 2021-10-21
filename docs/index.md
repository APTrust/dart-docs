# Welcome to DART

DART is the Digital Archivist's Resource Tool. It provides both a GUI and a command-line interface for packaging files and uploading them to remote repositories.

## Download

{! shared/downloads.md !}

After installation, check out the [Getting Started page](./users/getting_started)

## Supported Operations

The current release of DART 2.0.15 supports the following features:

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
    In the future, DART plans to support running workflows on servers. Currently, this is
    difficult because DART's underlying Electron framework insists on graphics capabilities
    even when running in command-line mode.

    We plan to release a stand-alone binary that will be able to run DART jobs and workflows
    on a server without requiring a UI or graphics capabilities. You'll find a draft of the
    proposed features [here ](https://docs.google.com/document/d/15s8tgoYPYTzr6ZIB0u1Nk-YZEGfhrj2CXKv9o9B3ACI/edit?usp=sharing).
    You can provide feedback through the survey at the end of the features document.

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
