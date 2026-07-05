# Networking — Placement Assessment

Answer in chat. Estimated time: **15 minutes**.

---

## Part A — Quick retrieval (MCQ)

**A1.** Which layer of TCP/IP handles routing between networks?

- a) Application
- b) Transport
- c) Internet
- d) Link

**A2.** `/24` subnet mask in dotted decimal?

- a) 255.255.255.0
- b) 255.255.0.0
- c) 255.255.255.128
- d) 255.0.0.0

**A3.** Which protocol is connection-oriented and guarantees ordering?

- a) UDP
- b) ICMP
- c) TCP
- d) ARP

**A4.** What does DNS query type `A` return?

- a) Mail server
- b) IPv4 address
- c) Canonical name alias
- d) IPv6 address

---

## Part B — Explain (2–4 sentences each)

**B1.** Walk through what happens (high level) when you `curl https://example.com` — name at least 4 steps from DNS to receiving HTML.

**B2.** Difference between a default gateway and a DNS resolver?

---

## Part C — Scenario puzzle

**C1.** You can `ping 8.8.8.8` but `ping google.com` fails with "Name or service not known."

List diagnosis steps in order (commands on Linux). What is the most likely root cause?

---

## Part D — CLI task (run on your machine)

**D1.** Find and report:
1. Your machine's default route
2. Which process (if any) listens on port 22
3. Result of resolving `example.com` (which tool, what IP)

Paste commands used.

---

## Scoring map

| Question | Topics tested |
|----------|---------------|
| A1 | net.models |
| A2 | net.ip-addressing |
| A3 | net.tcp-udp |
| A4 | net.dns |
| B1 | net.http-tls, net.dns, net.tcp-udp |
| B2 | net.ip-addressing, net.dns |
| C1 | net.dns, net.diagnostics |
| D1 | net.interfaces, net.diagnostics, net.dns |
