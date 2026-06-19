# Write Margin

## Objective & Learning Approach

The objective of this study was to understand the concept of write margin and how easily a stored bit can be overwritten in a 6T SRAM cell. AI-assisted discussions were used to analyze transistor strength relationships and write ability tradeoffs.

---

## Key Concepts Learned

* Write margin measures the ease of changing the stored state.
* Access NMOS and pull-up PMOS strengths directly affect write ability.
* Strong access transistors generally improve write margin.
* Strong pull-up PMOS devices can resist state transitions.
* Pull-Up Ratio (PR) is used to evaluate write capability.

---

## Circuit-Level Understanding

During a write operation:

* The write driver forces values onto BL and BLB.
* Access transistors transfer these values into the cell.
* Pull-up PMOS devices attempt to preserve the previous state.
* The resulting competition determines whether the cell successfully flips.

Conceptually:

```text
Access NMOS
      vs
Pull-Up PMOS
```

---

## Design Insights

* Better write margin allows easier state transitions.
* Excessively strong pull-up devices reduce write ability.
* SRAM design requires balancing read stability and write margin.

---

## Observations

* Write ability depends heavily on transistor sizing.
* Improving write margin may affect read stability.
* Pull-Up Ratio provides a useful metric for evaluating write performance.

---

## AI-Assisted Workflow

**Prompt Used:**
"Explain write margin in a 6T SRAM cell and describe the effect of access NMOS and pull-up PMOS sizing on write ability."

**AI Model:**
ChatGPT (GPT-5.5)

---

## Next Steps

Study inverter voltage transfer characteristics and understand how SRAM stability is evaluated using butterfly curves.
