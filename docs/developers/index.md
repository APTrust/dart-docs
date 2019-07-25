# Developer's Guide to DART

DART's primary purpose is to pack and ship digital materials for preservation. It includes a GUI for non-technical users who want to drag and drop file, and a [command line interface](../users/command_line.md) for more technical users who want to [script DART jobs](../users/scripting.md).

While DART's initial release supports the BagIt packaging format and uploads to S3-compatible services, future users may require additional package formats such as rar, parchive, OCFL, etc. They may also need to ship files using network protocols such as SFTP, rsync, scp and others.

DART has a plugin architecture that allows developers to contribute code that will add these package formats and network protocols. If you know how to write JavaScript, you can contribute [plugins](plugins/index.md) without having to understand DART's internals.

## Getting the Code

To get started developing DART plugins, download the souce from GitHub.

```
git clone git@github.com:APTrust/dart.git
```

Once you have the source, you'll need to install the dependencies. Change into the dart directory and run this:

```
npm install
```

To ensure all is working, run the tests.

```
npm test -- --runInBand
```

## Useful Links

[Writing Plugins for DART](plugins/index.md)

[Building DART Installers](building.md)

[DART API Documentation](https://aptrust.github.io/dart/)

[DART source on GitHub](https://github.com/APTrust/dart)
