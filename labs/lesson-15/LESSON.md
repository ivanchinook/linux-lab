# Lesson 15 — Containers & networking

**Duration target:** ~30–40 min (lecture + lab + retrieval)  
**Tracks:** networking (+ light Linux contrast)  
**Topics:** `net.containers`, `net.tcp-udp`, `net.diagnostics`, `net.interfaces`  
**Prerequisites:** lessons 1–14 (especially `net.tcp-udp`, `net.dns`, `net.diagnostics`, `linux.systemd`)  
**Lab path:** `labs/lesson-15/`

> **Curriculum note:** Lesson 14 is bash scripting (`linux.scripting`). Run that first if not done yet — this lesson is queued as **15**.

---

## Before you start

1. Docker Engine running (`systemctl is-active docker` → `active`).
2. Your user can run Docker without `sudo` (in the `docker` group), or use `sudo docker compose …`.
3. Optional: Portainer — visualize networks and ports alongside CLI.

```bash
cd labs/lesson-15
docker compose up -d
docker compose ps
```

Teardown when finished:

```bash
docker compose down
```

---

## Part A — Lecture (~10 min)

### 1. Three layers to keep separate

| Layer | What it is | Example |
|-------|------------|---------|
| **Host** | Your Debian machine | `systemctl`, `ip route`, real NIC |
| **Container** | Isolated process(es) sharing the host kernel | nginx in a box |
| **VM** | Full virtual machine with its own kernel | Cursor cloud agent VM |

Containers are **not** mini-VMs. They share the host kernel but get their own filesystem, network namespace, and process tree.

### 2. Port mapping vs container IP

- **`ports: "8080:80"`** — host port 8080 → container port 80.  
  From the host: `curl localhost:8080` hits nginx inside `web`.
- **Container IP** — on a bridge network (e.g. `172.18.0.2`).  
  Reachable from the host and from other containers on the same network.

Both work; they teach different ideas (NAT/publish vs L3 on a virtual LAN).

### 3. Docker bridge network

`docker compose` creates network `lesson-15_lab` (project name + network name).

- Each service gets an IP on that bridge.
- Containers on the **same** network can reach each other by **service name** (`web`, `toolbox`) — Docker’s embedded DNS.
- Containers on **different** networks cannot talk unless you connect them.

This mirrors real DevOps: app talks to `db:5432`, not a hard-coded IP.

### 4. Diagnostics inside vs outside

| Where you run | Good for |
|---------------|----------|
| **Host** | Published ports (`curl localhost:8080`), `docker ps`, `docker network inspect` |
| **Inside toolbox container** | Service-to-service (`curl http://web`), `dig web`, `ss -tlnp` |

Same tools from lessons 7–8 (`curl`, `ss`, `dig`) — different network namespace.

### 5. systemd vs containers (review from lesson 3)

- **Host service:** `systemctl enable nginx` — starts on boot, managed by systemd.
- **Container:** `docker compose up -d` — Docker daemon starts containers; no systemd unit inside by default.

DevOps pattern: run apps in containers; use systemd on the host for Docker itself.

### 6. DNS vs gateway (review)

- **Gateway** — routes packets **off** your subnet (`ip route show default`).
- **DNS** — resolves **names → IPs** (`/etc/resolv.conf`, `dig`).
- Docker embedded DNS resolves **Compose service names** on the bridge — same name→address idea, different scope.

---

## Part B — Hands-on lab (~15 min)

### Step 1 — Host → published port

```bash
curl -s localhost:8080 | head -3
```

Expect the HTML from `web/index.html`.

### Step 2 — Find container IP (host CLI)

```bash
docker compose ps
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' lesson-15-web-1
```

Or:

```bash
docker network inspect lesson-15_lab
```

Note the subnet (e.g. `172.18.0.0/16`) and `web`’s IP.

```bash
curl -s http://172.18.0.x/ | head -3   # replace x with web's last octet
```

### Step 3 — Container → container (service DNS)

```bash
docker compose exec toolbox curl -s http://web/ | head -3
docker compose exec toolbox dig +short web
docker compose exec toolbox ping -c 2 web
```

### Step 4 — Port vs service name

From **toolbox**, this should **fail** (8080 is published on the **host**, not inside `web`):

```bash
docker compose exec toolbox curl -s http://web:8080
```

From **toolbox**, this should **work** (nginx listens on 80 **inside** the container):

```bash
docker compose exec toolbox curl -s http://web:80/ | head -3
```

**Takeaway:** `8080:80` maps host→container; **inside** the network, use container port **80**.

### Step 5 — Optional Portainer

In Portainer: **Containers** → `lesson-15-web-1` → published ports and network.  
**Networks** → `lesson-15_lab` → attached containers and IPAM subnet.

Compare with `docker network inspect lesson-15_lab`.

---

## Part C — Retrieval quiz (~10 min)

Score using `framework/rubrics/scoring.md`. Target ~60–80%.

### Q1 (MCQ) — Port mapping

In `ports: "8080:80"`, what does **8080** represent?

- A) Port inside the container  
- B) Port on the host  
- C) Port on the default gateway  
- D) Ephemeral port assigned by DNS  

<details><summary>Answer</summary>B — host port. Container port is 80.</details>

---

### Q2 (MCQ) — Service discovery

Two containers are on the same Docker Compose network. App container needs to reach `web`. Best hostname?

- A) `localhost`  
- B) `127.0.0.1`  
- C) `web`  
- D) Host machine’s LAN IP  

<details><summary>Answer</summary>C — Compose service name via embedded DNS.</details>

---

### Q3 (Scenario) — Connection refused from toolbox

From `toolbox`, `curl http://web:8080` fails. `curl http://web:80` works. One sentence why?

<details><summary>Answer</summary>8080 is published on the **host**; nginx listens on **80 inside** the container. Service-to-service traffic uses container ports on the bridge network.</details>

---

### Q4 (CLI) — List and inspect

Write two commands: (1) show running compose services for this lab, (2) show IP addresses of containers on `lesson-15_lab`.

<details><summary>Answer</summary>`docker compose ps`; `docker network inspect lesson-15_lab`</details>

---

### Q5 (Scenario) — DNS vs gateway review

A colleague sets the container’s **default gateway** to `8.8.8.8` to “fix DNS.” What’s wrong?

<details><summary>Answer</summary>8.8.8.8 is a **public DNS resolver**, not a gateway. Gateway routes off-subnet traffic; DNS resolves names. Fix nameservers, not the default route.</details>

---

### Q6 (Explain) — Host vs container

In one or two sentences: how is a container different from a systemd service on the host?

<details><summary>Answer</summary>Container: isolated filesystem/network/process namespace, started by Docker, portable image. systemd service: runs on host namespaces, managed by systemctl, integrated with host journal and boot.</details>

---

## After the session

Update `progress/learner-model.json`:

- Bump `net.containers` based on quiz score.
- Log results in `progress/sessions/YYYY-MM-DD-lesson-15.md`.

**Suggested next session (lesson 16):** NAT, routing, and firewall basics (`net.nat-firewall`).

---

## Troubleshooting

| Problem | Check |
|---------|--------|
| `permission denied` on docker.sock | `sudo usermod -aG docker $USER`, re-login |
| Port 8080 in use | Change to `"8081:80"` in compose file |
| Container name not found | Run `docker compose ps` for exact name |
| `dig` missing on host | Use `toolbox` container — intentional |
