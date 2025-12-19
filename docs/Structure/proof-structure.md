# PROOF Overall Structure

<span style="color:blue">Target audience: Users and Developers</span>

PROOF is structured around the concept of *Workflows* and *Blocks*. There are six main components:
## Workflows 
- A workflow is a sequence of *Blocks* that are executed in a specific order.
- It defines how data flows between *Blocks* and how they are connected.
- A *Workflow* can be created, started, stopped, and monitored via the PROOF UI.
<br>

## Blocks (Templates) 
- A *Block* is a reusable unit of functionality that can be used multiple times in a single *Workflow* and in multiple *Workflows*.
- It is a copy of a *Block Template*, i.e., the structure and functionality is defined by the *Template* and can be reproduced as often as required,  
- Each *Block* has its own inputs, outputs, and execution logic.
- A *Block* can be connected with other *Blocks*, whereas output pins can be connected with input pins of another *Block*. 
<br>

## UI 
- The user interface (UI) provides a visual representation of *Workflows* and *Blocks*, allowing users to create, edit, and monitor *Workflows* easily.
- It allows users to create new *Workflows* by dragging and dropping *Blocks* from a template library onto a workflow canvas.
- Users can connect *Blocks* by linking their input and output pins to define the data flow between them.
- It provides configuration panels for *Workflows* and *Blocks*, allowing users to set parameters and options for each component.
<br>

## Orchestrator 
- The *Orchestrator* is built as a web service and is responsible for managing the execution of a *Workflow* and its *Blocks*, handling communication between them, and ensuring that the simulation runs controlled and efficiently.
- It creates the communication queues to the *Workers* via a message broker (RabbitMQ) and prepares the communication between the *Workers* in RabbitMQ, too.
- The *Orchestrator* starts (and stops) a *Workflow* by receiving a start (stop) message from the PROOF UI. 
- During a simulation the *Orchestrator* sends synchronisation messages (SYNC) to the *Workers* to initiate the next simulation step.
- It waits for notify messages (NOTIFY) sent by the *Workers* when they have finished a step or a simulation phase (initialization, execution, finalization, and shutdown).
<br>

## Worker 
- The *Worker* is responsible for executing the logic of each *Block*, including running the model (i.e. the executable) associated with the *Block*.
- During initialization, the *Worker* starts the model program (Python, Java, or Matlab) as a background process. During the simulation, the *Worker* communicates with the model program. 
- starts processing the model while it receives synchronisation messages (SYNC) from the *Orchestrator* and sends notify messages (NOTIFY) back to the *Orchestrator* when it has finished a simulation step or phase.
<br>

## Models 
- Models are the underlying programs or scripts that are executed by the *Blocks*. They can be written in various programming languages such as Python, Java, or MATLAB.
