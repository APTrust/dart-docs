# Command Line Reference

DART provides several ways of running jobs from the command line. The most convenient method, and the easiest to script, is to define a workflow and then create a [JobParams](workflows/job_params.md) object that describes which files to pass through the workflow and what custom metadata attributes to assign.

Note that in all of these examples, the `dart` command is followed by two dashes, and then the command-line parameters. Parameters to the left of the double dash will be consumed by the Node.js runtime, while those to the right will be passed to DART.



!!! note
        Replace `dart` in the examples below with the full path to DART on your computer.

        On Windows, the default path to the DART executable is `C:\Users\<USERNAME>\AppData\Local\Programs\DART\DART.exe`. Replace &lt;USERNAME&gt; with your user name.

        On Mac, the default path to DART is `/Applications/DART.app/Contents/MacOS/DART`.


* From a JobParams JSON file

    `dart -- --job path/to/job_params.json`

* From JobParams JSON passed through STDIN

    `echo "{ ... json ... }" | dart -- --stdin`

* From a Job JSON file

    `dart -- --job path/to/job.json`

    Note: DART does not yet export jobs to JSON. That feature is in the works.

* From Job JSON passed through STDIN

    `echo { ... json ... } | dart -- --stdin`

    !!! note
        Job JSON can be relatively large, since it may include a BagIt profile. Using JobParams is generally easier than using Jobs.

* By Job UUID

    `dart -- --job 00000000-0000-0000-0000-000000000000`

    !!! note
        The ability to extract Job UUIDs from DART is coming soon.

As of this writing (July 22, 2019), some elements of the CLI are subject to change and further development.

## Resource Usage and Known Issues

DART uses Node.js, which is a relatively fast, efficient JavaScript runtime. Node, however, can use large amounts of memory, often 10-30 times what a similar application written in Go or C++ may use. You can expect each DART command-line process to use at least 50 MB of RAM on startup, and sometimes over 150 MB at runtime if it when creating large packages.

### Potentially High Memory Usage During S3 Uploads

DART consumes the most memory when uploading large files to an S3-compliant endpoint. S3 clients need to load chunks of data into memory before streaming them across the network. When uploading a file of 100 MB, each chunk may be only 5-50 MB in size, increasing DART's memory usage by that amound until the chunk has been copied to the remote server.

S3 allows uploads of up to 5 TB, with a maximum of 10,000 chunks. This means that when uploading a 5 TB file, DART's underlying S3 client will load chunks of about 535 MB each into memory. That's a considerable amount of memory, particularly on a computer that likely has a number of other open appilcations. For this reason, it's best to avoid running multiple simultaneous multi-terabyte jobs.

### Disk Usage When Creating Large Packages

When you're packaging very large files, keep in mind that you may run out of disk space. For example, DART needs about 5 TB of disk space to create a 5 TB bag. DART is fairly efficient about this, writing contents directly into a tar archive when possible to avoid multiple copies, and calculating all checksums in a single pass during the writes. However, if you're creating a 5 TB bag and you don't have 5 TB of disk space, the job is going to fail.

Note that you can change the location of your bagging directory by choosing __Settings &gt; App Settings__ from the menu and then clicking __Bagging Directory__. You can also change your bagging directory for a single job by setting the Output Path for that job. If you need extra disk space for a particular job, consider using a network share or an external USB drive as your bagging directory.

## Checking the Logs for Failed Jobs

DART logs all of its work. If a job fails, check the [logs](logs.md) for details.

## Sharing Jobs

Note that because details of jobs, workflows, and storage services are stored on your local DART computer, workflows and JobParams defined on one machine will not run on another machine that does not have matching settings.
