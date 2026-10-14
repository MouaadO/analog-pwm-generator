# Analog PWM Generator — Design Notes

The circuit generates two complementary PWM outputs from a shared triangular carrier. Frequency, duty cycle and dead time are adjustable.

## Design targets

| Parameter | Target |
| --- | --- |
| Outputs | Two complementary PWM signals |
| Logic levels | 0 V and 5 V |
| Output current | More than 20 mA source and sink per output |
| Frequency | 20–100 kHz, adjusted by potentiometer |
| Dead time | 0–10% of the switching period |
| Duty-cycle input | 0–100%, potentiometer or SMA input |
| Supply | 7–10 V |
| Board dimensions | 68 × 53 mm, through-hole components |

## Triangle oscillator

The integrator and comparator generate the triangular carrier. Current through the timing resistance is:

`I = (CLOCK − Vm) / Rt`

The ramp slope is `dTRI/dt = −I/CINT`. When CLOCK is low, TRI rises; when CLOCK is high, TRI falls. The comparator detects the triangle limits and reverses the clock state.

With `Vm = V5/2 = 2.5 V` and `RH1/RH2 = 1/12`, the calculated triangle limits are 2.292 V and 2.708 V, giving a 0.4167 V span.

## Switching frequency

For `Rt = 12.3 kΩ + RF` and `CINT = 2.4 nF`:

`f ≈ 3 / (Rt × CINT)`

The calculated endpoints are 101.63 kHz at `RF = 0 Ω` and 20.06 kHz at `RF = 50 kΩ`. Changing RF changes the period while the resistor ratio keeps the carrier span fixed.

## Duty-cycle control

The command divider uses `RCM1 = 9.76 kΩ`, `RCM2 = 1 kΩ` and `Vm = 2.5 V`:

`CMD = (COMMAND + 9.76 × Vm) / 10.76`

| Command voltage | CMD reference |
| --- | --- |
| 0 V | 2.268 V |
| 2.5 V | 2.500 V |
| 5 V | 2.732 V |

The divider centres the command on the triangular carrier. The command defines the pulse allocation before dead time is applied.

## Dead time

Two thresholds separate the switching instants:

`LOW = (60.4 × CMD + L) / 61.4`

`HIGH = (60.4 × CMD + H) / 61.4`

L and H are the opposed dead-time potentiometer voltages. For `H − L = p × V5`, the normalised dead time is approximately `d = 0.09772 × p`.

The output duty cycles are approximately `D1 = D − d` and `D2 = 1 − D − d`. At mid-command and maximum dead time, each output has about 40.2% duty cycle.

## Command endpoints

Below 0 V, the lower detector forces OUT1 low and OUT2 high. Above 5 V, the upper detector forces OUT1 high and OUT2 low.

The thresholds are `END_LOW = 2.287 V` and `END_HIGH = 2.713 V`. The BAT85S diode allows the lower detector to pull PB low without directly connecting the two output stages.

## Components

| Component | Quantity | Function |
| --- | ---: | --- |
| TLC072CP | 1 | Triangle integrator and lower endpoint detector |
| LM319N/NOPB | 2 | Oscillator, PWM comparators and upper endpoint detector |
| SN74ACT240N | 1 | Clock inverter and output buffers |
| LF50ABV | 1 | 5 V supply with a 4.7 µF output capacitor |

The board uses 6-, 8-, 8- and 10-pin header strips. The component estimate is €31.81 including 20% VAT, excluding the power jack, PCB and shipping.

## LTspice results

Simulation settings: `VD = 2.5 V`, maximum dead time, `VIN = 8.5 V` and loaded outputs.

| Measurement | Result |
| --- | --- |
| Frequency endpoints | 19.99 kHz and 99.69 kHz |
| Triangle at the fast setting | 2.281–2.719 V |
| Output low/high levels | 0.082 V / 4.916 V |
| Source and sink current | About 26.86 mA per output |
| Dead-time gaps at the fast setting | 0.980 / 0.984 µs |

The dead-time gaps correspond to 9.77% and 9.81% of the switching period.
