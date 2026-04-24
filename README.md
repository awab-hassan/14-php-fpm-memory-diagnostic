# PHP-FPM / Apache Memory Diagnostic (`ps_mem.py`)

A copy of the classic **`ps_mem.py`** utility by P@draigBrady, used at production instances to diagnose PHP-FPM / Apache / MySQL memory pressure on production EC2 hosts. Unlike `ps` or `top`, `ps_mem` reports accurate **PSS (Proportional Set Size)** per program — shared memory divided proportionally among users — so you get a realistic "how much would I save if I killed this program" number instead of inflated RSS totals.

> Source: [https://www.pixelbeat.org/scripts/ps_mem.py](https://www.pixelbeat.org/scripts/ps_mem.py)
> Licence: LGPLv2

## Highlights

- **Per-program totals, not per-process** — all `php-fpm` workers are grouped; one row per program.
- **Most accurate method available** — uses `/proc/$pid/smaps` PSS on modern kernels; falls back gracefully on older kernels.
- **Third-party upstream** — vendored here for convenience; no custom modifications.

## Repository layout

```
@FPM/
├── README.md
├── .gitignore
└── ps_mem.py       # upstream ps_mem v3.14 (28 May 2022) — LGPLv2
```

## Usage

```bash
sudo python3 ps_mem.py           # per-program memory, smallest first
sudo python3 ps_mem.py -p 1234   # only PID 1234
sudo python3 ps_mem.py -t        # totals only
sudo python3 ps_mem.py --swap    # include swap usage
```

Root is required for accurate readings against other users' processes.

Typical output web host example:

```
Private  +  Shared  =  RAM used    Program
...
180.3 MiB + 45.0 MiB = 205.3 MiB   php-fpm8.1 (12)
  62.0 MiB +  8.0 MiB =  60.0 MiB   nginx (4)
...
```

## Notes

- This script is *not* my product — it's the upstream `ps_mem` tool, preserved here as an on-host diagnostic aid.
- For persistent monitoring, prefer CloudWatch Agent's `procstat` plugin; `ps_mem` is the interactive / incident-response counterpart.
- Demonstrates: operational tooling knowledge — knowing when RSS overstates reality and PSS is the right signal for "who's actually using RAM".
