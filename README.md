# rev2-master-keyboard
The [Prophet Rev2](https://www.sequential.com/product/prophetrev2/) is a fantastic polyphonic analog synthesizer, paired with a very capable 5-octave *[keybed](https://www.sweetwater.com/insync/keybed/)*. It also has modulation and pitch wheels, and inputs for expression and sustain pedals. For these reasons, it's a common scenario to use it to control other devices, via MIDI. However, since it wasn't designed to be used as master MIDI keyboard, it falls short in a [variety of ways](#rev2-limitations).

This project is the culmination of my quest of working around these limitations so that the Rev2 can be used as a fully-featured master MIDI keyboard.

![Mood](mood.jpg "Mood")

## Features

- Use the Rev2 as if it were two independent devices: a **Keyboard** and a **Synth**
- **Keyboard** outputs on **MIDI OUT A**
    - Note On
    - Note Off
    - Modulation Wheel
    - Pitch Wheel
    - Channel Pressure (*Aftertouch*)
    - Expression Pedal
    - Sustain Pedal
- **Synth** outputs on **MIDI OUT C**
    - CC (NRPN)
    - Clock
    - Program Change
- **Knobs** operate as if *local control* would be *on*
- **Keyboard zones**
    - **Keyboard** can be split into **zones A and B**, which are independent of the **Synth**'s layers
    - **Zones** can be assigned to different MIDI channels, independently of the channels the **Synth** operates on
    - **Zones** can be transposed independently, e.g. transpose zone A by -2 octaves, and zone B by +1 octave
- **Keyboard** can be controlled through MIDI CC on **MIDI INPUT A**, e.g. **Zone** split point
- **Synth** accepts MIDI input on **MIDI INPUT C**

## How it works

In essence, a [Midihub](https://blokas.io/midihub/) hardware MIDI processor is used to "split" the Rev2 into two logical devices:

- a keyboard with a MIDI input and a MIDI output
- a synthesizer with a MIDI input and a MIDI output

![Diagram](diagram.png "Diagram")

## Setup
For setup instructions go [here](setup.md).

## Rev2 limitations
This section documents some of the Rev2 limitations.

### Program change in Multimode
With Local Control Off, and Multimode On, Program Change messages triggered by the Program and Bank knobs are only sent to Layer A's MIDI channel, i.e. they aren't duplicated to Layer B's MIDI channel. This means the knobs only affect Layer A's program, with Layer B remaining in whatever Program it was set to before the knob was turned.

## Control plane
Input A, channel 16.

- CC 1: Zone A output channel
    - Valid values: 1 to 16
    - 0 is interpreted as 1
    - Values greater than 16 are interpreted as 16
- CC 2: Zone B output channel
    - Valid values: 1 to 16
    - 0 is interpreted as 1
    - Values greater than 16 are interpreted as 16
- CC 3: Zone A transpose
    - Bipolar (-64 to +63)
- CC 4: Zone B transpose
    - Bipolar (-64 to +63)
- CC 5: Zone B start key
    - Bipolar (-64 to +63)
