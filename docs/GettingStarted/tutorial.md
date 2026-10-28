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
   - `indoor_temp` (start value): Set the initial controller setpoint temperature (in °C)

3. Run the workflow to see how the building temperature evolves over time
