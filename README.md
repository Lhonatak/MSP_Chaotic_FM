# Chaotic FM

A monophonic FM synthesizer for Max MSP that uses the **Logistic Map** as its modulation source, producing sounds that range from stable tonal textures to unpredictable, chaotic timbres.

![Interface](assets/interface.png)

---

## How It Works

Instead of a conventional sine-wave modulator, this synth feeds the output of a chaotic algorithm — the Logistic Map — into the frequency of a carrier oscillator. The Logistic Map is defined by:

$$
x_{n+1} = r \cdot x_n \cdot (1 - x_n)
$$

where $x_n$ is the current state and $r$ is a control parameter you set with the **R** knob. By changing $r$ and the initial value of $x_n$, you move the system between three regimes — stable, periodic, and chaotic — each producing a fundamentally different character of modulation.

![Bifurcation Diagram](assets/Bifurcation.png)

The bifurcation diagram above is your roadmap. The horizontal axis is $r$; the vertical axis shows where $x_n$ settles. A single line means a steady tone. Forks mean the modulator oscillates between discrete values. The dense black region on the right is chaos.

---

## Interface Layout

The interface is split into three sections, each with its own role:

| Section | Location | Purpose |
|---------|----------|---------|
| **MOD** | Top-right | Controls the chaotic modulator (the Logistic Map engine) |
| **CARRIER** | Middle-right | Selects the carrier waveform, envelope, and output level |
| **GLOBAL PITCH** | Bottom-left | Transpose, glide, and pitch-shift quality |

A **waveform scope** on the left displays the live output so you can see the relationship between your parameter changes and the resulting signal.

---

## MOD Section

These controls shape the chaotic modulation signal before it hits the carrier.

### R (Range: 3.2 – 4.0)

This is the most important knob on the synth. It sets the $r$ parameter of the Logistic Map and directly determines the character of the modulation:

- **3.2 – 3.0** — *Stable region.* The map converges to a fixed point. The modulator outputs a near-constant value, so the carrier sounds clean and static. Use this as your "safe" starting position.
- **3.0 – 3.57** — *Periodic region.* The map begins period-doubling: first alternating between 2 values, then 4, then 8, and so on. You'll hear rhythmic, pulsing textures layered onto the carrier. Slowly sweeping $r$ through this range reveals the bifurcations one by one.
- **3.57 – 4.0** — *Chaos region.* The map becomes chaotic. Tiny differences in state lead to wildly different outputs. The modulation becomes dense, noisy, and complex. This is where the synth produces its most distinctive timbres.

> **Tip:** The boundary near 3.57 is the most sonically interesting zone. Dial in slowly — small movements here produce dramatic timbral shifts.

### Chaos Freq

Sets the central frequency (rate) of the chaotic waveform generator. This determines how fast the Logistic Map iterates and, by extension, the pitch-range of the modulation signal. Lower values produce slow, evolving modulation; higher values push the modulation into audio-rate territory for harsh, metallic FM textures.

### Modulation

Controls the modulation depth — how much the chaotic signal affects the carrier's frequency. At zero, the carrier sounds unmodulated. As you increase this, the logistic waveform exerts more influence. Paired with a high $r$ value, high modulation depth produces extreme spectral content.

---

## CARRIER Section

### Waveform Select (Tab Bar)

Choose one of four carrier oscillator shapes:

1. **Sine** (`cycle~`) — Pure tone. Best for hearing the modulation's effect in isolation.
2. **Saw** (`phasor~`) — Harmonically rich. Adds brightness even before modulation.
3. **Triangle** — Softer than saw, odd-harmonic content. Good middle ground.
4. **Square** — Hollow, reedy character. Combined with chaotic modulation, can produce very aggressive sounds.

### ADSR Envelope

Standard four-stage amplitude envelope, each controlled by its own dial:

- **Attack** — Time for the sound to reach full volume after a note-on.
- **Decay** — Time to fall from peak to the sustain level.
- **Sustain** — Level held while the note is held.
- **Release** — Time to fade to silence after note-off.

> **Tip:** Short attack + long release pairs well with chaotic modulation — you get an immediate hit followed by an evolving, unpredictable tail.

### Gain (Horizontal Slider)

Master output level. The signal passes through a limiter (`limi~`) before output, but it's still wise to keep this in check when exploring extreme modulation settings.

---

## GLOBAL PITCH Section

These controls sit below the waveform display and affect the overall pitch of the output.

### Transp (Transpose)

Shifts the pitch of incoming MIDI notes in semitones. Use this to quickly move the synth into a different register without retuning your controller.

### Glide

Sets a portamento time. When you play a new note, the pitch slides from the previous note to the new one over this duration. At zero there is no glide; increase it for smooth, legato pitch transitions.

### Quality

A dropdown menu that sets the quality mode of the internal pitch shifter (`pitchshift~`). Higher quality uses more CPU but produces cleaner pitch-shifting artifacts. For most use cases, the default is fine — experiment if you hear unwanted artifacts at extreme transpose values.

---

## Planned Features

- Polyphonic voices
- Additional modulator sources
- Wavetable option for the carrier oscillator
- more chaotic algorithms 
