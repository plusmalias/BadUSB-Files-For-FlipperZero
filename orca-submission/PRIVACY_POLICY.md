# ORCA Privacy Policy

**Last updated:** 2026-10-03

ORCA ("the app") is a companion application for Flipper Zero hardware. This
policy describes exactly what the app does and does not do with your data.

## Summary

ORCA is a **local-only** application. All data the app processes stays on your
device or on hardware you own (your Flipper Zero). ORCA does **not** operate a
backend server, does **not** have user accounts, and does **not** transmit your
data to us or to any third party.

## Data the app accesses

| Data | Why | Where it goes |
|------|-----|---------------|
| Bluetooth (BLE) signals | To connect to your Flipper Zero and, in Recon, to list nearby BLE advertisements | Stays on your device |
| Location (GPS) | Only when Wardrive is active, to geotag scan results you choose to save | Stays on your device |
| Local files / scan logs | To save scripts, captures, and scan sessions you create | Stays on your device |

On iOS, Bluetooth device identifiers are randomized, rotating UUIDs supplied by
the operating system. ORCA cannot read, and does not store, the permanent
hardware (MAC) addresses of third-party devices.

## Data we collect

**None.** ORCA has no analytics SDK, no advertising SDK, no crash-reporting
service, and no telemetry. We, the developers, receive nothing from your use of
the app.

## Permissions

ORCA requests each permission only when you first use the feature that needs it:

- **Bluetooth** — required to communicate with your Flipper Zero and to perform
  BLE scanning (Recon).
- **Location (When In Use)** — required only to geotag Wardrive results. iOS
  also requires Location permission before any app may perform BLE scanning;
  ORCA does not use your location for any other purpose.

You can revoke either permission at any time in iOS Settings. Features that
depend on a revoked permission will stop working, but the rest of the app
continues to function.

## Data sharing

We do not sell, rent, or share your data, because we never receive it.

## Children

ORCA is a technical security-research and hardware-control tool and is not
directed at children.

## Your control

Because all data lives on your device, you control it directly: delete saved
scripts, scans, and logs inside the app, or remove the app to erase everything
it stored.

## Changes

If this policy changes, the "Last updated" date above will change and the new
version will be published at this URL before it takes effect.

## Contact

Questions about this policy: **<your-support-email@example.com>**
