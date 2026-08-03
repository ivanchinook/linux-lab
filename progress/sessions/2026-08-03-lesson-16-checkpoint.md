# Session 2026-08-03 — Lesson 16 (end-of-track checkpoint)

## Summary
- Duration: ~30 min
- Score: 8.45/10 (~85%)
- Type: mixed assessment across lessons 1–15

## Assessment results

| Item | Result | Notes |
|------|--------|-------|
| Q1 file vs dir perms | 1/1 | File group lacks write |
| Q2 dpkg installed check | 0.25/1 | dpkg -L lists files; dpkg -s confirms installed |
| Q3 DNS vs gateway | 0.95/1 | DNS broken; gateway fix wrong |
| Q4 nginx/logs/ss CLI | 1/1 | reload, journalctl -n 20, ss -tlnp |
| Q5 TLS layer | 1/1 | Cert expired; ping can work |
| Q6 fstab verify | 0.7/1 | mount -a ok; manual mount test also expected |
| Q7 DNAT/firewall | 0.65/1 | iptables ok; ip route weak for inbound filter |
| Q8 script output | 1/1 | critical |
| Q9 SSH publickey | 0.9/1 | Strong server-side checks |
| Q10 kill -9 + journal | 1/1 | SIGKILL + journalctl -u myapp |

## Track summary

- Placement: 40% → End-of-track: 85%
- Lessons completed: 15 + 2 checkpoints
- Weakest topic: linux.package-mgmt (dpkg -s vs -L)

## Mastery updates

| Topic | Old | New |
|-------|-----|-----|
| linux.package-mgmt | 2 | 2 |

## Next session plan

Lesson 17: Containers & networking (optional follow-on, labs/lesson-15/)
