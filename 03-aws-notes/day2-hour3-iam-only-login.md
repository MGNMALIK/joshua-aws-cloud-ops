Excellent. That’s clean ops hygiene.

## Hour 3 — Step 5: Write the GitHub deliverable + push (GitHub Desktop)

### 5A) Create this file in your repo

Path:
**`03-aws-notes/day2-hour3-iam-only-login.md`**

# Day 2 — Hour 3: IAM-Only Console Access + Root Break-Glass

## Goal

Stop using the AWS root user for daily work. Use an IAM admin user for normal operations and keep root as break-glass only.

## What I configured

* Account alias created: `joshua-cloud-ops`
* IAM sign-in URL: `https://joshua-cloud-ops.signin.aws.amazon.com/console`
* Confirmed IAM user `joshua-admin`:

  * Console access: Enabled
  * Permissions: `AdministratorAccess`
  * MFA: Enabled
* Root account:

  * MFA: Enabled (configured previously)
  * Root access keys: **None**

## Change in daily workflow

* Daily AWS console work: **IAM user only**
* Root usage: **break-glass only** (account recovery/support/billing edge cases)

## Added redundancy (recommended)

* Created second admin IAM user: `joshua-admin2`

  * Console access enabled + MFA enabled
  * `AdministratorAccess` attached

## Proof

![IAM dashboard proof](C:\Users\Joshua\OneDrive\Documents\GitHub\joshua-aws-cloud-ops\03-aws-notes\screenshots\day2-hour3\day2-hour3-02-iam-dashboard-alias.png)
