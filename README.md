# Analog PWM Generator with Adjustable Dead Time

An LTspice circuit study for generating complementary PWM outputs from a shared triangle waveform. The design provides frequency and duty adjustment, two thresholds for dead time, and endpoint handling for the command input.

## Tools used

LTspice schematic, custom symbols, SPICE device models and functional models for the regulator and output driver.

## How to run

1. Open `simulation/analog_pwm_dead_time.asc` in LTspice.
2. Keep all `.asy` and `.lib` files beside the schematic so local models resolve.
3. Run the supplied transient analysis.
4. Probe `TRI`, `CMD`, `LOW`, `HIGH`, `OUT1` and `OUT2`.
5. Adjust the `.param` values for frequency (`FP`), duty (`DP`), dead time (`DTP`), source selection (`SOURCE`) and external command (`VD`).

## Files

| Folder / file | Purpose |
| --- | --- |
| `simulation/analog_pwm_dead_time.asc` | Main circuit schematic |
| `simulation/*.asy` | Local circuit symbols |
| `simulation/*.lib` | Semiconductor and functional models |

## Limitations

Nominal target frequency is approximately 20–100 kHz. The schematic models a 7805-type regulator, while the supplied purchasing documents select LF50ABV; these versions must be reconciled before hardware work. Output loading, command transitions, component tolerances and regulator corners require further validation. No hardware-performance claim is made. Generated waveforms and prebuilt outputs are excluded.

## Project context

Prepared from academic project files. Collaborative work remains team work; personal names are omitted from this public copy. Original technical comments and interfaces may be in French. No new license has been assigned.

## Supporting documents

- [Design notes](docs/design-notes.md) extracted from the supplied slides.
- [Component bill of materials](docs/component-bill-of-materials.xlsx), preserved with its formulas. Prices are historical estimates.
- Manufacturer datasheets in `docs/datasheets/`. Supplier names and third-party attribution are retained.

## Verification status

Local symbol and library references were checked. The LTspice simulation was not rerun in this preparation pass. Historical measurements in the design notes are source claims.
