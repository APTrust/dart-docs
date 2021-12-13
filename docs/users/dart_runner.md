# DART Runner

Because DART uses the Electron framework, it requires the presence of a graphical user interface and a windowing system, even when it's not going to use a GUI. That means it can't even start in comman-line mode unless it's running in a desktop environment. This limitation is inherent in Electron and makes DART unsuitable for running on a Linux server.

For this reason, APTrust built dart-runner, which is a lightweight command-line version of DART that can run in server environments without a GUI. Dart-runner is intended to run workflows that were created and tested in DART.

The general process is:

1. Create a Workflow in DART and test it out locally to ensure it does what you want.
2. Export the workflow [as described on the Workflows page](../workflows/#exporting-a-workflow).
3. Run dart-runner on the server with the exported workflow file and a list of items you want to run through that workflow. If you're running a batch job, the list must conform to the workflow [CSV format used for batch jobs](../workflows/batch_jobs/). If you're running one-off jobs using Job Params, be sure your JSON conforms to the format below. See below for details.

!!! note
    When scripting jobs and workflows on Mac and Windows, you should
    stick with the DART CLI, since it's more mature as of late 2021.
    Since Windows and Mac include a GUI environment by default, DART
    and the DART CLI will always work.

## Downloads

Download the [0.91 beta version of dart-runner for Linux](https://s3.amazonaws.com/aptrust.public.download/dart-runner/v0.91-beta/linux-x64/dart-runner).

There's also a [Mac version of the beta](https://s3.amazonaws.com/aptrust.public.download/dart-runner/v0.91-beta/mac-x64/dart-runner) if you want to experiment, but for now, APTrust suggests using the DART CLI on Mac.

Because it's a single binary with no dependencies, there's no installation process for dart-runner. Simply copy the binary onto your computer and run.

If you're interested, the souce code is available on GitHub at [https://github.com/APTrust/dart-runner](https://github.com/APTrust/dart-runner).

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

## Differences in Job Params JSON

The Job Params JSON format for dart-runner differs slightly from the JSON used in the DART CLI. Specifically:

* tags use "value" instead of "userValue" and
* you don't need to specify the workflow name or ID in the json, because it's specified as a file name on the command line

The help text below shows an example of valid Job Params JSON for dart-runner.

## Usage Examples

### Running a Workflow

The following command runs all items in the CSV file through the specified workflow:

`dart-runner --workflow=workflow.json --batch=batch.csv --output-dir=/home/user/bags`

Bags will be written the --output_dir and deleted after successful upload. If you want to keep the bags after upload, add this to the end of the command:

`--delete=false`

We'll run this example, which creates three bags (as defined in the batch file) and uploads them to a local S3 server.

```
dart-runner --workflow=./testdata/files/postbuild_test_workflow.json \
            --batch=./testdata/files/postbuild_test_batch.csv \
            --output-dir=/Users/apd4n/tmp/bags
```

### Running One-Off Jobs

You can run individual jobs through DART runner by sending JSON to the command's standard input. For example:

`echo '{ json }' | dart-runner --workflow=workflow.json --output-dir=/dir`

`cat job_params.json | dart-runner --workflow=workflow.json --output-dir=/dir`

Typically, you'd have a script create the json, then pipe it to dart-runner. The JSON format looks like this:

```json
{
	"packageName": "TestBag.tar",
	"files": [
	    "/Users/apd4n/aptrust/dart-runner/core",
	    "/Users/apd4n/aptrust/dart-runner/bagit"],
	"tags": [{
		"tagFile": "aptrust-info.txt",
		"tagName": "Title",
		"value": "Runner Sample Bag"
	}, {
		"tagFile": "aptrust-info.txt",
		"tagName": "Description",
		"value": "Sample bag made with DART runner"
	}, {
		"tagFile": "aptrust-info.txt",
		"tagName": "Access",
		"value": "Institution"
	}]
}
```

To view the built-in docs, run `dart-runner --help`, the contents of which appear below.

```
DART Runner: Bag and ship files from the command line.

To use DART Runner, you typically want to define a job or workflow in the DART
UI, then export it as a json file to be consumed by DART Runner. See the
Resources section below.

-------
Options
-------

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
                 work is reading from and writing to disk. This setting only
                 makes sense for workflows. For a single job, you can omit
                 this.

  --help         Show this help document.


--------
Examples
--------

If you're running a single job, you can send job params to dart-runner through
STDIN, like this:

    echo '{ json }' | dart-runner --workflow=workflow.json --output-dir=/dir

The job params json tells dart-runner which files to bag and what tag values
to set. The workflow tells dart-runner which BagIt profile to use and where to
send the bag.

You can also feed the contents of a Job Params file to STDIN, like this:

    cat job_params.json | dart-runner --workflow=workflow.json --output-dir=/dir

This runs the job described in the job_params.json file, writing the bag to the
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

----------------------
Sample Job Params JSON
----------------------

The following job params tell dart-runner to bag all of the files in
/home/linus/documents and /home/linus/files. This also tells the bagger to
write two tags with the assigned values into the bag-info.txt file and
one tag into the aptrust-info.txt file.

These job params would be combined with a workflow JSON file that would
tell dart runner which BagIt profile to use when creating the bag, and where
to send the bag when it's done.

{
	"packageName": "TestBag.tar",
	"files": [
		"/home/linus/documents",
		"/home/linus/photos"
	],
	"tags": [
		{
			"tagFile": "bag-info.txt",
			"tagName": "Tag-One",
			"value":   "Value One"
		},
		{
			"tagFile": "bag-info.txt",
			"tagName": "Tag-Two",
			"value":   "Value Two"
		},
		{
			"tagFile": "aptrust-info.txt",
			"tagName": "Tag-Three",
			"value": "Value Three"
		}
	]
}

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
