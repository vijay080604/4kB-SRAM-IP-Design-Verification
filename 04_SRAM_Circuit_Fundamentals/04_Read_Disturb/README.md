# Read Disturb

## Objective & Learning Approach

The objective of this study was to understand how the read operation can unintentionally disturb the stored data in a 6T SRAM cell. AI-assisted discussions were used to analyze read stability, transistor sizing, and cell ratio concepts.

---

## Key Concepts Learned

* Read operation can temporarily disturb internal storage nodes.
* The storage node holding logic 0 is most vulnerable during read.
* Access NMOS and pull-down NMOS compete during the read cycle.
* Cell ratio (CR) is a key parameter affecting read stability.

---

## Circuit-Level Understanding

During read:

* BL and BLB are precharged to VDD.
* WL is asserted.
* The storage node at logic 0 becomes connected to a precharged bitline.

This creates a tug-of-war between:

* Pull-down NMOS
* Access NMOS

If the access transistor is too strong, the storage node voltage may rise excessively and potentially cause a read upset.

---

## Design Insights

* Read stability depends heavily on transistor sizing.
* A stronger pull-down NMOS improves resistance to read disturb.
* Cell ratio is a primary metric used to evaluate read robustness.

---

## Observations

* Reading a cell is not a completely non-invasive operation.
* Excessive node voltage rise can lead to data corruption.
* Proper sizing ensures safe read functionality.

---

## AI-Assisted Workflow

**Prompt Used:**
"Explain read disturb in a 6T SRAM cell and describe the role of cell ratio in read stability."

**AI Model:**
ChatGPT (GPT-5.5)

---

## Next Steps

Study write operation and understand how stored data can be intentionally overwritten.
