# Frequently Asked Questions (FAQ)

## Technical Questions

### How do I handle errors in my workflow?
PROOF uses *Notify Messages* to communicate errors. When an error occurs, the orchestrator receives notification about the error location and reason. Check the monitoring panel and logs for error details.

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

### Who should I contact for support?
For questions, issues, or suggestions, please contact the PROOF development team at KIT (Karlsruhe Institute of Technology) at *proof@iai.kit.edu*.
