# rev2-master-keyboard

The [Prophet Rev2](https://www.sequential.com/product/prophetrev2/) is a polyphonic analog synthesizer, paired with a 5-octave keyboard. It also has modulation and pitch wheels, and inputs for expression and sustain pedals. For these reasons, it's a common scenario to use it to control other devices, via MIDI. However, since it wasn't designed to be used as master MIDI keyboard, it falls short in a [variety of ways](rev2-limitations.md).

Using a [Midihub](https://blokas.io/midihub), this project works around these limitations, so that the Rev2 can be used as a **master MIDI keyboard**.

---

![Mood](mood.jpg "Mood")

# Table of Contents
1. [Features](#features)
1. [Setup Instructions](setup.md)
1. [Usage](#usage)
1. [Zones](#zones)
1. [Caveats](#caveats)
1. [Control Plane](#control-plane)
1. [Rev2 Limitations](rev2-limitations.md)

# Features

- Split MIDI related to the **Keyboard** into its own MIDI channel, independent of the **Synth**'s channel.
- [**Control**](#control-plane) the **Keyboard**'s MIDI channel, [**Zones**](#zones) and other parameters through MIDI CC.
- **Knobs** operate as if `Local control` would be `On`, i.e. they control the **Synth** and also trigger MIDI messages.
- [**Zones**](#zones)
    - Split the **Keyboard** into **zones A and B**, independently of the **Synth**'s layers.
    - Assign **Zones** to different MIDI channels, independently of the channels the **Synth** operates on.
    - Transpose each **Zone** independently, e.g. transpose Zone A by -2 octaves, and Zone B by +1 octave.
- Fixes Rev2 issues:
    - When Local Control is `Off` and Multimode is `On`, using the Program and Bank knobs now also changes Layer B's preset.

![Diagram](diagram.png "Diagram")

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

In practice, this means the `Split A|B` button on the device's panel continues to work as expected in what concerns the Synth's *layers*, but has no effect whatsoever on the keyboard. When the `Split A|B` button is lit, each layer continues to be addressable through `IN A`, on the channels you've previously [configured](setup.md#change-the-rev2s-midi-channel) (assuming Multimode is `On`).

You can configure Zones through the [Control Plane](#control-plane).

On an initialized patch, only zone A is enabled, and it occupies the totality of the keyboard. Once zone B is [enabled](#cc6-zone-b-enabledisable), it occupies the right side of the keyboard, starting at the 3rd C note. Zone B's Start Key (i.e. the split point), can also be configured through the Control Plane.

# Caveats
## NRPN
The Rev2 must be set to send and receive NRPN instead of CC, i.e. both the `MIDI Param Send` and `MIDI Param Rcv` settings must be set to NRPN. See [here](rev2-limitations.md#lfo-and-other-parameters-not-sent-in-cc-mode) for why this is the case.

This means, the Synth (`OUT A`) will output NRPN, and will only accept NRPN as input (`IN A`). This can be limiting when using the Rev2 with other gear, since most do not support NRPN. For example, you won't be able to automate the filter cutoff frequency with an external sequencer that does not support NRPN, which is the case with many sequencers.

However, there are [plans](https://community.blokas.io/t/convert-cc-to-nrpn/2359/2) to add NRPN-to-CC conversion to the Midihub, which would likely allow this limitation to be addressed.

# Control Plane
You can control various parameters of the Midihub's patch through MIDI CC messages sent to `IN A`, on channel 16.

- [Zone A MIDI Channel](#cc1-zone-a-midi-channel)
- [Zone B MIDI Channel](#cc2-zone-b-midi-channel)
- [Zone A Octave](#cc3-zone-a-octave)
- [Zone B Octave](#cc4-zone-b-octave)
- [Zone B Start Key](#cc5-zone-b-start-key)
- [Zone B Enable/Disable](#cc6-zone-b-enabledisable)
- [Request CC Dump](#cc127-request-cc-dump)

## CC1: Zone A MIDI Channel
Set the MIDI Channel of messages produced by Zone A. Note this does **not** control the channel of the Synth's Layer A, that's still configurable through the Rev2's settings. If you do change the channel the Rev2 operates on, will also need to [adjust it accordingly in the Midihub's patch](setup.md#change-the-rev2s-midi-channel).

Values of an initialized patch are indicated as **default**.

| CC1 Value | Zone A MIDI Channel |             |
|:---------:|:-------------------:|-------------|
| 0, **1**  |        **1**        | **default** |
|     2     |          2          |             |
|     3     |          3          |             |
|    ...    |         ...         |             |
| 16 - 127  |         16          |             |

## CC2: Zone B MIDI Channel
Set the MIDI Channel of messages produced by Zone B. Note this does **not** control the channel of the Synth's Layer B, that's still configurable through the Rev2's settings. If you do change the channel the Rev2 operates on, will also need to [adjust it accordingly in the Midihub's patch](setup.md#change-the-rev2s-midi-channel).

Values of an initialized patch are indicated as **default**.

| CC2 Value | Zone B MIDI Channel |             |
|:---------:|:-------------------:|-------------|
|   0, 1    |          1          |             |
|   **2**   |        **2**        | **default** |
|     3     |          3          |             |
|    ...    |         ...         |             |
| 16 - 127  |         16          |             |

## CC3: Zone A Octave
Transpose Zone A up or down N octaves.

Values of an initialized patch are indicated as **default**. The *first-key note* is the note produced by the 1st key on the keyboard, starting from the left.

| CC3 Value | Zone A Octave | First-key Note |             |
|:---------:|:-------------:|:--------------:|-------------|
|     0     |      -3       |      C-2       |             |
|     1     |      -2       |      C-1       |             |
|     2     |      -1       |       C0       |             |
|   **3**   |    **+0**     |     **C1**     | **default** |
|     4     |      +1       |       C2       |             |
|     5     |      +2       |       C3       |             |
|     6     |      +3       |       C4       |             |
|  7 - 127  |      +4       |       C5       |             |

## CC4: Zone B Octave
Transpose Zone B up or down N octaves.

Values of an initialized patch are indicated as **default**. *First-key note* is the note produced by the 1st key on the keyboard, starting from the left. Note than with an initialized patch, the first-key is on Layer A. You can use [CC5](#cc5-zone-b-start-key) to control where on the keyboard Zone B starts.

| CC4 Value | Zone B Octave | First-key Note |             |
|:---------:|:-------------:|:--------------:|-------------|
|     0     |      -3       |      C-2       |             |
|     1     |      -2       |      C-1       |             |
|     2     |      -1       |       C0       |             |
|   **3**   |    **+0**     |     **C1**     | **default** |
|     4     |      +1       |       C2       |             |
|     5     |      +2       |       C3       |             |
|     6     |      +3       |       C4       |             |
|  7 - 127  |      +4       |       C5       |             |

## CC5: Zone B Start Key
Set the key on the keyboard from which Zone B starts, inclusively. In other words, it's the split point between Zones A and B.

Values of an initialized patch are indicated as **default**.

| CC5 Value | Zone B Start Key |                      |
|:---------:|:----------------:|----------------------|
|   0, 1    |      1st C       | *Zone A is disabled* |
|     2     |      2nd C       |                      |
|   **3**   |    **3rd C**     | **default**          |
|     4     |      4th C       |                      |
|  5 - 127  |      5th C       |                      |

## CC6: Zone B Enable/Disable
Enable or disable Zone B. When disabled, all keys on the keyboard are assigned to Zone A.

Values of an initialized patch are indicated as **default**.

| CC6 Value |    Zone B status    |             |
|:---------:|:-------------------:|-------------|
|   **0**   | **Zone B Disabled** | **default** |
|  1 - 127  |   Zone B Enabled    |             |

## CC127: Request CC Dump
Requests a dump of all CCs. Upon receiving this message, for each of the CCs above (CC1, CC2, CC3, etc), one message will be output to `OUT A`, on channel 16, containing the current value of the respective CC.

This is useful so an external MIDI controller can be "refreshed" to the internal state of the Midihub.

| CC127 Value |      CC Dump       |
|:-----------:|:------------------:|
|   0 - 127   | All CCs are dumped |
