# Task 4 – Physical Verification & Post-Layout Characterization
# AI Prompt Log

| Step | Problem / Objective | Prompt Summary | AI Assistance | Verified |
|------|----------------------|----------------|---------------|----------|
| 1 | Understand Task 4 requirements | Act as a Senior SRAM Memory Designer and analyze Task 4. Break the internship task into smaller engineering phases and explain the complete implementation workflow. | Decomposed the task into SRAM generation, output verification, physical verification (DRC/LVS/PEX), characterization, analysis, and documentation. Provided a structured execution roadmap. | ✅ Verified against the official Task 4 description |
| 2 | Documentation Strategy | Suggest a documentation methodology that can be maintained throughout implementation instead of writing everything at the end. | Recommended maintaining a live README, recording debugging steps incrementally, documenting terminal commands, observations, fixes, and maintaining an AI Prompt Log for engineering traceability. | ✅ Adopted |
| 3 | Python Initialization Error | Analyze the OpenRAM startup failure caused by `NameError: false is not defined` and explain the root cause. | Identified the issue as an invalid Python boolean (`false` → `False`), explained why Python rejected it, and suggested verifying the configuration using `grep` before rerunning OpenRAM. | ✅ Fixed |
| 4 | Technology Module Loading Failure | Investigate why OpenRAM failed to load the SKY130 technology module and identify missing dependencies. | Guided a systematic investigation using directory inspection (`tree`), file search (`find`), and comparison with the latest OpenRAM technology package. Helped identify outdated and missing technology components. | ✅ Fixed |
| 5 | API Migration for SKY130 Technology | Explain why `design_rules` and related modules were no longer recognized after upgrading OpenRAM. | Determined that the SKY130 technology package was written for an older OpenRAM API. Assisted in migrating imports and updating references to the newer `openram.drc` interface. | ✅ Fixed |
| 6 | Missing Python Dependency | Analyze the `ModuleNotFoundError: No module named 'sklearn'` encountered during characterization. | Identified Scikit-Learn as a required dependency for the characterization engine and recommended installing it using `pip3 install scikit-learn`. | ✅ Fixed |
| 7 | Engineering Debug Documentation | Convert the entire debugging process into a professional engineering log suitable for GitHub documentation. | Generated a structured troubleshooting log including issue description, root cause, investigation commands, command purpose, implemented fix, supporting screenshots, and debugging summary table. | ✅ Adopted |
| 8 | README Refinement | Improve the repository documentation by making it concise, engineering-focused, and reproducible for future users. | Refined the README structure with task overview, workflow, debugging log, AI Prompt Log, execution steps, generated outputs, and project status while avoiding unnecessary explanations. | ✅ Adopted |
## Objective

Validate an OpenRAM-generated **2-word × 16-bit single-port SKY130 SRAM** through functional verification, physical verification, post-layout characterization, and stability analysis using AI-assisted workflows.

---

## Task Understanding

Unlike previous tasks that focused on SRAM generation, this task validates the generated SRAM from **layout generation to post-layout characterization**, ensuring the design is functionally correct, physically clean, and accurately characterized.

---

## Execution Plan

- [ ] Generate 2×16 OpenRAM SRAM
- [ ] Audit generated design files (GDS, LEF, LIB, Verilog, SPICE)
- [ ] Verify pin, bus width, address width, and timing consistency
- [ ] Develop self-checking regression testbench
- [ ] Run Magic DRC
- [ ] Run Netgen LVS
- [ ] Perform PEX
- [ ] Simulate extracted SRAM (TT, FF, SS)
- [ ] Measure timing, leakage, and energy
- [ ] Compare results with Liberty (.lib)
- [ ] Calculate Hold, Read, and Write SNM
- [ ] Document observations, AI prompts, and conclusions

---

## Workflow

```text
OpenRAM Generation
        ↓
Output Audit
        ↓
Functional Verification
        ↓
DRC → LVS → PEX
        ↓
Post-Layout Simulation
        ↓
Measurements
        ↓
SNM Analysis
        ↓
Documentation
```

---

## AI Assistance

AI will be used to:

- Audit generated outputs
- Analyze DRC/LVS/PEX reports
- Assist in debugging and verification
- Support documentation

> All AI suggestions will be verified using the respective EDA tools.

---
# Debugging & Issue Resolution Log

During the OpenRAM + SKY130 integration, several compatibility and environment-related issues were encountered. This section documents each problem, how it was investigated, the commands used, and the corresponding fix.

---

# Issue 1: Invalid Python Boolean

## Error

```text
NameError: name 'false' is not defined
```

### Why did this happen?

OpenRAM failed during initialization because Python boolean values are case-sensitive. The configuration file contained `false` instead of the valid Python keyword `False`, causing the compiler to terminate before loading any modules.

---

## Investigation

```bash
grep -n "use_nix" $OPENRAM_HOME/options.py
```

**Purpose**

Searches for the `use_nix` configuration inside `options.py`.

---

## Fix

Replace

```python
use_nix = false
```

with

```python
use_nix = False
```

---

## Proof

<p align="center">
  <img src="screenshots/issue1a.png" width="900">
</p>
<p align="center">
  <img src="screenshots/issue1b.png" width="900">
</p>
---

# Issue 2: Technology Module Could Not Load

## Error

```text
ERROR: Could not load tech module
ModuleNotFoundError: No module named 'design_rules'
```

### Why did this happen?

After fixing the Python syntax issue, OpenRAM started loading the SKY130 technology package. However, the technology files were incompatible with the latest OpenRAM release, resulting in missing module dependencies.

---

## Investigation

### Check whether the required file exists

```bash
find ~/internship/SRAM_SKY130/OpenRAM -name "design_rules.py"
```

**Purpose**

Searches the repository for the required technology file.

---

### Inspect the SKY130 directory structure

```bash
tree -L 3 ~/internship/SRAM_SKY130/OpenRAM/sky130A
```

**Purpose**

Displays the technology directory hierarchy to verify whether all expected files are available.

---

### Observation

The technology package lacked helper modules expected by the newer OpenRAM version, such as:

```text
design_rules.py
module_type.py
custom_cell_properties.py
custom_layer_properties.py
```

---

## Fix

Compared the SKY130 technology package with the latest OpenRAM stable release and updated the technology implementation to match the current compiler structure.

---

## Proof
<p align="center">
  <img src="screenshots/issue2a.png" width="900">
</p>
<p align="center">
  <img src="screenshots/issue2b.png" width="900">
</p>
<p align="center">
  <img src="screenshots/issue2c.png" width="900">
</p>
<p align="center">
  <img src="screenshots/issue2d.png" width="900">
</p>
---

# Issue 3: design_rules Not Defined

## Error

```text
NameError: name 'design_rules' is not defined
```

### Why did this happen?

Although the technology module was now loading successfully, it still used an older OpenRAM API. Several classes had been moved into the `openram.drc` module, making the original import statements invalid.

---

## Investigation

Locate the implementation of `design_rules`.

```bash
grep -R "class design_rules" ~/internship/tools/OpenRAM -n
```

**Purpose**

Finds where the `design_rules` class is implemented inside OpenRAM.

---

## Observation

The class still exists but is now accessed through the OpenRAM DRC module rather than being imported directly.

---

## Fix

Updated the technology file.

Replace

```python
from design_rules import *
from module_type import *
from custom_cell_properties import cell_properties
from custom_layer_properties import layer_properties
```

with the latest OpenRAM API.

```python
from openram import drc as d
```

Also update the following:

```python
tech_modules = d.module_type()

cell_properties = d.cell_properties()

layer_properties = d.layer_properties()
```

---

## Proof

<p align="center">
  <img src="screenshots/issue3a.png" width="900">
</p>
<p align="center">
  <img src="screenshots/issue3b.png" width="900">
</p>
<p align="center">
  <img src="screenshots/issue3c.png" width="900">
</p>
<p align="center">
  <img src="screenshots/issue3d.png" width="900">
</p>
---

# Issue 4: Missing Python Dependency (Scikit-Learn)

## Error

```text
ModuleNotFoundError: No module named 'sklearn'
```

### Why did this happen?

Once the technology package loaded successfully, OpenRAM entered the characterization stage. The delay modeling module depends on the Scikit-Learn library, which was not installed in the Python environment.

---

## Fix

```bash
pip3 install scikit-learn
```

**Purpose**

Installs the Scikit-Learn package required by the OpenRAM characterization engine.

---

## Proof

> <p align="center">
  <img src="screenshots/issue4a.png" width="900">
</p>

---

# Issue 5: Successful OpenRAM Initialization

## Result

After resolving the environment and technology compatibility issues, OpenRAM successfully initialized and generated the SRAM design.

Generated files include:

```text
.gds
.sp
.v
.lef
.lib
.log
.html
.py
.lvs
```

---

## Proof

> <p align="center">
  <img src="screenshots/issue5a.png" width="900">
</p>

---

# Current Status

The OpenRAM compiler initializes successfully and generates all output files. The remaining issue related to `poly_to_contact` compatibility is currently under investigation and will be documented in the next update.

---

# Debugging Summary

| Issue | Root Cause | Resolution | Status |
|--------|------------|------------|--------|
| Invalid Python Boolean | Incorrect boolean keyword (`false`) | Changed to `False` | ✅ Fixed |
| Technology Module Loading Failure | Outdated SKY130 technology package | Updated technology files | ✅ Fixed |
| `design_rules` Not Defined | Old OpenRAM API | Migrated to `openram.drc` API | ✅ Fixed |
| Missing `sklearn` | Python dependency missing | Installed Scikit-Learn | ✅ Fixed |
| OpenRAM Initialization | Environment validation | Compiler initialized successfully | ✅ Fixed |

