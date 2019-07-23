# Scripting with DART

The easiest way to script DART jobs is to:

1. Create a [workflow](workflows/index.md).

    !!! info
        If your workflow includes generating bags, you should also create a
        custom BagIt profile with default values so that your script will only
        need to create a few bag-specific tag values at runtime. See
        [Customizing BagIt Profiles](bagit/customizing.md) for more info.

1. Create a simple data object that can be transformed into a JSON structure that matches DART's [JobParams](workflows/job_params.md) object.

1. Pass the JSON to the DART command through STDIN.

The examples below show how to do this in Ruby and Python. Note that both examples create the following JSON structure.

```json
{
	"workflowName": "DART Test Workflow",
	"packageName": "test.edu.my_files.tar",
	"files": ["/Users/apd4n/aptrust/dart-docs/site", "/Users/apd4n/tmp/logs"],
	"tags": [{
		"tagFile": "bag-info.txt",
		"tagName": "Bag-Group-Identifier",
		"userValue": "TestGroup_001"
	}, {
		"tagFile": "aptrust-info.txt",
		"tagName": "Title",
		"userValue": "Workflow Test Files"
	}, {
		"tagFile": "aptrust-info.txt",
		"tagName": "Description",
		"userValue": "Contains miscellaneous files for workflow testing."
	}]
}
```

# Example: Ruby

To script DART actions, you'll need one simple Ruby class that allows you to define and run a job. You can cut and paste this to get started, but note that you will likely have to change the value of `@@dart_command` to point to the DART executable on your local machine.

```ruby
require 'json'

class Job

  # Be sure to set this appropriately for your system.
  # The command 'npm start' is for DART development use only.
  @@dart_command = 'npm start'

  attr_accessor :workflow_name, :package_name, :files, :tags

  def initialize(workflow_name, package_name)
    @workflow_name = workflow_name
    @package_name = package_name
    @files = []
    @tags = []
  end

  def to_json(options={})
    {
      workflowName: workflow_name,
      packageName: package_name,
      files: files,
      tags: tags
    }.to_json
  end

  def add_file(path)
    @files << path
  end

  def add_tag(tag_file, tag_name, value)
    @tags << {
      tagFile: tag_file,
      tagName: tag_name,
      userValue: value
    }
  end

  def run
    json_string = self.to_json
    puts json_string
    puts "Starting job"
    opts = {
      external_encoding: "UTF-8",
      err: [:child, :out]
    }
    IO.popen("#{@@dart_command} -- --stdin", "r+", opts) {|pipe|
      pipe.write json_string + "\n"
      pipe.close_write
      puts pipe.read
    }
    return $?.exitstatus
  end
end
```

Assuming you saved the code above to a file called `job.rb`, you can create a script like the following to run a job based on a pre-defined DART workflow.

```ruby
require './job'

# Create a new job using the "DART Test Workflow."
# This job will create a tarred bag called test.edu.my_files.tar
# in your DART bagging directory.

job = Job.new("DART Test Workflow", "test.edu.my_files.tar")

# Add two directories to the list of items that should go into
# the bag's payload. Note that you can add a mix of files and
# directories.

job.add_file("/Users/apd4n/aptrust/dart-docs/site")
job.add_file("/Users/apd4n/tmp/logs")

# "DART Test Workflow" uses a BagIt profile with a number of
# preset default values, which are fine for tags like Contact-Email,
# which doesn't change from bag to bag. Here we set bag-specific
# tag values.

job.add_tag("bag-info.txt", "Bag-Group-Identifier", "TestGroup_001")
job.add_tag("aptrust-info.txt", "Title", "Workflow Test Files")
job.add_tag("aptrust-info.txt", "Description", "Contains miscellaneous files for workflow testing.")

# Run the job and check the exit code. 0 indicates success.
# Non-zero values indicate failure.
exit_code = job.run()

if exit_code == 0
  puts "Job completed"
else
  puts "Job failed. Check the DART log for details."
end
```

# Example: Python

Coming soon...