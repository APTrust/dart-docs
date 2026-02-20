# Command Line Usage

Unlike DART 2, DART 3 does not support command-line operations. It runs only as a graphical UI.

Under the hood, DART 3 uses DART Runner to bag, validate, upload and download files. This means that you can still run workflow batches from the command line, but you will be issuing the commands to DART Runner instead of DART.

The best way to think of the relationship between DART and DART Runner is that DART provides a UI to help you define _what needs to be done_, while DART Runner carries out the actual work. DART contains an internal, embedded version of DART Runner, but if you want to script DART Runner separately, you will need to install it as a stand-alone app. (See details below.)

A typical use case for command line usage would look like this:

1. You define a [Workflow](workflows/index.md) in DART 3.
2. You [export that Workflow](workflows/index.md#exporting-a-workflow) as a JSON file.
3. You create a [Batch file](workflows/batch_jobs.md) describing which folders that you want to run through this workflow, bagging them all according to the same profile and uploading them all to the same place.
4. You tell DART Runner to [run all of the folders in the batch file](../../runner/index.md#usage) through the specified workflow.

At this point, you can let DART runner run unattended, as building and uploading thousands of bags may take hours or even days. For one-off jobs, you can create a batch file with just a single entry describing one folder to bag and upload, then run that through your workflow using DART Runner.

Note that if you want to run DART Runner from the command line, you will have to download and install it separately. See the downloads section of the [DART Runner](../../runner/index.md#downloads) page for info on where to get it.

DART Runner supports a number of advanced scripting options. See the [DART Runner](../../runner/index.md) page for details.
