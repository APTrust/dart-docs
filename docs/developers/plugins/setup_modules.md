# Setup Modules

Setup modules ease the process of configuring DART for users who will require common settings. These modules allow a developer or organization to:

* Automatically install a number of default settings.

* Walk the user through a series of simple questions to help them finish the configuration.

For example, a setup plugin could do the following for your organization or user group:

* Install BagIt profiles.

* Install pre-configured [StorageService](../../users/settings/storage_services.md) objects that include common information such as the URLs and protocols needed to connect.

* Install pre-configured [RemoteRepository](../../users/settings/remote_repositories.md) objects that include common information such as the URLs and protocols needed to connect.

After silently installing these objects, the walk-through questions can ask users for their specific login names and passwords to fill out the remaining information required to make the storage services and remote repositories usable.

The process of automated configuration followed by simple questions is easier and less error-prone than processes that require users to search out a number of setting options and then fill out many fields. Setup modules have the added benefit of being repeatable. If a user's credentials have changed and he/she doesn't know which settings to update, they can go through the setup process again, enter their credentials where prompted, and the setup plugin will store them in the correct place.

## API

Descended from SetupBase...

TODO: WRITE ME...

## Supporting Files

TODO: WRITE ME...

### app_settings.json

### bagit_profiles.json

### end_message.html

### internal_settings.json

### logo.png

### questions.json

### remote_repositories.json

### start_message.html

### storage_services.json

## Mechanism

If you're curious about how setup plugins are run, see the [SetupController documentation](https://aptrust.github.io/dart/SetupController.html) and [SetupController source](https://github.com/APTrust/dart/blob/master/ui/controllers/setup_controller.js).