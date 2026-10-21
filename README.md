# Install REA in Muse — Step-by-step guide

Get [REA (Reverse Engineer Anything)](https://github.com/morluto/rea) running inside your **Muse** account to analyze and clone software with evidence — no terminal needed: everything is done with prompts to your Muse.

*Leer en español: [README_es.md](README_es.md)*

![REA](https://img.shields.io/badge/REA-6.7.0-blue) ![Ghidra](https://img.shields.io/badge/Ghidra-12.1.4-green) ![JDK](https://img.shields.io/badge/JDK-21-orange) ![License](https://img.shields.io/badge/license-MIT-lightgrey)

> ⚠️ **Muse is not Claude Code.** The `rea setup` client list (Claude Code, Codex, Cursor, Gemini CLI, Grok Build…) does not include Muse, so the automatic installer does not apply here. The route that works — the one in this guide — is: **REA via CLI inside your Muse's machine + a custom MCP bridge**. It is tested end to end; the full log lives in the sister repo, [hugolopezferrer/muse-rea-log](https://github.com/hugolopezferrer/muse-rea-log).

## The two repositories

This integration is published as a pair:

- **[hugolopezferrer/muse-rea-guide](https://github.com/hugolopezferrer/muse-rea-guide)** — this repo: the step-by-step installation guide for any Muse user.
- **[hugolopezferrer/muse-rea-log](https://github.com/hugolopezferrer/muse-rea-log)** — the security log, with every chronological step, hash and verification.

## How to use this guide

1. Copy each phase's prompt and paste it into your chat with Muse.
2. Let Muse install and **verify** — each phase ends with a check that must come back green before moving on.
3. If something fails, look up the symptom in [troubleshooting](docs/troubleshooting.md).

**Requirements:** a Muse account, ~4 GB free on its machine (it will confirm it for you), and some patience: Phase 1 downloads ~800 MB.

## The three phases at a glance

| Phase | What it unlocks | Check that closes it |
|---|---|---|
| [1 — Native engine](docs/fase-1.md) | REA pinned and working + Ghidra for binaries | `rea doctor` green for Ghidra and an exact test decompilation |
| [2 — Per target](docs/fase-2.md) | APKs with JADX, pcap, WASM, firmware, browser | A real APK inspected and one method decompiled |
| [3 — MCP bridge](docs/fase-3.md) | REA as a custom connector, with stateful sessions | Handshake with 207 tools and a plan matching the CLI |

## Kickoff prompt (optional)

If you'd rather give the whole order at once, paste this to your Muse:

```text
I want you to integrate REA (github.com/morluto/rea) into your machine
following the "Install REA in Muse" guide (three phases: native engine with
Ghidra, per-target tools with JADX, and an MCP bridge as a custom
connector). Install everything under ~/workspace/tools so it survives VM
replacement, verify the SHA-256 of every artifact against its official
source before running it, and do not call a phase done without showing me
its verification. You will only analyze or clone software that is mine,
authorized, or permissively licensed (MIT or similar).
```

## Rules this guide keeps

- Only owned, authorized, or permissively licensed software — no cracking, and the original license preserved in every clone.
- Every artifact is downloaded from its official source and SHA-256 verified.
- Nothing counts as installed without a real test in front of you.

## Quick questions

**Do I need to code?** No. You need to ask, and to read the verifications Muse shows you.
**Do I really need Ghidra?** Only for native binaries. JavaScript works from minute one with no engine at all.
**Is this Claude Code?** No. It's the personal Muse (muse.ai / the app), with its own machine.
**What about Hopper or IDA?** They are paid and would need your license. Ghidra covers that role for free.

## Layout

- `docs/` — one page per phase, each with its copy-paste prompt, plus symptom-based troubleshooting.
- `scripts/` — the idempotent installers for each phase, for re-running or auditing.

## License

MIT — see [LICENSE](LICENSE). REA is by morluto (MIT); this guide is an independent integration built on their work.
