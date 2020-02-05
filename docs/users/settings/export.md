# Exporting Settings

If you're setting up DART for a number of users, you may want to configure your local DART installation, then export your settings for others to use. For example, you can set up a BagIt Profile, application settings, remote repositories and storage services for your institution, then publish them to a URL for others to install via DART's import feature.

!!! note "Data Export Does Not Export Credentials"

    When you export StorageService and RemoteRepository records, DART will not export login and password fields, unless they begin with "env:", which indicates the logins/passwords are to be loaded from the environment.

    You may still export these records, but you will then have to pass login and password information through another channel, such as phone, email, or <a href="https://privnote.com/#" target="_blank">PrivNote</a>.

    Also note that if you export StorageService and RemoteRepository records and then import them back into your own DART installation, you may overwrite the records' existing login and password info with blank data.

To export settings from DART:

1. Choose __Settings > Export Settings__ from the main menu.
2. Check the boxes next to the settings you want to export.
3. Click __Export__.

![Choose settings to export](../../img/settings/export/choose.png)

After exporting your settings, click __Copy to Clipboard__ to copy them.

![Exported settings](../../img/settings/export/exported.png)

You can then post the settings to a public URL, or email them to other users for import.
