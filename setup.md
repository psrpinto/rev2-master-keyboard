# Setup Instructions
> This documentation is currently being improved.

## Changing the Rev2's MIDI channel
The Rev2's factory default for Layer A's MIDI channel is channel `1` (which implicitly sets Layer B's MIDI channel to `2`). We use the same default internally in the Midihub's patch.

If you wish to change this default, you must set the channel in the Rev2's global settings, and [edit the Midihub's patch](how.md#set-the-midi-channel) accordingly.

## Rev2 Global Settings
Set Rev2 global settings as follows:

- MIDI Channel: 1
- MIDI Param Send: NRPN
- MIDI Param Rcv: NRPN
- MIDI Control: On
- MIDI Program Enable: On
- MIDI Program Send: On
- MIDI Out Select: MIDI
- Local Control :Off
