# Frequently Asked Questions

## What is DART's bag size limit?

DART can create bags of almost any size, provided you have enough disk space.
Several APTrust depositors have used DART to create and upload bags in the
500 GB - 800 GB range. The largest so far has been 1.5 TB.

### Things to look out for when creating large bags

* Be sure you have enough disk space to actually bag the files. In some cases,
may need to reset the output path for a large bag, to point to an external
drive. See the Output Path setting in [packaging](jobs/packaging.md) for
details.
* If you're bagging files from a shared network drive with a spotty connection,
the bagging operation may fail if the network connection is interrupted. In
these cases, you may have more success creating ten 10 GB bags than one 100 GB
bag.
* DART validates bags after it creates them, and can take hours to validate
very large bags. DART spends most of this time validating file checksums against
the manifests. Be patient!
* Uploading large bags to an S3 or SFTP server can take a long time, especially
if your network connection is slow or unreliable.
* The maximum size for S3 objects is 5 TB. If you create a bag larger than that,
you will not be able to upload it to S3. SFTP file size limits are set
individually for each system by the administrator.

## Why doesn't DART preserve empty folders when bagging?

DART ignores empty folders to keep in line with the original APTrust bagging guidelines from 2014. APTrust uses a number of S3-compliant storage backends to preserve depositor data. We take the bag apart, store files individually, then reassemble the bag in the latest BagIt format for restoration. (Restoration may occur years after ingest, when the BagIt spec has changed.)

S3 can store empty files, but not empty folders. While we could accept a bag containing empty folders, we would have no way of restoring those empty folders later.

The workaround for this is to put empty `.keep` files in the empty folders you want to preserve. PHP and some other programming languages use this practice.

## Where can I report bugs or request new features?

We keep track of bug reports and feature requests at https://github.com/APTrust/dart/issues.

When reporting an issue, please check the list to see if someone else has already reported it. If the issue exists, add your comments instead of opening a new duplicate issue.

For bug reports, try to include the following:

* A one-line summary of the problem
* A screenshot or copy of an error message you encountered
* The steps you took to produce or expose the problem
* What you expected to happen after taking these steps
* What actually happened
