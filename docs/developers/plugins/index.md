# Plugins

Plugins allow DART to read and write data formats (such as tar, zip, etc.), to upload and download files using various protocols (such as s3, ftp, etc.), to communicate with the REST APIs of remote repositories (such as APTrust) and to help users through the complexities of initial setup and configuration (such as the APTrust and DPN setup modules).

DART plugins are written in plain JavaScript and HTML. Developers may contribute new plugins without having to learn DART's internals. A plugin simply has to conform to a simple API, which it typically limited to a small number of well-defined methods.

The documentation in this section gives a high-level overview plugin structure and behavior as a starting point for developing your own plugins. You'll find more detail information in DART's API documentation and source code, as well as in the unit tests that accompany existing plugins. Along with an overview of each plugin type, this documentation provides relevant links to source and test code for further study.

## The Base Plugin

DART's base [Plugin](https://github.com/APTrust/dart/blob/master/plugins/plugin.js) object is simply an [EventEmitter](https://nodejs.org/api/events.html#events_class_eventemitter) with a static method that returns a description. The description includes the following fields:

* __id__ - A UUID string that uniquely identifies the plugin. When writing a plugin, you should generate this once. It lives from then on as a hard-coded identifier.

* __name__ - The name of the plugin. This will appear in parts of the DART UI that use the plugin.

* __description__ - A description of what the plugin does.

* __version__ - The plugin version, which is a string. E.g. '1.0.14'.

* __readsFormats__ - A list of strings, this applies only to plugins that can read data from formats such as zip, tar, rar, parchive, etc. The values in this field should be the file extensions of the types of files the plugin can read. For example, ['.tar.', '.gz', '.gzip', '.tar.gz']. This should be empty for plugins that are not intended to read file formats. Use all lower case letters.

* __writesFormats__ - A list of strings indicating the file formats the plugin can write. E.g. ['.tar.', '.gz', '.gzip', '.tar.gz']. This should be empty for plugins that are not intended to write file formats. Use all lower case letters.

* __implementsProtocols__ - The network protocols that this plugin implements. For example, an FTP plugin may implement ['ftp', 'sftp', 'ftps']. This applies only to plugins of type NetworkClient. If your plugin is not a NetworkClient, this should be an empty list. Use all lower case letters.

* __setsUp__ - This describes what general configuration your plugin provides. For example, the 'aptrust' setup plugin helps the user configure some basic APTrust settings, such as the URL of the APTrust repository, the user's API keys, etc. This applies only to plugins of type Setup. If your plugin is not a Setup plugin, this should be empty. Use all lower case letters.

* __talksToRepository__ - This describes what type of repository your plugin talks to. For example, 'fedora', 'aptrust', etc. This applies only to plugins of type Repository. If your plugin is not a Repository plugin, this should be empty. Use all lower case letters.

The [PluginManager](manager.md) uses these descriptions to tell the application what plugins are available and what capabilities they have.

Plugins are EventEmitters. They can work synchronously or asynchronously, but they must emit a standard set of events to communicate with the UI (or the [JobRunner](../job_flow/#the-jobrunner-step-by-step), when DART runs in command-line mode).

## Plugin Types

[Format Readers](format_readers.md) read file formats, such as tar, zip, and anything else a developer might want.

[Format Writers](format_writers.md) write file formats.

[Network Clients](network_clients.md) provide methods for uploading and download via different network protocols.

[Repo Clients](repo_clients.md) provide communication between DART and the REST APIs of remote repositories.

[Setup Modules](setup_modules.md) automate pars of the setup and configuration process, and provide one-at-a-time walkthrough questions to help users configure their DART environment.

The [PluginManager](manager.md) provides methods for discovering and loading installed plugins.
