# Using FMUs in PROOF

[Functional Mock-up Units (FMUs)](https://fmi-standard.org/) are a standardized format for packaging and
sharing simulation models. In PROOF, an FMU is treated as just another kind of *executable* that can be wrapped
into a *Block* and connected into a *Workflow*.

This guide explains how FMU *Blocks* work, how to configure them, how to bring your own FMU into PROOF, and
how to troubleshoot common issues.

---

## How PROOF executes FMUs

FMU *Blocks* run inside a Python worker image that ships the [`fmpy`](https://github.com/CATIA-Systems/FMPy/)
FMU library. A wrapper (`BaseFmuRunner`) is responsible for:

1. **Loading** your FMU, either from a *configuration file* (Config FMU Runner), from a directly specified
   path (Single FMU Runner), or from a pre-built template dedicated to that FMU.
2. **Applying** static parameters and streaming inputs before/during each PROOF step.
3. **Stepping** the FMU by a configured time step (`fmu_step_size`).
4. **Emitting** results as structured output: a JSON string (`fmu_output_json`), a CSV string (`fmu_output_csv`),
   and one attribute per FMU output variable so you can connect it to downstream *Blocks*.

---

## Prerequisites

FMU *Blocks* require a worker image that includes the `fmpy` package. Use a `-fmpy` image variant, for example:

- `ghcr.io/kit-iai-proof/proof-worker-python:1.4.0-fmpy`

Some FMUs additionally depend on system libraries that are not part of the standard worker image. For example,
EnergyPlus-based FMUs require `libx11`. For such cases a variant that bundles the extra system libraries is
provided:

- `ghcr.io/kit-iai-proof/proof-worker-python:1.4.0-fmpy-libx11`

All published `proof-worker-python` images and their available versions (tags) can be browsed on the GitHub
Container Registry: <https://github.com/orgs/KIT-IAI-PROOF/packages/container/package/proof-worker-python>.

> The `<version>-<package-list>` image naming scheme is described in the
> [Installation & Setup](../GettingStarted/installation.md#the-image-naming-scheme) guide and in the
> [FAQ](../faq.md#which-docker-images-can-be-used-in-block-templates).

### If your FMU has additional requirements

If your FMU needs dependencies (libraries, solvers, native tools) that are not covered by a provided image
variant (`-fmpy`, `-fmpy-libx11`, ...), you can **build your own worker image** based on the standard PROOF worker
image and add the required packages. See [Custom Images](../CustomImages/custom-images.md) for how to build and
use a custom worker image.

---

## The FMU Runner Blocks

PROOF provides three ways to run an FMU, all built on the same `BaseFmuRunner` wrapper. They differ in
*how the FMU and its variables are configured*:

| Block | Configuration source | Best for |
|-------|----------------------|----------|
| **Single FMU Runner** | Everything is set directly as input parameters on the block itself | A self-contained FMU with explicit inputs, outputs, step size and license |
| **Config FMU Runner** | The same settings live in a JSON config file (e.g. `facility_param.json`) shared by many FMUs | Reusing one parameter file across multiple FMU blocks / workflows |
| **Dedicated FMU template** | A template pre-built for exactly one FMU, exposing its variables as friendly, typed inputs/outputs | A specific, fixed FMU used repeatedly (e.g. the EBM-FMU building blocks) |

> Both the **Single FMU Runner** and the **Config FMU Runner** exist in **two variants**: one **with streaming
> inputs** (the FMU receives new input values each PROOF step) and one **without inputs** (the **"- no input"**
> variant, for FMUs that only use static parameters). The "no input" variant simply omits the `fmu_input_values`
> input (and, for the Single runner, `fmu_input_list`).

In every case the block ultimately exports a JSON and a CSV result string, which you typically feed into the *csv_column_selector block* to extract individual result columns and continue working with them downstream or into *FileWriter blocks* to persist results, (see the
[FMU demo workflows](../GettingStarted/tutorial.md#fmu-functional-mock-up-unit-demo-workflows-version-140)).

---

## Telling PROOF where files live

FMU *Blocks* resolve file references (the config file, the `.fmu` file, license files) in this order:

1. Inside the **`userdata`** directory of your PROOF environment, or
2. Inside the **`Attachment`** folder shipped with the wrapper as a fallback.

So you can either place your files in `userdata` (recommended, e.g. `fmus/adder.fmu`) or rely on the
attachments bundled with the block/program.

---

## Single FMU Runner

The **Single FMU Runner** block configures the FMU entirely via its input parameters (no config JSON required).
This section documents the **with-inputs** variant; the **"Single FMU Runner - no input"** variant is identical
except that it drops `fmu_input_list` and `fmu_input_values` (it runs the FMU with static parameters only):

| Input | Type | Required | Description |
|-------|------|----------|-------------|
| `fmu_file_path` | `STRING` | yes | Path to the `.fmu` file, e.g. `fmus/adder.fmu` |
| `fmu_parameter_dict` | `STRING` | no | Static parameter dictionary, e.g. `{"x": 0, "y": 1}` |
| `fmu_input_list` | `STRING` | yes | Streaming input variables: a list `["x", "y"]` or a dict with start values `{"x": 0, "y": 1}` |
| `fmu_output_list` | `STRING` | no | Output variables, e.g. `["sum"]`. If omitted, no per-variable attributes are published. |
| `fmu_step_size` | `FLOAT` | no | FMU time step (default `15` (assuming *minutes* as fmu time step)) depending on the step size of the fmu |
| `fmu_output_interval` | `FLOAT` | no | Output sampling interval (`<= fmu_step_size`, default `fmu_step_size`) |
| `license_file_name` | `FILE_NAME` | no | License file required to run the FMU |
| `fmu_input_values` | `STRING` | yes (EXECUTE) | Schedule of input values (see [Input value scheduling](#input-value-scheduling)) |

Its outputs are `fmu_output_json` and `fmu_output_csv`.

---

## Config FMU Runner

The **Config FMU Runner** block reads the same kind of settings from a JSON file instead of block inputs. This
section documents the **with-inputs** variant; the **"Config FMU Runner - no input"** variant is identical except
that it drops the `fmu_input_values` input (the FMU's streaming inputs are then supplied by the start values in
`input_dict`). Its input parameters are:

| Input | Type | Required | Description |
|-------|------|----------|-------------|
| `config_file_name` | `FILE_NAME` | yes | Name of the JSON config file (default `facility_param.json`) |
| `fmu_name` | `STRING` | yes | Key of the FMU entry **inside** the config JSON |
| `fmu_step_size` | `FLOAT` | no | Duration of each FMU time step (default `15`; typically *minutes* or *seconds* depending on the FMU). Alternatively set via the config file `metadata.fmu_step_size`. |
| `fmu_output_interval` | `FLOAT` | no | Output sampling interval. Must be `<= fmu_step_size`. Defaults to `fmu_step_size`. Smaller values produce more output rows per PROOF step. |
| `fmu_input_values` | `STRING` | yes (EXECUTE) | Schedule of input values for the current PROOF step (see [Input value scheduling](#input-value-scheduling)) |

Its outputs are `fmu_output_json` (results as a JSON string) and `fmu_output_csv` (results as a CSV string).

### The configuration file format

`facility_param.json` maps each FMU *name* to an object holding the FMU's parameters, inputs, outputs and
metadata. The keys are read by the block:

```json
{
  "Adder": {
    "parameter_dict": {},
    "input_dict": ["input_1", "input_2", "input_3"],
    "output_list": ["sum"],
    "metadata": {
      "fmu_directory": "fmus/adder.fmu",
      "fmu_step_size": 1
    }
  }
}
```

Field semantics:

- **`parameter_dict`** — static (start) values applied at initialization, e.g. `{"Q_HP_max": 1e6}`.
  Set to `{}` if the FMU has no static parameters.
- **`input_dict`** — the streaming input variables: a list of variable names (`["input_1", "input_2"]`) or,
  to also provide start values, a dict like `{"input_1": 0, "input_2": 0}`.
- **`output_list`** — the output variables to record, e.g. `["sum"]`.
- **`metadata.fmu_directory`** — path to the `.fmu` file (relative to `userdata`/`Attachment`).
- **`metadata.fmu_step_size`** *(optional)* — per-FMU step size; overrides the block default of `15`.
- **`metadata.License`** *(optional)* — path to a license file required by commercial FMUs.

> When a block input parameter is provided for a value, the block input parameter takes precedence over the config file
> (e.g. `fmu_file_path` overrides `metadata.fmu_directory`, and `fmu_step_size` overrides
> `metadata.fmu_step_size`).

---

## Dedicated FMU template

For a specific, fixed FMU you can create a **template dedicated to exactly that one FMU**. Here the FMU's raw
variable names are wrapped into friendly, typed block inputs/outputs, and `fmu_file_path` is already set to the
FMU's file. There is no configuration file and no manual in-/output lists — the template is ready to be used as a block in a
workflow.

The *EBM-FMU* workflow is the typical example (see the
[EBM with FMUs](../GettingStarted/tutorial.md#ebm-with-fmus) demo workflow). It provides one template per
building component, for example the **3. Building FMU** template exposes the *Building.fmu*:

| Block input | Type | Phase | Description |
|-------------|------|-------|-------------|
| `fmu_file_path` | `STRING` | INIT | Path to the FMU file (default `Building.fmu`) |
| `initial_indoor_temp` | `FLOAT` | INIT | Initial indoor temperature in °C |
| `p_hvac` | `FLOAT` | EXECUTE | HVAC power |
| `outdoor_temp` | `FLOAT` | EXECUTE | Outdoor temperature in °C |

and produces a single block output `indoor_temp`. Similar templates exist for the data feeder
(*1. DataFeeder FMU*), solar generator (*2. StaticGenerator FMU*) and controller (*4. Controller FMU*).

To build such a template for your own FMU, orient on the model file implementations of saif templates: implement a small wrapper (subclassing
the `FmuBaseRunner`), register the FMU's variables as block inputs/outputs, and add the `.fmu` in the attachment-id folder.

---

## Input value scheduling (`fmu_input_values`)

`fmu_input_values` provides the values the FMU should use during the PROOF step. Any variable declared in
`input_dict` (Config FMU Runner) or `fmu_input_list` (Single FMU Runner) may appear.

- **Flat list for a single input**: `'[10, 20, 30]'`
- **List of lists for multiple inputs**: `'[[10, 5], [20, 6], [30, 7]]'` — one entry per `fmu_step_size`
  interval, in the order the variables were declared.
- **FMI 3.0 array values are space-separated** within an entry.
- Leave it empty to fall back to the configured input start values.

When using the block in a workflow, you typically connect the runtime input values from a *FileReader* or
another *Block* (see the [ConfigFmuRunner](../GettingStarted/tutorial.md#configfmurunner-workflow) and
[SingleFmuRunner](../GettingStarted/tutorial.md#singlefmurunner-workflow) demo workflows).

---

## Bringing your own FMU

1. **Copy your `.fmu` into the environment**, e.g. `proof-environment/data/userdata/fmus/adder.fmu`.
2. **Choose how to run it**:
   - Use **Single FMU Runner** and set `fmu_file_path` to the file and declare inputs/outputs directly, **or**
   - Use **Config FMU Runner** and add an entry to `facility_param.json` (in `userdata`) with the required
     `parameter_dict` / `input_dict` / `output_list` / `metadata.fmu_directory` keys, then set `fmu_name` to
     that entry, **or**
   - Use (or create) a **dedicated FMU template** for exactly that FMU (see
     [Dedicated FMU template](#dedicated-fmu-template)).
3. **Ensure the variable names you use match the FMU's model variables** (the block looks them up in the FMU's
   model description; any unknown variable is skipped with a warning).
4. **For commercial FMUs**, provide a license file either via `license_file_name` (Single FMU Runner) or
   `metadata.License` in the config file.
5. **Run and monitor** the workflow, then use the `fmu_output_json` / `fmu_output_csv` outputs (e.g. with *csv_column_selector* and/or
   *FileWriter* blocks) to use the values downstream or persist the results.

---

## Troubleshooting

- **`ModuleNotFoundError: No module named 'fmpy'`** — the block is running in a worker image without `fmpy`.
  Use a `-fmpy` image variant such as `ghcr.io/kit-iai-proof/proof-worker-python:1.4.0-fmpy`.
- **`FMU name '<name>' not found in configuration file`** — the `fmu_name` doesn't match a key in
  `facility_param.json`. Check casing and that the config file is found in `userdata`.
- **`Please provide the fmu file path ... is not a file`** — `fmu_file_path` / `metadata.fmu_directory` is wrong
  or the file isn't in `userdata`/`Attachment`/`<attachment-id>`.
- **Unknown variables skipped** — the model variable name in `input_dict`/`output_list` doesn't match the FMU.
  Inspect the FMU's `modelDescription.xml` for the correct variable names.
- **Executions that take long per step** (e.g. complex FMU models) may time out — increase the block's
  timeout/step setting in the workflow configuration (see
  [Creating Workflows](../UI/creating-workflows.md) and the [FAQ](../faq.md#troubleshooting)).

---

## See also

- [Tutorial & Demo Workflows](../GettingStarted/tutorial.md#fmu-functional-mock-up-unit-demo-workflows-version-140)
  — EBM, Config and Single FMU demo workflows
- [Block Overview](../Block/overview.md) — what Blocks and Templates are
- [Creating new Block Templates](../Block/creating-new-templates.md) — how to build your own wrapper-driven blocks
- [Installation & Setup](../GettingStarted/installation.md#the-image-naming-scheme) — worker image naming scheme
- [FAQ](../faq.md) — supported Docker images and general troubleshooting
