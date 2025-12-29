# Incident Response – SOC Tier-1 Workflow

## 1) Triage (first minutes)
Goal: confirm if this is Noise / Exposure / Incident.

Steps:
1. Validate the alert / log:
   - correct host? correct time? correct user?
2. Look for pattern:
   - repetition? multiple accounts? same source?
3. Identify scope quickly:
   - single endpoint vs multiple
   - standard user vs admin/service
4. Decide classification:
   - Noise / Exposure / Incident

## 2) Enrichment (collect evidence)
Goal: gather context without overreacting.

Collect:
- source IP / workstation
- account name + privilege
- target host/service
- time window start/end
- event IDs in sequence (timeline)

## 3) Decision & Escalation
Exposure:
- document, monitor, recommend containment if needed
Incident:
- escalate immediately with a clean note
- recommend containment actions (block source, isolate host) based on org policy

## 4) Documentation (minimum expected)
A Tier-1 note must include:
- summary (1–2 lines)
- classification
- evidence (event IDs + key fields)
- timeline (simple)
- scope/impact guess (admin? DC? spread?)
- recommendation / escalation reason
