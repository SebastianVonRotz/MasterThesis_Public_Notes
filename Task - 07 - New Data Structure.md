### Goal
* [x] Create the new data structure for automated workflow
* [x] Define the new structure
* [x] Setup new task commands for the new structure
* [x] Moved all files from script to script_old
* [x] Created new Basecalling Guppy Array file
* [x] Created a new env file
* [x] Create new run-script() which combines upload and running of files
* [x] Create new Demutplexing files
* [x] Create an overview of the files created
* [ ] Create demultiplex_qcat_job script
# New structure
### File types and task commands
* Manifest File
* Slurm Job Files
* Workflow File
* Reporting File
* New Task for uploading and executing a workflow

### Manifest File
* Defines Parameters for each Manifest File
* Input for the Workflow File
	* None

### Workflow File
* Defines raw data input
* Defines a workflow-run-name
* Creates directory strucutre based on the workflow steps
* Runs each Slurm job file with the necessary inputs
* Input for the Workflow File
	* None 

### Slurm Job Files
* **Input** for the Slurm Job File
	* Parameters from the Manifest files
	* workflow-run-name for data input and output
*** Output** of the Slurm Job File
	* Data into directory name according to workflow-run-name

### Reporting File

# Setup and test new workflow file concept
### Create files
* Manifest file: "wp_02_manifest"
* Workflow file: "wp_02_workflow.sh"
* Slurm Job files: "wp_02_basecalling_guppy.sh" / "wp_02_demultiplexing_qcat.sh"

### Adapt files

Adapted Grep command with: 
	
	| egrep -v "(^#.*|^$)"

This makes it able to write comments in the manifest file

	egrep -v      means leave the following out
	^#.*          means patterns that begin with a #
	|             means or
	^$            means patterns that are empty
	
**!!! This is replaced with the source command (Problems with comments was probably more about the unix dos format of the file) !!!**
	
### Testrun
Uploaded the scripts manually on ther server and used:
	
	./wp_02_workflow.sh	

Run IDs: 960804 / 960803

### Error Handling adaption

Added

	if [ -f crash ]; then exit 1; fi
	
to check if a previous slurm has crashed. slurm job crashed the following coded generates the crash file

	if [ $? != 0 ]; then touch crash; fi

Also changed to the export of the manifest file content with

	source $MANIFEST_FILE

Tested with

	./wp_02_basecalling_guppy.sh 
	
Run ID: The script wp_02_basecalling_guppy.sh is running with the job id 960861
The script wp_02_demultiplexing_qcat.sh is running with the job id 960862

___
# !!!!!!!!!!!
# v0.1
**From here on commands and the setup changes drastically, the new setup is outlined in [[WP - 03]]**
# !!!!!!!!!!!
___
# New Repository structure
### Goals
* Make the repository available and for every user
* Results are aggregated by the name of the dataset and the name of the processing run
* Create standalone scripts and workflow scripts

### New Task commands
* The command "task run-script" is added which uploads and executes a script at the same time

### New files
#### env
The remote path is defined for each user

	REMOTE_PATH=/cfs/earth/scratch/voro/scripts

A name for the dataset has to be defined

	DATASET_NAME=Dataset_5

And were the raw data is located (Only necessary for basecalling steps)

	DATA_PATH_IN=../data/Dataset_5_Array/
	
A run name for the input and for the output has to be defined. Normally they have the same name, except if a certain step needs to be processed and the new data should not be replaced
	
	RUN_NAME_IN=Step_01
	RUN_NAME_OUT=Step_01
	
The flowcell and kit need to be defined for several scripts

	FLOWCELL=FLO-MIN106
	KIT=SQK-RAD002
	
Define where the guppy_basecaller is installed

	GUPPY_BASECALLER_PATH=../local_apps/ont-guppy-cpu/bin/guppy_basecaller
	
Define where the guppy_barcoder is insstalled
	
	GUPPY_DEMULTIPLEXING_PATH=../local_apps/ont-guppy-cpu/bin//guppy_barcoder

### scripts
[[script_definition]]