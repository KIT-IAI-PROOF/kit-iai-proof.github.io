# JSON Structure of PROOF Elements

## JSON structure of a Workflow

For the import and export of PROOF *Workflows*, a JSON structure is used to define the *Workflow's* metadata and the references to associated **Blocks**.

A *Workflow* JSON file typically includes the following key components:

- **Workflow Metadata**: This section contains general information about the *Workflow*, such as its ID, name, description, etc.
- **Blocks**: This section contains an array of *Block* references that are part of the *Workflow*. Each *Block* reference includes its unique identifier and configuration details.
- **Connections**: This section defines the connections between the *Blocks* in the *Workflow*, specifying how data flows from one *Block* to another.
- **Parameters**: This section includes any additional parameters or settings required for the *Workflow* execution.



Here is as an example the JSON structure for the PROOF *Workflow* *FileReader2FileWriter*:

```json
{
  "id": "WF-FileReader2FileWriter",
  "name": "FileReader2FileWriter-Test",
  "description": "The FileReader2FileWriter demo workflow reads an arbitrary file and writes the content to another file.",
  "communicationParadigm": "STEPBASED",
  "blocks": [
    {
      "localId": 0,
      "globalId": "block_file_line_provider",
    },
    {
      "localId": 1,
      "globalId": "block_file_writer"
    }
  ],
  "connections": [
    {
      "inbound": {
        "blockId": "0",
        "portName": "line"
      },
      "outbound": {
        "blockId": "1",
        "portName": "data"
      }
    }
  ],
  "stepBasedConfig": {
    "startPoint": 1,
    "endPoint": 10,
    "duration": 500,
    "stepSizeDefinitions": {
      "1": {
        "startPoint": 2,
        "endPoint": 10,
        "defaultSize": 1,
        "stepSizes": {}
      }
    }
  },
  "simulationStrategy": "WAIT_AND_CONTINUE"
}

```
This JSON structure can be used to import the *Workflow* into a PROOF installation or export it for sharing with others. 
It is assumed that the referenced blocks already exist in th e PROOF database when the workflow is imported. 
@TODO : Add link to 'import workflows'.


### Description of the JSON Fields

- **id**: A unique identifier for the *Workflow*.
- **name**: The display name of the *Workflow*.
- **description**: A brief description of the *Workflow's* functionality.
- **communicationParadigm**: The communication paradigm used in the *Workflow* (e.g., STEPBASED).
- **blocks**: An array of *Block* references included in the *Workflow*. Each entry contains:
  - **localId**: A unique local identifier for the *Block* within the *Workflow*.
  - **globalId**: The global identifier of the *Block* template used.
- **connections**: An array defining the connections between the referenced *Blocks*. Each connection includes:
  - **inbound**: The localId of the source *Block* and port name. The port name must match the name of an output name of the source *Block*.
  - **outbound**: The localId of the destination *Block* and port name. The port name must match the name of an input name of the target *Block*.
- **stepBasedConfig**: Configuration details specific to step-based communication, including start and end points, duration, and step size definitions.
- **simulationStrategy**: The strategy used for simulation execution (e.g., WAIT_AND_CONTINUE).









## JSON structure of a Block

For the import and export of PROOF *Blocks*, a JSON structure is used to define the *Block's* metadata, inputs, outputs, and other configuration details. This structure allows for easy sharing and reuse of *Blocks* across different PROOF installations.
A *Block* JSON file typically includes the following key components:

- **Block Metadata**: This section contains general information about the *Block*, such as its ID, label, etc.
- **Inputs**: This section defines the inputs that the *Block* accepts, including their names, types, and whether they are required or optional.
- **Outputs**: This section defines the outputs that the *Block* produces, including their names and types.

Here is as an example the JSON structure for the PROOF *Block* *FileWriter*:

```json
{
  "id": "block_file_writer",
  "name": "File Writer",
  "description": "Write data to a file",
  "containerImage": "ghcr.io/kit-iai-proof/proof-worker-python:1.0.0",
  "communicationParadigm": "STEPBASED",
  "programId": "prog_file_writer",
  "inputs": [
    {
      "name": "file name",
      "type": "string",
      "phase": "INIT",
      "required": true,
      "modelVarname": "file_name",
      "communicationType": "STEPBASED_STATIC",
      "metadata": {}
    },
    {
      "name": "mode",
      "type": "string",
      "phase": "INIT",
      "modelVarname": "mode",
      "required": false,
      "communicationType": "STEPBASED_STATIC",
      "metadata": {}
    },
    {
      "name": "waitForSync",
      "type": "string",
      "phase": "INIT",
      "modelVarname": "waitForSync",
      "required": false,
      "communicationType": "STEPBASED_STATIC",
      "metadata": {}
    },
    {
      "name": "data",
      "type": "string",
      "phase": "EXECUTE",
      "modelVarname": "data",
      "required": false,
      "communicationType": "STEPBASED",
      "metadata": {}
    }
  ],
  "outputs": [],
  "startParameters": {}
}
```
This JSON structure can be used to import the *Block* into a PROOF installation or export it for sharing with others. The defined inputs and outputs ensure that the *Block* can be correctly integrated into PROOF *Workflows*.

### Description of the JSON Fields

- **id**: A unique identifier for the *Block*.
- **name**: The display name of the *Block*.
- **description**: A brief description of the *Block's* functionality.
- **containerImage**: The Docker container image used to run the *Block*.
- **communicationParadigm**: The communication paradigm used by the *Block* (e.g., STEPBASED).
- **programId**: The identifier of the *Program* associated with the *Block*. This is an own JSON file.
- **inputs**: An array of input definitions for the *Block*. Each input includes:
  - **name**: The visible name of the input.
  - **type**: The data type of the input (e.g., string, integer).
  - **phase**: The phase during which the input is used (e.g., INIT, EXECUTE).
  - **required**: A boolean indicating whether the input is required at startup.
  - **modelVarname**: The variable name used in the model for this input. In most cases, it matches the name.
  - **communicationType**: The type of communication for this input (e.g., STEPBASED_STATIC, STEPBASED).
  - **metadata**: Additional metadata for the input (can be any structure or empty).
- **outputs**: An array of output definitions for the *Block*. Each output includes:
  - **name**: The visible name of the output.
  - **type**: The data type of the output (e.g., string, integer).
  - **phase**: The phase during which the output is produced (e.g., EXECUTE).
  - **modelVarname**: The variable name used in the model for this output. In most cases, it matches the name.
  - **communicationType**: The type of communication for this output (e.g., STEPBASED).
  - **metadata**: Additional metadata for the output (can be any structure or empty).
- **startParameters**: A section for any additional parameters needed at the start and run of the *Block* (can be empty).



## JSON structure of a Program

For the import and export of PROOF *Programs*, a JSON structure is used to define the *Program's* metadata, associated *Attachments*, and other configuration details. This structure allows for easy sharing and reuse of *Programs* across different PROOF installations.

A *Program* JSON file typically includes the following key components:
- **Program Metadata**: This section contains general information about the *Program*, such as its ID, label, description, etc.
- **Attachments**: This section defines the *Attachments* associated with the *Program*, including the entry point file and any additional files required for execution.

Here is as an example the JSON structure for the PROOF *Program* *FileWriter*:

```json
{
  "id": "prog_file_writer",
  "name": "prog_file_writer",
  "description": "write data to a file",
  "tag": "test",
  "runtime": "PYTHON",
  "entryPoint": "attachment_file_writer",
  "attachments": [
    {
      "id": "attachment_file_writer"
    }
  ]
}
```

This JSON structure can be used to import the *Program* into a PROOF installation or export it for sharing with others. The defined *Attachments* ensure that the *Program* has all the necessary files for execution.

### Description of the JSON Fields

- **id**: A unique identifier for the *Program*.
- **name**: The display name of the *Program*.
- **description**: A brief description of the *Program's* functionality.
- **tag**: A tag or label for the *Program* (can be used for categorization).
- **runtime**: The runtime environment for the *Program* (e.g., PYTHON, JAVA).
- **entryPoint**: The identifier of the *Attachment* that serves as the entry point for the *Program* (i.e., the main executable file).
- **attachments**: An array of *Attachment* definitions associated with the *Program*. Each attachment includes:
  - **id**: The unique identifier of the *Attachment*.  

## JSON Structure of an Attachment

For the import and export of PROOF *Attachments*, a JSON structure is used to define the *Attachment's* metadata and file information. This structure allows for easy sharing and reuse of *Attachments* across different PROOF installations.
A *Attachment* JSON file typically includes the following key components:
- **Attachment Metadata**: This section contains general information about the *Attachment*, such as its ID, label, description, etc.
- **File Information**: This section defines the file associated with the *Attachment*, including its name and type.

Here is as an example the JSON structure for the PROOF *Attachment* *FileWriter*:

```json
{
  "id": "attachment_file_writer",
  "fileName": "file_writer.py",
  "description": "write data to a file"
}
```
