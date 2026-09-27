# DE10-Nano LED/Switch Bring-Up Test

A minimal VHDL design for the **Terasic DE10-Nano (Cyclone V SoC, version C)**
that connects the four on-board slide switches directly to four LEDs. Used
as a basic hardware sanity check — confirming the board, JTAG programming
path, and I/O pins are all functioning correctly before moving on to more
complex FPGA designs.

Top-level VHDL structure originally provided as course material (MIT licensed) 
by Ross K. Snider, Montana State University; 
modified to add switch/LED pass-through logic.

## What it does

- `SW[3:0]` → `LED[3:0]`, direct pass-through
- `LED[7:4]` held at `0` (off) — unused for this test

Flipping a switch up lights its corresponding LED; flipping it down turns
the LED off. Simple, immediate visual confirmation that the FPGA fabric,
pin assignments, and board I/O are all working.

## Hardware

- **Board:** Terasic DE10-Nano (Cyclone V SoC 5CSEBA6U23I7, revision C)
- **Programming interface:** On-board USB-Blaster II (JTAG)

## Toolchain

- **Intel Quartus Prime** (Standard or Lite edition)

## Repository Structure

```
Lab2/
├── DE10_Top_Level.vhd     # Top-level VHDL source
├── DE10_Top_Level.qsf     # Active project settings (pin assignments, etc.)
├── DE10_Nano.qsf          # Reference pin-assignment file (Terasic-provided),
│                          # imported via Assignments → Import Assignments
├── DE10_Nano.sdc          # Timing constraints
├── Lab2.qpf               # Quartus project file
└── README.md
```

Build artifacts (`db/`, `incremental_db/`, `output_files/`, `simulation/`,
`.bak` files) are excluded via `.gitignore` — see that file for details.

## Building & Programming

1. Open `Lab2.qpf` in Quartus Prime.
2. **Processing → Start Compilation** (generates the `.sof` bitstream in
   `output_files/`).
3. **Power on the DE10-Nano** and connect a USB cable to its **USB-Blaster
   II** port (separate from the power connection).
4. Open **Tools → Programmer**.
5. Click **Hardware Setup** and confirm the USB-Blaster II is detected.
6. Add the generated `.sof` file, check its box, and click **Start** to
   program the FPGA.

## Expected Result

Flip any of the four leftmost slide switches (`SW0`–`SW3`) on the board —
the corresponding LED (`LED0`–`LED3`) should turn on when the switch is up,
and off when down. `LED4`–`LED7` should remain off at all times.

## Notes

- Pin assignments for `SW`, `LED`, `KEY`, and clock inputs come from
  Terasic's official DE10-Nano `.qsf`, imported into this project rather
  than hand-entered, to guarantee correctness against the board's actual
  schematic.
- This design uses purely combinational logic (no clocked process) since
  it's a direct wire-through with no state or sequencing required.
