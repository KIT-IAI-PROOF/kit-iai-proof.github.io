# PROOF Tutorial

This tutorial will guide you through getting started with PROOF by running demo workflows. Whether you're testing your installation or exploring PROOF's capabilities, these step-by-step examples will help you understand how to build and execute workflows.

## Demo Workflows

### Test Your Installation: FileReader-to-FileWriter

The **FileReader-to-FileWriter** workflow is the perfect starting point to verify that your PROOF installation is working correctly. This simple workflow demonstrates the basic concept of how data flows between blocks.

**How it works:**
- The **FileReader** block reads a file line by line, providing one line each execution step
- The **FileWriter** block receives the data and appends it to an output file

**Step-by-step instructions:**

1. Start the *FileReader-to-FileWriter* workflow in PROOF
2. Configure the FileReader block:
   - Set the input file to `input.txt` (this file is already available in the `userdata` folder)
3. Configure the FileWriter block:
   - Set the output file to a custom name, for example `output.txt`
   - The output file will be automatically stored in the `userdata` directory
4. Run the workflow and monitor execution until the status shows **SHUT_DOWN**
5. Verify the results by checking that `output.txt` contains the same content as `input.txt`:
   ```bash
   cd proof-environment/data/userdata
   cat output.txt
   ```

**Expected result:** The output file should be identical to the input file.

---

### Energy Building Model (EBM) Demo Workflow

The **Energy-Building-Model (EBM)** workflow simulates a building's thermal behavior over time. This demo helps you understand how PROOF can model complex physical systems with multiple interacting components.

**What it demonstrates:**
- How different building components interact (structure and heating/cooling)
- How PROOF handles continuous simulation of physical models through iterative loops
- How input parameters affect system behavior
- **Feedback loops:** The controller's output influences the building's state, which feeds back as input to the controller (see [Loop Connections](../UI/creating-workflows.md#loop-connections) for detailed explanation)

**Configuration steps:**

1. **Building Block** - Sets up the physical building structure and simulates the building's thermal properties
   - `initial_indoor_temp`: Set the starting indoor temperature (in °C)

2. **Controller Block** - Manages the heating/cooling system and regulates temperature based on feedback from the building
   - **Inputs**/`indoor_temp` (start value): Set the initial controller setpoint temperature (in °C)

3. Run the workflow to see how the building temperature evolves over time

---

### Java Reader2Writer Demo Workflow (Version 1.4.0+)

The **Java Reader2Writer** workflow demonstrates the Java support in PROOF (available since version 1.3.0), reading data with a Java-based *FileReader* block and writing it to a file with a Java-based *FileWriter* block.

**What it demonstrates:**
- How PROOF executes Java-based *Model Programs* through the Java wrapper
- How Java *Block Templates* (Java FileReader / Java FileWriter) are used in a workflow
- The equivalent data-flow concept to the *FileReader-to-FileWriter* workflow, but implemented in Java

**Step-by-step instructions:**

see [FileReader-to-FileWriter](#test-your-installation-filereader-to-filewriter)

---

## FMU (Functional Mock-up Unit) Demo Workflows (Version 1.4.0+)

FMU workflows allow you to integrate external simulation models and tools into PROOF. An FMU is a standardized format for sharing simulation models.

### EBM with FMUs 

The **EBM with FMUs** workflow extends the *Energy-Building-Model* by using [Functional Mock-up Units (FMUs)](https://fmi-standard.org/) to model the building components. It combines the FMU runner with the energy building model, allowing you to integrate external FMU models into a building simulation.

**What it demonstrates:**
- Usage of FMU templates which out- and inputs are specifically designed to run with one Fmu.

**What it adds beyond ConfigFmuRunner:**
- Uses FMU-specific configuration (FMU location and model name)
- Adds additional output file handling:
  - `file_name`: Custom output filename
  - `additional_CRLF` (optional): Set to `true` to ensure the result of every step is written into a new line
  - `printCP` (optional): Set to `true` to include communication point information in the output

---

### ConfigFmuRunner Workflow

The **ConfigFmuRunner** workflow demonstrates how to use a pre-configured FMU that accepts multiple inputs and produces outputs. This workflow uses an **Adder** FMU as an example—a simple model that sums up to three input values.

**What it does:**
- Reads input data from a file
- Passes it to the Adder FMU simulation model
- Processes and writes the results to output files

**Configuration steps:**

1. **Config FMU Runner block:** This block manages the FMU execution
   - `fmu_name`: Enter `Adder` (this must match the name defined in your *facility_param.json* located in `userdata`)

2. **CSV Column Selector block:**
   - `column_name`: Enter `sum` (selects which output column to extract)

3. **File Reader - Python block:**
   - `file_name`: Enter `adder_input.txt` (contains the input data for the FMU, already present in `userdata` directory)

4. **File Writer - Extracted block** (for processed results):
   - `file_name`: Enter a custom output filename
   - `additional_CRLF` (optional): Set to `true` to ensure the result of every step is written into a new line

5. **File Writer - Raw block** (for unprocessed results):
   - `file_name`: Enter a custom output filename
   - `additional_CRLF` (optional): Set to `true` to ensure the result of every step is written into a new line

**Output:** You'll get two files—one with extracted/processed data and one with raw simulation data.

---

### SingleFmuRunner Workflow

The **SingleFmuRunner** workflow provides more control over FMU execution. Instead of using a configuration file, you directly specify the FMU file and which inputs/outputs to use.

**What it does:**
- Loads an FMU file directly
- Allows you to define specific inputs and outputs
- Processes results through configurable file operations

**Configuration steps:**

1. **SingleFmuRunner block:**
   - `fmu_file_path`: Enter the path to your FMU file, for example `fmus/adder.fmu`
   - `fmu_input_list`: Specify inputs as a JSON array, for example `["input_1", "input_2", "input_3"]`
   - `fmu_output_list` (optional): Specify outputs as a JSON array, for example `["sum"]`. If not specified, all outputs are used.

2. **CSV Column Selector block:**
   - `column_name`: Enter `sum` (selects which output column to extract)

3. **File Reader - Python block:**
   - `file_name`: Enter `adder_input.txt` (contains the input data for the FMU, already present in `userdata` directory)

4. **File Writer - Extracted block:** (for processed results):
   - `file_name`: Enter your custom output filename
   - `additional_CRLF` (optional): Set to `true` to ensure the result of every step is written into a new line

5. **File Writer - Raw block:** (for unprocessed results):
   - `file_name`: Enter your custom output filename
   - `additional_CRLF` (optional): Set to `true` to ensure the result of every step is written into a new line

**Output:** Similar to ConfigFmuRunner, you'll generate both extracted and raw output files.
