# Job Flow

All DART jobs are run as individual processes. When you run a job from the DART UI, the application forks a child process and runs the job in that process. The code that forks the child is in the run method of the [JobRunController](https://github.com/APTrust/dart/blob/master/ui/controllers/job_run_controller.js), which is [documented here](https://aptrust.github.io/dart/JobRunController.html).

Forking child processes has a number of advantages over running jobs in the same process as the UI.

* You can run multiple jobs simultaneously.
* You can continue to use the UI while jobs run.
* A crashed job will not crash the UI.

The downside of running jobs in child processes is that if you close the DART application, you kill all of the running jobs. This is why DART always displays a badge in the upper right corner when jobs are running.

![Running jobs badge](../img/common/running_jobs_badge.png)

All DART jobs, whether run from the UI or from the command line, are launched from the run() method of [main.js](https://github.com/APTrust/dart/blob/master/main.js), which does the following:

1. Calls the [JobLoader](https://aptrust.github.io/dart/JobLoader.html) to load the job.

    !!! info
        The JobLoader figures out whether you're passing in a Job object or
        some other type of object from which it can construct a job. You'll
        typically pass in JSON representing a lightweight
        [JobParams](https://aptrust.github.io/dart/JobParams.html) object
        and the JobLoader will construct the job from that. The JobParams
        object simply names a set of files, a [Workflow](../users/workflows/index.md)
        to pass them through, and a set of metadata tags to add to the package.
        The [user documentaiton for JobParams](../users/workflows/job_params.md)
        will be helpful to developers.

1. Passes the [Job](https://aptrust.github.io/dart/Job.html) object to the [JobRunner](https://aptrust.github.io/dart/JobRunner.html) and then calls the JobRunner's run method.

1. Returns the exit code so that the script or application that launched the job can know whether it succeeded. An exit code of zero indicates success. Any other code indicates failure. These codes are spelled out in [core/constants.js](https://github.com/APTrust/dart/blob/master/core/constants.js) as follows:

```javascript
    /**
     * This exit code indicates a process completed successfully,
     * with no errors.
     *
     * @type {number}
     */
    EXIT_SUCCESS: 0,
    /**
     * This exit code indicates a process ran to completion, but one
     * or more errors occurred along the way.
     *
     * @type {number}
     */
    EXIT_COMPLETED_WITH_ERRORS: 1,
    /**
     * This exit code indicates a process did not complete
     * due to invalid parameters.
     *
     * @type {number}
     */
    EXIT_INVALID_PARAMS: 2,
    /**
     * This exit code indicates that the process did not complete
     * due to an unexpected runtime error.
     *
     * @type {number}
     */
     EXIT_RUNTIME_ERROR: 3,
```

## The JobRunner, Step by Step

The [JobRunner](https://aptrust.github.io/dart/JobRunner.html) does the following:

1. If the Job includes a [PackageOperation](https://aptrust.github.io/dart/PackageOperation.html), the JobRunner packages the files specified in the operation. The PackageOperation describes how DART should create a bag, or a tar file, or a zip file, or some other package. If a user wants to upload files without packaging, there will be no PackageOperation.

1. If the PackageOperation produced a BagIt bag (tarred or not), DART validates the bag. The JobRunner will create a [ValidationOperation](https://aptrust.github.io/dart/ValidationOperation.html) to describe what needs to be validated and how.

1. If the job includes any [UploadOperations](https://aptrust.github.io/dart/UploadOperation.html), DART will execute them one at a time, in sequence.

1. The JobRunner sets and returns the appropriate exit code. (Note that the most common cause of the EXIT_COMPLETED_WITH_ERRORS code is a job whose packaging and validation succeeded, along with some but not all of the upload operations.)

The JobRunner logs its work, and you can watch the operations unfold in the [logs](logging.md).

Each of the objects [PackageOperation](https://aptrust.github.io/dart/PackageOperation.html), [ValidationOperation](https://aptrust.github.io/dart/ValidationOperation.html), and [UploadOperation](https://aptrust.github.io/dart/UploadOperation.html) include an [OperationResult](https://aptrust.github.io/dart/OperationResult.html) object.

After a job is complete, each of the operations and their results are stored with the job record in the DART's jobs database, which is a plain text JSON file. You can open the file directly to examine the full state of the object. See [Data Storage](data_storage.md) for more information.

All of the code for running jobs lives in the [workers](https://github.com/APTrust/dart/tree/master/workers) folder of the DART source. The four key files have the following tasks:

* workers/job_runner.js orchestrates the work of bagging, validating, and uploading. Links: [source code](https://github.com/APTrust/dart/blob/master/workers/job_runner.js), [documentation](https://aptrust.github.io/dart/JobRunner.html), [tests](https://github.com/APTrust/dart/blob/master/workers/job_runner.test.js).

* workers/bag_creator.js creates bags for the PackageOperation step of the job. Links: [source code](https://github.com/APTrust/dart/blob/master/workers/job_runner.test.js), [documentation](https://aptrust.github.io/dart/BagCreator.html), [tests](https://github.com/APTrust/dart/blob/master/workers/bag_creator.test.js).

* workers/bag_validator.js validates that a bag is complete and conforms to a specified BagIt profile. Links: [source](https://github.com/APTrust/dart/blob/master/workers/bag_validator.js), [documentation](https://aptrust.github.io/dart/BagValidator.html), [tests](https://github.com/APTrust/dart/blob/master/workers/bag_validator.test.js).

* workers/uploader.js uploads files to a remote upload target. Links: [source](https://github.com/APTrust/dart/blob/master/workers/uploader.js), [documentation](https://aptrust.github.io/dart/Uploader.html), [tests](https://github.com/APTrust/dart/blob/master/workers/uploader.test.js).
