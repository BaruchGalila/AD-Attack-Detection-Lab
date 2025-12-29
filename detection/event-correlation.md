# Detection – Event Correlation Guide (SOC Tier-1)

## Why correlation matters
Single events are often ambiguous. SOC value comes from connecting:
- authentication method
- source/destination context
- privilege changes
- persistence actions

## Core event IDs (practical set)
- 4624: successful logon
- 4625: failed logon
- 4768: Kerberos TGT
- 4769: Kerberos TGS
- 4672: special privileges assigned
- 4673: sensitive privilege use
- 7045: service installation (persistence)

## Correlation patterns (examples)

### Pattern A – Password Spray (Exposure → Incident)
1) Many 4625 failures
   - same source IP
   - many usernames
2) If any 4624 success for a targeted user:
   - classify as Incident
3) Check for follow-up:
   - 4672 / 7045 or share access logs (if available)

### Pattern B – Likely Pass-the-Hash Lateral Movement
1) 4624 success
   - NTLM
   - logon type 3
   - unusual source
2) Followed by:
   - 4672 privileges OR
   - admin share access OR
   - service installation (7045)

### Pattern C – Kerberos ticket abuse / suspicious TGS
1) 4769 TGS requests
   - RC4 in AES-baseline
   - privileged services (CIFS/HOST/LDAP)
2) Followed by:
   - 4624 + privileged actions (4672/4673)
   - server/DC touch points
3) Escalate if pattern indicates admin compromise

## Quick triage checklist (Tier-1)
- Who? (account name, privilege level)
- From where? (source IP/host)
- To what? (service name, host name)
- When? (time window / repetition)
- What next? (privilege/persistence indicators)
- Impact? (admin? DC? many hosts?)

## Output expectation
A Tier-1 analyst should be able to produce:
- short timeline
- classification (noise/exposure/incident)
- escalation note with evidence
