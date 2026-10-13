# Analog PWM Design Notes

Technical text extracted from the supplied presentation. Values below are historical source claims, not newly reproduced measurements. Personal names and presentation metadata are omitted. See the README for the regulator mismatch.

## Source slide 1

01

Analog PWM

generator

Design and validation

Complementary outputs

with adjustable dead time

Nominal LTspice simulation, 99.69 kHz

A shared triangle sets both output switching instants.

## Source slide 2

Parameter

Requirement

Outputs

Two complementary PWM signals

Logic levels

Nominal LOW 0 V and HIGH 5 V

Output current

More than 20 mA source and sink, each output

Frequency

20 kHz < f < 100 kHz, potentiometer adjustment

Dead time

0 < t_dead < 10% of the period

Duty control

0–100%, manual potentiometer or SMA input

External limits

VD < 0 V gives 0%. VD > 5 V gives 100%

Board and supply

68 × 53 mm, through-hole parts, 7–10 V supply

Design targets

02

## Source slide 3

The integrator and comparator form an oscillator

03

Current through the timing resistance

I = (CLOCK − Vm)/Rₜ

dTRI/dt = −I/CINT

CLOCK LOW: TRI rises.

CLOCK HIGH: TRI falls.

OSC detects a limit.

U5A reverses CLOCK

and the ramp direction.

## Source slide 4

The resistor ratio fixes the triangle limits

04

Vm = V5/2 = 2.5 V

α = RH1/RH2 = 1 kΩ / 12 kΩ = 1/12

Vmix = (12TRI + CLOCK)/13

At switching: Vmix = Vm

TRI = Vm + α(Vm − CLOCK)

CLOCK = 0 V

Upper limit

2.708 V

CLOCK = 5 V

Lower limit

2.292 V

ΔVtri = 2.7083 − 2.2917 = 0.4167 V

## Source slide 5

Frequency follows the ramp slope

05

Rₜ = 12.3 kΩ + RF

|slope| = Vm / (Rₜ CINT)

T ≈ 2ΔVtri / |slope|

f ≈ 3 / (Rₜ CINT)

RF = 0 Ω

101.63 kHz

RF = 50 kΩ

20.06 kHz

CINT = 2.4 nF. RF is the frequency-pot resistance.

Changing RF changes the period while the resistor ratio keeps the triangle span fixed.

## Source slide 6

The duty input becomes a narrow reference

06

RCM1 = 9.76 kΩ    RCM2 = 1 kΩ    Vm = 2.5 V

CMD = (COMMAND + 9.76Vm) / 10.76

Selected command

Ideal CMD reference

0 V

2.268 V

2.5 V

2.500 V

5 V

2.732 V

The divider centres the 0–5 V command on the triangle range.

The command sets the pulse allocation before dead time.

## Source slide 7

Two thresholds create dead time directly

07

LOW = (60.4CMD + L)/61.4

HIGH = (60.4CMD + H)/61.4

L and H are the opposed

dead-time-pot voltages.

H − L = pV5

d ≈ 0.09772p

d = (HIGH − LOW) / (2ΔVtri)

D₁ ≈ D − d    D₂ ≈ 1 − D − d

At mid-command and p = 1: about 40.2% per output.

## Source slide 8

External input

Endpoint action

Output state

Below 0 V

U1A and the diode pull PB LOW

OUT1 LOW

OUT2 HIGH

Above 5 V

END_HIGH pulls PA LOW

OUT1 HIGH

OUT2 LOW

The endpoint detectors enforce constant states

08

END_LOW = 5 × 105 / 229.52 = 2.287 V

END_HIGH = 5 − END_LOW = 2.713 V

The BAT85S lets the lower detector pull PB low

without directly connecting both output stages.

## Source slide 9

Component

Qty

Function

TLC072CP

1

Triangle integrator and lower endpoint detector

LM319N/NOPB

2

Oscillator, two PWM decisions and upper endpoint

SN74ACT240N

1

Clock inverter and output current buffers

LF50ABV

1

5 V supply with a 4.7 µF output capacitor

Component choices and PCB integration

09

68 × 53 mm PCB. Header strips: 6 + 8 + 8 + 10 pins.

Component estimate: €31.81 including 20% VAT.

Power jack, PCB and shipping excluded.

## Source slide 10

Measurement

Nominal simulation

Frequency endpoints

19.99 kHz and 99.69 kHz

Triangle at fast setting

2.281–2.719 V

Output LOW / HIGH

0.082 V / 4.916 V

Source and sink current

About 26.86 mA per output

Dead-time gaps at fast setting

0.980 / 0.984 µs (9.77 / 9.81% T)

LTspice results and remaining validation

10

VD = 2.5 V. Maximum dead-time setting. VIN = 8.5 V. Loaded outputs.

The slow frequency limit requires calibration.

Next: PCB routing, component tolerances and command-transition checks.

