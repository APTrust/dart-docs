# DART Runner

Because DART uses the Electron framework, it requires the presence of a graphical user interface and a windowing system, even when it's not going to use a GUI. That means it can't even start in comman-line mode unless it's running in a desktop environment. This limitation is inherent in Electron and makes DART unsuitable for running on a Linux server.

For this reason, APTrust built dart-runner, which is a lightweight command-line version of DART that can run in server environments without a GUI. Dart-runner is intended to run workflows that were created and tested in DART.

The general process is:

1. Create a Workflow in DART and test it out locally to ensure it does what you want.
2. Export the workflow [as described on the Workflows page](../workflows/#exporting-a-workflow).
3. Run dart-runner on the server with the exported workflow file and a list of items you want to run through that workflow. The list must conform to the workflow [CSV format used for batch jobs](../workflows/batch_jobs/).

!!! note
    When scripting jobs and workflows on Mac and Windows, you should
    stick with the DART CLI, since it's more mature as of late 2021.
    Since Windows and Mac include a GUI environment by default, DART
    and the DART CLI will always work.

## Features

Dart-runner:

* is a single, lightweight binary, less than 10MB in size, with no external dependencies. To install, just copy the binary to your server.
* is much lighter on CPU and memory than DART.
* can run huge workflows unattended.
* can run multiple jobs in parallel.
* can be scripted from other languages like the DART CLI, though its syntax differs somewhat from the CLI.
* outputs clean, machine-readable JSON describing the outcome of every job. The output format is [JSON Lines](https://jsonlines.org/), with each line describing the outcome of a single job in the workflow.
* provides meaningful error messages when errors occur.
* supports piping, so you can send its output to any file you want for later analysis.
* provides meaningful return codes, so your script knows whether a job succeeded or failed.

## Limitations

Dart-runner is currently in beta and has the following limitations:

* It supports only the BagIt packaging format.
* It supports only S3 uploads (no SFTP).
* It's intended primarily for use on Linux servers.

## Getting Started

Download the initial [0.9 beta version of dart-runner for Linux](https://s3.amazonaws.com/aptrust.public.download/dart-runner/v0.9-beta/linux-x64/dart-runner).

There's also a [Mac version of the beta](https://s3.amazonaws.com/aptrust.public.download/dart-runner/v0.9-beta/mac-x64/dart-runner) if you want to experiment, but for now, APTrust suggests using the DART CLI on Mac.

Because it's a single binary with no dependencies, there's no installation process for dart-runner. Simply copy the binary onto your computer and run.

If you're interested, the souce code is available on GitHub at [https://github.com/APTrust/dart-runner](https://github.com/APTrust/dart-runner).

To view the built-in docs, run `dart-runner --help`, the contents of which appear below.

```
DART Runner: Bag and ship files from the command line.

To use DART Runner, you typically want to define a job or workflow in the DART
UI, then export it as a json file to be consumed by DART Runner. See the
Resources section below.

-------
Options
-------

  --job          Path to job json file. Use this option only if you are running
                 a single job (as opposed to a workflow). Job json files can
                 be exported from the DART UI.

  --workflow     Path to workflow json file. Use this option if you are running
                 a workflow against a batch of files. If you specify a workflow
                 file, you must also specify --batch. Workflows can be exported
                 from the DART UI.

  --batch        Path to CSV batch file. Use this option with --workflow to
                 specify a set of files or directories to run through a
                 workflow.

  --output-dir   Path to package output directory. Jobs and workflows will
                 create bags in this directory. This option is always REQUIRED.

  --delete       Delete bags after job completes? Set this to true or false.
                 The default is true for jobs and workflows that include
                 uploads: the bags will be deleted after successful uploads.
                 Default is false for jobs and workflows that do not include
                 uploads because you probably want to do something with the bag
                 after it's created.

  --concurrency  Number of jobs to run concurrently. Default is 1. Max value
                 for this param should be less than or equal to the number of
                 processors on your machine. You may get diminishing returns
                 when setting this above 2 because most of the DART runner's
                 work is reading from and writing to disk.

  --help         Show this help document.


--------
Examples
--------

To run a single job:

    dart-runner --job=path/to/job.json --output-dir=path/to/output

This runs the job described in the job.json file, writing the bag to the
specified output directory.

To run a workflow:

    dart-runner --workflow=path/to/workflow.json  \
                --batch=path/to/batch.csv         \
                --output-dir=path/to/directory    \
                --concurrency=2                   \
                --delete=false

The command above runs all of the items listed in the --batch CSV file through
the workflow described in the --workflow json file. Bags are written to the
output directory. Setting the delete flag to false means the bags will not be
deleted from the output directory after successful upload.

The --concurrency flag above tells DART runner to work on 2 bags at a time
(instead of the default 1 at a time) when bagging and uploading.

Setting --delete to true (or omitting --delete) will cause bags to be deleted
after successful upload.

---------
Resources
---------

DART
    Source:        https://github.com/APTrust/dart
    User Guide:    https://aptrust.github.io/dart-docs/

DART Runner
    Source:        https://github.com/APTrust/dart-runner
    User Guide:    https://aptrust.github.io/dart-docs/users/dart-runner/

DART and DART Runner are free and open source projects from APTrust.org.

```
