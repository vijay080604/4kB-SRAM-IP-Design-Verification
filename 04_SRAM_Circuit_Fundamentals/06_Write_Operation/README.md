# Write Operation

## Objective & Learning Approach

The objective of this study was to understand how new data is written into a 6T SRAM cell and how the existing stored state is intentionally overwritten. AI-assisted discussions were used to analyze bitline forcing, access transistor behavior, and state transitions during write operations.

---

## Key Concepts Learned

* Write operation updates the stored value in the SRAM cell.
* The write driver forces complementary values on BL and BLB.
* The wordline enables access to the storage nodes.
* Positive feedback assists the cell in settling into a new stable state.
* Writing requires overcoming the previously stored value.

---

## Circuit-Level Understanding

Write sequence:

1. Desired data is applied to BL and BLB.
2. WL is asserted.
3. Access transistors connect the bitlines to the storage nodes.
4. Internal node voltages begin changing.
5. Cross-coupled inverters transition to the new state.
6. Positive feedback reinforces the updated value.

Example:

To write:

* Q = 0
* QB = 1

The write driver applies:

* BL = 0
* BLB = 1

---

## Design Insights

* Writing is fundamentally a controlled disturbance of the cell.
* The access transistor strength significantly influences write success.
* Positive feedback accelerates the state transition once the switching threshold is crossed.

---

## Observations

* The write driver must overcome the existing stored state.
* Stronger access transistors generally improve write ability.
* Cell stability and write ability must be balanced carefully.

---

## AI-Assisted Workflow

**Prompt Used:**
"Explain the write operation in a 6T SRAM cell and describe how new data overwrites the existing stored value."

**AI Model:**
ChatGPT (GPT-5.5)

---

## Next Steps

Study write margin and investigate how transistor sizing affects write success and cell stability.
