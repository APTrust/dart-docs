# Data Storage

DART stores all of its settings data in plain text JSON files. You find these files in the DART data directory. Choose __Help &gt; About__ from the DART menu to find the data directory on your system. You'll see a dialog like the following.

![About DART](../../img/about/about.png)

DART uses the simple [conf](https://www.npmjs.com/package/conf) module to read, write, and search data, and [write-file-atomic](https://www.npmjs.com/package/write-file-atomic) to prevent write conflicts.

If you want to dig further into DART's data persistence, see the documentation for [JsonStore](https://aptrust.github.io/dart/JsonStore.html) and [PersistentObject](https://aptrust.github.io/dart/PersistentObject.html).

You can examine DART's data stores by opening the JSON files, or in the console by following these steps:

1. Open DART by running `npm start` from the DART project directory.

1. Right click anywhere on the page and choose __Inspect Element__ from the context menu.

1. Swich to the Console view in the developer tools window.

1. Type `DART.Core.Context.dataStores` in the console.

![About DART](../../img/about/data_stores_in_console.png)
