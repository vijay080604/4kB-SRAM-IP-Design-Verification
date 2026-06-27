# AI_Assisted_Research_Workflow

## Objective

Document the AI-assisted engineering workflow followed during Task 2 of the SRAM IP Design internship, including prompt engineering, research, generated SPICE/netlists, debugging iterations, simulation validation, and engineering observations.

---

## Understanding the Task

The objective of Task 2 was carefully analyzed before continuing the remaining SRAM peripheral circuit implementations.

The task description emphasized the following requirements:

* Use AI tools for circuit exploration and research.
* Generate low-token prompts for circuit design.
* Generate SPICE/netlist examples.
* Validate generated circuits using Xschem, NGSpice and SKY130 models.
* Document AI prompts, generated netlists, simulation attempts, debugging process, fixes, waveforms and engineering observations.
* Focus on individual SRAM circuit blocks instead of the complete OpenRAM compiler flow or full SRAM macro implementation.

### Prompt Used to Establish the Workflow

The following prompt was used to understand the actual expectation of the task and define a practical implementation strategy.

> I have already completed several SRAM circuit blocks using Xschem and NGSpice, but I have not documented the AI prompts, generated netlists, simulation attempts, debugging process or engineering observations. The remaining tasks include Wordline Control, Bitline Behaviour, Sense Amplifier, Write Driver, Row/Column Decoder Basics and SRAM Timing Sequence.
>
> Analyze the Task 2 requirements carefully and determine whether the expectation is to design every circuit completely from scratch or to perform AI-assisted research, generate SPICE/netlist examples, validate them using Xschem and NGSpice, document the engineering workflow, and build a reproducible development process. Based on the task requirements, propose a practical workflow that satisfies the internship objectives while maintaining proper engineering validation.

### Outcome

After analyzing the task requirements, the following workflow was adopted.

Research

↓

Reference Circuit Study

↓

Prompt Engineering

↓

AI Generated SPICE / Netlist

↓

Manual Review

↓

Xschem Implementation

↓

NGSpice Simulation

↓

Waveform Validation

↓

Debugging (if required)

↓

Documentation

---

## Initial Stage

Before introducing this workflow, several SRAM circuit blocks had already been implemented and verified using Xschem and NGSpice.

Available:

* Circuit schematics
* Simulation outputs
* Waveforms
* Experimental results

Missing:

* AI prompts
* Prompt refinements
* Generated SPICE/netlists
* Research notes
* Debugging iterations
* Engineering observations

This workflow was introduced to document those engineering artifacts while continuing the remaining Task 2 implementations.

---

## Engineering Workflow

The workflow documented in this directory is applied to the remaining SRAM peripheral circuit implementations.

For every circuit, AI is used for research, prompt engineering and initial SPICE generation, while the final implementation is manually reviewed, validated in Xschem and NGSpice, and supported with simulation evidence and concise engineering observations.
# Wordline Driver

## Objective

Design and validate a two-stage CMOS Wordline Driver using SKY130 devices while establishing a reusable AI-assisted workflow for SRAM peripheral circuit development.

---

# Initial AI Prompt

```text
Design a two-stage CMOS wordline driver for an SRAM using SKY130 devices.

Requirements:

- Use sky130_fd_pr__nfet_01v8 and sky130_fd_pr__pfet_01v8
- VDD = 1.8 V
- Generate an NGSpice compatible SPICE netlist
- Use two cascaded CMOS inverters
- Include transient simulation
- Generate a netlist compatible with Xschem hierarchy
- Do not rename device instances or nodes unnecessarily.
```

---

# Initial AI Generated Netlist

The first AI-generated SPICE netlist was created directly from the prompt without considering the exact SKY130 PDK implementation available in the local environment.

> *(Paste the initial AI-*
>
> *-----------------------------------------------------------
>
> * Two-Stage CMOS Wordline Driver (SKY130)
>   *-----------------------------------------------------------
>
> .lib "./libs.tech/ngspice/sky130.lib.spice" tt
>
> *-----------------------------
>
> * Stage 1 Inverter
>   *-----------------------------
>   XM1 n1 din gnd gnd sky130_fd_pr__nfet_01v8 w=0.42 l=0.15
>   XM2 n1 din vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
>
> *-----------------------------
>
> * Stage 2 Inverter (Larger Drive)
>   *-----------------------------
>   XM3 wl n1 gnd gnd sky130_fd_pr__nfet_01v8 w=0.84 l=0.15
>   XM4 wl n1 vdd vdd sky130_fd_pr__pfet_01v8 w=1.68 l=0.15
>
> *-----------------------------------------------------------
>
> * Power Supply
>   *-----------------------------------------------------------
>
> VDD vdd gnd DC 1.8
>
> *-----------------------------------------------------------
>
> * Input Stimulus
>   *-----------------------------------------------------------
>
> VIN din gnd PULSE(
> +0
> +1.8
> +1n
> +50p
> +50p
> +5n
> +10n)
>
> *-----------------------------------------------------------
>
> * Output Load
>   *-----------------------------------------------------------
>
> CWL wl gnd 20f
>
> *-----------------------------------------------------------
>
> * Simulation
>   *-----------------------------------------------------------
>
> .tran 10p 20n
>
> .control
> run
> plot v(din)+2.5 v(n1)+1.2 v(wl)
> .endc
>
> .end
>
> *generated netlist here.)*

---

# Error

```text
could not find a valid modelname
Simulation interrupted due to error.
```

---

# Debugging Workflow

### Verify NGSpice Installation

**Command**

```bash
ngspice -v
```

Confirmed:

```text
ngspice-36
```

---

### Verify SKY130 Library Path

**Command**

```bash
find ~ -name "sky130.lib.spice"
```

Selected Library

```text
/home/vijaykumar/pdk/open_pdks/sky130/sky130A/libs.tech/ngspice/sky130.lib.spice
```

---

### Verify SKY130 Model Availability

**Command**

```bash
grep -R "nfet_01v8" \
/home/vijaykumar/pdk/open_pdks/sky130/sky130A/libs.tech/ngspice/ | head -30
```

The required SKY130 NMOS and PMOS models were present in the installed PDK.

---

# Root Cause

The AI-generated netlist used a transistor/model invocation style that was incompatible with the locally installed SKY130 PDK.

Comparison with a previously verified Xschem-generated SRAM netlist confirmed that the issue was related to the transistor instantiation syntax rather than missing models or an incorrect library path.

---

# Updated Netlist

The following changes were applied before regenerating the netlist:

* Updated the SKY130 library path.
* Matched the transistor instantiation syntax used by Xschem.
* Preserved the SKY130 device names.
* Reused the verified SPICE structure from an existing working netlist.

** sch_path: /home/vijaykumar/internship/SRAM_SKY130/task2/schematics/wordline_driver.sch
**.subckt wordline_driver

---

* First Stage CMOS Inverter

---

XM1 N1 DIN 0 0 sky130_fd_pr__nfet_01v8 L=0.15 W=0.42
XM2 N1 DIN Vdd Vdd sky130_fd_pr__pfet_01v8 L=0.15 W=1

---

* Second Stage CMOS Inverter

---

XM3 WL N1 0 0 sky130_fd_pr__nfet_01v8 L=0.15 W=0.84
XM4 WL N1 Vdd Vdd sky130_fd_pr__pfet_01v8 L=0.15 W=2

---

* Supply

---

V1 DIN 0 PULSE(0 1.8 2n 100p 100p 5n 10n)

V2 Vdd 0 1.8

---

* Wordline Load

---

C1 WL 0 20f

**** begin user architecture code

.lib /home/vijaykumar/pdk/open_pdks/sky130/sky130A/libs.tech/ngspice/sky130.lib.spice tt

.control

tran 0.05n 20n

plot v(DIN) v(N1) v(WL)

.endc

**** end user architecture code

.end

> *(Paste the final corrected Wordline Driver SPICE netlist here.)*

---

## NGSpice Result

**Waveform**

`simulation_results/wordline_driver_output_waveform.png`

![Wordline Driver Output](simulation_results/wordline_driver_output_waveform.png)

### Observation

* DIN switches between 0 V and 1.8 V.
* N1 generates the complementary intermediate signal.
* WL restores the original logic level after the second inverter.
* The propagation delay between DIN and WL confirms correct operation of the two-stage CMOS Wordline Driver.

---

# Engineering Outcome

This debugging process established a reusable workflow for future SRAM peripheral circuits:

* Generate the initial AI netlist.
* Validate it using NGSpice.
* Verify the local SKY130 library and model availability.
* Compare with a working Xschem-generated reference when compatibility issues arise.
* Update the netlist to match the local PDK syntax.
* Re-simulate and verify the generated waveforms before documenting the implementation.
