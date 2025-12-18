# Import and Export of PROOF Elements

PROOF provides functionality to import and export various elements such as *Workflows*, *Block Templates*, *Programs*, and *Attachments* . This allows users to easily share and reuse these elements across different PROOF instances or projects.

<!--The import and export functionality is accessible through the PROOF UI, where users can select the desired element to import or export. 
The elements are typically exported in a standardized format, such as JSON or ZIP files, which can then be imported into another PROOF instance. 
When exporting an element, PROOF ensures that all dependent elements are included in the export package. For example, when exporting a *Workflow*, all associated *Blocks*, *Programs*, and *Attachments* are also included to ensure that the *Workflow* can be imported and used in another PROOF instance without any missing dependencies

-->

The PROOF elements are ordered hierarchically, meaning that some elements rely on others to function correctly. The hierarchy is as follows:

- **Workflows**: The top-level element that defines the overall process and structure of a simulation or analysis.
- **Blocks**: The building blocks of a *Workflow*, which define specific tasks or operations to be performed.
- **Programs**: The executable code or scripts that are associated with *Blocks* to perform specific functions.
- **Attachments**: Additional files or resources that are required by *Programs* to execute correctly.

All elements are defined using a JSON structure, which includes metadata, configurations, and references to dependent elements. This standardized format allows for easy import and export of elements between different PROOF instances.
For more details on the JSON structure of each element, please refer to the respective documentation sections:

- [JSON Structure of a Workflow](structure/json-structure-of-a-workflow.md)
- [JSON Structure of a Block](structure/json-structure-of-a-block.md)
- 

A PROOF *Workflow* can be exported as a JSON file that contains all the necessary information about the workflow, including its structure, references to *Blocks", connections, and configurations. This JSON file can then be imported into another PROOF instance to recreate the same workflow.