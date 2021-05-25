# How it Works

> This documentation is currently being improved.

In essence, a [Midihub](https://blokas.io/midihub/) hardware MIDI processor is used to "split" the Rev2 into two logical devices:

- a keyboard with a MIDI input and a MIDI output
- a synthesizer with a MIDI input and a MIDI output

![Diagram](diagram.png "Diagram")

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
