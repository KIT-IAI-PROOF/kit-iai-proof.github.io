# Import and Export of PROOF Elements

PROOF provides functionality to <!--import and -->export various elements such as *Executions*, *Workflows* and *Templates*. This allows users to easily share these elements across different PROOF instances or projects.

The <!--import and -->export functionality is accessible through the PROOF UI, where users can select the desired element to <!--import or -->export.
The elements are exported as a ZIP file with JSON files per attachment, program, template and workflow. <!--This can then be imported into another PROOF instance.-->
When exporting an element, PROOF ensures that all dependent elements are included in the export package. For example, when exporting a *Workflow*, all associated *Blocks*, *Programs*, and *Attachments* are also included<!-- to ensure that the *Workflow* can be imported and used in another PROOF instance without any missing dependencies-->.

## Element Hierarchy

The PROOF elements are ordered hierarchically, meaning that some elements rely on others to function correctly. The hierarchy is as follows:

- **Workflows**: The top-level element that defines the overall process and structure of a simulation or analysis.
- **Blocks**: The building blocks of a *Workflow*, which define specific tasks or operations to be performed.
- **Programs**: The executable code or scripts that are associated with *Blocks* to perform specific functions.
- **Attachments**: Additional files or resources that are required by *Programs* to execute correctly.

All elements are defined using a JSON structure, which includes metadata, configurations, and references to dependent elements. This standardized format allows for easy import and export of elements between different PROOF instances.

For more details on the JSON structure of each element, please refer to the respective documentation sections:

- [JSON Structure of a Workflow](../Structure/json-structure-of-a-workflow.md)
- [JSON Structure of a Block](../Structure/json-structure-of-a-block.md)

## Exporting Elements

Starting with version 1.4.0, *Workflows* and *Templates* can be exported from the *Configs Tab* in the PROOF UI:

- **Export Workflows**: A PROOF *Workflow* can be exported as a package that contains all the necessary information about the *Workflow*, including its structure, references to *Blocks*, connections, and configurations.<!--This package can then be imported into another PROOF instance to recreate the same *Workflow*.-->
- **Export Templates**: A *Block Template* can be exported together with its associated *Programs* and *Attachments* for reuse in other *Workflows* or PROOF instances.

See [Configuration - Export Configs](../UI/configs.md#5-export-configs) for details on how to export elements in the UI.
