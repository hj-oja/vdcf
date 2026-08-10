# hj Firmware — A custom firmware for volca drum

An unofficial firmware modification for the volca drum, based on the official firmware v1.14. It adds performance features and fixes a handful of bugs.

## At a glance

- FUNC + knob shows the current parameter value without changing it
- SWING extended to negative values
- FUNC + SELECT cycles SRC only
- ACCENT extended to negative values (for ghost notes)
- SLICE extended — sub-step patterns
- PROBABILITY gains Elektron-style trigger conditions — `A-B`, first bar, FILL
- MUTE + STEP 7–16 clears motion sequences per knob
- TOUCH FX on STEP 9–16
- MIDI IN velocity finally works properly

## Disclaimer
This firmware is an unofficial project created independently of KORG and is not endorsed by, affiliated with, or supported by KORG. Installing it is a modification to the hardware and may void any warranty or support coverage.

The flashing path used for updates is handled by a separate bootloader area that this firmware does not overwrite, so a failed write can usually be recovered by re-entering update-receive mode and flashing again.

I accept no responsibility for any damage, malfunction, or unexpected results that may arise from installing or using this firmware. **Use it at your own risk.** If you run into trouble, please report it by opening a GitHub issue.

---

## What's new

### FUNC + knob shows the current parameter value

Turning a knob on the volca drum is a jump — the parameter snaps to wherever the knob physically sits. Holding FUNC while you turn now displays the stored value instead of changing it.

### SWING extended to negative values

The stock range was `0...75`. It has now been expanded to `-75...75`.

### FUNC + SELECT cycles SRC only

SELECT normally steps through every combination of SRC, MOD and EG. Hold FUNC and it cycles the SRC waveform on its own.

### ACCENT extended to negative values — ghost notes

Stock ACCENT is `0...16`, so a step could only ever be louder than the LEVEL knob setting. Now `-15...16`: negative values duck a step below the base level, which is what you want for ghost notes and dynamics.

### SLICE extended — sub-step patterns

Stock SLICE is `1...16`: divide the step into N parts and play all of them. The negative side adds 17 sub-step patterns, up to 4 subdivisions.

### PROBABILITY gains Elektron-style trigger conditions

The negative side of PRB adds trigger conditions:

| Display | Meaning |
|---|---|
| `A-B` | Fires on A-th bar of every B-bar cycle (B = 2...8) |
| `1St` | Fires only on the very first bar after PLAY |
| `_1St` | The inverse — every bar except the first |
| `FILL` | Fires only while the FILL touch FX is held |
| `_FIL` | The inverse — muted while FILL is held |

Bars are counted per part. The counter resets on PLAY.

### MUTE + STEP 7–16 clears motion sequences per knob

Stock firmware can only clear motion per part (`CLR MPRT`) or all of it at once (`CLR MALL`). Now every knob has its own `CLR` / `UND` pair: the first press clears that knob's motion on the selected part and layer, the second press undoes it.

| STEP | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 |
|---|---|---|---|---|---|---|---|---|---|---|
| Target | PITCH | MOD AMT | MOD RATE | ATTACK | RELEASE | SEND | WAVE GUIDE (all) | WG DECAY | WG BODY | WG TUNE |
| Display | `MPIt` | `MAMt` | `MrAt` | `MAtk` | `MrEL` | `MSNd` | `MWGd` | `MdEC` | `MbdY` | `MtUN` |

### TOUCH FX on STEP 9–16

Momentary performance effects, active only while the step key is held.

| STEP | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 |
|---|---|---|---|---|---|---|---|---|
| Display | `OCuP` | `OCdn` | `Stut` | `rev` | `bit` | `SEnd` | `WEt` | `FILL` |
| Effect | Octave up | Octave down | Stutter gate | Reverse | Bit crush | Wave guide full send | Wave guide wet only | FILL condition |

- **Several FX can be held at once.** Only three pairs are mutually exclusive — `OCuP`/`OCdn`, `SEnd`/`WEt`, `Stut`/`rev`.

### MIDI IN velocity finally works properly

Velocity has always driven the accent amount, which is why in the stock firmware it could only ever make a note louder than the LEVEL knob — 17 steps, none of them below it. Now that ACCENT reaches into the negative, velocity works in the quiet direction too.

It now covers the full `-15...16` accent range in 32 discrete values, with velocity 63 playing the note exactly at the LEVEL knob setting.

---

## Bug fixes

**SLICE did not divide the step evenly for some of its values.**
Only eight of the sixteen values were correct; the rest fired extra hits at uneven intervals. Every value now gives exactly N evenly spaced hits.

**Undoing a part motion clear displayed `UND PART`.**
The clear side reads `CLR MPRT`, so the undo side now reads `UND MPRT` to match.

**AMOUNT behaved oddly when edited with LAY 1-2 selected.**
Editing both layers at once did not treat AMOUNT as a bipolar parameter. It is now centered on 0.

**Parameters saved with a negative value lost their default marker.**
Reloading a program would not show the "default value" mark if the stored value was negative.

**TEMPO was hard to dial in with TEMPO Range set to Full.**
The knob mapping has been redistributed — roughly 30 % of knob travel for 10–60 BPM, 50 % for 60–240 BPM, 20 % for 240–600 BPM.

---

## Installing

The update ships as an audio file, exactly the same way as KORG's official updater.

1. Connect your computer's headphone output to the volca drum's `SYNC IN` jack with a 3.5 mm stereo cable.
2. Hold FUNC and REC together and turn the power on. The unit is now in update-receive mode.
3. Play `volca_drum_sys_0120.wav` all the way through and wait.
4. If the display shows `UPD End`, turn off the volca drum.

If the display shows `Dcd Err` during installation, the audio didn't decode cleanly — turn the playback volume up and switch off any EQ, effects or loudness normalization, then try again. A failed transfer leaves the existing firmware intact.

To check which firmware is installed, hold PLAY while turning the power on. After a successful update it reads `1.20`.

### Returning to the official firmware

KORG's updater refuses to install a version older than the one already on the unit. That check has been removed here, so you can play KORG's own `volca_drum_sys_0114.wav` at any time and be back on stock 1.14.

---

## Acknowledgements
Many thanks to KORG for developing such a great product!