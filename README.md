# Claudio Retro Arcade Soundpack

8-bit and 16-bit console and arcade sounds for [Claudio](https://claudio.click):
menu blips, a coin, a jump, a power-up, a pause jingle, a laser, damage, and
a stage clear. Every sound is synthesized from a text recipe in this
repository; there are no samples.

## Install

```bash
claudio soundpack add gh:ctoth/claudio-soundpack-retro-arcade --name retro-arcade --default
```

Already installed? Pull the latest:

```bash
claudio soundpack update retro-arcade
```

## What Is In It

44 sounds answering for 132 sound keys.

The pack is built the way a sound chip of that era was: pulse waves at
12.5%, 25% and 50% duty for leads and blips, a triangle wave for bass, and a
noise channel for clicks, thumps, snares and explosions. Chords are played
as fast arpeggios, because the chips had too few voices to hold one.

- **One key.** Everything pitched is drawn from C major pentatonic
  (C D E G A), so sounds that overlap stay in tune with each other. Success
  sounds land on C. Error sounds step outside the scale (D flat, A flat,
  B flat, F sharp), which is why they sound wrong without being loud.
- **Call and response.** A loading sound and its result share a voice: the
  shell starts with a hollow square G and succeeds by stepping up to C, or
  fails by dropping a tritone to D flat. An edit starts with two taps and
  ends with a coin, or with the same two taps and a sour note.
- **Short where it counts.** The sounds that fire constantly are 30 to
  220 ms: a menu-cursor tick, single blips, two-note flicks. The tunes are
  kept for what happens rarely: session start (2.7 s), a finished turn, a
  commit, a passed build.
- **Level.** Every sound sits at -18 LUFS with true peaks under -1 dBTP, so
  one volume setting suits all of them. 48 kHz 16-bit WAV, the player's own
  format.

Some of the idioms and where they are used:

| Idiom | Event |
| --- | --- |
| Menu cursor tick | default, generic success |
| Coin | file edited or written |
| Jump | prompt sent |
| Laser | subagent dispatched |
| Scanner warble | search |
| Power-up | build passed |
| Item get | commit landed |
| Pause jingle | plan presented |
| Damage, bonk | errors |
| Siren and explosion | critical error |
| Stage clear | celebration |
| Insert coin, start tune | session start |

## Layout

```text
source/recipe.json     the recipe: every sound, and which key it answers for
source/synth/          rendered from the recipe by `soundpack synth`
source/soundpack.json  written by `soundpack synth`
soundpack.json         built: sound key -> sounds/...wav
sounds/                built: mastered WAVs
```

Everything except `source/recipe.json` is a build product. Do not edit the
others by hand; change the recipe and rebuild.

## Changing The Pack

Needs a `claudio` with the `soundpack synth`, `soundpack master` and
`soundpack audit` commands.

1. Edit `source/recipe.json`. `claudio soundpack synth --help` lists every
   setting. Each sound has an `about` line saying what it is meant to be.
2. Rebuild and check:

   ```bash
   rm -rf sounds soundpack.json source/synth source/soundpack.json
   claudio soundpack synth source/recipe.json --out source
   claudio soundpack master source/soundpack.json --out .
   claudio soundpack audit soundpack.json --strict
   claudio soundpack validate soundpack.json
   ```

3. Read the audit table against the `about` lines. A sound described as
   "three rising notes" should show 3 under `HIT` and `rising` under
   `TREND`. If `master` reports a sound as `truncated`, it was longer than
   its category allows; shorten it in the recipe.

CI runs the same validate and audit on every push, and rebuilds the pack
from `source/soundpack.json` to check that the committed build matches.

## Source Notice

The recipe and the sounds rendered from it are original work made for this
pack. No recordings or samples from any game are included.
