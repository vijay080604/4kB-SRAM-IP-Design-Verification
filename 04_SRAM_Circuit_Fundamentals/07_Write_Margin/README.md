# Write Margin Analysis

## Objective

Analyze the writeability of the 6T SRAM cell by gradually reducing the bitline voltage difference and determining the minimum differential required to successfully overwrite the stored data.

---

## Initial Condition

The cell was initialized in the hold state:

* Q = 1.8 V
* Qbar = 0 V

During the write operation:

* Wordline (WL) was enabled.
* Differential voltages were applied to BL and BLbar.
* Storage node voltages were monitored to determine whether the cell flipped successfully.

---

## Simulation Setup

| BL (V) | BLbar (V) | Observation      |
| ------ | --------- | ---------------- |
| 0.0    | 1.8       | Successful write |
| 0.2    | 1.6       | Successful write |
| 0.4    | 1.4       | Successful write |
| 0.6    | 1.2       | Write failed     |

---

## Practical Observations

### Case 1: BL = 0 V, BLbar = 1.8 V

The applied bitline differential was strong enough to completely overwrite the previous state.

Result:

* Q transitioned from 1.8 V to 0 V.
* Qbar transitioned from 0 V to 1.8 V.

---

### Case 2: BL = 0.2 V, BLbar = 1.6 V

Although the bitline differential was reduced, the cell still flipped successfully.

This indicates sufficient write strength.

---

### Case 3: BL = 0.4 V, BLbar = 1.4 V

The SRAM cell continued to switch states successfully.

This represents the weakest differential voltage that still produced a successful write in the current sizing configuration.

---

### Case 4: BL = 0.6 V, BLbar = 1.2 V

The internal storage nodes were disturbed but failed to change states completely.

Observed behavior:

* Q decreased only to approximately 0.4 V.
* Qbar increased partially but never reached VDD.
* Once WL was disabled, the cross-coupled inverters restored the original state.

This demonstrates the inherent regenerative feedback of the 6T SRAM cell.

---

## Key Insights

* Writeability decreases as the bitline voltage differential becomes smaller.
* Positive feedback inside the cross-coupled inverters resists state changes.
* A sufficiently strong bitline differential is required to overcome the internal feedback.
* In this design, successful writing was observed up to:

```
BL = 0.4 V
BLbar = 1.4 V
```

* Write failure occurred at:

```
BL = 0.6 V
BLbar = 1.2 V
```

Therefore, the write margin lies between these two operating points.

---

## Files

```
06_Write_Margin/
├── README.md
├── schematics/
│     └── write_margin.sch
├── netlists/
│     └── write_margin.spice
└── images/
      ├── write_margin_success.png
      └── write_margin_failure.png
```

---

## Conclusion

The write margin analysis demonstrated that reducing the bitline differential eventually prevents the write driver from overpowering the cross-coupled inverter pair. The SRAM cell preserved its original data when the applied differential became insufficient, highlighting the trade-off between writeability and cell stability.

---

## Next Step

Generate the Butterfly Curve and perform Static Noise Margin (SNM) analysis to quantitatively evaluate the stability of the 6T SRAM cell.
