# MixMind

**A bartender that listens to *how* you talk, not what you say.**

Two people can both say "yeah, fine, long day" and mean completely different
things. One says it fast and bright; the other slow, quiet, with gaps. Every
voice interface we've used throws that away. MixMind measures it and pours a
drink about it.

## The demo

Say "I'm fine, really" twice -- once brightly, once flatly. You get two
different drinks, and the machine tells you why:

> *"You said you're fine, but you left a lot of space between your words and
> you spoke softly, so this is mostly citrus and soda -- easy to sip, not to
> swallow."*

## How it works

Talk for ten to twenty seconds. While you speak, MixMind measures six things
about your voice: pitch, how much it moves, loudness, how much of the time you
pause, your speaking rate, and the cycle-to-cycle wobble that rises with
strain. Those six numbers choose the drink through a deterministic rule
engine, so every drink can be explained rather than hand-waved.

The words are transcribed too, but only to catch claims the voice contradicts.
The voice decides the drink; the words change what the machine says about it.

**Speech recognition and voice analysis run on the Pi itself** -- no cloud, no
keys, nothing in the pour path that needs the internet. Analysis takes about
1.6 seconds. Claude writes the line the machine says about you; if the network
drops, it falls back to a local voice and keeps serving.

Then the Pi sends each pour over Wi-Fi to an Arduino UNO Q, which drives six
peristaltic pumps through an opto-isolated relay board. Every recipe is
checked for drinkability before a pump can run, and any failure switches all
channels off.

## Where to look

| repo | what's in it |
|---|---|
| **voice_decipher_2** | the machine: voice analysis, the drink engine, the kiosk server, the pump link, and the tests |
| voice-pour-pro-interface | the touchscreen UI (1024x600 kiosk) |
| calibration-prototype | runtime calibration experiments |
| vocal-LM-exp | vocal language model experiments |
| m4a-editor | a small tool for trimming our voice samples |

## Three things we're proud of

- **It works with the internet off.** Whisper, the voice measurements and the
  recipe logic all run on a $60 computer.
- **Every failure has a fallback.** Dead network, silent guest, refused pump --
  the machine degrades instead of stopping.
- **The numbers are real.** Pitch tracking is checked against synthetic voices
  with known answers; the pump link is tested against a simulated UNO Q; seven
  test suites run on the Pi itself.

*HackMIT 2026.*
