# rev2-master-keyboard

The [Prophet Rev2](https://www.sequential.com/product/prophetrev2/) is a polyphonic analog synthesizer, paired with a 5-octave keyboard, with modulation and pitch wheels, and inputs for expression and sustain pedals. For these reasons, it's a common scenario to use it to control other devices, via MIDI. However, since it wasn't designed to be used as master MIDI keyboard, it falls short in a [variety of ways](#rev2-limitations).

Using a [Midihub](https://blokas.io/midihub), this project works around these limitations, so that the Rev2 can be used as a **master MIDI keyboard**.

---

![Mood](mood.jpg "Mood")

## Features

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

## Usage
Once you have followed the [Setup Instructions](setup.md), you can think of the Rev2 as two independent devices: a Synth and a Keyboard. Both are accessible through Midihub's `IN A` and `OUT A` but on separate channels. The Synth operates on the channels you've configured during setup (1 and 2 by default), and the Keyboard operates on an assignable channel, from 1 to 16.

You set the Keyboard's channel through MIDI CC messages sent to `IN A`, on channel 16. To do this, you can use any device capable of sending MIDI CC, e.g. a computer, a MIDI sequencer, or a MIDI controller like the one in the picture above ([Faderfox EC4](http://faderfox.de/ec4.html)). See [Control Plane](#control-plane) for a list of all parameters you can control.

The Keyboard outputs the following types of messages, on the assigned channel:

- Note On/Off
- Channel Pressure (aka Aftertouch)
- Pitch Bend
- Modulation Wheel (CC1)
- Expression Pedal (CC11)
- Sustain Pedal (CC64)

You can also optionally assign the Expression Controls (Channel Pressure, Pitch Bend, Modulation Wheel, Expression Pedal and Sustain Pedal) to a different channel than Note On/Off messages.

When you turn a knob in the Rev2, it is immediately reflected in the sound (as if Local Control would be `On`), **and** [NRPN](#nrpn) messages are sent through `OUT A`. You can also send clock, transport, NRPN or any other MIDI message to the Rev2, through `IN A` on the channels you've configured (1 and 2 by default).

Note that the Midhub keeps its memory across power cycles. If, for example, you set the Keyboard's MIDI channel to 7, then turn the Midihub off and back on, the Keyboard's MIDI channel will still be set to 7.

## Zones
You can split the Keyboard into two **Zones** (A and B), which are unrelated and operate independently of the Synth's *splits*. In fact, the Rev2's feature of splitting the keyboard into two *splits* is no longer used, with Zones being implemented solely on the Midihub. This gives us the flexibility to, for example, change the octave of each Zone independently, a feature the Rev2 does not provide.

In practice, this means the `Split A|B` button on the device's panel continues to work as expected in what concerns the Synth's *layers*, but has no effect whatsoever on the keyboard. When the `Split A|B` button is lit, each layer continues to be addressable through `IN A`, on the channels you've previously [configured](setup.md#change-the-rev2s-midi-channel) (assuming Multimode is `On`).

You can configure Zones through the [Control Plane](#control-plane).

On an initialized patch, only zone A is enabled, and it occupies the totality of the keyboard. Once zone B is [enabled](#cc6-zone-b-enabledisable), it occupies the right side of the keyboard, starting at the 3rd C note. Zone B's Start Key (i.e. the split point), can also be configured through the Control Plane.

## Caveats
### NRPN
The Rev2 must be set to send and receive NRPN instead of CC, i.e. both the `MIDI Param Send` and `MIDI Param Rcv` settings must be set to NRPN. See [here](#lfo-and-other-parameters-not-sent-in-cc-mode) for why this is the case.

This means, the Synth (`OUT A`) will output NRPN, and will only accept NRPN as input (`IN A`). This can be limiting when using the Rev2 with other gear, since most do not support NRPN. For example, you won't be able to automate the filter cutoff frequency with an external sequencer that does not support NRPN, which is the case with many sequencers.

However, there are [plans](https://community.blokas.io/t/convert-cc-to-nrpn/2359/2) to add NRPN-to-CC conversion to the Midihub, which would likely allow this limitation to be addressed.

## Control Plane
You can control various parameters of the Midihub's patch through MIDI CC messages sent to `IN A`, on channel 16.

- [Zone A MIDI Channel](#cc1-zone-a-midi-channel)
- [Zone B MIDI Channel](#cc2-zone-b-midi-channel)
- [Zone A Octave](#cc3-zone-a-octave)
- [Zone B Octave](#cc4-zone-b-octave)
- [Zone B Start Key](#cc5-zone-b-start-key)
- [Zone B Enable/Disable](#cc6-zone-b-enabledisable)
- [Expression Controls MIDI Channel](#cc7-expression-controls-midi-channel)
- [Request CC Dump](#cc127-request-cc-dump)

### CC1: Zone A MIDI Channel
Set the MIDI Channel of messages produced by Zone A of the Keyboard.

> This does **not** control the channel of the Synth's Layer A, that's still configurable through the Rev2's Global Settings.

| CC1 Value | Zone A MIDI Channel |             |
|:---------:|:-------------------:|-------------|
| 0, **1**  |        **1**        | **default** |
|     2     |          2          |             |
|     3     |          3          |             |
|    ...    |         ...         |             |
| 16 - 127  |         16          |             |

### CC2: Zone B MIDI Channel
Set the MIDI Channel of messages produced by Zone B of the Keyboard.

> This does **not** control the channel of the Synth's Layer B, that's still configurable through the Rev2's Global Settings.

| CC2 Value | Zone B MIDI Channel |             |
|:---------:|:-------------------:|-------------|
|   0, 1    |          1          |             |
|   **2**   |        **2**        | **default** |
|     3     |          3          |             |
|    ...    |         ...         |             |
| 16 - 127  |         16          |             |

### CC3: Zone A Octave
Transpose Zone A up or down N octaves.

> *First-key note* is the note produced by the 1st key on the keyboard.

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

### CC4: Zone B Octave
Transpose Zone B up or down N octaves.

> *First-key note* is the note produced by the 1st key on the keyboard.

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

### CC5: Zone B Start Key
Set the key on the keyboard from which Zone B starts, inclusively.

> In other words, the split point between Zones A and B.

| CC5 Value | Zone B Start Key |                                   |
|:---------:|:----------------:|-----------------------------------|
|   0, 1    |      1st C       | *Zone B occupies entire keyboard* |
|     2     |      2nd C       |                                   |
|   **3**   |    **3rd C**     | **default**                       |
|     4     |      4th C       |                                   |
|  5 - 127  |      5th C       |                                   |

### CC6: Zone B Enable/Disable
Enable or disable Zone B.

> When disabled, all keys on the keyboard are assigned to Zone A.

| CC6 Value |    Zone B status    |             |
|:---------:|:-------------------:|-------------|
|   **0**   | **Zone B Disabled** | **default** |
|  1 - 127  |   Zone B Enabled    |             |

### CC7: Expression Controls MIDI channel
Set the MIDI Channel of Channel Pressure, Modwheel, Pitchbend, Expression Pedal and Sustain Pedal.

| CC7 Value | Expression Controls MIDI Channel |             |
|:---------:|:--------------------------------:|-------------|
|   **0**   | **Same channel as Zone A notes** | **default** |
|     1     |                1                 |             |
|     2     |                2                 |             |
|     3     |                3                 |             |
|    ...    |               ...                |             |
| 16 - 127  |                16                |             |

### CC127: Request CC Dump
Requests a dump of all CCs.

> Upon receiving this message, for each of the CCs above (CC1, CC2, CC3, etc), one message will be output to `OUT A`, on channel 16, containing the current value of the respective CC. This is useful so an external MIDI controller can be "refreshed" to the internal state of the Midihub.

| CC127 Value |      CC Dump       |
|:-----------:|:------------------:|
|   0 - 127   | All CCs are dumped |

## Rev2 limitations
This section lists some of Rev2's limitations, some of which this project addresses.

### No hybrid Local Control mode
It's not possible to have the knobs control the device, while the keyboard just sends MIDI. If it would, this project would probably not need to exist, or its scope would be significantly reduced.

### LFO and other parameters not sent in CC mode
With `Local Control` set to `Off` and `MIDI Param Send` set to `CC`, for many of the Rev2's parameters, no MIDI message is triggered when the parameter changes. This is because there are more than 127 parameters in the Rev2. Setting `MIDI Param Send` to `NRPN` fixes this issue.

### Program Change in Multimode
With `Local Control` set to `Off`, and `Multimode` set to `On`, Program Change messages triggered by the Program and Bank knobs are only sent to Layer A's MIDI channel, i.e. they aren't duplicated to Layer B's MIDI channel. In a setting where these messages are re-routed back to the Rev2, this causes Layer A's Program to change, while Layer B's Program remains unchanged.

## Contributing
If you use this project and have any issues with it, or have any kind of feedback, please consider [opening an issue](https://github.com/psrpinto/rev2-master-keyboard/issues/new).