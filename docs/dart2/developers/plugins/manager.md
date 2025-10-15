# Plugin Manager

DART's PluginManager provides methods that allow the application to discover and load plugins. DART uses the findById method to load individual plugins, and the following methods to discover available plugins:

* getModuleCollection() - This returns information about plugins of a specified type, such as [Repo Clients](repo_clients.md) or [Network Clients](network_clients.md). The list of plugins returned by this method appears on some configuration screens. For example, when a user sets up a new [StorageService](../../users/settings/storage_services.md), a list of available network clients appears so the user can choose which client/protocol should be used to communicate with that service.

* DART's JobRunner uses the methods canRead(), canWrite(), implementsProtocol(), talksTo(), and setsUp() to figure out which plugins to use to complete each operation within a job.

See also: [PluginManager API](https://aptrust.github.io/dart/PluginManager.html), [PluginManager source](https://github.com/APTrust/dart/blob/master/plugins/plugin_manager.js)
