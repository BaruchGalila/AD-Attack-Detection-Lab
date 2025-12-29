# Scenario: Golden / Silver Ticket (SOC Detection Notes)

## Goal
Recognize signs of forged Kerberos tickets (Golden/Silver) through anomalies in ticket usage,
encryption, lifetime, and authentication patterns.

## What it can look like
- Tickets with abnormal lifetime (very long validity)
- Unusual service tickets for sensitive services
- Unexpected AES/RC4 patterns compared to baseline
- Activity that “looks legitimate” unless correlated

## Key Logs to collect
### Event ID 4768 – TGT Request (AS)
Look for:
- Frequent or abnormal requests from a source
- Failure patterns (e.g., pre-auth issues) as symptom on user side

### Event ID 4769 – TGS Request (Service Ticket)
Look for:
- RC4 usage in an AES-baseline domain
- Requests to high value services (CIFS/HOST/LDAP on DCs/servers)
- Abnormal volume patterns

### Event ID 4624 – Logon Success
Look for:
- Administrator/service accounts authenticating in unusual patterns
- Network logons (Type 3) that precede privileged actions

## SOC Tier-1 Triage (Decision)
### Exposure
- Suspicious Kerberos encryption usage or ticket behavior,
  but no proof of privilege/persistence yet.

### Incident
- Kerberos anomalies PLUS:
  - admin-level actions (4672/4673)
  - lateral movement (multiple hosts touched)
  - persistence (7045)
  - repeated access to DC services

## Tier-1 Actions (practical)
1. Correlate events into a timeline:
   - 4768/4769 → 4624 → 4672/4673 → 7045 (if present)
2. Identify impacted accounts:
   - Admins, service accounts, DC-related services
3. Escalate quickly:
   - Ticket forgery suspicion usually requires Tier-2/IR involvement
   - Recommend ticket invalidation strategy / account resets (handled by senior)

## Notes
Golden/Silver Ticket detection is mostly about **anomaly recognition + correlation**,
not “one magic event”.
