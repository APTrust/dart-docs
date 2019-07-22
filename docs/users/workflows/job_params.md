# Job Params

The JobParams object is a simple JSON structure that gives a DART command-line job four essential pieces of information it needs to run a job. These are:

* __workflowName__ - The name of the workflow to run.
* __packageName__ - The name of the package to create.
* __files__ - A list of files and/or directories to put into the package and/or
  upload to a remote server. If the workflow incudes one or more upload operations
  but no packaging operation, the files will be uploaded directly.
* __tags__ - A list of tag names and values to add to the package.

Below is sample JobParams JSON describing which files should be passed through the "APTrust Demo System Workflow" and what tag values should be applied. In this case, two directories and one file will be packaged with four custom-defined tags.

```json
 {
 	"workflowName": "APTrust Demo System Workflow",
 	"packageName": "test.edu.my_files.tar",
 	"files": [
 		"/path/to/first/directory",
 		"/path/to/second/directory",
 		"/path/to/some/document.pdf"
 	],
 	"tags": [
 		{
 			"tagFile": "bag-info.txt",
 			"tagName": "Bag-Group-Identifier",
 			"userValue": "Photos_2019"
 		},
 		{
 			"tagFile": "aptrust-info.txt",
 			"tagName": "Title",
 			"userValue": "Photos from 2019"
 		},
 		{
 			"tagFile": "aptrust-info.txt",
 			"tagName": "Description",
 			"userValue": "What I did with my summer vacation."
 		},
 		{
 			"tagFile": "custom/legal-info.txt",
 			"tagName": "License",
 			"userValue": "https://creativecommons.org/publicdomain/zero/1.0/"
 		}
 	]
 }
```

Let's assume the hypothetical "APTrust Demo System Workflow" includes the following steps:

* Package all files according to our customized version of the APTrust BagIt profile.
* Validate the resulting bag.
* Upload the bag to our ingest bucket for the APTrust Demo system.
* Upload the bag to the S3 bucket in our local owncloud S3 service.

Passing the JSON above to the DART command line application will result in the following:

* The file `/path/to/some/document.pdf` and the contents of the directories `/path/to/first/directory` and `/path/to/second/directory` will be packed into the data directory of a new BagIt bag.

* DART will create and populate all of the tag files required by the APTrust BagIt profile. It will use the default values saved in your locally customized version of the profile where possible, overriding the defaults with the bag-specific values supplied in the JSON.

    !!! note
        Note that certain BagIt tag values, such as Source-Organization, Contact-Email, etc. will not change from bag to bag, so it makes sense to set default values for them in your customized profile. Other values, such as bag title and description, will change for every bag, so it makes sense to set them in the JobParams JSON.

* DART will save the entire package to a file whose name matches the package name. DART will write the file into your bagging directory, which is usually a directory called `.dart` under your home directory. You can verify (and change) the name of your bagging directory by choosing __App Settings__ from the DART menu and clicking on the setting called __Bagging Directory__.

* DART will verify the bag according to the APTrust BagIt profile (or whichever profile the Workflow specified).

* DART will upload the verified bag to APTrust's S3 ingest bucket.

* DART will upload the verified bag to your local ownCloud S3 bucket.

See also: [Workflows](index.md), [Command Line Reference](../command_line.md), [Scripting with DART](../scripting.md)

### Further Reading

You'll find more detailed developer documentation for the JobParams object in our [DART API documentation](https://aptrust.github.io/dart/JobParams.html).
