# Scenario: Password Spraying (SOC Tier-1)

## Goal
Detect a password spraying attempt against multiple accounts (often many usernames, few passwords)
and decide if it is **Exposure** or **Incident**.

## What it looks like (pattern)
- Many **4625** failures
- Same source IP (or small set of IPs)
- Many different usernames within a short window
- Sometimes followed by a **4624** success for one of the targeted accounts

## Key Logs to collect
### Event ID 4625 – Failed Logon
Focus fields:
- Account Name
- Logon Type (commonly 3 = network)
- Source Network Address (attacker IP)
- Workstation Name
- Authentication Package (NTLM/Kerberos)

### Event ID 4624 – Successful Logon (if compromise happens)
Focus fields:
- Account Name
- Logon Type
- Source Network Address
- Authentication Package

## SOC Tier-1 Triage (Decision)
### Noise
- Small number of failures for a single account, normal hours, normal source, no repetition.

### Exposure
- Repeated **4625** across multiple accounts from same source,
  but **no** successful 4624 and no further suspicious activity.

### Incident
- Same spray pattern **plus**:
  - a successful **4624** for a targeted account, OR
  - a sudden privilege event (4672) / lateral movement indicator, OR
  - the spray is ongoing at scale and targets privileged accounts.

## Tier-1 Actions (practical)
1. Confirm the pattern:
   - same source IP
   - many usernames
   - time window
2. Identify if success occurred:
   - search for 4624 for those usernames from the same source IP
3. Document clearly:
   - start time, end time
   - list of targeted accounts
   - source IP(s)
4. Containment suggestion (Tier-1):
   - recommend blocking the source IP at perimeter (if relevant)
   - recommend temporary lockout / password reset for targeted privileged accounts (policy-dependent)
5. Escalate:
   - **Immediate** escalation if success occurred or privilege accounts are impacted

## Common mistakes to avoid
- Treating every 4625 as an incident.
- Ignoring the “many users, same source” pattern.
- Escalating without checking whether any 4624 succeeded.
