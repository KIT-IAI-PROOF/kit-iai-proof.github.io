# PROOF Documentation

> **⚠️ Work in Progress**  
> This documentation is currently under active development. Some sections may be incomplete or subject to change. 
> We are continuously working on improving and expanding the content. If you encounter any issues or have suggestions, 
> please feel free to reach out to the development team.

## Introduction
PROOF is a co-simulation framework that allows users to create and manage complex workflows. 

The PROOF UI provides a user-friendly interface for creating and managing workflows.

Basic Docker knowledge is required to start and interact with the first version.

### Basic Execution Options
PROOF is designed to run in the following<!--different--> environments:

1. **Docker**: Use Docker to containerize your PROOF environment, making it easy to use on the system.

<!--
2. **Local Execution**: Run PROOF on your local machine without Docker or Kubernetes. 
This is suitable for developers, who want to create and test own *Blocks*. Here, PROOF runs directly on the host operating system and each *Block* is performed in an own terminal.
**This feature will be available in the next release (2026)**.

3. (**Kubernetes**): Deploy PROOF on a Kubernetes cluster for scalable and distributed execution. This is ideal for production environments where you need to manage multiple workflows and blocks efficiently. 
**This feature will be available in the next release (2026)**.
-->

<!--
The concept of PROOF is to provide a co-simulation platform, which is easy to use for non-expert users. They can create workflows by using predefined *Blocks* or by creating their own *Blocks* based on custom models without being forced to program. 
Only if the user wants to create an own *Block*, he must provide an individually programmed model execution file. For more details see [Creating new Block Templates](Block/creating-new-templates).

It is based on three platforms, where the PROOF UI provides a user-friendly interface for creating and managing workflows, 
the PROOF Orchestrator manages the execution of workflows and their blocks, and the PROOF Worker executes the logic of each block.

This documentation provides an overview of the structure and components of the *PROOF Workflows* and *PROOF Blocks*, 
which are reusable units within the PROOF environment.
-->

<!--
The first published version of PROOF focuses on Docker-based simulations, while local execution and Kubernetes-based deployments will be available in the near future (2026).
Furthermore, the recipient of this documentation is an ordinary user, who likes to run simulations in an easy way.
-->

<br>

### Table of Contents
<!--
- [Introduction](#introduction)
  - [Basic Execution Options](#basic-execution-options)
    - [Local Execution, Docker, and Kubernetes](#local-docker-kubernetes)
-->

- [PROOF Overall Structure](Structure/proof-structure.md)
<br>
- PROOF *Workflows*
    - [Workflow Structure](Workflow/proof-workflows.md#workflow-structure)
    - [Creating new Workflows](Workflow/proof-workflows.md#creating-new-workflows)
    - [Starting a Workflow](Workflow/proof-workflows.md#starting-a-workflow)
    - [Testing Workflows](Workflow/proof-workflows.md#testing-workflows)
<br><br>
- PROOF *Blocks* and *Templates*
    - [Block Components](Block/overview.md#block-components)
    - [Use PROOF Blocks](Block/using-proof-blocks.md)
    - [Create new Block Templates](Block/creating-new-templates)
    - [Create Model Programs](Block/creating-model-programs)
    - [Test Blocks and its Program](#testing-blocks-and-program)
<br><br>
- PROOF UI  
    - [Create a new *Workflow*](Workflow/creating-workflows.md)
    - [Start a *Workflow*](Workflow/proof-workflows.md#testing-workflows)
    - [Monitor a running *Workflow*](Workflow/monitoring-workflows.md)
    - [Configure Elements](Workflow/configs-workflows.md)
        - [Configure a *Workflow*](Workflow/configure-workflow.md)
        - [Configure a *Block Template*](Workflow/configure-block-template.md)
        - [Configure a *Program*](Workflow/configure-program.md)
        - [Configure an *Attachment*](Workflow/configure-attachment.md)
<br><br>
- Frequently Asked Questions [(FAQ)](faq.md)      


<!--
- [PROOF Blocks](Block/proof-blocks.md)
  - [Block Structure](Block/overview.md)
  - [Block Components](Block/overview.md#block-components)
  - [Use PROOF Blocks](Block/using-proof-blocks.md)
  - [Create new Blocks](Block/creating-new-templates.md)
- [PROOF Programs](Program/proof-programsmd)
- [Test Blocks and its Program](#testing-blocks-and-program) -->

## Documentation for Developers  

- [Creating Model Programs](Block/creating-model-programs.md)
- [Testing Blocks and its Program](#testing-blocks-and-program)
- [Import and Export of PROOF Elements](ImportExport/import-and-export-of-elements.md)
    - [JSON Structures of PROOF Elements](structure/json-element-structures.md)

<!-- [PROOF UI](#proof-ui)  
  - [Workflow Editor](#ui-workflow-editor)
    - [Creating new Workflows](#ui-creating-new-workflows)
    - [Editing existing Workflows](#ui-editing-existing-workflows)
    - [Starting Workflows](#ui-starting-workflows)
  - [Monitoring Window](#ui-monitoring-window)
    - [Starting and stopping Workflows](#ui-starting-and-stopping-workflows)
    - [Monitoring Workflow Execution](#ui-monitoring-workflow-execution)
  - [Configuration Window](#ui-configuration-window)
    - [Configure Workflow Settings](#ui-configure-workflow-settings)
    - [Configure Block Settings](#ui-configure-block-settings)
    - [Configure Block Templates](#ui-configure-block-templates)
    - [Configure Block Attachments](#ui-configure-block-attachments)
    - [Configure Block Programs](#ui-configure-block-programs)
-->
- Prospects for Version 2 coming End of 2025


<!--
# PROOF Structure

PROOF is structured around the concept of workflows and blocks. There are three main components:
- **Workflows**: A workflow is a sequence of blocks that are executed in a specific order. 
- It defines how data flows between blocks and how they are connected.
- **Blocks (Templates)**: A block is a reusable unit of functionality that can be used in multiple workflows. 
- Each block has its own inputs, outputs, and execution logic.
- **Orchestrator**: The orchestrator is responsible for managing the execution of a workflow and its blocks, handling communication between them, and ensuring that the simulation runs controlled and efficiently.
- **UI**: The user interface (UI) provides a visual representation of workflows and blocks, allowing users to create, edit, and monitor workflows easily.
- **Worker**: The worker is responsible for executing the logic of each block, including running the model or executable associated with the block.
- **Models**: Models are the underlying programs or scripts that are executed by the blocks. They can be written in various programming languages such as Python, Java, or MATLAB.
 -->
