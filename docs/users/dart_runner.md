# DART Runner

Because DART uses the Electron framework, it requires the presence of a graphical user interface and a windowing system, even when it's not going to use a GUI. That means it can't even start in comman-line mode unless it's running in a desktop environment. This limitation is inherent in Electron and makes DART unsuitable for running on a Linux server.

For this reason, APTrust built dart-runner, which is a lightweight command-line version of DART that can run in server environments without a GUI. Dart-runner is intended to run workflows that were created and tested in DART.

The general process is:

1. Create a Workflow in DART and test it out locally to ensure it does what you want.
2. Export the workflow [as described on the Workflows page](workflows/index.md#exporting-a-workflow).
3. Run dart-runner on the server with the exported workflow file and a list of items you want to run through that workflow. If you're running a batch job, the list must conform to the workflow [CSV format used for batch jobs](workflows/batch_jobs.md/). If you're running one-off jobs using Job Params, be sure your JSON conforms to the format below. See below for details.

!!! note
    When scripting jobs and workflows on Mac and Windows, you should
    stick with the DART CLI, since it's more mature as of late 2021.
    Since Windows and Mac include a GUI environment by default, DART
    and the DART CLI will always work.

## Downloads

!!! danger
    Versions of DART Runner prior to 0.96-beta have a bug that leads
    to incorrect tag values being written into bags when using
    workflow batch mode. Please download version 0.96-beta or later.

Latest version is v0.96-beta, released March 9, 2023.

Download the [0.96 beta version of dart-runner for Linux](https://s3.amazonaws.com/aptrust.public.download/dart-runner/v0.96-beta/linux-x64/dart-runner).

There's also a [Mac-Intel version of the beta](https://s3.amazonaws.com/aptrust.public.download/dart-runner/v0.96-beta/mac-x64/dart-runner) and an [ARM version for M1 and M2 Macs](https://s3.amazonaws.com/aptrust.public.download/dart-runner/v0.96-beta/mac-arm64/dart-runner) if you want to experiment, but for now, APTrust suggests using the DART CLI on Mac.

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

## Usage

### Options

```
  --workflow     Path to workflow json file. Use this option if you are running
                 a workflow against a batch of files. If you specify a workflow
                 file, you must also specify --batch. Workflows can be exported
                 from the DART UI.

  --batch        Path to CSV batch file. Use this option with --workflow to
                 specify a set of files or directories to run through a
                 workflow. The batch file format is described at
                 https://aptrust.github.io/dart-docs/users/workflows/batch_jobs/

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
```

### Examples

If you're running a single job, you can send job params to dart-runner through
STDIN, like this:

    echo '{ json }' | dart-runner --workflow=workflow.json --output-dir=/dir

The job params json tells dart-runner which files to bag and what tag values
to set. The workflow tells dart-runner which BagIt profile to use and where to
send the bag.

You can also feed the contents of a Job Params file to STDIN, as in either example below:

    dart-runner --workflow=workflow.json --output-dir=/dir < job_params.json

    cat job_params.json | dart-runner --workflow=workflow.json --output-dir=/dir

This runs the job described in the job_params.json file, writing the bag to the
specified output directory.

To run a workflow:

    dart-runner --workflow=path/to/workflow.json  \
                --batch=path/to/batch.csv         \
                --output-dir=path/to/directory    \
                --concurrency=2                   \
                --delete=false

The command above runs all of the items listed in the `--batch` CSV file through
the workflow described in the `--workflow` json file. Bags are written to the
output directory. Setting the delete flag to false means the bags will not be
deleted from the output directory after successful upload.

The `--concurrency` flag above tells DART runner to work on 2 bags at a time
(instead of the default 1 at a time) when bagging and uploading.

Setting `--delete` to true (or omitting `--delete`) will cause bags to be deleted
after successful upload.

### Exit Codes

```
    0 - Normal exit. This means there were no errors and all tasks
        succeeded.

    1 - Runtime error. This means dart runner was able to start the
        job, but encountered one or more errors along the way.

        In a single job, this usually means at least one step of the job
        failed: bagging, validation, or upload. Check the JSON output
        for more details about where the failure occurred and what
        happened.

        When running a batch of jobs through a workflow, this exit code
        means that one or more of the jobs in the batch failed. In this
        case, you should find an error message on stderr saying something
        like "2 Job(s) failed".

    2 - Usage error. This means dart runner didn't even attempt to start
        the job because something was wrong with the parameters. This
        usually means you've forgotten to provide a necessary parameter
        such as --workflow, or that the parameter points to a non-existant
        or unreadable file. This error also occurs when a parameter contains
        invalid or unparsable data (bad JSON or bad CSV format).

        You should see a message on stderr describing the problem.
```


### Sample Job Params JSON


The following job params tell dart-runner to bag all of the files in
/home/linus/documents and /home/linus/files. This also tells the bagger to
write two tags with the assigned values into the bag-info.txt file and
one tag into the aptrust-info.txt file.

These job params would be combined with a workflow JSON file that would
tell dart runner which BagIt profile to use when creating the bag, and where
to send the bag when it's done.

``` json
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
```

### Output Format

For each completed job, DART Runner prints one line of JSON to stdout (standard output, which is usually a terminal). This means that when running a single job, you'll get one line of output. When running a batch job, you'll get one line for each entry in the CSV batch file.

DART Runner also prints summary messages to stderr (standard error) when errors occur, though the JSON output on stdout will have more details about what actually went wrong.

Output from a successful job looks like this:

``` json
{
	"jobName": "TestBag.tar",
	"payloadByteCount": 170742,
	"payloadFileCount": 55,
	"succeeded": true,
	"packageResult": {
		"attempt": 1,
		"completed": "2021-12-14T14:50:50.684655-05:00",
		"errors": {},
		"fileMtime": "2021-12-14T14:50:50.684637052-05:00",
		"filepath": "/home/linustmp/bags/TestBag.tar",
		"filesize": 224256,
		"info": "",
		"operation": "package",
		"provider": "Bagger - DART Runner v0.91-beta-1-g70b06da for Darwin x86_64 (Build 70b06da 2021-12-14)",
		"remoteChecksum": "",
		"remoteURL": "",
		"started": "2021-12-14T14:50:50.66122-05:00",
		"warning": ""
	},
	"validationResult": {
		"attempt": 1,
		"completed": "2021-12-14T14:50:50.684665-05:00",
		"errors": {},
		"fileMtime": "2021-12-14T14:50:50.684637052-05:00",
		"filepath": "/home/linustmp/bags/TestBag.tar",
		"filesize": 224256,
		"info": "",
		"operation": "validation",
		"provider": "Validator - DART Runner v0.91-beta-1-g70b06da for Darwin x86_64 (Build 70b06da 2021-12-14)",
		"remoteChecksum": "",
		"remoteURL": "",
		"started": "2021-12-14T14:50:50.684656-05:00",
		"warning": ""
	},
	"uploadResults": [{
		"attempt": 1,
		"completed": "2021-12-14T14:50:50.733585-05:00",
		"errors": {},
		"fileMtime": "2021-12-14T14:50:50.684637052-05:00",
		"filepath": "/home/linustmp/bags/TestBag.tar",
		"filesize": 224256,
		"info": "Output file at /home/linustmp/bags/TestBag.tar was deleted at 2021-12-14T14:50:50-05:00",
		"operation": "upload",
		"provider": "Uploader - DART Runner v0.91-beta-1-g70b06da for Darwin x86_64 (Build 70b06da 2021-12-14)",
		"remoteChecksum": "09f4a6d159e07cd11c82ead5d2a3e95c-1",
		"remoteURL": "s3://localhost:9899/dart-runner.test/TestBag.tar",
		"started": "2021-12-14T14:50:50.684666-05:00",
		"warning": ""
	}],
	"validationErrors": null
}
```

The most important elements in this output are:

```
  succeeded       - true or false, indicating whether all steps of the job
                    succeeded

  errors          - Within each result, this will contain a set of name-value
                    pairs indicating what went wrong. This element will be
                    empty if the operation succeeded.

  remoteURL       - The URL to which the package was uploaded. (Applies
                    only to uploadResults.)

  remoteChecksum  - The ETag returned by the remote S3 server after a
                    successful upload. (Applies only to uploadResults for
                    S3 uploads.)
```


## Scripting

Most scripting languages provide ways of capturing the stdout and stderr output of external programs called from within the script. Most languages also let you capture the external process' return code.

When scriping DART Runner, you should generally do the following:

1. Capture the exit code. If it's not zero, log a message or perform some
   other kind of error handling.
2. Redirect stdout to a file if you want to save all the JSON instead of
   capturing it.

You can redirect stdout to a file like this:

    dart-runner [args] > output_log.json


## Additional Resources

* [DART Runner Source on GitHub](https://github.com/APTrust/dart-runner)
* [DART Batch File Format](https://aptrust.github.io/dart-docs/users/workflows/batch_jobs/)
