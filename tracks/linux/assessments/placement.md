# Linux — Placement Assessment

Answer in chat. Mix of formats by design. Do **not** look up answers unless a question says "open man page allowed."

Estimated time: **15 minutes** (half of placement session).

---

## Part A — Quick retrieval (MCQ)

**A1.** What does `chmod 754 file.txt` set for owner / group / others?

- a) rwx / r-x / r--
- b) rwx / r-x / --x
- c) rwx / rw- / r--
- d) r-x / r-x / r--

**A2.** Which command shows processes in a tree hierarchy?

- a) `ps aux`
- b) `pstree` or `ps -ejH`
- c) `top`
- d) `jobs`

**A3.** Where does systemd store unit files you create as admin?

- a) `/etc/systemd/system/`
- b) `/usr/lib/systemd/user/`
- c) `/run/systemd/system/` only
- d) `/etc/init.d/`

**A4.** Signal number for SIGTERM?

- a) 9
- b) 15
- c) 2
- d) 1

---

## Part B — Explain (2–4 sentences each)

**B1.** Difference between hard link and symbolic link — when would you use each?

**B2.** What happens when you run `sudo systemctl restart nginx` vs `reload`?

---

## Part C — Scenario puzzle

**C1.** A user reports: "I can't write to `/var/www/html/index.html` even though I'm in group `www-data`."

Given: file is `-rw-r----- 1 root www-data`, user is in group `www-data`, directory `/var/www/html` is `drwxr-xr-x root root`.

What are the **two most likely** causes? How would you verify each?

---

## Part D — CLI task (run on your machine)

**D1.** Without changing anything permanent, find:
1. Your current user ID and primary group
2. How many listening TCP sockets exist
3. The last 5 lines of the system journal

Paste the commands you used (not just output).

---

## Scoring map

| Question | Topics tested |
|----------|---------------|
| A1 | linux.permissions |
| A2 | linux.processes |
| A3 | linux.systemd |
| A4 | linux.processes |
| B1 | linux.shell-basics |
| B2 | linux.systemd |
| C1 | linux.permissions |
| D1 | linux.shell-basics, linux.processes, linux.logs |
