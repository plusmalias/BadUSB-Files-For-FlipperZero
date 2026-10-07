# AIWA AWS400G (40" FHD TV) — Flipper Zero IR Remote

Your remote's gone, and your TV is an AIWA Model **AWS400G** (per the rating
label on the back). Here's the honest technical situation and the actual
fix.

## Why there's no ready-made file

I checked the two real sources that would have this:

1. **Flipper-IRDB** (the community database of ~3000+ captured `.ir`
   files, organized by brand) — has no `AIWA` folder under `TVs/` at all.
   It has a couple of Aiwa audio receivers and CD players, but no TV.
2. **Flipper's own built-in Universal Remote dictionary** (ships in
   firmware, no file needed) — covers a few hundred manufacturer code
   sets, but there's no public listing confirming AIWA is one of them.

AIWA-branded TVs sold in AU/NZ are rebadged ODM sets (the board inside
almost certainly isn't made by "Aiwa" — the brand is licensed), so there's
no guarantee it even uses a documented code set. **Any hex codes I typed
in here would be a guess dressed up as data — they'd very likely be wrong
and I'm not going to hand you a file that looks real but doesn't work.**
`AIWA_AWS400G.TEMPLATE.ir` in this folder is a syntactically valid Flipper
IR file with every signal zeroed out as a placeholder — it's a skeleton,
not a working remote.

## The actual fix (10 minutes, no guessing)

Flipper Zero already has a feature built exactly for "TV, no remote,
unknown brand": **Universal Remote brute-force**.

1. On the Flipper: **Apps → Infrared → Universal Remote → TVs**.
2. Stand ~2-3 m from the TV, point the Flipper's IR transmitter (top
   edge) at it.
3. Press **Power** in the app. It cycles through its whole dictionary of
   known manufacturer power codes until the TV responds (powers off/on).
   This can take anywhere from a few seconds to a minute or two — let it
   run.
4. Once the TV reacts, the Flipper has found a working protocol/address
   for your set. The same menu's **Vol+/Vol-/Ch+/Ch-/Mute** buttons will
   very likely also work immediately, since universal codebooks bundle
   the whole button set per manufacturer entry, not just power.

That gives you working power/volume/channel/mute with zero file editing.

## Getting the rest of the buttons (Menu, Source, arrows, OK, Home)

The Universal Remote only exposes the basic button set. For the rest, you
need a real signal source to learn from — the Flipper can't invent these,
and neither can I. Two options:

- **Any cheap universal remote** (the $8 kind from a supermarket/hardware
  store) set to the code the brute-force step above found, or run through
  its own AIWA/TV auto-search. Once it drives your TV, use Flipper's
  **Infrared → Learn New Remote**, name each button, and point the
  universal remote at the Flipper to capture each signal. This writes a
  fully valid `.ir` file automatically — correctly formatted, no manual
  editing needed.
- **A phone with a built-in IR blaster** running a universal remote app
  set to your TV, same process.

Once you've learned real signals, either:
- let Flipper save its own file and use that directly, or
- drop the captured `address`/`command` byte pairs into
  `AIWA_AWS400G.TEMPLATE.ir` in place of the `00 00 00 00` placeholders,
  then rename it to `AIWA_AWS400G.ir`.

## File format reference

Flipper's parsed IR format (what the template uses):

```
Filetype: IR signals file
Version: 1
#
name: Power
type: parsed
protocol: NEC
address: XX 00 00 00
command: XX 00 00 00
```

Each `#` line separates one signal block. `protocol` is usually `NEC`,
`NECext`, `Samsung32`, `RC5`, or `RC6` — whatever Flipper reports after a
successful Learn is the correct one to use; don't assume NEC if Learn
reports something else.
