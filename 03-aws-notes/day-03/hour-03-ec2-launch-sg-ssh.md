# Day 3 — Hour 3: EC2 Launch + SG (SSH from my IP only) + SSH Access

## Objective
Launch an EC2 instance, restrict SSH to my public IP only, SSH in successfully using a key pair, then terminate to control costs (no Free Tier available).

## Cost Control
- Free Tier: Not available/expired on this account.
- Budget: Monthly budget enabled with email alerts (50% / 80% / 100%).
- Practice rule: Terminate instance immediately after SSH verification.

## Key Outputs
- Key Pair: joshua-ec2-key (PEM) stored locally, NOT committed
- Security Group: joshua-sg-ssh-myip (SSH 22 from MY_IP/32 only)
- EC2 Instance: joshua-day3-ec2-ssh (Ubuntu 22.04, micro)
- Result: SSH successful; instance terminated after verification

## Steps

### 1) Capture my public IP
Command:
```bash
curl -s https://checkip.amazonaws.com
MY_IP = x.x.x.x/32

2) Create key pair

Name: joshua-ec2-key

Local path: ~/.ssh/joshua-cloud-ops/joshua-ec2-key.pem
Permissions:

chmod 400 ~/.ssh/joshua-cloud-ops/joshua-ec2-key.pem

3) Create Security Group

Inbound:

SSH TCP 22 from MY_IP/32 only

4) Launch EC2

Name: joshua-day3-ec2-ssh

AMI: Ubuntu Server 22.04 LTS

Type: t2.micro/t3.micro

Public IPv4: enabled

SG: joshua-sg-ssh-myip

5) SSH access
ssh -i ~/.ssh/joshua-cloud-ops/joshua-ec2-key.pem ubuntu@PUBLIC_IP


Verification:

whoami
hostnamectl
ip a

6) Termination

Terminated instance immediately after verification to control cost.


