The [Prophet Rev2](https://www.sequential.com/product/prophetrev2/) is a polyphonic analog synthesizer, paired with a 5-octave *[keybed](https://www.sweetwater.com/insync/keybed/)*. It also has modulation and pitch wheels, and inputs for expression and sustain pedals. For these reasons, it's a common scenario to use it to control other devices, via MIDI. However, since it wasn't designed to be used as master MIDI keyboard, it falls short in a [variety of ways](#rev2-limitations).

Using a [Midihub](https://blokas.io/midihub), this project works around these limitations, so that the Rev2 can be used as a fully-featured master MIDI keyboard.

---

# Table of Contents
1. [Setup Instructions](setup.md)
1. [How it Works](how.md)
1. [Summary of Features](#features)
1. [Usage](#usage)
1. [Zones](#zones)
1. [Control Plane](#control-plane)
1. [Rev2 Limitations](#rev2-limitations)

![Mood](mood.jpg "Mood")

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
- **Knobs** operate as if `Local control` would be `On`
- [**Keyboard zones**](#zones)
    - **Keyboard** can be split into **zones A and B**, which are independent of the **Synth**'s layers
    - **Zones** can be assigned to different MIDI channels, independently of the channels the **Synth** operates on
    - **Zones** can be transposed independently, e.g. transpose zone A by -2 octaves, and zone B by +1 octave
- **Keyboard** can be [controlled](#control-plane) through MIDI CC on **MIDI INPUT A**, e.g. **Zone** split point
- **Synth** accepts MIDI input on **MIDI INPUT C**

# Usage
> This section is yet to be written.

# Zones
> This section is yet to be written.

# Control Plane
This section describes how to control various parameters of the Midihub's patch, through MIDI CC messages sent to Midihub's Input A. By default, **only messages sent on channel 16 are considered**, messages on any other channel are dropped. However, the channel can easily be changed through Midihub's editor.

## Zone A MIDI Channel (CC1)
Values of an initialized patch are indicated as **default**.

| CC1 Value | Zone A MIDI Channel ||
|:---------:|:-------------------:|-|
| 0, **1**  | **1**               | **default** |
| 2         | 2                   ||
| 3         | 3                   ||
| ...       | ...                 ||
| 16 - 127  | 16                  ||

## Zone B MIDI Channel (CC2)
Values of an initialized patch are indicated as **default**.

| CC2 Value | Zone B MIDI Channel ||
|:---------:|:--------------------:|-|
| 0, **1**  | **1**               | **default** |
| 2         | 2                   |
| 3         | 3                   |
| ...       | ...                 |
| 16 - 127  | 16                  |

## Zone A Octave (CC3)
Values of an initialized patch are indicated as **default**. The *first-key note* is the note of the 1st key on the keyboard, starting from the left.

| CC3 Value | Zone A Octave | First-key Note ||
|:---------:|:-------------:|:--------------:|-|
| 0         | -4            | C-2            ||
| 1         | -3            | C-1            ||
| 2         | -2            | C0             ||
| **3**     | **+0**        | **C1**         | **default** |
| 4         | +1            | C2             |
| 5         | +2            | C3             |
| 6         | +3            | C4             |
| 7 -127    | +4            | C5             |

## Zone B Octave (CC4)
Values of an initialized patch are indicated as **default**. *First-key note* is the note of the 1st key on the keyboard, starting from the left. The values presented here assume Zone B is occupying the totality of the keyboard. Note that if Zone A is active, it will occupy part of the keyboard. You can use [CC5](#zone-b-start-key-cc5) to control where on the keyboard Zone B starts.

| CC4 Value | Zone B Octave | First-key Note ||
|:---------:|:-------------:|:--------------:|-|
| 0         | -4            | C-2            ||
| 1         | -3            | C-1            ||
| 2         | -2            | C0             ||
| **3**     | **+0**        | **C1**         | **default** |
| 4         | +1            | C2             |
| 5         | +2            | C3             |
| 6         | +3            | C4             |
| 7 -127    | +4            | C5             |


## Zone B Start Key (CC5)

| CC5 Value | Zone B Start Key |
|:---------:|:----------------:|
| 0 - 39    | First C (1st white key |
| 40        | First D (2nd white key) |
| ...       | ...              |
| 62        | Second B (14th white key) |
| **63**    | **Third C (15th white key)** |

# Rev2 limitations
This section documents some of the Rev2's limitations.

## No hybrid Local Control mode

## LFO and other parameters not sent in CC mode
With `Local Control` set to `Off` and `MIDI Param Send` set to `CC`, for many of the Rev2's parameters, no MIDI message is triggered when the parameter changes. This is because there are more than 127 parameters in the Rev2. Setting `MIDI Param Send` to `NRPN` fixes this issue.

## Program Change in Multimode
With `Local Control` set to `Off`, and `Multimode` set to `On`, Program Change messages triggered by the Program and Bank knobs are only sent to Layer A's MIDI channel, i.e. they aren't duplicated to Layer B's MIDI channel. In a setting where these messages are re-routed back to the Rev2, this causes Layer A's Program to change, while Layer B's Program remains unchanged.
