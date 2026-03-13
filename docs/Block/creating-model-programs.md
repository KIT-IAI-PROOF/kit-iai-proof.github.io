# Creating Model Programs

<span style="color:blue">Target audience: Developers</span>

PROOF provides the user with predefined *Block Templates* that can be used to create *Blocks* in *Workflows*. 
If a user wants to implement a specific functionality or model that is not covered by the existing *Block Templates*, it is possible to create new *Block Templates*.

This page describes how to create custom *Model Programs* that are required to build a new *Block Template* in PROOF.

## What is a Model Program?
A *Model Program* is an executable file (such as a Python script, Java program, or MATLAB script) that contains the logic to be executed by a *Block* in PROOF.

Since the current version of the PROOF Docker containers use Python as programming language for *Model Programs*, the following documentation refers to Python specifically. The extension to other programming languages (Java, MATLAB) will be provided in future releases of PROOF (2026).

A *Model Program* must be able to run independently and perform the required computations to ensure it works as expected when integrated into a *Block* within a *Workflow*.

### Python Implementation Requirements

The *Model Program* must be implemented as a subclass of the `proofcore.base.basewrapper.BaseWrapper` class and must implement (override) the required methods:

- **`init()`**: Initialization method called once at the start
- **`step()`**: Execution method called for each simulation step
- **`finalize()`**: Termination method called at the end

### Template Script

To ease the generation of new *Model Programs*, a template Python script is provided in the PROOF GitHub repository **proof-sim-core** under `proofcore/templates/WrapperTemplate.py`. This template includes:

- Skeleton structure with all required methods
- Example input/output handling
- Proper initialization and cleanup patterns
- Best practices for PROOF integration

### Next Steps

Once you have developed your Model Program:

1. **Test the Model Program independently** to ensure it works correctly outside of PROOF
2. **Create a Block Template** using the PROOF UI to integrate your Model Program into workflows

For detailed instructions on creating Block Templates in the PROOF UI, including:
- Creating Attachments (including your Model Program)
- Creating Program entities
- Defining Template metadata, inputs, and outputs

See the complete guide: [Creating new Block Templates](creating-new-templates.md)
