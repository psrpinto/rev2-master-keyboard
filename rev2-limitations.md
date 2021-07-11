# Rev2 limitations
This document lists some of Rev2's limitations, some of which this project addresses.

## No hybrid Local Control mode
It's not possible to have the knobs control the device, while the keyboard just sends MIDI. If it would, this project would probably not need to exist, or its scope would be significantly reduced.

## LFO and other parameters not sent in CC mode
With `Local Control` set to `Off` and `MIDI Param Send` set to `CC`, for many of the Rev2's parameters, no MIDI message is triggered when the parameter changes. This is because there are more than 127 parameters in the Rev2. Setting `MIDI Param Send` to `NRPN` fixes this issue.

## Multimode splits voices between layers even in single-layer patches
Is this true?

## Program Change in Multimode
With `Local Control` set to `Off`, and `Multimode` set to `On`, Program Change messages triggered by the Program and Bank knobs are only sent to Layer A's MIDI channel, i.e. they aren't duplicated to Layer B's MIDI channel. In a setting where these messages are re-routed back to the Rev2, this causes Layer A's Program to change, while Layer B's Program remains unchanged.
