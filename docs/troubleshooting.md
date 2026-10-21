# Troubleshooting — by symptom

Only real errors, lived through in the original integration.

## `rea setup` doesn't know Muse

Expected: Muse is not on the supported-client list. It is not your install; this guide's route (CLI + custom MCP bridge) is the alternative.

## The doctor keeps flagging Hopper, IDA or Kuna

Expected without a license: they are commercial or user-supplied. Ghidra covers native analysis and JADX covers APKs. Nothing in the guide is blocked by this.

## The doctor flags the skill as missing

That check looks for the skill in the official REA clients' paths, not in Muse's `~/workspace/custom-skills/`. Verify with your Muse's skill search: if `reverse-engineer-anything` appears, it is installed where Muse reads it.

## Ghidra won't start or the doctor rejects it

- It needs a **full 64-bit JDK 21** (with compiler). A JRE or a JDK 17 will not do for Ghidra 12.1.
- `GHIDRA_INSTALL_DIR` must point at the unzipped `ghidra_12.1.4_PUBLIC` folder, and REA expects the **12.1.x** series: another series may be rejected.

## REA says WABT is missing even though it's on the PATH

REA does not look for it on the PATH: it requires the **`REA_WABT_BIN_DIRECTORY`** variable with the absolute path to WABT 1.0.42's `bin` folder.

## Playwright's Chromium download keeps getting cut

Symptom: `server closed connection` on every installer retry. Before blaming the machine, diagnose:

1. Do other large downloads work (GitHub, Google)? If yes, the general network is fine and the cut is specific to that route.
2. Playwright's CDN redirects (307) to Google storage: try a ranged request against the final file with `curl -r 0-20971519` — if it arrives complete, the route is healthy.
3. Proven fix (the "chunked bridge"): download the ZIP in **8 parallel ranges with retries**, verify its integrity, unzip it into `~/.cache/ms-playwright/chromium_headless_shell-1208/` (or `chromium-1208/`) and create the empty `INSTALLATION_COMPLETE` file. Ready-made script in `../scripts/install-playwright-bridge.sh`.
4. Valid stopgap: REA accepts the environment's own Chrome by passing its path as `executable_path` in the browser commands.

## REA won't analyze a page's or app's JavaScript

If the code lives in an inline `<script>` inside the HTML, REA's JavaScript analysis cannot see it (a stated limitation). Extract the script to a `.js` file in a working copy — without touching the original — and analyze the copy.

## Ghidra or JADX are very slow or run out of memory

Muse's machine has roughly 8 GB of RAM: run one analysis at a time, raise the call timeout instead of reloading, and for large APKs cap JADX's heap with `REA_JADX_HEAP_MIB`. Huge targets may be out of reach; say so with evidence rather than forcing it.
