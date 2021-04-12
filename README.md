# rev2-master-keyboard
This project is the culmination of my obsession with using a [Prophet Rev2](https://www.sequential.com/product/prophetrev2/) synthesizer as the master MIDI keyboard in my home studio. The idea is to have the knobs on the device's panel control the device itself, while the keyboard can be "routed" to any synthesizer, including the Rev2 iself.

## Features
TODO

## How it works

In essence, a [Midihub](https://blokas.io/midihub/) is used to "split" the Rev2 into two logical devices: a synthesizer with one MIDI input and one MIDI output, and a keyboard with one MIDI input and one MIDI output:

## Keyboard control
Input A, any channel.

- CC 1: Split A output channel
    - Valid values: 1 to 16
    - 0 is interpreted as 1
    - Values greater than 16 are interpreted as 16
- CC 2: Split B output channel
    - Valid values: 1 to 16
    - 0 is interpreted as 1
    - Values greater than 16 are interpreted as 16
- CC 3: Zone A transpose
    - Bipolar (-64 to +63)
- CC 4: Zone B transpose
    - Bipolar (-64 to +63)

## Rev2 limitations
TODO

## Ports
### Input ports:
- A: From external device
- D: From Rev2

### Output ports:
- A: Rev2 CC (NRPN), Clock
- B: Keyboard (Note On, Note Off, Pressure, Pitch Bend, Poly Aftertourch, Modwheel)
- D: To Rev2

### Virtual ports:
- D: Program Change and CCs 0 and 32 (Bank Select MSB and LSB, respectively), from Rev2
- E: CC (NRPN) from Rev2

## Pipelines
### Port A
TODO

### Port D - Program Change
- Thru Program Change messages back to Device, on Channel 1.
- Duplicate Program Change messages from Channel 1 to Channel 2, send to Device.

### Port D - CC (NRPN)
- Thru CC (NRPN) messages on Channels 1 and 2 back to Device.
- Thru CC (NRPN) messages on Channels 1 and 2 to Port A.

### Port D - Clock
- Thru Clock messages to Port A.

### Port D - Keyboard
- Thru Note On, Note Off, Pressure, Pitch Bend, Poly Aftertouch and Modwheel, Expression Pedal and Sustain Pedal to Port B
