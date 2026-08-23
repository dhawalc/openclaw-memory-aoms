# DEPRECATED — not wired into the live runtime

This TypeScript OpenClaw memory plugin is **not installed** in OpenClaw and is
**not** part of the live memory path. OpenClaw talks to AOMS directly over HTTP
(`~/.openclaw/workspace/boot_aoms.py` for reads; the `openclaw-session-sync`
systemd timer for writes).

Its tool payloads have also drifted from the current AOMS contract
(`cortex-mem/cortex-mem/service/models.py`): `memory_write`/`memory_weight` would
return HTTP 422 against AOMS v1.2.0. Kept for reference only.

Source of truth: `cortex-mem/cortex-mem` (GitHub: `dhawalc/cortex-mem`).
Marked during the 2026-05-26 AOMS audit/repair.
