# hj Firmware — A custom firmware for volca drum

An unofficial firmware modification for the volca drum, based on the official firmware v1.14.

## Contents

- [Disclaimer](#disclaimer)
- [Control reference](#control-reference)
- [New features and changed behavior](#new-features-and-changed-behavior)
  - [Menu navigation](#menu-navigation)
    - [Using the FUNC + SELECT menu](#using-the-func--select-menu)
    - [PLY — round-robin polyphony](#ply--round-robin-polyphony)
    - [WGT — Wave Guide tuning](#wgt--wave-guide-tuning)
    - [WGG — Wave Guide glide control](#wgg--wave-guide-glide-control)
    - [WGM — Wave Guide MOTION owner](#wgm--wave-guide-motion-owner)
    - [TRP — transpose](#trp--transpose)
    - [TUN — master tuning](#tun--master-tuning)
    - [TNV — choose what KBD and MIDI control](#tnv--choose-what-kbd-and-midi-control)
    - [MCH — MIDI channel layout](#mch--midi-channel-layout)
    - [EXP IMP — Audio IMPORT / EXPORT](#exp-imp--audio-import--export)
  - [Performance, polyphony and MIDI](#performance-polyphony-and-midi)
    - [KBD mode](#kbd-mode)
    - [Pitched MIDI performance](#pitched-midi-performance)
    - [MIDI CC additions](#midi-cc-additions)
    - [Real-time PITCH and ACCENT recording](#real-time-pitch-and-accent-recording)
    - [Wave Guide TUNE MOTION recording](#wave-guide-tune-motion-recording)
  - [Sound design](#sound-design)
    - [CLP — soft or hard DRV clipping](#clp--soft-or-hard-drv-clipping)
    - [FLT — a bipolar filter for every Part](#flt--a-bipolar-filter-for-every-part)
    - [FM — musical modulation ratios](#fm--musical-modulation-ratios)
  - [Editing controls](#editing-controls)
    - [SRC-only waveform selection](#src-only-waveform-selection)
    - [FUNC + knob saved-value preview](#func--knob-saved-value-preview)
    - [MUTE + parameter knob soft takeover](#mute--parameter-knob-soft-takeover)
  - [Part sequencing and step editing](#part-sequencing-and-step-editing)
    - [GLD, WRP and SPD PART](#gld-wrp-and-spd-part)
    - [RESYNC / RSY — restart all Part timelines together](#resync--rsy--restart-all-part-timelines-together)
    - [STEP JUMP / SJP — choose the playback start position](#step-jump--sjp--choose-the-playback-start-position)
    - [ACT MSTR — one Active Step pattern for every Part](#act-mstr--one-active-step-pattern-for-every-part)
    - [ACCENT, SLICE and PROBABILITY](#accent-slice-and-probability)
    - [Previewing, editing and clearing STEP MOTION](#previewing-editing-and-clearing-step-motion)
    - [Enter PITCH MOTION with SELECT/PARAM](#enter-pitch-motion-with-selectparam)
    - [Enter PITCH MOTION into held STEPs from MIDI](#enter-pitch-motion-into-held-steps-from-midi)
    - [Per-knob MOTION clear and undo](#per-knob-motion-clear-and-undo)
  - [Performance effects and system behavior](#performance-effects-and-system-behavior)
    - [TOUCH FX on STEP 9–16](#touch-fx-on-step-916)
    - [Other behavior changes](#other-behavior-changes)
  - [Saving custom settings](#saving-custom-settings)
    - [What is saved where](#what-is-saved-where)
- [Bug fixes](#bug-fixes)
- [Installing](#installing)
  - [Returning to the official firmware](#returning-to-the-official-firmware)
- [Acknowledgements](#acknowledgements)

## Disclaimer

This is an unofficial firmware project, independent of KORG. Installing it may void your warranty or affect support eligibility.

This firmware does not overwrite the separate bootloader used for updates. If an update fails, re-enter update-receive mode and try again.

I accept no responsibility for any damage, malfunction, or unexpected results that may arise from installing or using this firmware. **Use it at your own risk.** If you run into trouble, please report it by opening a GitHub issue.

---

## Control reference

| Control | Function |
|---|---|
| Hold FUNC + turn SELECT, then release FUNC | Choose a menu item and enter it |
| Hold FUNC + turn a sound knob | Preview its saved value without editing |
| FUNC + ACT. STEP | Edit the shared ACT MSTR pattern |
| Hold MUTE + press REC | Toggle chromatic KBD mode |
| During playback, hold MUTE + press PLAY | RESYNC: restart all Part timelines together |
| MUTE + SELECT | Cycle SRC only |
| Hold MUTE + turn a sound knob | Soft-takeover editing |
| In EDIT/STEP, hold MUTE + turn LEVEL/VALUE | Soft-takeover editing for the selected step parameter |
| Hold MUTE + LAYER, then STEP 1–6 | Toggle GLD for Parts 1–6 |
| Hold MUTE + ACT. STEP, then STEP 1–6 | Toggle WRP for Parts 1–6 |
| Hold MUTE + STEP JUMP, then STEP 1–6 | Cycle SPD for Parts 1–6 |
| In ACT. STEP, FUNC + STEP 15 / 16 | Clear/undo the selected Part / all Part Active Step patterns |
| In ACT MSTR, FUNC + STEP 15 / 16 | Fit the master pattern to the selected Part / clear the master pattern; repeat to undo |
| In EDIT/STEP, hold one STEP | Preview its PRB, SLC or ACC value |
| While holding one STEP in EDIT/STEP, press FUNC | Enter MOTION previews or move to the next item |
| On a MOTION preview, press REC | Move to the previous item without changing recording state |
| With MOTION on, turn SELECT/PARAM during a MOTION preview | Edit the displayed parameter's MOTION |
| On a MOTION preview, press MUTE | Clear that parameter's MOTION from the held STEP |
| With QPI and MOTION on in EDIT/STEP, hold STEPs + turn SELECT/PARAM | Adjust PITCH MOTION in semitone steps |
| Hold MUTE + STEP 8–16 | Clear/undo one MOTION lane |
| Hold STEP 9–16 on the normal screen | Apply TOUCH FX |

## New features and changed behavior

### Menu navigation

#### Using the FUNC + SELECT menu

1. Hold FUNC and turn SELECT to choose an item.
2. Release FUNC to enter that item's adjustment screen.
3. Turn SELECT to change the value. Changes take effect immediately.
4. Press FUNC once to leave the adjustment screen and return to normal operation.

The item order is:

`PLY / WGT / WGG / WGM / TRP / TUN / TNV / MCH / EXP IMP`

`EXP IMP` is different from the value items: releasing FUNC on it opens the audio-transfer screen described below.

#### PLY — round-robin polyphony

PLY combines Parts 1 through n (n = 2–6) into an n-voice round-robin instrument. Choose `OFF`, `2`, `3`, `4`, `5` or `6` from the FUNC + SELECT menu.

- All voices use Part 1's sound and QPI setting.
- With QPI ON, the voices receive independent note pitches. With QPI OFF, the voices use Part 1's base PITCH behavior.
- Turning PLY OFF restores the original sounds and saved settings of Parts 2–n.
- PLY is stored in the KIT.

In the MUTE, CHOKE, GLD PART, ACT. STEP, WRP PART and SPD PART views, pressing STEP 1–n edits Part 1's setting. Each member Part's original setting is retained and restored when PLY is turned off.

`CLR/UND PART` and `CLR/UND MPRT` apply to all Parts assigned to PLY.

During playback, choking is disabled whenever either Part belongs to PLY. Parts outside PLY still choke one another normally.

#### WGT — Wave Guide tuning

WGT changes how Wave Guide TUNE is interpreted. Its SELECT order is `OFF / DLY / NOTE / 1 / 2 / 3 / 4 / 5 / 6`:

| Mode | Behavior |
|---|---|
| `OFF` | KORG's original nonlinear, unquantized TUNE behavior, displayed as `0...255`; OCuP/OCdn do not alter it |
| `DLY` | Tempo-synchronized Wave Guide delay with 23 straight, dotted and triplet note values; OCuP/OCdn do not alter it |
| `NOTE` | The Wave Guide is an independent chromatic resonator, displayed as a note name |
| `1...6` | Referenced Part QPI ON: TUNE is a relative offset of `-24...+24` semitones from that Part's note.<br>Referenced Part QPI OFF: TUNE is an absolute chromatic note; KBD/MIDI changes the Wave Guide pitch without assigning chromatic PITCH to the referenced Part |

In WGT `1...6` modes, single-channel MIDI notes retain the original note-to-Part trigger mapping and do not set chromatic Part PITCH. WGT still follows the referenced Part's QPI setting, including PITCH MOTION when QPI is ON. With QPI OFF, accepted KBD/MIDI notes write their pitch directly to Wave Guide TUNE. KBD changes only Wave Guide TUNE; MIDI velocity can still change the Part's ACCENT according to TNV. In a six-channel layout, send chromatic notes on the referenced Part's channel.

In WGT `NOTE` and `1...6` modes, TRP and OCuP/OCdn also transpose the Wave Guide pitch. DLY follows the current tempo and ignores master TUN, TRP and octave pitch transforms.

In DLY, the normal knob label remains `TUN`, and the right display shows the selected note value. `D` means dotted and `T` means triplet; compact names such as `16D` are used where the four-character display requires them. `LIM` appears on the left when the requested time exceeds the Wave Guide's capacity, so long values such as `1/1` may be shortened at slow tempos. DLY uses the existing TUBE/STR Wave Guide processing and feedback rather than adding a separate general-purpose delay effect.

| Display | Note value | Quarter-note beats |
|---|---|---:|
| `1/1` | Whole | 4 |
| `1/2D` | Dotted half | 3 |
| `1/1T` | Whole-note triplet | 8/3 |
| `1/2` | Half | 2 |
| `1/4D` | Dotted quarter | 3/2 |
| `1/2T` | Half-note triplet | 4/3 |
| `1/4` | Quarter | 1 |
| `1/8D` | Dotted eighth | 3/4 |
| `1/4T` | Quarter-note triplet | 2/3 |
| `1/8` | Eighth | 1/2 |
| `16D` | Dotted sixteenth | 3/8 |
| `1/8T` | Eighth-note triplet | 1/3 |
| `1/16` | Sixteenth | 1/4 |
| `32D` | Dotted 32nd | 3/16 |
| `16T` | Sixteenth-note triplet | 1/6 |
| `1/32` | 32nd | 1/8 |
| `64D` | Dotted 64th | 3/32 |
| `32T` | 32nd-note triplet | 1/12 |
| `1/64` | 64th | 1/16 |
| `128D` | Dotted 128th | 3/64 |
| `64T` | 64th-note triplet | 1/24 |
| `128` | 128th | 1/32 |
| `128T` | 128th-note triplet | 1/48 |

#### WGG — Wave Guide glide control

WGG controls interpolation for the entire Wave Guide, independently of the six Part GLD settings. It applies to DECAY, BODY and TUNE MOTION, and also to the pitch transition used by live KBD/MIDI notes and manual TUNE changes while stopped.

- ON: Wave Guide MOTION uses its normal interpolation, and resonance-pitch changes use the normal glide time.
- OFF: DECAY, BODY and TUNE MOTION change at their recorded points without interpolation, and resonance-pitch changes are immediate.

WGG is stored in the PROGRAM.

#### WGM — Wave Guide MOTION owner

WGM selects the Part timeline used by Wave Guide DECAY, BODY and TUNE MOTION for playback, recording and the automatic REC end. It is stored in the PROGRAM.

| Value | Timeline |
|---|---|
| `Auto` | The Part with the greatest number of Active Steps |
| `1...6` | The selected Part; a selected PLY member is treated as Part 1 |

PLY members are counted once as their shared Part 1 owner. SPD does not affect which Part `Auto` chooses, but the chosen owner's Active Step, WRP and SPD timing controls how its MOTION timeline runs.

#### TRP — transpose

TRP applies a session-wide transpose of `-12...+12` semitones to the normal sound engine and the Wave Guide. It returns to zero after power-off.

#### TUN — master tuning

TUN sets the saved global master tuning from `427.5...453.0 Hz` in 0.5 Hz steps. It applies to the normal sound engine and the Wave Guide.

#### TNV — choose what KBD and MIDI control

TNV is a saved global setting for Trigger, Note and Velocity.
The left, middle and right characters are T, N and V respectively; `_` means OFF and `o` means ON. The default is `ooo`.

| Element | ON | OFF |
|---|---|---|
| Trigger | The input triggers the Part and may write a trigger STEP while recording | The input does not trigger the Part or write a trigger STEP; PLY round-robin position still advances |
| Note | The incoming note updates Part/Wave Guide pitch according to QPI and WGT | The previous internal pitch is retained |
| Velocity | Incoming velocity or KBD ACCENT updates ACCENT | The previous internal ACCENT is retained |

With Velocity ON, a normal QPI OFF KBD key supplies its STEP-position ACCENT, while QPI ON KBD uses neutral ACCENT `0`. In the QPI OFF WGT 1–6 Wave-Guide-only layout, KBD does not overwrite the source Part's ACCENT. For example, turn Trigger OFF and Note ON to change the pitch of a running sequence from KBD or MIDI without adding another trigger.

#### MCH — MIDI channel layout

The global MCH setting provides:

- Single channels `ch01...ch16`
- Six-channel layouts `0106...1116`, assigning six consecutive MIDI channels to Parts 1–6

The current setting can be changed with SELECT from the FUNC + SELECT `MCH` item. It is saved globally.

The same setting is available by holding REC while turning the power on:

- Press STEP 1–16 once to select single channel `ch01...ch16`.
- Press the currently selected STEP again to select a six-channel layout beginning at that channel. If the starting channel is above 11, the layout is set to `1116` (channels 11–16).
- One lit STEP shows a single channel; six consecutive lit STEPs show a six-channel layout.

Press REC to save and restart, or PLAY to cancel.

#### EXP IMP — Audio IMPORT / EXPORT

The SYNC connection can transfer sound and sequence data as recordable audio. Releasing FUNC on `EXP IMP` opens the transfer screen. Entering it stops the sequencer because SYNC OUT is used for the data signal. Normal SYNC clock pulses are suppressed only while the transfer screen is open and resume immediately after leaving it.

> **EXPORT safety:** SYNC OUT sends a harsh 5 V data signal. Do not listen to or monitor it through headphones or speakers. Disconnect any equipment other than the intended recorder/interface, and check that its input can accept this signal before connecting.

On the selection screen, FUNC and PLAY blink:

- Turn SELECT to choose what an EXPORT will send.
- Press FUNC to cancel and return to normal operation.
- Press PLAY to begin EXPORT of the displayed type.
- IMPORT does not require selecting the matching type. While this screen is waiting, play a compatible export into SYNC IN; its header selects the type automatically and the display changes to `IMP / rECV`.

The available data types, in SELECT order, are:

| Type | Contents |
|---|---|
| `CPRT` | Currently selected Part's timbre only |
| `CKIT` | Current edited KIT, including unsaved edits, all six Part timbres and KIT-level custom settings |
| `CPRG` | Current edited PROGRAM, including unsaved edits, sequence, MOTION and PROGRAM-level custom settings |
| `CALL` | Current edited KIT followed by the current edited PROGRAM, including unsaved edits |
| `MKIT` | All 16 saved KIT slots |
| `MPRG` | All 16 saved PROGRAM slots |
| `MALL` | All 16 KIT slots followed by all 16 PROGRAM slots |

CPRT excludes STEP data, sequence triggers, MOTION, MUTE, GLD, WRP and SPD. Importing CPRT replaces only the Part that is selected on the receiving unit.

If CPRT targets Part 2–n while that Part belongs to an active PLY voice, it replaces that physical Part's hidden saved timbre. PLY playback continues to use Part 1's effective sound; the imported member timbre becomes directly audible again when PLY is turned OFF.

For EXPORT, connect SYNC OUT only to the intended recorder or audio interface input, start recording first, then press PLAY. Wait until the transfer result is displayed, then stop the recorder before pressing the blinking PLAY button to exit. Leaving earlier restores normal SYNC pulses and can add them to the end of the recording.

For IMPORT, connect the player's output to SYNC IN, enter the transfer screen, then play the WAV from its beginning without EQ, normalization, fades or notification sounds. A direct unit-to-unit transfer is also possible by connecting the sending unit's SYNC OUT to the receiving unit's SYNC IN, putting both in the transfer screen, and starting EXPORT on the sender. After the result is shown, press the blinking PLAY button to exit.

On a successful import, `MKIT` replaces all 16 saved KIT slots, `MPRG` replaces all 16 saved PROGRAM slots, and `MALL` replaces both. Back up any slots you want to keep before importing these types.

Imported data is CRC-checked one item at a time before it replaces its destination. If an error occurs, the affected Part, KIT or PROGRAM stays unchanged and the transfer stops. In CALL, MKIT, MPRG and MALL transfers, any earlier items that completed successfully remain imported.

The error display uses `ERR` on the left and identifies the failure on the right:

| Right display | Failure |
|---|---|
| `CPRT` | Part timbre unit |
| `CKIT` | Current KIT unit |
| `CPRG` | Current PROGRAM unit |
| `Ki01...Ki16` | The numbered KIT in a multi-KIT transfer |
| `Pr01...Pr16` | The numbered PROGRAM in a multi-PROGRAM transfer |
| `Hdr` | Header CRC, magic/type or block number |
| `SynC` | Initial synchronization could not be established |

If a stereo recorder captured the SYNC signal on only one channel, normal stereo playback may route the silent channel to SYNC IN. Route or copy the signal channel to both outputs, or convert the recording to mono before playback.

### Performance, polyphony and MIDI

#### KBD mode

Hold MUTE and press REC (`MUTE + REC`) to toggle KBD mode, whether playback is running or stopped.

On the normal performance screen, KBD uses the STEP keys for playing the keyboard instead of selecting Parts or applying TOUCH FX. STEP-based editing operations retain their normal controls.

**With QPI ON**, the selected Part uses this chromatic layout:

- STEP 1–13: C through the next C in semitone steps
- STEP 14: unused
- STEP 15: octave down, to a minimum of `-4`
- STEP 16: octave up, to a maximum of `+4`
- STEP 15 + 16: return to octave `0`

The display shows `OCT` and the new octave after an octave operation.

**With QPI OFF**, STEP 1–16 use the ACCENT layout when the selected Part is not the active WGT 1–6 reference: each key triggers the selected Part with its corresponding ACCENT position without changing the Part's PITCH. If that Part is the WGT reference, KBD instead uses the chromatic layout above to play only the Wave Guide; the Part's PITCH and ACCENT remain unchanged.

In PLY, Part 1's QPI—not the currently selected member's QPI—selects the KBD behavior, and chromatic notes feed the round-robin voices.

#### Pitched MIDI performance

In a six-channel layout, MIDI can send independent note pitches to QPI ON Parts 1–6. A QPI OFF Part keeps its knob-defined PITCH.

In single-channel mode, MIDI retains the volca drum's original Part-triggering behavior. The incoming note number determines which Part is triggered. MIDI note input does not change the Part selected for panel editing or set chromatic Part PITCH. In a six-channel layout, the first channel addresses Part 1, the next Part 2, and so on through Part 6; messages outside the selected six-channel range are ignored.

Channel-based parameter CC messages use the same six-channel-to-Part mapping.

#### MIDI CC additions

In a six-channel layout, hj Firmware adds direct CC control for FLT (CC54) and CLP (CC55). Send the message on the channel assigned to the Part. MIDI RX ShortMessage must be enabled.

| Parameter | CC number (decimal) | Value |
|---|---:|---|
| FLT | 54 | 0 = -100 (low-pass), 64 = 0 (bypass), 127 = +100 (high-pass) |
| CLP | 55 | 0–63 = SFT; 64–127 = HRD |

These CCs use absolute 7-bit values. Single-channel MIDI keeps the original CC assignments. QPI and FLT changes can affect a sounding voice without retriggering it.

#### Real-time PITCH and ACCENT recording

Start PLAY, enable REC and perform from KBD or MIDI. The recording window starts when an accepted KBD or MIDI input first writes data to the current STEP, not simply when REC is enabled.

- QPI ON notes are quantized to the recorded STEP and written to both layers' PITCH MOTION.
- In PLY, trigger recording uses the common one-bar window that begins with the first accepted voice. Each voice's PITCH lane can continue for one full bar from that voice's own first recorded note.
- MIDI velocity is recorded into ACCENT when Velocity is ON in TNV. Physical KBD input also records its ACCENT value for a QPI OFF Part when Velocity is ON.
- ACCENT is recorded on the same STEP as the performance event, taking Active Step and WRP timing into account.
- Recording positions and the automatic one-bar end follow SPD PART, Active Step and WRP PART. In PLY, Part 1 is the timing owner.

#### Wave Guide TUNE MOTION recording

- With WGT `1...6` and a QPI ON reference, the Wave Guide follows the pitch currently applied to that Part, including PITCH MOTION. Live KBD/MIDI notes record the Part's PITCH MOTION, not an absolute TUNE lane. TUNE MOTION remains available as a relative interval from the Part pitch, so turning TUNE while recording—or holding STEP keys while turning it—records interval changes without breaking pitch tracking.
- With a QPI OFF reference, the Part keeps its knob-defined PITCH and chromatic KBD/MIDI notes may record the absolute Wave Guide TUNE instead.

Existing PITCH or TUNE MOTION on a STEP is protected from automatic live-note overwrite. The TUNE lane also participates in Part/MOTION clear and undo and in the individual `MUTE + STEP 16` operation.

### Sound design

In EDIT/STEP, the parameter order is `BIT / FLD / DRV / CLP / FLT / PAN / GAN / QPI / FM`.

#### CLP — soft or hard DRV clipping

CLP is stored separately for each Part in the KIT.

- `SFT`: the original KORG DRV response.
- `HRD`: a hard-clipping path.

#### FLT — a bipolar filter for every Part

Each Part has a filter stored in the KIT:

- `-100...-1`: 2-pole low-pass filter
- `0`: bypass
- `+1...+100`: 2-pole high-pass filter

In EDIT/STEP, use PARAM to select `FLT`, then turn LEVEL/VALUE to edit it.

#### FM — musical modulation ratios

FM is a per-Part KIT setting. Enter EDIT/STEP, select `FM` with PARAM and use LEVEL/VALUE to choose `OFF` or `ON`. When a layer's MOD type is LFO, FM turns RATE into a frequency ratio relative to the played pitch. FM has no effect on a layer whose MOD type is not LFO.

FM provides 56 frequency ratios from `1/4` to `16x`.

The ratio display uses an underscore as the decimal separator. Values below 10 use one integer and two decimal digits (`1_25`, with thirds rounded to `_33` or `_67`); values from 10 use two integers and one decimal digit (`10_0`).

With LAY 1-2 selected, RATE uses the ratio display only if both layers' MOD types are LFO; otherwise it uses the `0...255` display. FM continues to apply independently to each layer whose MOD type is LFO.

With FM ON, the carrier and modulation oscillators reset to zero phase on each trigger. Repeated notes with identical settings therefore start with a consistent FM timbre. With FM OFF, the oscillators retain their original free-running behavior.

### Editing controls

#### SRC-only waveform selection

Use `MUTE + SELECT` to cycle SRC only. SELECT continues to cycle the complete SRC/MOD/EG combinations.

#### FUNC + knob saved-value preview

Holding FUNC while moving a physical parameter knob previews its saved value without changing it.

This applies to the ten sound knobs: LEVEL, PITCH, AMOUNT, RATE, ATTACK, RELEASE, SEND, DECAY, BODY and TUNE. The FM RATE format follows the rules described in the [FM section](#fm--musical-modulation-ratios).

In EDIT/STEP, the same FUNC preview is available for the currently selected `BIT / FLD / DRV / CLP / FLT / PAN / GAN / QPI / FM` item by turning LEVEL/VALUE. It shows the saved value without editing it.

#### MUTE + parameter knob soft takeover

Holding MUTE while turning any of the ten sound knobs listed above starts editing from the current value without jumping. The remaining travel toward each end of the knob is mapped to the parameter's minimum or maximum. In EDIT/STEP, MUTE + LEVEL/VALUE works the same way for the selected parameter, including FLT. TEMPO, SWING, SELECT and PARAM are unaffected. Releasing MUTE resets the starting point for the next edit.

### Part sequencing and step editing

#### GLD, WRP and SPD PART

Hold both buttons to open these Part-selection views. Either button order is accepted. While both buttons are held, STEP 1–6 represent Parts 1–6; releasing LAYER, ACT. STEP or STEP JUMP returns to `MUT PART` if MUTE remains held.

- Hold `MUTE + LAYER` for **GLD PART**. Press STEP 1–6 to toggle each Part; lit means ON. OFF makes that Part's MOTION values, including PITCH, change in discrete steps.
- Hold `MUTE + ACT. STEP` for **WRP PART**. Press STEP 1–6 to toggle each Part; lit means ON. With fewer than 16 Active Steps, ON distributes the enabled steps across the whole bar instead of shortening the cycle.
- Hold `MUTE + STEP JUMP` for **SPD PART**. Press STEP 1–6 repeatedly to cycle that Part through `1/1` (LED off), `1/2` (lit) and `1/4` (fast blink).

All three are stored per Part in the PROGRAM. SPD slows the Part's complete timeline—STEP advance, triggers, PRB, SLICE and MOTION. A `1/2` pattern takes two physical bars and `1/4` takes four. WRP is applied first, then SPD slows that resulting cycle; recording length follows the relevant Part or PLY owner's speed.

An SPD Part plays STEP 1 immediately after ordinary PLAY; choosing a start STEP while stopped instead uses the SJP behavior below. On an automatic pattern-chain change, the new PROGRAM likewise begins its SPD Parts on the transition boundary. Loading a PROGRAM manually while PLAY is running instead preserves each SPD Part's remaining wait, so repeated LOAD operations do not advance the sequence faster.

#### RESYNC / RSY — restart all Part timelines together

During playback, press PLAY while holding MUTE (`MUTE + PLAY`) to restart all six Part timelines. Hold MUTE first; PLAY on its own keeps its normal transport function. Restart timing depends on the clock source and RSY setting, as described below. RESYNC has no effect while stopped.

Each Part returns to the beginning of its Active Step layout, including its SPD, WRP and MOTION timing. If STEP 1 is off, it starts at the first enabled STEP; a Part with no enabled STEPs remains silent. The first STEP runs for its normal duration from the restart, according to the current SPD, WRP and ACT MSTR settings, even when RESYNC is pressed partway through a STEP or used repeatedly. The tempo and clock source are retained, and playback or recording continues with the current KIT and PROGRAM settings.

RESYNC also resets each Part's PRB bar counter and PRE/NEI conditional history, so first-bar conditions begin again.

The TEMPO knob LED starts a new beat at the restart. With the internal clock, SYNC OUT also starts a new pulse cycle; `IMMD` starts a fresh pulse with either the 1 STEP or 2 STEP output setting (Global Setting 8). With SYNC IN or MIDI clock, the incoming clock and SYNC OUT keep their existing grid. The restarted sequence begins with the configured SWING from its first STEP.

After receiving MIDI Start, stopping and restarting with the volca drum's PLAY button while MIDI clock continues preserves the sender's STEP grid. Playback resumes at its next straight STEP boundary.

**RSY — internal-clock RESYNC timing** is the ninth item in the power-on Global Settings screen (`Glb Stng`):

| Value | Behavior with the internal clock |
|---|---|
| `STEP` (default) | Restart at the next straight STEP boundary |
| `IMMD` | Restart immediately, even partway through a STEP |

To change it, hold FUNC while turning the power on, then press STEP 9 to toggle between `STEP` and `IMMD`. Press REC to save the Global Settings and restart, or PLAY to cancel the changes. With SYNC IN or MIDI clock, RESYNC waits for the next straight STEP boundary with either RSY setting, including `IMMD`. Straight STEP boundaries are unaffected by SWING.

#### STEP JUMP / SJP — choose the playback start position

The volca drum already has a start-selection operation that is not described in the official manual: while the sequencer is stopped in STEP JUMP mode, press a STEP key. If several STEPs are pressed together, the lowest-numbered one is selected. Its lower LED blinks, and the next PLAY starts from that STEP.

SJP extends this existing operation to keep Parts with SPD `1/2` or `1/4` at the position and phase corresponding to the normal-speed start time, rather than starting every Part at the same STEP number. STEP advance, SLICE and MOTION retain the phase they would have at that point in normal playback. If a slowed Part starts partway through a STEP, selecting that position does not add a new trigger; playback continues with the triggers due from that phase onward.

**SJP — selected-start position** is the tenth item in the power-on Global Settings screen (`Glb Stng`):

| Value | Behavior |
|---|---|
| `PICK` (default) | Begin with a pickup before the first loop, placing slowed Parts near the end of their preceding cycle so they reach STEP 1 with the normal-speed Part |
| `1ST` | Begin at the corresponding elapsed position within the first loop |

For a full 16-STEP pattern, choosing STEP 13 gives these starting positions:

| SJP | SPD `1/1` | SPD `1/2` | SPD `1/4` |
|---|---|---|---|
| `PICK` | Pickup STEP 13 | Pickup STEP 15 | Pickup STEP 16 |
| `1ST` | First-loop STEP 13 | First-loop STEP 7 | First-loop STEP 4 |

With `PICK`, the following STEP 1 begins the first PRB loop for every Part. With `1ST`, the selected position is already in the first loop. Selecting STEP 1 behaves like ordinary PLAY with either setting: all Parts start at their beginning, and PRB `A-B` and first-bar conditions count it as the first loop. WRP, ACT MSTR and the PLY timing owner are taken into account when mapping slowed Parts' positions.

To change SJP, hold FUNC while turning the power on, then press STEP 10 to toggle `PICK` and `1ST`. Press REC to save the Global Settings and restart, or PLAY to cancel.

#### ACT MSTR — one Active Step pattern for every Part

Hold FUNC and press ACT. STEP to edit a PROGRAM-wide 16-STEP pattern. If one or more master STEPs are off, the master pattern controls every Part's playback timing. Returning all 16 master STEPs to on disables ACT MSTR and restores each Part's normal Active Step behavior.

The ordinary ACT. STEP mode remains editable while ACT MSTR is active. For Parts with WRP off, these changes become audible when ACT MSTR is disabled. For Parts with WRP on, they affect playback while ACT MSTR is active, as described below.

`FUNC + STEP 10` applies `RND PTTN` to the pattern, randomizing which STEPs are active.

FUNC adds the following reversible Active Step operations:

| View | Control | Behavior |
|---|---|---|
| ACT. STEP | FUNC + STEP 15 | `CLR PACT`: reset the selected Part's pattern to all on / `UND PACT` |
| ACT. STEP | FUNC + STEP 16 | `CLR AACT`: reset all six Part patterns to all on / `UND AACT` |
| ACT MSTR | FUNC + STEP 15 | `FIT MSTR`: copy the selected Part's pattern to the master pattern / `UND MSTR` |
| ACT MSTR | FUNC + STEP 16 | `CLR MSTR`: reset the master pattern to all on / `UND MSTR` |

The master pattern is stored with the PROGRAM and is used by sequencing, MOTION, recording limits, PLY, WGM Auto and pattern chaining.

When both WRP and ACT MSTR are active, WRP first maps the Part's enabled STEPs across the 16-STEP bar, then ACT MSTR removes the time intervals where the master is off. The remaining trigger spacing is preserved rather than redistributed; SPD slows the resulting compressed cycle.

#### ACCENT, SLICE and PROBABILITY

**ACCENT** covers `-15...+16` instead of `0...16` in KORG v1.14, allowing ghost notes below the LEVEL setting. MIDI velocity maps across all 32 values, with velocity 63 at the base LEVEL.

Positive **SLICE** values `1...16` divide a STEP into that many evenly spaced hits; `1` plays a single hit. Its negative values select the following sub-step patterns. Read each pattern from left to right within one STEP: `o` plays and `_` rests.

| SLICE | Pattern | SLICE | Pattern | SLICE | Pattern |
|---:|:---:|---:|:---:|---:|:---:|
| `-1` | `_o` | `-7` | `oo__` | `-13` | `_oo_` |
| `-2` | `oo_` | `-8` | `o__o` | `-14` | `_o_o` |
| `-3` | `o_o` | `-9` | `ooo_` | `-15` | `_ooo` |
| `-4` | `_o_` | `-10` | `oo_o` | `-16` | `__oo` |
| `-5` | `_oo` | `-11` | `o_oo` | `-17` | `___o` |
| `-6` | `__o` | `-12` | `_o__` |  |  |

The positive, probabilistic side of **PRB** is now spaced in 5% increments: `5, 10, ... 100%`. Its negative side adds:

| Display | Meaning |
|---|---|
| `A-B` | Bar A of each B-bar cycle, B = 2...8 |
| `1St` | First bar after PLAY or RESYNC only |
| `_1St` | Every bar except the first |
| `FILL` | While the FILL touch effect is held |
| `_FIL` | While FILL is not held |
| `PRE` | Follow the most recent conditional result in the same Part |
| `_PRE` | Invert PRE |
| `NEI` | Follow the most recent conditional result in the preceding Part |
| `_NEI` | Invert NEI |

Part 1 has no preceding Part, so NEI is false and `_NEI` is true. The condition order at the left end of the knob is `FILL / _FIL / 1St / _1St / PRE / _PRE / NEI / _NEI / 8-8 / 7-8 ...`. Conditional history resets on PLAY/STOP and RESYNC and is not saved. The bar counter is independent per Part. Ordinary PLAY and RESYNC begin with the first loop; a start chosen while stopped follows SJP as described above.

#### Previewing, editing and clearing STEP MOTION

In EDIT/STEP, hold a single STEP to preview its PRB, SLC or ACC value. Switching among these modes while holding the STEP changes the value shown. Releasing after the preview does not toggle the STEP; a quick tap retains the normal toggle behavior.

Press FUNC while holding the STEP to browse its recorded MOTION values. FUNC moves to the next item; REC moves to the previous item without changing recording state.

`PIT → AMT → RAT → ATK → REL → SND → DEC → BDY → TUN → PIT`

Pressing FUNC before the hold preview appears opens PIT immediately. To view PRB, SLC or ACC again, release the STEP and hold it again.

Each MOTION preview shows the recorded value at the beginning of the STEP, or `---` if no value is recorded there.

With MOTION on, turn SELECT/PARAM to edit the displayed parameter for the selected layer. Editing starts from the recorded value, or the current parameter setting if unrecorded, and applies the adjusted value throughout the STEP.

With LAY 1-2 selected, both layers move by the same amount and stop when either reaches its limit. PIT shows LAY 1's value when QPI is ON. AMT shows the value furthest from zero, keeping its sign; ties use LAY 1. Other layer-specific parameters show the higher value. If only one layer has a recorded value, that layer is shown. Once SELECT/PARAM editing begins, the display follows the same layer until the STEP is released or the preview item or layer selection changes.

On a MOTION preview, press MUTE to clear that parameter's MOTION from the STEP for the selected layer. With LAY 1-2 selected, both layers are cleared.

#### Enter PITCH MOTION with SELECT/PARAM

In EDIT/STEP, select a QPI ON Part, turn MOTION on, hold one or more STEPs, and turn SELECT/PARAM to adjust PITCH MOTION in semitone steps. Each STEP starts from its recorded PITCH, or the current PITCH setting if unrecorded.

#### Enter PITCH MOTION into held STEPs from MIDI

This is an explicit STEP-editing operation, separate from real-time recording:

1. Choose a six-channel MCH layout, such as `0106`. This operation is disabled in single-channel MIDI mode.
2. Select the Part to edit, turn QPI and MOTION ON, and choose LAY 1, LAY 2 or both in EDIT/STEP.
3. Hold one or more STEP keys.
4. Send a MIDI Note On on the channel assigned to the selected Part.

The note's pitch is written to every held STEP in the selected layer, or to both layers when both are selected. It works even when TNV Trigger and Note are OFF, and the editing Note On is consumed without sounding. Only the selected Part's assigned channel performs the edit.

#### Per-knob MOTION clear and undo

Hold MUTE and press a STEP to clear one MOTION lane on the selected Part and layer; repeat it to undo.

| STEP | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 |
|---|---|---|---|---|---|---|---|---|---|
| Target | PITCH | MOD AMT | MOD RATE | ATTACK | RELEASE | SEND | WG DECAY | WG BODY | WG TUNE |
| Display | `MPIt` | `MAMt` | `MrAt` | `MAtk` | `MrEL` | `MSNd` | `MdEC` | `MbdY` | `MtUN` |

### Performance effects and system behavior

#### TOUCH FX on STEP 9–16

On the normal performance screen, hold STEP 9–16 to apply the corresponding effect:

| STEP | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 |
|---|---|---|---|---|---|---|---|---|
| Display | `OCuP` | `OCdn` | `Stut` | `rev` | `bit` | `SEnd` | `WEt` | `FILL` |
| Effect | Octave up | Octave down | Stutter gate | Reverse | Bit crush | WG full send | WG wet only | FILL condition |

Effects are active while held and several can be combined. `OCuP/OCdn`, `Stut/rev` and `SEnd/WEt` are mutually exclusive within each pair.

#### Other behavior changes

- QPI LAY 1-2 PITCH editing transposes both layers while preserving their semitone interval and stopping at the first limit. When turning the PITCH knob with QPI ON and LAY 1-2 selected, the display shows the LAY 1 note name.
- With Global Setting 5 set to `FuLL`, the TEMPO knob uses approximately 30% of its travel for 10–60 BPM, 50% for 60–240 BPM and 20% for 240–600 BPM.
- At the low-battery warning level, `Lo Batt` is displayed for 1.5 seconds at a time and repeats approximately every 15 seconds, compared with every 4 seconds in KORG v1.14. A button or knob action that displays its own text dismisses the warning. The next warning is delayed briefly after a parameter value is displayed.
- During shutdown, `WRITING` or `Lo Batt` takes priority over temporary parameter displays.

These display changes leave saving behavior, voltage thresholds and shutdown timing unchanged.

### Saving custom settings

#### What is saved where

| Scope | Custom settings |
|---|---|
| Per Part in the KIT | FLT, CLP, FM |
| Per KIT | PLY, WGT |
| Per Part in the PROGRAM | GLD PART, WRP PART, SPD PART |
| Per PROGRAM | WGG, WGM, ACT MSTR |
| Persistent global | TUN, TNV, MCH, RSY, SJP |
| Session only; not saved | TRP, KBD mode, KBD octave |

---

## Bug fixes

The following issues were present in KORG's official v1.14 firmware.

**Showing an unsupported character could overwrite Part 1 / LAY 2 / STEP 9 PITCH MOTION.**  
Invalid characters are now rejected before the display routine can write outside its character table.

**Clock phase correction could skip a STEP trigger or repeat a PROGRAM in a chain.**  
When a correction moves playback forward, crossed STEP and pattern-chain boundaries are now processed once, in order.

**Part activity LEDs could stay lit permanently.**  
The LED timer and its state bitmap could get out of sync; they are now updated together and corrected if inconsistent. CPY PART's multi-blink pattern is unchanged.

**Positive SLICE values produced uneven hits.**  
All values now produce the requested number of evenly spaced hits.

**Undoing a Part MOTION clear displayed `UND PART`.**  
It now displays `UND MPRT`, matching `CLR MPRT`.

**MOD AMOUNT could jump when crossing zero with LAY 1-2 selected.**  
Editing with LAY 1-2 selected now follows the same bipolar rule as editing a single layer.

**The saved-value marker could display incorrectly for signed values.**  
The `-` marker on the right of the value display indicates a match with the saved setting. It is now shown and cleared correctly for signed values.

## Installing

The update ships as an audio file, using the same update method as KORG's official firmware.

1. Connect your computer or audio player's headphone output to the volca drum's `SYNC IN` jack with a 3.5 mm stereo cable.
2. Hold FUNC and REC together and turn the power on. The unit is now in update-receive mode.
3. Play `Volca_Drum_0122.wav` all the way through. Disable EQ, effects, fades, loudness normalization and notification sounds.
4. If the display shows `UPD End`, turn off the volca drum.

If `Dcd Err` or `Sum Err` appears, check the cable, playback volume and audio-processing settings, then re-enter update-receive mode and retry.

To check which firmware is installed, hold PLAY while turning the power on. After a successful update it reads `1.22`.

### Returning to the official firmware

KORG's updater normally refuses to install a version older than the one already on the unit. That version-downgrade check has been removed here, so you can play KORG's own `volca_drum_sys_0114.wav` and return to stock firmware v1.14.

KORG v1.14 does not understand settings and data introduced by hj Firmware. Save or export anything important first, and review your KITs and PROGRAMs after returning to v1.14.

---

## Acknowledgements

Many thanks to KORG for developing such a great product!
