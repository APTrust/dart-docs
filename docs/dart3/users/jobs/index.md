# Jobs

A job is a set of actions that may include one or more of the following:

* Packaging a number of files into a defined format, such as BagIt, tar, etc.
* Validating the package.
* Uploading the package to one or more remote locations.

DART was initially designed for APTrust depositors to bag files according to the APTrust BagIt profile, validate the bags, and then send them to an S3 bucket for ingest into APTrust's preservation repository.

DART plugin architecture will allow it to handle similary patterned jobs that use different packaging formats and network protocols such as zip, rar, or parchive formats sent via FTP or rsync.

The process for creating and running jobs invlolves these steps:

1. [Adding files](files.md).
1. [Choosing a package format](packaging.md).
1. [Adding metadata](metadata.md).
1. [Choosing upload targets](upload.md).
1. [Reviewing and running the job](run.md).
1. [Troubleshooting](troubleshooting.md).

## Jobs and Workflows

Jobs can be converted to workflows. A [workflow](../workflows/index.md) is essentially a job template that can be run in the DART UI or from the command line. A workflow enables you to run a number of jobs that all follow the same pattern (same packaging format, same default metadata values, and same upload targets).
