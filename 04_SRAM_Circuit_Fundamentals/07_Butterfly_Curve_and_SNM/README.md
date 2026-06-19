# Butterfly Curve and Static Noise Margin (SNM)

## Objective & Learning Approach

The objective of this study was to understand SRAM stability analysis using butterfly curves and static noise margin (SNM). AI-assisted discussions were used to connect inverter voltage transfer characteristics with SRAM cell stability.

---

## Key Concepts Learned

* The butterfly curve is generated using the voltage transfer characteristics (VTCs) of two cross-coupled inverters.
* One inverter characteristic is mirrored and superimposed on the other.
* The intersections represent stable operating points of the SRAM cell.
* Static Noise Margin (SNM) quantifies the stability of the cell.
* Larger SNM values indicate better immunity to noise and disturbances.

---

## Circuit-Level Understanding

The SRAM cell consists of two cross-coupled inverters.

Procedure:

1. Generate the VTC of a CMOS inverter.
2. Mirror the VTC about the line y = x.
3. Overlay both curves.
4. Identify the largest square that can fit between the curves.

The side length of this square represents the Static Noise Margin (SNM).

---

## Design Insights

* SNM is a key indicator of SRAM robustness.
* A larger butterfly opening corresponds to higher stability.
* Read operations typically reduce SNM due to read disturb effects.
* Butterfly curves provide a graphical method for evaluating cell stability.

---

## Observations

* Stable SRAM cells exhibit larger SNM values.
* Noise immunity can be quantified using the butterfly curve.
* Hold SNM is generally greater than Read SNM.
* Cell sizing directly influences the resulting SNM.

---

## AI-Assisted Workflow

**Prompt Used:**
"Explain butterfly curves and static noise margin in SRAM cells, including their relationship to inverter VTCs and cell stability."

**AI Model:**
ChatGPT (GPT-5.5)

---

## Next Steps

Generate inverter VTCs using Xschem and Ngspice, construct butterfly curves, and perform SNM extraction using SKY130 transistor models.
