# Scenario: Pass-the-Ticket (SOC Tier-1)

## Goal
Detect suspicious Kerberos service ticket activity that could indicate ticket reuse / PTT.

## What it looks like (pattern)
- Abnormal **4769** (TGS requests)
- Unusual encryption type (often **RC4-HMAC** in environments that should prefer AES)
- Tickets used from unexpected hosts
- Lateral movement into server/DC services (CIFS/HOST/LDAP)

## Key Logs to collect
### Event ID 4769 – Kerberos Service Ticket (TGS)
Focus fields:
- Account Name
- Service Name (e.g., CIFS/HOST/LDAP)
- Ticket Encryption Type (RC4 vs AES)
- Client Address / source host (if present)

### Optional context
- **4624** logon success around same time
- **4672** privileges (admin usage)
- DNS SRV lookups / anomalies (if relevant in your environment)

## SOC Tier-1 Triage (Decision)
### Noise
- Known admin system doing expected Kerberos service auth
- Encryption type matches baseline (AES)
- Source host is expected

### Exposure
- Suspicious 4769 (RC4) but no proof of privilege use or lateral movement yet.

### Incident
- RC4 TGS requests for privileged services + unusual source AND
  - subsequent admin access or persistence activity, OR
  - repeated TGS requests across many services (indicative of abuse)

## Tier-1 Actions (practical)
1. Validate baseline:
   - Is AES expected? Is RC4 unusual here?
2. Identify the service target:
   - CIFS/HOST/LDAP to critical hosts (servers/DCs) increases severity
3. Correlate:
   - 4624 network logons
   - 4672 privilege
   - 7045 persistence
4. Escalate:
   - If it touches DC/service accounts
   - If RC4 abnormality is confirmed + follow-up suspicious actions

## Notes
PTT suspicion often comes from:
- **where** the ticket is used (targets)
- **how** it’s encrypted (RC4 vs AES baseline)
- **what** happens next (movement / privilege / persistence)
