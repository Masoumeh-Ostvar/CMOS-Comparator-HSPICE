# CMOS Magnitude Comparator Design and Analysis Using HSPICE

This project presents the transistor-level design, simulation, and performance analysis of 2-bit and 8-bit CMOS magnitude comparators using Synopsys HSPICE and TSMC 0.18 μm CMOS technology.

The project focuses on CMOS logic design, transistor sizing, circuit simulation, timing characterization, power analysis, and hierarchical circuit design. A 2-bit comparator was first implemented using reusable CMOS subcircuits and then extended to an 8-bit comparator through hierarchical construction.

---

## Comparator Functionality

The comparator receives two unsigned binary inputs and determines their relative magnitude.

For the 2-bit design:

```text
A = A1 A0
B = B1 B0
```

The circuit produces three outputs:

```text
GT → A > B
EQ → A = B
LT → A < B
```

Only one output is asserted for each valid input combination.

---

## Technology and Tools

| Parameter | Value |
|------------|--------|
| Circuit Description | SPICE Netlists |
| Technology | TSMC 0.18 μm CMOS |
| Simulator | Synopsys HSPICE |
| Supply Voltage | 5 V |
| CMOS Model | mosistsmc180.lib |
| Output Load | 10 fF |
| Transient Step | 10 ps |
| Simulation Time | 160 ns |

> **Note:** The `mosistsmc180.lib` technology model is not included in this repository if redistribution is restricted. Please provide the appropriate model file before running simulations.

---

## Circuit Architecture

The 2-bit comparator was implemented using reusable transistor-level CMOS building blocks:

- CMOS Inverter
- 2-Input NAND
- 2-Input AND
- 3-Input NAND
- XNOR

These subcircuits were combined to implement the GT, EQ, and LT comparison logic.

### High-Level Architecture

```text
             ┌─────────────────────┐
A1 ─────────►│                     │
A0 ─────────►│   CMOS 2-bit        │───► GT
B1 ─────────►│   Comparator        │───► EQ
B0 ─────────►│                     │───► LT
             └─────────────────────┘
```

---

## Transistor Sizing

| Subcircuit | PMOS (W/L) | NMOS (W/L) |
|------------|------------|------------|
| Inverter | 0.72 / 0.18 μm | 0.36 / 0.18 μm |
| NAND2 | 0.72 / 0.18 μm | 0.72 / 0.18 μm |
| AND2 | 0.72 / 0.18 μm | 0.72 / 0.18 μm |
| NAND3 | 0.72 / 0.18 μm | 1.08 / 0.18 μm |
| XNOR | 1.44 / 0.18 μm | 0.72 / 0.18 μm |

The transistor dimensions were selected according to the structure of the pull-up and pull-down networks while maintaining delay characteristics comparable to a reference CMOS inverter.

---

## 2-Bit Comparator Design

### Equality Detection

```text
EQ = (A1 XNOR B1) AND (A0 XNOR B0)
```

The equality output is asserted only when both input numbers are identical.

### Greater-Than Detection

The GT output determines whether:

```text
A > B
```

Comparison is performed starting from the most significant bit (MSB). If the MSBs are equal, the least significant bits determine the result.

### Less-Than Detection

Similarly, LT is asserted when:

```text
A < B
```

using the same MSB-priority comparison strategy.

---

## Functional Simulation

Periodic pulse sources were applied to all input signals:

```text
A0
A1
B0
B1
```

Different pulse periods were used to generate multiple input combinations during a single transient simulation.

Simulation command:

```spice
.tran 10ps 160ns
```

A 10 fF capacitive load was connected to each output node.

### Functional Verification Waveforms

> Insert simulation screenshots here.

```markdown
results/functional/comparator_2bit.png
```

---

## Timing Analysis

The following timing parameters were measured for GT, EQ, and LT:

- Rise Time (Tr)
- Fall Time (Tf)
- Low-to-High Delay (TPLH)
- High-to-Low Delay (TPHL)
- Average Propagation Delay (TP)

Input conditions were selected such that a single input transition produced a measurable output transition.

### Timing Results

| Output | Tr | Tf | TPLH | TPHL | TP |
|----------|-------|-------|-------|-------|-------|
| GT | TBD | TBD | TBD | TBD | TBD |
| EQ | TBD | TBD | TBD | TBD | TBD |
| LT | TBD | TBD | TBD | TBD | TBD |

### Timing Waveforms

> Insert GT, EQ, and LT timing waveforms here.

---

## Power Analysis

Power consumption was evaluated using HSPICE measurement statements under selected test conditions.

### Measured Power

| Output/Test Case | Power |
|------------------|--------|
| GT | TBD |
| EQ | TBD |
| LT | TBD |

---

## 8-Bit Comparator

After verification of the 2-bit design, it was reused as a hierarchical building block for an 8-bit magnitude comparator.

The comparator receives:

```text
A = A7 A6 A5 A4 A3 A2 A1 A0
B = B7 B6 B5 B4 B3 B2 B1 B0
```

and produces:

```text
GT → A > B
EQ → A = B
LT → A < B
```

### Conceptual Architecture

```text
      ┌──────────────────┐
A7:A6 │                  │
B7:B6 │   2-bit CMP      │
      └────────┬─────────┘
               │
      ┌────────▼─────────┐
A5:A4 │                  │
B5:B4 │   2-bit CMP      │
      └────────┬─────────┘
               │
              ...
               │
      ┌────────▼─────────┐
A1:A0 │                  │
B1:B0 │   2-bit CMP      │
      └──────────────────┘
```

This design demonstrates hierarchical circuit construction and reusable SPICE subcircuits.

---

## 8-Bit Verification

Example test vectors:

| A | B | Expected Result |
|----|----|----------------|
| 00000000 | 00000000 | EQ |
| 00000001 | 00000000 | GT |
| 00000000 | 00000001 | LT |
| 10101010 | 01010101 | GT |
| 00110011 | 00110011 | EQ |
| 01010101 | 10101010 | LT |

### 8-Bit Simulation Results

> Insert 8-bit simulation waveforms here.

---

## Repository Structure

```text
CMOS-Comparator-HSPICE
│
├── README.md
├── src/
│   ├── comparator_2bit.sp
│   ├── comparator_8bit.sp
│   └── logic_gates.sp
│
├── simulation/
│   ├── tb_comparator_2bit.sp
│   └── tb_comparator_8bit.sp
│
├── results/
│   ├── functional/
│   ├── timing/
│   └── power/
│
└── report/
    └── report.pdf
```

---

## Skills Demonstrated

- Transistor-Level CMOS Design
- CMOS Logic Implementation
- NMOS and PMOS Network Design
- HSPICE Simulation
- SPICE Subcircuits
- CMOS Transistor Sizing
- Timing Characterization
- Rise/Fall Time Measurement
- Propagation Delay Analysis
- Power Analysis
- Hierarchical Circuit Design
- Digital Comparator Design
- VLSI Fundamentals
