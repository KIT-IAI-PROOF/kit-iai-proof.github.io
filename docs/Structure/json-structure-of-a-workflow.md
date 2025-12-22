# JSON structure of a Workflow

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


##Description of the JSON Fields

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

