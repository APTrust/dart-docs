# Getting Started

DART is the Digital Archivist's Resource Tool. Its primary purpose is to package digital materials and send them off to long-term preservation storage. DART's initial release focuses on packaging materials in BagIt format and uploading them to S3 buckets for ingest in APTrust. DART can be extended through plugins to produce other packaging formats and to communicated via additional network protocols.

DART runs in both graphical and command-line modes on Windows, Mac, and Linux.

## Installation

Download the DART installer for your system. The current version is 2.0.8 (released May 14, 2020).

__Mac__: [https://s3.amazonaws.com/aptrust.public.download/DART/DART-2.0.8.dmg](https://s3.amazonaws.com/aptrust.public.download/DART/DART-2.0.8.dmg)

__Windows__: [https://s3.amazonaws.com/aptrust.public.download/DART/DART+Setup+2.0.8.exe](https://s3.amazonaws.com/aptrust.public.download/DART/DART+Setup+2.0.8.exe)

__Linux__: [https://s3.amazonaws.com/aptrust.public.download/DART/DART_2.0.8_amd64.deb](https://s3.amazonaws.com/aptrust.public.download/DART/DART_2.0.8_amd64.deb)

Double-click the installer after download and follow the prompts on screen.

!!! note "Note for Mac Users"
    If you see a message that states it can’t open DART.app because it is from an unidentified developer: Open System Preferences > Security & Privacy and click Open Anyway. APTrust is working on getting an Apple Developer certificate, so this issue should be resolved in a later release.

## Set Up

After installation, DART includes two BagIt profiles by default, one for APTrust and one for DPN. You can use one of these two profiles to create your first job. While jobs can include packaging, validation, and upload operations, you won't be able to upload anything until you've set up a [Storage Service](settings/storage_services.md) to receive an upload. With this, your first job will be limited to creating and validating a BagIt bag.

## Running Your First Job

Follow these steps to create a valid local bag that conforms to the APTrust BagIt profile:

1. Choose __Jobs &gt; New__ from the main menu.

    ![New job](../img/getting_started/new_job.png)

2. Drag some files or folders into the files window. To avoid a long-running job that will consume a lot of disk space, choose only a few megabytes of files.

    ![Job files](../img/jobs/files.png)

3. Click __Next__ and set the following:

    1. __Packaging Format__: BagIt
    1. __BagIt Profile__: APTrust
    1. __Package Name__: test_bag
    1. __Output Path__: Don't edit this for now. DART will set it for you.

    ![Job packaging](../img/jobs/packaging.png)

4. Click __Next__ and set the required metadata attributes, which are marked with a red asterisk. For an APTrust bag, these include the __Access__ and __Title__ tags in the aptrust-info.txt file, and the __Source-Organization__ tag in the bag-info.txt file.

    ![Job metadata](../img/jobs/metadata.png)

5. Click __Next__ to choose the upload targets. Since there will be no available upload targets after initial installtion, Click __Next__ again.

6. The __Review and Run__ screen shows the details of the job you've just defined. Click __Run Job__ to run the job.

    ![Job run](../img/jobs/run.png)

When the job is complete, you'll find your bag in the __Output Path__ displayed on the __Review and Run__ screen.

## Further Reading

[BagIt Profiles](../bagit/) describes how to build an customize BagIt profiles, so DART can build bags exactly as you want them. Be sure to read the section on tag default values under [Editing a Tag](../bagit/customizing/#editing-a-tag). That will save you from re-entering common tag values, such as Source-Organization, every time you create a new bag.

The [Dashboard](dashboard.md) displays information about currently running and recently completed jobs. If you've configured [Remote Repository](settings/remote_repositories.md) connections, the dashboard can show the status of recent and pending ingests.

To get details about what DART is doing, check the [Logs](logs.md).

The [Settings](settings/index.md) section describes how to set up upload targets, remote repository connections, and more.

Organizations that distribute DART to their members can [Export Settings](settings/index.md) to help users quickly configure DART for their needs.

After you've defined and tested a successful job, you can convert it to a [Workflow](workflows/index.md) to run other files through the same process.
