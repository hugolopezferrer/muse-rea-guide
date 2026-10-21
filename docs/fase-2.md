# Phase 2 — Per-target tools

**What it unlocks:** Android APKs with JADX, pcap network captures, WebAssembly, firmware, and the Playwright browser for REA's web commands.
**Rough time:** 15-30 minutes.

## Prompt for your Muse

```text
Install Phase 2 of REA on your machine (Phase 1 is already done):
1) The jadx-headless-mcp 0.7.1 JAR from the official release named in REA's
   documentation, verifying its SHA-256 against the hash pinned by REA
   (6e5eacf500b64292bfb73c49797c1958f6ee44646e43e868039ae7feb573ff75), and the
   REA_JADX_MCP_JAR variable pointing at it.
2) adb from Google's official platform-tools, portable.
3) tshark, capinfos, gdb, binwalk and sigrok-cli via apt.
4) WABT 1.0.42 portable, plus the REA_WABT_BIN_DIRECTORY variable with its
   bin folder (REA does not look for it on the PATH).
5) mitmproxy in its own venv, and pwntools + unblob in another.
6) Playwright's Chromium using the playwright-core bundled with rea-agents.
   If the CDN cuts the download, don't declare it impossible: diagnose first
   and, if the route is healthy, download it in parallel ranges straight
   from Google storage and place it in ~/.cache/ms-playwright with its
   INSTALLATION_COMPLETE marker.
Update rea-env.sh with everything, capture it in a reproducible script, and
verify for me: `rea doctor` green for ghidra and jadx, a real open-source
APK inspected with one method decompiled, a pcap capture analyzed, and a
WASM module validated.
```

## Verification (don't move on without it)

- `rea doctor`: providers **ghidra and jadx configured and available**.
- The test APK (the F-Droid client worked in the original: 10,765 classes) inspects and one method comes back as readable Java.
- `inspect-pcap-client` detects your tshark 4.x and a toy pcap analyzes with endpoints and protocols.
- A minimal `.wasm` validates with its export interface.

## Notes

- Hopper, IDA and Kuna will stay red in the doctor: they are commercial and need your license. That is not a failure of your install.
- The doctor will also flag the skill as missing: that check looks in other clients' paths, not in Muse's `~/workspace/custom-skills/`. Verify with your Muse's skill search that `reverse-engineer-anything` appears.
