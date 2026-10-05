# Claudio Retro Arcade Soundpack

8-bit and 16-bit console and arcade sounds for [Claudio](https://claudio.click):
menu blips, a coin, a jump, a power-up, a pause jingle, a laser, damage, and
a stage clear.

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

Every sound is built on a recording: 20 chip-style effects and jingles by
Kenney (CC0), sliced, retuned and layered by a text recipe in this
repository. A held note from an NES-style jingle is the lead voice, a
decaying zap is the ringing voice, a short hollow tone plays the shell and
edit blips, and a low bong is the bass. Recorded phaser slides, a retro
laser, an engine drone and an explosion are used as they are. Synthesis adds
only what a sound chip would add around them: a triangle bass line, a second
pulse voice, noise-channel clicks and hats, and an arpeggio shimmer.

- **One key.** Every recording is retuned so that pitched notes fall in C
  major pentatonic (C D E G A), so sounds that overlap stay in tune with
  each other. Success sounds land on C. Error sounds step outside the scale
  (D flat, A flat, B flat, a whole-tone run, a semitone trill), which is why
  they sound wrong without being loud.
- **Call and response.** A loading sound and its result share a voice: the
  shell starts with a hollow G and succeeds by stepping up to C, or fails by
  dropping a tritone to D flat. An edit starts with two taps and ends with a
  coin, or with the same two taps and a sour note.
- **Short where it counts.** The sounds that fire constantly are 20 to
  200 ms: a menu-cursor tick, single blips, two-note flicks. The tunes are
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
| Scanner sweep | search |
| Gear change | build started |
| Power-up | build passed |
| Item get | commit landed |
| Pause jingle | plan presented |
| Damage, bonk | errors |
| Alarm and explosion | critical error |
| Stage clear | celebration |
| Insert coin, start tune | session start |

## Layout

```text
source/recipe.json     the recipe: every sound, and which key it answers for
source/raw/            the recordings the recipe slices, as downloaded
source/sources.json    author, source and licence of every file in raw/
source/synth/          rendered from the recipe by `soundpack synth`
source/soundpack.json  written by `soundpack synth`
soundpack.json         built: sound key -> sounds/...wav
sounds/                built: mastered WAVs
```

`source/synth/`, `source/soundpack.json`, `soundpack.json` and `sounds/` are
build products. Do not edit them by hand; change the recipe and rebuild.

## Changing The Pack

Needs a `claudio` with the `soundpack synth`, `soundpack master` and
`soundpack audit` commands.

1. Edit `source/recipe.json`. `claudio soundpack synth --help` lists every
   setting. Each sound has an `about` line saying what it is meant to be. A
   layer with `"sample"` plays a slice of a file under `source/raw/`:
   `start` and `end` pick the slice, `speed` retunes it (speed = wanted Hz /
   recorded Hz). Recordings are never cut by hand; every cut is in the
   recipe.
2. If you add a recording, put it under `source/raw/<collection>/` and add
   its author, source and licence to `source/sources.json` and to the
   Sources section below. Remove a recording the recipe no longer uses.
3. Rebuild and check:

   ```bash
   rm -rf sounds soundpack.json source/synth source/soundpack.json
   claudio soundpack synth source/recipe.json --out source
   claudio soundpack master source/soundpack.json --out .
   claudio soundpack audit soundpack.json --strict
   claudio soundpack validate soundpack.json
   ```

4. Read the audit table against the `about` lines. A sound described as
   "three hits, rising" should show 3 under `HIT` and `rising` under
   `TREND`. If `master` reports a sound as `truncated`, it was longer than
   its category allows; shorten it in the recipe.

CI runs the same validate and audit on every push, and rebuilds the pack
from `source/soundpack.json` to check that the committed build matches.

## Sources

All 20 recordings under `source/raw/` are by **Kenney**
([kenney.nl](https://kenney.nl)) and are released under
[CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) (public
domain dedication). No credit is legally required; it is given here with
thanks.

| Collection | Files used | Licence |
| --- | --- | --- |
| [Digital Audio](https://kenney.nl/assets/digital-audio) | phaseJump3, phaserDown1, phaserUp4, phaserUp7, powerUp1, threeTone2, tone1, zap1, zap2, zapThreeToneUp | CC0 1.0 |
| [Music Jingles](https://kenney.nl/assets/music-jingles) | jingles_NES00, jingles_NES11, jingles_NES13 | CC0 1.0 |
| [Interface Sounds](https://kenney.nl/assets/interface-sounds) | bong_001, maximize_006, minimize_006, tick_002 | CC0 1.0 |
| [Sci-Fi Sounds](https://kenney.nl/assets/sci-fi-sounds) | engineCircular_002, explosionCrunch_000, laserRetro_000 | CC0 1.0 |

`source/sources.json` lists each file with its author, source page and
licence. The recipe and the synthesized layers are original work made for
this pack. No recordings or samples from any commercial game are included.
