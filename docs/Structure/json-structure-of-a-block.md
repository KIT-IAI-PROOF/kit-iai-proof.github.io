# JSON structure of a Block

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

##Description of the JSON Fields

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

