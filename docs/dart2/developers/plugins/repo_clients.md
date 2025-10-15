# Repository Clients

Repository clients communicate with the REST APIs of remote repositories such as APTrust, Fedora, etc. For safety, these clients should be limited to read operations, such as showing a list of ingested objects or a list of pending work items from a queue.

## API

Repository clients should inherit from [RepositoryBase](https://aptrust.github.io/dart/RepositoryBase.html). They must implement the following methods:

* A __constructor__ that takes a [RemoteRepository](https://aptrust.github.io/dart/RemoteRepository.html) object as its first parameter.

* A __provides__ method that takes no parameters and returns a list of object describing what methods this object provides. Each of the described methods should return a promise that ultimately returns HTML to be displayed in the DART dashboard. See the [APTrustClient](https://aptrust.github.io/dart/APTrustClient.html) for an example.

* A __hasRequiredConnectionInfo__ method that takes no parameters and returns true or false to indicate whether the client's [RemoteRepository](https://aptrust.github.io/dart/RemoteRepository.html) object appears to have enough info to query the repository. If this returns false, the DART dashboard will skip its automatic attempt to connect to the repo.

## Report Templates

Because repository clients return HTML, they may use templates to translate the repository's JSON responses to fomatted markup. These templates should be in a directory whose name matches the file name of repository client, minus the .js at the end. For example, the [APTrustClient](https://aptrust.github.io/dart/APTrustClient.html) is in the file plugins/repository/aptrust.js. It's templates are in plugins/repository/aptrust/.

DART uses [Handlebars](https://handlebarsjs.com/) templates. The [APTrustClient](https://aptrust.github.io/dart/APTrustClient.html) includes templates to display [ingested objects](https://github.com/APTrust/dart/blob/master/plugins/repository/aptrust/objects.html) and [work items](https://github.com/APTrust/dart/blob/master/plugins/repository/aptrust/work_items.html), which are items queued for ingest or restoration.

Since the APTrust client is currently the only existing repository client plugin, its [source code](https://github.com/APTrust/dart/blob/master/plugins/repository/aptrust.js) is probably the best reference for writing your own client. The [APTrustClient tests](https://github.com/APTrust/dart/blob/master/plugins/repository/aptrust.test.js) show how the client is expected to behave.

You can see what the output of the APTrust client looks like in the [dashboard documentation](../../users/dashboard.md). If you're interested in seeing how the dashboard loads reports from repository clients, see the `show()` method of the [DashboardController](https://github.com/APTrust/dart/blob/master/ui/controllers/dashboard_controller.js).

!!! Warning
    The API for repository clients is subject to change. Most changes will likely be additions.
