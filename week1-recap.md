# Week 1 Recap (Day 1)

## What I built
- Ubuntu Server VM (VirtualBox)
- SSH access from Windows using port forwarding (host port 2223 → guest port 22)
- Verified host networking with ping/tracert/nslookup and curl.exe

## Key outputs
- VM hostname: joshua-lab
- VM IP (NAT): 10.0.2.15
- Host Wi-Fi IPv4: 10.0.0.131

## What broke / how I fixed it
- Port 2222 SSH forwarding failed because it was already in use on Windows.
- Fix: used host port 2223 and connected successfully.

## Next
- AWS: document root MFA + budget alerts + IAM user
- Publish Week 1 Linux + Networking notes with screenshots
