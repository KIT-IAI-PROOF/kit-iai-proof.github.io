# PROOF Blocks and Templates

## Blocks

Blocks are functional nodes in the Workflow schedule.
Each Block delivers functionality, receives input data, and creates output data used by other Blocks.
Each computational model/executable (such as Python, Java, MATLAB and Functional Mock-up Unit) is represented as a 
Block with its dependencies (e.g., libraries, solvers, and configurations).
<!--Each Block is defined by a JavaScript Object Notation (JSON)-based description including detailed input and output
information. --> 
This information is used when combining multiple Blocks into a Workflow to ensure interfaces work together. 
The description holds information about the executable/model and how life cycles and events triggered by 
the Orchestrator should be handled.
This information/instructions are integrated into a Python script to define the Block's life cycle, such as
initialization, execution, and termination, along with commands for handling input and output data.

## Templates

Blocks are created from templates, which are reusable definitions of Blocks that can be instantiated multiple times.
This can be compared to classes in object-oriented programming, where a class defines the structure and behavior, 
and an unlimited number of instances can be built from that class.

This means for the UI, that a Block is created when a Template is dragged onto the canvas.

<br>

# Block Components

Blocks consist of several components that define their structure and functionality.
The main components of a Block are *Worker, Wrapper, Inputs, Outputs, and Communication via Messages (Events)*.

## Worker
<span style="color:blue">Target audience: Developers</span>

The Worker is the core component of a Block that executes the main functionality.
It is responsible for running the model or executable associated with the Block and can be regarded as the neutral part of the Block.
It handles the execution logic, including initialization, execution, and termination of the model or executable.

## Wrapper
<span style="color:blue">Target audience: Developers</span>

The Wrapper is the model itself that can be realized in various programming languages such as Python, Java, or MATLAB.
It is started by the Worker as a background process and is responsible for executing the model's logic and serves as a bridge between the Worker and the model, providing the necessary interface for the Worker to interact with the model.
The Wrapper contains the logic for handling inputs and outputs, as well as any additional configuration or dependencies required by the model.


## Inputs

Inputs are the data that a Block receives from other Blocks or external sources. There are two types of inputs:

1. **Dynamic Inputs**: These are the actual data that the Block needs to process. They can be of various types, such as numerical values, strings, or complex data structures.
2. **Static Data Inputs**: These are parameters or settings for the initialization of the Wrapper at startup. They are not changed during the execution of the Block and are used to configure the Worker's behavior.

Both types of inputs can be **required** or **optional**, depending on the Block's definition (see [Creating new Blocks and Templates](creating-new-templates.md)).

## Outputs

Outputs are the data that a Block produces after processing the inputs. Similar to inputs, outputs can be of various types.
They are used to connect Blocks in a Workflow. The outputs of one Block can be used as inputs for another Block, allowing for data flow between Blocks in a Workflow.

## Communication via Messages

The communication between Blocks and the orchestrator is done via messages, which are events that trigger specific actions in the Blocks.
Messages are used to notify Blocks about changes in the state of the Workflow, such as the completion of a Block's execution or the availability of new data.
There are three types of messages:

### Synchronization Messages (SYNC)
These messages are sent to the workers by the orchestrator to synchronize the simulation process. Different synchronization messages exist for the several simulation phases:

   1. **Initialization Messages (INIT)**: The orchestrator sends these messages to the Workers to initialize the Blocks with the necessary (static) inputs and configuration.
   2. **Execution Messages (EXECUTE)** are sent to the Workers to start the execution of the Block's logic. Execution Messages are sent multiple times during the simulation process.
   3. **Finalization Messages (FINALIZE)** are sent to the Workers to terminate the Block's execution.
   4. **Shutdown Messages (SHUTDOWN)** are sent to the Workers to clean up resources.

### Value Messages (VALUE)
They are used to transfer data between Blocks. They can contain the actual data that the Block needs to process or the results of the Block's execution.

### Notify Messages (NOTIFY)
They are used to notify the orchestrator when a Block has finished the simulation step   and other Blocks about errors that occur during the execution of a Block. They contain information about the status of a Block and the Wrapper, too, when a Block ...

   - has finished the INIT phase to indicate that it is ready for execution.
   - has finished the EXECUTE phase to indicate that it is ready for the next step.
   - has finished the FINALIZE phase to indicate that it is ready for shutdown.
   - has finished the SHUTDOWN phase to indicate that it is ready for cleanup.

   or it provides information about the reason and position of an error and how to handle it

   - when an error occurred during any phase of a Block.

See also:

- [Creating new Blocks and Templates](creating-new-templates.md)
    - [Creating Model Programs](creating-model-programs.md)
    - [Testing Blocks and its Program](testing-blocks-and-program.md)
