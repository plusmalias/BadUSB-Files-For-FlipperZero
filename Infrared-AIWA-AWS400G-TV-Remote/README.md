# AIWA AWS400G (40" FHD Google TV) — Flipper Zero IR Remote

Your remote broke. The TV is an AIWA **AWS400G** — a **Google TV** on an
**MSD9216 (MStar/MediaTek)** chassis. There's no published IR codeset for
this exact model anywhere, so this is a search problem: send real-world TV
power codes until one toggles the set, then build the full remote around it.

## Status / what's been ruled out

- The Flipper's IR transmitter is **known-good** (it controls other devices).
- The first guess — a `NECext 1E E1` generic-scaler codeset (GVA/Bauhn/Soniq
  family) — and a 52-code hand-picked sweep **all failed**. That set turned
  out to be missing most common TV protocols.
- So the real code is simply a protocol/address this TV uses that wasn't in
  that first net.

## START HERE: `AIWA_AWS400G_POWER_discovery_v2.ir`

This is the real tool. It contains **every unique TV power code in the entire
Flipper-IRDB (74 of them)** — NEC, NECext, Samsung, Sony (SIRC), Panasonic
(Kaseikyo), RCA, RC5, RC6, Pioneer — **ordered most-common-first**. The most
common single TV power code (NEC `04/08`, used by 56 different TVs) is signal
#1.

1. Load `AIWA_AWS400G_POWER_discovery_v2.ir` on the Flipper
   (Infrared → Saved Remotes).
2. Point it at the TV from ~1–2 m, direct line of sight.
3. Send each signal **in order from the top**, pausing ~1 s on each, watching
   for the TV to actually **power on/off** (not just a standby-LED blink).
4. When one toggles the TV, **note its name** — e.g. `SIRC_a0100_c1500` or
   `NECext_a027D_c46B9` — and send it back to me.

From that one winning signal I know the exact protocol + address, and I'll
generate a complete `AIWA_AWS400G.ir` with every button (Power, Vol, Ch,
Mute, Source, D-pad, OK, Menu, Back) on that codeset.

## Faster option (custom firmware only)

If your Flipper runs **Unleashed / RogueMaster / Momentum**, these 74 codes
can instead be loaded as a universal TV asset so a single **Power** press
auto-sweeps them all hands-free. Tell me if you're on custom firmware and I'll
package it that way.

## The other files (first-round attempts — lower priority)

- `AIWA_AWS400G.ir`, `try_Bauhn_01-72.ir`, `try_Soniq_72-DD.ir`,
  `AIWA_AWS400G_POWER_discovery.ir` — the original `1E E1`-family attempts.
  Already tested, didn't work. Kept only for reference; the v2 sweep
  supersedes them.

## Honest fallback

If you cycle all 74 codes and **none** toggles the TV, then this board's
codeset isn't in any public database — no `.ir` file can be built for it
without capturing from a working remote. In that case the guaranteed fix is
a **~AU$30 non-Bluetooth generic IR replacement remote** sold for the
AWS400G / AWS500G / AWC320G (e.g. RemoteOz, AU eBay). That remote *is* the
exact codeset, physically. It won't do voice/Google-Assistant, but it does
Power, Volume, Channel, Source, and the D-pad over IR.

## One caveat about the D-pad

This is a Google TV, so the original remote was IR **+ Bluetooth**. Power and
Volume are almost certainly IR. The **D-pad / OK may be Bluetooth-only** on
the panel — if, once power works, the arrows don't respond to any code,
that's the TV limiting IR for navigation, not a bad code.

## File format reference

Flipper parsed-IR format; `address`/`command` are 4 bytes, least-significant
byte first:
```
name: Power
type: parsed
protocol: NEC
address: 04 00 00 00
command: 08 00 00 00
```

Sibling/source `.ir` data is from the CC0-licensed
[Flipper-IRDB](https://github.com/Lucaslhm/Flipper-IRDB).
