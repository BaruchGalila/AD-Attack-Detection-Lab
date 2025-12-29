# Scenario: Pass-the-Hash (SOC Tier-1)

## Goal
Identify likely Pass-the-Hash behavior by detecting **NTLM-based authentication**
used for remote access/lateral movement (without normal interactive login behavior).

## What it looks like (pattern)
- **4624** successful logon
- Authentication Package: NTLM
- Logon Type: 3 (Network)
- Often from an unexpected source host/IP
- Often followed by access to admin shares / remote execution indicators

## Key Logs to collect
### Event ID 4624 – Successful Logon
Focus fields:
- Account Name
- Logon Type (3 is common for network)
- Authentication Package (NTLM)
- Source Network Address
- Workstation Name
- Elevated Token (if present)
- Impersonation Level (contextual)

### Event ID 4625 – Failed Logons (optional context)
- Sometimes attackers test credentials before success.

### Follow-up indicators (if available in your environment)
- **4672** Special privileges assigned (admin context)
- **5140/5145** share access (ADMIN$, C$, IPC$)
- **7045** service install (persistence / remote execution)

## SOC Tier-1 Triage (Decision)
### Noise
- Known admin maintenance window
- Known IT jump box
- Expected NTLM usage for legacy system
- Source host is approved + documented

### Exposure
- NTLM type 3 logon looks odd but no follow-up activity and source cannot be verified yet.

### Incident
- NTLM type 3 logon from suspicious source AND
  - admin share access, OR
  - privilege events (4672/4673), OR
  - service install (7045), OR
  - multiple hosts show similar pattern (spread)

## Tier-1 Actions (practical)
1. Validate context:
   - Is this account expected to use NTLM?
   - Is the source host expected?
2. Correlate:
   - Look for 5145 (ADMIN$ / C$)
   - Look for 4672/4673 near the same timestamp
   - Look for 7045 soon after
3. Document:
   - user, source, destination, timestamps, event IDs
4. Escalate immediately if:
   - admin activity is confirmed
   - lateral movement indicators exist
   - same source hits multiple targets

## Notes
Pass-the-Hash detection is not about “one event”.
It’s about **authentication method + context + follow-up actions**.
