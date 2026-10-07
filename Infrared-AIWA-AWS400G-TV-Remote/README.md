# AIWA AWS400G (40" FHD Android TV) — Flipper Zero IR Remote

Your remote broke and the TV is an AIWA **AWS400G**. The standby light
flickers when IR is sent at it, which confirms the **IR receiver is alive**
— so this is purely a matter of sending the right codes, not a hardware
dead-end.

## The approach (why these files exist)

There is no published codeset for the AWS400G specifically. But AIWA's
AU-market TVs are ODM rebadges that share scaler boards — and their
codesets — with other AU generic brands (GVA at The Good Guys, Bauhn at
ALDI, Soniq). Those brands' codes are known. They use the Flipper `NECext`
protocol with power command `1E E1` (the classic generic-scaler power code),
differing only by **address**. So the only unknown for your TV is which
address it answers to. These files find that address and give you a working
remote.

## What to do, in order

### 1. Try the best bet first
Load **`AIWA_AWS400G.ir`** and press **Power**. This is the GVA-family map
at address `00 7F` — the single most likely match. If the TV turns off/on,
you're done: Vol, Channel, Mute, Source, arrows, OK, Menu, Exit are all in
that file.

### 2. Try the other two families
If `AIWA_AWS400G.ir` Power does nothing, try **Power** in:
- `try_Bauhn_01-72.ir` (address `01 72`, same `1E E1` command)
- `try_Soniq_72-DD.ir` (address `72 DD`, different command family — full
  number keys 0–9 included)

If one of these powers the TV, that whole file is your remote.

### 3. Run the address sweep
If none of the three work, load **`AIWA_AWS400G_POWER_discovery.ir`**. It
holds 52 power signals: the `1E E1` command across many candidate
addresses, plus plain-NEC, Samsung, and RC5/RC6 long-shots. Point the
Flipper at the TV and send each signal in turn, watching for the TV to
actually **power toggle** (not just the standby LED flicker). Go in order;
it only takes a minute or two.

### 4. Tell me which one worked
Each signal is named by its protocol/address/command, e.g.
`NECext_a0472_c1EE1` or `NEC_a04_c08`. Note the name of the signal that
toggled power and send it back. From that address I can generate the
**complete** button set (all keys) at the correct address in one clean
`AIWA_AWS400G.ir`.

## Important caveat for this Android TV

Because it's an Android/smart set, the original remote was probably
Bluetooth. The TV panel still takes IR for core functions (that's the
flicker you saw), so **Power / Volume / Channel / Mute / Source are very
likely to work over IR**. Full menu navigation (arrows / OK / app launches)
*may* be Bluetooth-only on some Android sets — if the arrows don't respond
even once power works, that's the TV limiting IR, not a bad code. Getting
power and volume back is the primary win here.

## File format reference

Flipper parsed-IR format (what every file here uses):

```
name: Power
type: parsed
protocol: NECext
address: 00 7F 00 00      # 16-bit, least-significant byte first
command: 1E E1 00 00
```

The sibling remote files bundled here are from the community
[Flipper-IRDB](https://github.com/Lucaslhm/Flipper-IRDB) (CC0-licensed).
