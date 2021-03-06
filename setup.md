# Setup Instructions
> Make sure you follow all instructions in this document, until the end.

## Set Rev2's global settings
The first thing to do is to make sure the Rev2's global settings are correctly set:

- MIDI Channel: 1 (See also: [Change the Rev2's MIDI channel](#change-the-rev2s-midi-channel))
- MIDI Param Send: NRPN
- MIDI Param Rcv: NRPN
- MIDI Control: On
- MIDI Program Enable: On
- MIDI Program Send: On
- MIDI Out Select: MIDI
- Local Control :Off

## Connect MIDI cables
The next step is to connect all the MIDI cables (see image below for a visual aid):

- From Midihub's `OUT C` to Rev2's `IN`
- From Rev2's `OUT` to Midihub's `IN C`
- From Midihub's `OUT B` to Midihub's `IN B`.
- From Midihub's `OUT A` to a MIDI interface or MIDI thru box, depending on your setup.
- (Optional) From a MIDI interface, sequencer, or MIDI controller's `OUT` to Midihub's `IN A`.

![](diagram.png)

## Install Midihub's editor
You'll need the Midihub Editor to install the patch. Download and install the editor for your operating system:

- [Windows](https://blokas.io/midihub/downloads/latest/windows/)
- [MacOS](https://blokas.io/midihub/downloads/latest/mac/)
- [Linux](https://blokas.io/midihub/downloads/latest/linux/)
- [Raspberry Pi](https://blokas.io/midihub/downloads/latest/linux_arm/)

## Install Midihub's patch

1. Connect the Midihub to your computer through USB, and open the Midihub Editor that you installed in the previous step.
2. Click the `Connect` button to connect the Editor to the Midihub.
3. In the `View` menu, make sure `Patchstorage` is enabled.
4. In the `Patchstorage` panel, search for `rev2` and locate the `REV2 MASTER KEYBOARD` patch
5. Double-click it, then click `Append`
6. Click the button with the down arrow on top left of the Editor, and `Store to current preset` (or to whatever preset you wish).
7. Click the `Disconnect` button
8. (optional) Disconnect the Midihub from your computer.

## Change the Rev2's MIDI channel
> This is an optional step. If you wish to keep the Rev2 set to channels 1 and 2, skip this step.

The Rev2's factory default for Layer A's MIDI channel is channel `1` (which implicitly sets Layer B's MIDI channel to `2`, when Multimode is `On`). We use the same default in the Midihub's patch. If you wish to change this default, you must set the channel in the Rev2's global settings, then edit the Midihub's patch accordingly.

Locate the following _pipe_, and set the `Set Channel To` field of **both** `TRANSFORM` blocks to **the Layer B's channel**, e.g, if you set the channel in the Rev2's Global Settings to 7, this should be set to 8.

![](patch-point.png)

## Setup done!
See [usage instructions](README.md#usage).