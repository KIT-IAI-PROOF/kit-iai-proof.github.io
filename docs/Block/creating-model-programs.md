# Creating Model Programs

<span style="color:blue">Target audience: Developers</span>

PROOF provides  the user with predefined *Block Templates* that can be used to create *Blocks* in *Workflows*. 
If a user wants to implement a specific functionality or model that is not covered by the existing *Block Templates*, it is possible to create new *Block Templates*.
This page describes how to create own *Model Programs* that is required to build a new *Block Template* in PROOF.
The process of creating new *Block Templates* in PROOF UI is described in the [Creating new Block Templates](creating-new-templates.md) documentation.

## What is a Model Program?
A *Model Program* is an executable file (such as a Python script, Java program, or MATLAB script) that contains the logic to be executed by a *Block* in PROOF.
Since the current version of the PROOF Docker containers use Python as programming language for *Model Programs*, the following documentation refers to Python specifically. The extension to other programming languages (Java, MATLAB) will be provided in future releases of PROOF (2026).

A *Model Program* must be able to run independently and perform the required computations to ensure it works as expected when integrated into a *Block* within a *Workflow*.
The *Model Program* must be implemented as a subclass of the `proofcore.base.basewrapper.BaseWrapper` class and implements (overrides) the required methods for initialization ('init'), execution ('step'), and termination ('finalize').
To ease the generation of new *Model Programs*, a template Python script is provided in the PROOF GitHub repository **proof-sim-core** under *proofcore/templates/WrapperTemplate.py*.




In PROOF, a *Block Template* defines the structure and functionality of a *Block*, i.e. a *Block* is an instance of a *Template*. 
Such a *Block* can be reused multiple times in different workflows. 
Creating a new *Block Template* involves several steps, including defining the *Block's* inputs, outputs, and execution logic.

Creating new *Templates* offers the possibility to insert a desired behaviour or to implement specific functionalities or models to a Workflow.
For this case you need to create a new *Block Template* that defines the structure and functionality of the block.
This can be done with the PROOF UI and the menu *config*:

<figure markdown="span">
  ![config menu](../images/2025-06-27_Start_menu_Config_selected.jpg){ width="600" }
</figure>

# Structure of a Template

A *Template* consists of three main parts:

1. A *Program* entity that represents the physical simulation model as an executable file (such as a Python script, Java program, or MATLAB script) that contains the logic to be executed.

2. Optionally, additional files or dependencies required for the model's execution.

3. For Import/Export: A description file (in JSON format) that defines the *block's* metadata, inputs, outputs, and other configuration details.


# Steps to Create a New Template

1. **Develop the Program**:

    <span style="color:red">**Note:** in PROOF version 1.0, only Python is supported. Therefore, the model file must be a Python script and the documentation hereafter refers to Python specifically.</span>

    Create the executable model file that contains the model's logic. This file should be able to run independently and perform the required computations. For this, you can use any programming language supported by PROOF (e.g., Python, Java, MATLAB).

    This file will be the core of your *Block Template*. It can be built and should be able to be tested outside of PROOF to ensure it works as expected.
The model file must be a subclass of *proofcore.base.basewrapper.py* and implement the required methods for initialization, execution (*step*), and termination.

2. **Identify Attachments**

    *Attachments* are additional files or dependencies required for the model's execution. They are attached to the *Program* entity and can include configuration files, libraries, or other resources needed by the model.
One *Attachment* must be the model file created in step 1. This *Attachment* is called the **entry point** of the *Program* entity. Additional *Attachments* can be added as needed. 

3. **Define the Template Metadata**

    This step is only needed, if you want to import the *Template* from a JSON file and not create it directly in the PROOF UI. 
For this, you need to create a JSON file that defines the *Template's* metadata, including its ID, label, description, inputs, outputs, and other configuration details. Further, the Program and the Attachments must be defined in the JSON file, too.
The fixed structure of the JSON file is described in the [PROOF JSON Schema documentation](link to be added).


# Create a Template in PROOF UI
To create a new *Template* in the PROOF UI, follow these steps:

(**Note:**) It's recommended to create the *Attachments* at first, followed by the *Program* creation and finally the *Template* creation.

1. Select the *Config* menu on the left side of the UI. Here you can select the desired button for the configuration or creation of new elements. 

2. Create *Attachments*:
    - Click on the *Attachments* button to open the *Attachments management interface*.
    - Click on the "**+ ADD**" button to create a new *Attachment*.
    - Fill in the required fields, such as ID, label, description, and upload the model file.
    - Save the *Attachment*.
    - Repeat this process for any additional *Attachments* needed for the *Program*.
   
    <br>

3. Create a *Program*:
    - Click on the *Programs* button to open the *Programs management interface*.
    - Click on the "**+ ADD**" button to create a new *Program*.
    - Fill in the required fields, such as ID, label, description, and select the previously created *Attachments*.
    - Specify which *Attachment* is the entry point (the model file).
    - Save the *Program*.

    <br>

4. Create a *Template*:
    - Click on the *Templates* button to open the *Templates management interface*.
    - Click on the "**+ ADD**" button to create a new *Template*.
    - Fill in the required fields, such as ID, label, description, and select the previously created *Program*.
    - Define the inputs and outputs of the *Template* according to the model's requirements.
    - Save the *Template*.

After having created a new *Template*, it can be used immediately to instantiate *Blocks* in *Workflows* using the  *PROOF Workflow Editor*. 
You can find the *Template* in the right sidebar of the *PROOF Workflow Editor* and drag and drop it onto the *Workflow* canvas.
