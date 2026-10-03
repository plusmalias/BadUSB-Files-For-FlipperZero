# App Review Notes — ORCA

Paste the "Review Notes" section into App Store Connect → App Review
Information → Notes. The rest is a checklist/cover for you.

---

## Review Notes (paste this)

ORCA is a companion app for Flipper Zero, a consumer multi-tool device that the
user owns. The app is a remote control, file manager, and script editor for
that hardware — comparable to a manufacturer companion app.

**Hardware required to exercise all features.** Most functions (Sub-GHz,
Infrared, BadUSB/Ducky, Remote, file transfer) operate the user's own Flipper
Zero over BLE and do nothing without it connected. A reviewer without the
hardware can still verify the UI, the local script editor, and the BLE Recon
scanner. A demo video showing a connected device is attached below.

**All processing is local.** ORCA has no server, no account system, and no
analytics. Nothing the app reads leaves the device. See the privacy policy.

**Intended use.** The app is for authorized security testing, education, and
control of hardware the user owns — the same lawful, user-owned-hardware context
in which Flipper Zero itself is sold. BadUSB/Ducky scripts are plain-text
payloads the user authors and sends to their own device; Sub-GHz and Infrared
transmit through the user's own Flipper. The app does not bypass, disable, or
gain unauthorized access to any Apple software, service, or third-party system.

Demo video: <link>
Test instructions: Connect a Flipper Zero via Bluetooth, open Remote, and the
live screen mirrors the device. Recon works without hardware and lists nearby
BLE advertisements.

---

## Pre-submission checklist

### Blocking (auto-reject if missing)
- [ ] `PrivacyInfo.xcprivacy` added to the app target and included in the bundle
- [ ] Every permission has a matching `Info.plist` usage string
- [ ] Privacy Policy URL is live and publicly reachable (no login wall)
- [ ] Support URL resolves
- [ ] App icon 1024×1024, no alpha channel / transparency
- [ ] Privacy Nutrition Label filled in App Store Connect
      (local-only: "Data Not Collected")
- [ ] Export compliance answered (BLE/TLS → typically "uses exempt encryption")

### Guideline-risk (category: 2.5.1 / 1.4.x)
- [ ] App description frames ORCA as a Flipper companion, not a "hacking tool"
- [ ] In-app links to Privacy Policy + Terms (Settings screen)
- [ ] Demo video recorded with Flipper connected
- [ ] Age rating set (17+ is the safe choice for this tooling)
- [ ] No "Always" location / background BLE declared unless actually used

### Technical hygiene
- [ ] Permission prompts fire on first feature use, not at launch
- [ ] App degrades gracefully when a permission is denied
- [ ] Screenshots: 6.7" + 6.5" (+ iPad if universal)
- [ ] Recon UI shows randomized UUIDs, not implied real MACs (iOS can't read MACs)
