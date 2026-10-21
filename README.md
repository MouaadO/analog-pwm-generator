# Analog PWM Generator with Adjustable Dead Time

I designed an analog PWM generator with two complementary outputs and adjustable frequency, duty cycle and dead time. The project includes the LTspice circuit, a complete KiCad schematic and a compact two-layer PCB.

**Project team:** Amani Mouaad and Cheiffou Melissa.

![Analog PWM generator PCB — 3D view](docs/images/pcb-3d.png)

## Operating principle

An integrator and comparator generate a triangular carrier. The duty-cycle command and the dead-time potentiometer establish two comparison thresholds. Comparing the carrier with these thresholds produces complementary PWM signals separated by a non-overlap interval. Endpoint detectors set constant output states when the external command is outside its operating range, and the output buffers provide the current drive.

## Design specifications

| Parameter | Design specification |
| --- | --- |
| Outputs | Two complementary PWM signals with adjustable dead time |
| Switching frequency | 20–100 kHz |
| Duty-cycle command | 0–100%, selected from a potentiometer or external SMA input |
| Dead time | Adjustable up to 10% of the switching period |
| Output levels | Nominal 0 V / 5 V |
| Output drive target | More than 20 mA source and sink per output |
| Input supply | 7–10 V DC, centre-positive barrel jack |
| PCB | 68 × 53 mm, two copper layers, through-hole components |

## PCB design

The board contains 39 fitted components, including five IC packages. Three outward-facing potentiometers adjust duty cycle, frequency and dead time. The external command enters through the SMA connector, and a three-pin selector chooses the command source. The PCB uses an LF50ABV 5 V regulator, project-local footprints and local 3D models.

### Top view

![PWM PCB — top 3D view](docs/images/pcb-3d-top.png)

### Copper layouts

| Front copper | Back copper |
| --- | --- |
| ![Front PCB layout](docs/images/pcb-front.png) | ![Back PCB layout](docs/images/pcb-back.png) |

### Complete schematic

![Complete analog PWM schematic](docs/images/schematic.png)

## Open the KiCad project

1. Download or clone the complete repository.
2. Open [pcb/Analog_PWM.kicad_pro](pcb/Analog_PWM.kicad_pro) in KiCad 8 or newer.
3. Open `Analog_PWM.kicad_sch` for the schematic or `Analog_PWM.kicad_pcb` for the board.
4. In the PCB Editor, press **Alt+3** to view the assembled board in 3D.

Keep the local symbol library, footprint library, library tables and `3dmodels` folder beside the project files. [Gerber and drill files](pcb/Manufacturing) are included, together with the [PCB assembly BOM](pcb/BOM_PCB.csv) and [purchasing BOM](pcb/PWM_BOM_AMANI_CHEIFFOU.xlsx).

## LTspice simulation results

The existing simulation uses the 7805 supply model. The chart compares the calculated switching frequencies with the LTspice results; the circuit calculations are detailed in the [design notes](docs/design-notes.md).

![Analytical and simulated switching frequencies](docs/results/frequency-results.png)

| LTspice simulation result | Value |
| --- | --- |
| Switching frequency endpoints | 19.99 kHz and 99.69 kHz |
| Output LOW / HIGH levels | 0.082 V / 4.916 V |
| Source and sink current | Approximately 26.86 mA per output |
| Fast-setting dead-time gaps | 0.980 / 0.984 microseconds |

To run the simulation, open `simulation/analog_pwm_dead_time.asc` in LTspice and run the transient analysis. Probe `TRI`, `CMD`, `LOW`, `HIGH`, `OUT1` and `OUT2`. Adjust `FP`, `DP`, `DTP`, `SOURCE` and `VD` to explore the control settings. Keep the supplied symbols and component libraries in the simulation folder.

## Repository contents

- `pcb/`: KiCad project, complete schematic, routed board, local libraries, 3D models, BOMs and fabrication files.
- `simulation/`: LTspice circuit, symbols and component models.
- `docs/images/`: PCB renders, front/back layouts and schematic preview.
- `docs/design-notes.md`: circuit operation, equations and simulation results.
- `docs/datasheets/`: component datasheets.

Academic group project at the National Polytechnic School of Algiers.
