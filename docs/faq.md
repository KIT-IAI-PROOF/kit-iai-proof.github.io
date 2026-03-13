# Frequently Asked Questions (FAQ)

## Technical Questions

### How do I handle errors in my workflow?
PROOF uses *Notify Messages* to communicate errors. When an error occurs, the orchestrator receives notification about the error location and reason. Check the monitoring panel and logs for error details.

### Where do I place files for my workflows to access?
Place your files in the `proof-environment/data/userdata/` directory. Files in this directory can be accessed by workflow blocks such as *FileReader*. Workflow-specific attachments are stored in the `proof-environment/data/attachments/` directory.

### How can I export workflow executions?
**Version 1.1.0 and later:**
- **Export**: All users can export their workflow executions as ZIP files from the monitoring interface. The export includes all configurations, inputs, and attachments.

### How do I track the progress of my workflow execution?
The monitoring interface displays real-time execution progress, including:
- Current communication point (CP) of the execution
- Visual progress bars showing completion status
- Input values and workflow parameters
- Status of individual blocks

This feature is available in version 1.1.0 and later.

## Troubleshooting

### My workflow won't start. What should I check?
1. Ensure all required inputs are configured
2. Check that all blocks are properly connected
3. Verify that the workflow is saved
4. Check the monitoring panel for error messages
5. Review logs for detailed error information

### Blocks are not connecting. Why?
Blocks can only connect if their inputs and outputs are compatible (same data type). Compatible connections are indicated by matching colors. Check that you're connecting compatible pin types.

### Where can I find logs?
Logs are available in the *Monitoring* panel of the PROOF UI. Select a workflow execution to view its logs and status information.

## Getting Help

### Where can I find more documentation?
- [Getting Started](GettingStarted/getting-started.md): Getting started with PROOF
- [Structure](Structure/proof-structure.md): PROOF structure and concepts
- [Blocks](Block/overview.md): Block structure and components
- [Workflows](Workflow/proof-workflows.md): Workflow creation and management
- [Programs](Program/proof-programs.md): Program structure and creation
- [Release Notes](About/release-notes.md): Version history and changes

### Who should I contact for support?
For questions, issues, or suggestions, please contact the PROOF development team at KIT (Karlsruhe Institute of Technology) at *proof@iai.kit.edu*.
