# rev2-master-keyboard

The [Prophet Rev2](https://www.sequential.com/product/prophetrev2/) is a polyphonic analog synthesizer, paired with a 5-octave keyboard. It also has modulation and pitch wheels, and inputs for expression and sustain pedals. For these reasons, it's a common scenario to use it to control other devices, via MIDI. However, since it wasn't designed to be used as master MIDI keyboard, it falls short in a [variety of ways](rev2-limitations.md).

Using a [Midihub](https://blokas.io/midihub), this project works around these limitations, so that the Rev2 can be used as a **master MIDI keyboard**.

---

![Mood](mood.jpg "Mood")

# Table of Contents
1. [How it Works](how.md)
1. [Features](#features)
1. [Setup Instructions](setup.md)
1. [Usage](#usage)
1. [Zones](#zones)
1. [Caveats](#caveats)
1. [Control Plane](#control-plane)
1. [Rev2 Limitations](rev2-limitations.md)

# Features

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
- **Knobs** operate as if `Local control` would be `On`, i.e. they control the **Synth**
- [**Keyboard zones**](#zones)
    - Split the **Keyboard** into **zones A and B**, independently of the **Synth**'s layers
    - Assign **Zones** to different MIDI channels, independently of the channels the **Synth** operates on
    - Transpose each **Zone** independently, e.g. transpose Zone A by -2 octaves, and Zone B by +1 octave
- [Control](#control-plane) the **Keyboard** through MIDI CC on **MIDI INPUT A**, e.g. **Zone** split point
- Send MIDI to the **Synth** on **MIDI INPUT C**

# Usage
> This section is yet to be written.

- Assuming setup is done, you know have a master MIDI keyboard AND a Rev2. They don't talk to eachother unless you set that up.
- Only Zone A is active
- Standalone mode
- Controlling through MIDI CC
- Set MIDI channel
- Initialized patch

# Zones
The Keyboard can be split into two **Zones** (A and B), which are unrelated and operate completely independently of the Synth's *splits*. In fact, the Rev2's feature of splitting the keyboard into two *splits* is no longer used, with Zones being implemented solely on the Midihub. This gives us the flexibility to, for example, change the octave of each Zone independently, a feature the Rev2 does not provide.

In practice, this means the `Split A|B` button on the device's panel continues to work as expected in what concerns the Synth's *layers*, but has no effect whatsoever on the keyboard. When the `Split A|B` button is lit, each layer continues to be addressable through `MIDI IN C`, on the channels you've previously [configured](setup.md#changing-the-rev2s-midi-channel). This is also the case when the `Stack A|B` button is lit.

You can configure Zones through the [Control Plane](#control-plane).

On an initialized patch, only zone A is enabled, and it occupies the totality of the keyboard. Once zone B is [enabled](#zone-b-enabledisable-cc6), it occupies the right side of the keyboard, starting at the 3rd C note. Zone B's Start Key (i.e. the split point), can also be configured through the Control Plane.

# Caveats
## Multimode
When working with Rev2 patches which use both layers, `Multimode` must be enabled. Multimode splits voices between layers even when only layer A is active. Is this true?

## NRPN
The Rev2 must be set to send and receive NRPN instead of CC, i.e. both the `MIDI Param Send` and `MIDI Param Rcv` settings must be set to NRPN. See [here](rev2-limitations.md#lfo-and-other-parameters-not-sent-in-cc-mode) for why this is the case.

This means, the Synth (`OUT C`) will output NRPN, and will only accept NRPN as input (`MIDI IN C`). This can be limiting when using the Rev2 with other gear, since most do not support NRPN. For example, you won't be able to automate the filter cutoff frequency with an external sequencer that does not support NRPN, which is the case with many sequencers.

However, there are [plans](https://community.blokas.io/t/convert-cc-to-nrpn/2359/2) to add NRPN-to-CC conversion to the Midihub, which would likely allow this limitation to be addressed.

# Control Plane
You can control various parameters of the Midihub's patch through MIDI CC messages sent to `MIDI IN A`. By default, **only messages sent on channel 16 are considered**, messages on any other channel are dropped. However, the channel can easily be changed through Midihub's editor.

## Zone A MIDI Channel (CC1)
Set the MIDI Channel of messages produced by Zone A. Note this does **not** control the channel of the Synth's Layer A, that's still configurable through the Rev2's settings. Note that if you do change the channel the Rev2 operates on, will also need to adjust it accordingly in the Midihub's patch. 

Values of an initialized patch are indicated as **default**.

| CC1 Value | Zone A MIDI Channel ||
|:---------:|:-------------------:|-|
| 0, **1**  | **1**               | **default** |
| 2         | 2                   ||
| 3         | 3                   ||
| ...       | ...                 ||
| 16 - 127  | 16                  ||

## Zone B MIDI Channel (CC2)
Set the MIDI Channel of messages produced by Zone B.

Values of an initialized patch are indicated as **default**.

| CC2 Value | Zone B MIDI Channel ||
|:---------:|:--------------------:|-|
| 0, 1      | 1                   ||
| **2**     | **2**               |**default**|
| 3         | 3                   |
| ...       | ...                 |
| 16 - 127  | 16                  |

## Zone A Octave (CC3)
Transpose Zone A up or down N octaves.

Values of an initialized patch are indicated as **default**. The *first-key note* is the note produced by the 1st key on the keyboard, starting from the left.

| CC3 Value | Zone A Octave | First-key Note ||
|:---------:|:-------------:|:--------------:|-|
| 0         | -3            | C-2            ||
| 1         | -2            | C-1            ||
| 2         | -1            | C0             ||
| **3**     | **+0**        | **C1**         | **default** |
| 4         | +1            | C2             |
| 5         | +2            | C3             |
| 6         | +3            | C4             |
| 7 - 127   | +4            | C5             |

## Zone B Octave (CC4)
Transpose Zone B up or down N octaves.

Values of an initialized patch are indicated as **default**. *First-key note* is the note produced by the 1st key on the keyboard, starting from the left. Note than with an initialized patch, the first-key is on Layer A. You can use [CC5](#zone-b-start-key-cc5) to control where on the keyboard Zone B starts.

| CC4 Value | Zone B Octave | First-key Note ||
|:---------:|:-------------:|:--------------:|-|
| 0         | -3            | C-2            ||
| 1         | -2            | C-1            ||
| 2         | -1            | C0             ||
| **3**     | **+0**        | **C1**         | **default** |
| 4         | +1            | C2             |
| 5         | +2            | C3             |
| 6         | +3            | C4             |
| 7 - 127   | +4            | C5             |

## Zone B Start Key (CC5)
Set the key on the keyboard from which Zone B starts, inclusively. In other words, it's the split point between Zones A and B.

Values of an initialized patch are indicated as **default**.

| CC5 Value | Zone B Start Key  ||
|:---------:|:-----------------:|-|
| 0, 1      | 1st C             | *Zone A is disabled* |
| 2         | 2nd C             ||
| **3**     | **3rd C**         | **default** |
| 4         | 4th C             |
| 5 - 127   | 5th C             |

## Zone B Enable/Disable (CC6)
Enable or disable Zone B. When disabled, all keys on the keyboard are assigned to Zone A.

Values of an initialized patch are indicated as **default**.

| CC6 Value | Zone B status       ||
|:---------:|:-------------------:|-|
| **0**     | **Zone B Disabled** |**default** |
| 1 - 127   | Zone B Enabled      |
