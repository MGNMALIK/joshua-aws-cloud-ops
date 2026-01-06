# Networking Troubleshooting Cheat Sheet (Host + Ubuntu VM)

## Scope

Use this when something breaks to isolate the layer fast: Link → IP → Route → DNS → Port → Service/App.

Lab reference:

* Windows host → VirtualBox NAT port forward
* SSH path: Host 127.0.0.1:2223 → Guest :22

---

## 0) Golden Rules

1. Don’t guess. Identify the layer.
2. Change one thing at a time.
3. Collect proof (command + output) before “fixing.”
4. If it broke suddenly, verify what changed: VM state, NAT rule, ssh service, firewall.

---

## 1) Quick Triage (Ubuntu VM)

A) Do I have an IP?

* `ip a`

B) Is the interface up?

* `ip link`

C) Do I have a default route?

* `ip route`

D) Can I reach the NAT gateway? (VirtualBox NAT is often 10.0.2.2)

* `ping -c 3 10.0.2.2`

E) Can I reach the internet by IP? (bypasses DNS)

* `ping -c 3 1.1.1.1`

F) Is DNS working?

* `resolvectl status`
* `getent hosts google.com`

---

## 2) Port + Service Checks (Ubuntu VM)

Is SSH listening on port 22?

* `sudo ss -tulpn | grep :22`

Is sshd running and healthy?

* `sudo systemctl status ssh --no-pager`
* `sudo journalctl -u ssh -n 50 --no-pager`

Firewall check (Ubuntu)

* `sudo ufw status verbose`

---

## 3) Host-Side Checks (Windows PowerShell)

Is the forwarded port reachable?

* `Test-NetConnection 127.0.0.1 -Port 2223`

Is something else using port 2223?

* `netstat -ano | findstr :2223`

SSH client debug (when auth/connect is weird)

* `ssh -vvv -p 2223 joshua@localhost`

---

## 4) VirtualBox NAT Port Forward Sanity Checklist

If `ssh -p 2223 joshua@localhost` fails with “Connection refused”:

1. VM is Running (not Saved/Paused)
2. VM Settings → Network → Adapter 1:

   * Attached to: NAT
3. Port Forward rule matches:

   * Protocol: TCP
   * Host IP: 127.0.0.1 (or blank)
   * Host Port: 2223
   * Guest IP: 10.0.2.15 (or blank)
   * Guest Port: 22
4. Restart SSH service inside VM:

   * `sudo systemctl restart ssh`
   * `sudo systemctl status ssh --no-pager`
5. If you edited rules: shutdown VM (not Save State) → start VM again

---

## 5) Common Symptoms → Likely Causes

“Connection refused” on localhost:2223

* VM not running OR NAT rule missing
* ssh service down in VM
* port forward misconfigured (wrong port/protocol)

“Connection timed out”

* firewall blocking
* wrong port / wrong target
* VM networking broken (interface down, no route)

Can ping 1.1.1.1 but not google.com

* DNS issue

SSH works but apt update fails

* DNS or proxy issue
* routing/NAT issue

---

## 6) Deeper Debug (Ubuntu VM)

* `ip neigh`
* `arp -n`
* `sudo tcpdump -ni any port 22`
* `traceroute 1.1.1.1`
* `curl -I https://example.com`

---

## 7) Deeper Debug (Windows)

* `ipconfig /all`
* `route print`
* `nslookup google.com`

---

### Optional proof screenshots (1–2 max)

1. Windows: `Test-NetConnection 127.0.0.1 -Port 2223` showing success
2. Ubuntu: `ip a` and `ip route` showing IP + default route

---