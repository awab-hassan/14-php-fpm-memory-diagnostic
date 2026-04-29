# php-fpm-memory-diagnostic

A vendored copy of [`ps_mem.py`](https://www.pixelbeat.org/scripts/ps_mem.py) by Pádraig Brady, used as an on-host diagnostic tool for PHP-FPM / Apache / MySQL memory pressure on EC2 instances.

Unlike `ps` or `top`, `ps_mem` reports **PSS (Proportional Set Size)** per program — shared memory divided proportionally across all users — giving an accurate picture of real memory consumption rather than inflated RSS totals.

> Source: https://www.pixelbeat.org/scripts/ps_mem.py  
> Licence: LGPLv2

## Usage

```bash
sudo python3 ps_mem.py              # per-program memory summary, smallest first
sudo python3 ps_mem.py -p 1234      # inspect a single PID
sudo python3 ps_mem.py -t           # totals only
sudo python3 ps_mem.py --swap       # include swap usage
```

Root is required for accurate readings against other users' processes.

## Notes

- This is an unmodified upstream tool, vendored here for convenience as an incident-response aid.
- Uses `/proc/$pid/smaps` PSS on modern kernels; falls back gracefully on older kernels.
- For persistent monitoring, prefer CloudWatch Agent's `procstat` plugin. `ps_mem` is the interactive counterpart for live diagnosis.

## Repository Layout

```
php-fpm-memory-diagnostic/
├── ps_mem.py       # upstream ps_mem v3.14 (28 May 2022) — LGPLv2
├── .gitignore
└── README.md
```
