# Phase 1 — Native engine

**What it unlocks:** REA installed permanently (not from the `npx` cache) and Ghidra as the native-binary analysis engine.
**Rough time:** 10-20 minutes, depending on the network.

## Prompt for your Muse

```text
Install Phase 1 of REA on your machine, everything under ~/workspace/tools:
1) Portable JDK 21 (Temurin LTS) in ~/workspace/tools/jdk-21.
2) Ghidra 12.1.4 from the official NSA release on GitHub, verifying the
   ZIP's SHA-256 against the official digest before unzipping it, in
   ~/workspace/tools/ghidra_12.1.4_PUBLIC.
3) rea-agents 6.7.0 global with the npm prefix in ~/workspace/tools/npm-global.
4) The reverse-engineer-anything skill bundled in the package, copied to
   ~/workspace/custom-skills/.
5) A ~/workspace/tools/rea-env.sh file with JAVA_HOME, GHIDRA_INSTALL_DIR
   and the PATH, plus a `rea` wrapper in ~/workspace/tools/bin.
Make it reproducible in a script and verify two things for me: that
`rea doctor --provider ghidra` comes back green, and a real test
decompilation: compile a small ELF binary with a simple rule and show me the
pseudocode REA+Ghidra recover from it.
```

## What Muse will do

It will download the JDK and Ghidra (about 800 MB between them), place them portably in `~/workspace/tools` so they survive a replacement of its virtual machine, pin REA's version, and prepare the environment so any future session finds it ready.

## Verification (don't move on without it)

- `rea doctor` shows the provider **ghidra: configured, available, 12.1.4**, with the "64-bit JDK 21 accepted" check passing.
- The test binary's pseudocode reproduces the compiled rule (in the original integration: multiply by 100, add 250 on a streak, double by level, and the literal string recovered).

## If something fails

- *Ghidra won't start* → almost always the JDK: it needs a **64-bit JDK 21**, not a JRE or an older version.
- *Doctor can't see Ghidra* → `GHIDRA_INSTALL_DIR` is missing or pointing at the wrong folder; load it from `rea-env.sh`.
