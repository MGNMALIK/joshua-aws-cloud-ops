# Week 1 — AWS Security + Cost Guardrails

## Root Account
- Root MFA enabled: YES

## IAM Admin User
- IAM user created (joshua-admin): YES
- IAM user MFA enabled: YES
- Root used for daily work: NO

## Budget Alerts
- Budget created: YES
- Budget amount: $10
- Alert thresholds: 50% / 80% / 100%
- Alert email: jrials312@gmail.com

## Rules I Follow
- Terminate EC2 the same day unless actively needed.
- Avoid leaving NAT Gateways running (can cost money).
- Never commit secrets/keys to public repos.
