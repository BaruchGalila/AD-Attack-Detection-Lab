# Active Directory Attack & Detection Lab (SOC Tier 1)

A hands-on Active Directory authentication detection lab built from a **SOC Tier-1 analyst perspective**.

This repository focuses on defensive security operations:
- Understanding identity-based attacks in Active Directory
- Detecting them through Windows Security and Kerberos logs
- Performing Tier-1 triage (Noise / Exposure / Incident)
- Correlating events into an investigation timeline
- Documenting incidents in a SOC-ready format before escalation

The goal is **not exploitation**, but learning how attacks **appear in logs** and how a Tier-1 analyst should respond.



## Lab Environment

- **Domain Controller:** Windows Server (AD DS, DNS, Kerberos / KDC)
- **Client:** Windows 10 (domain-joined)
- **Attacker:** Kali Linux (isolated lab only)
- **Domain:** `lab.loc`



## High-Level Topology

```text
[Kali Linux]  --->  [Windows Server DC: lab.loc]  <---  [Windows 10 Client]
      (attacker)            (AD DS / DNS / KDC)           (domain-joined)

Repository Structure
AD-Attack-Detection-Lab/
├── lab-setup/
│   └── environment.md
├── attack-scenarios/
│   ├── password-spraying.md
│   ├── pass-the-hash.md
│   ├── pass-the-ticket.md
│   └── golden-silver-ticket.md
├── detection/
│   └── event-correlation.md
└── incident-response/
    ├── tier1-workflow.md
    └── incident-template.md
```
## Repository Structure & SOC Tier-1 Responsibilities

Each folder reflects a **SOC Tier-1 responsibility**:

- **attack-scenarios/**  
  What malicious authentication activity looks like in practice.

- **detection/**  
  How Windows and Kerberos logs are correlated into a clear investigation timeline.

- **incident-response/**  
  How triage decisions, documentation, and escalation are performed in a real SOC.


## Attack Scenarios Covered

- **Password Spraying**
- **Pass-the-Hash** *(detection-focused)*
- **Pass-the-Ticket** *(detection-focused)*
- **Golden / Silver Ticket** *(detection-focused)*

Each scenario follows a consistent Tier-1 investigation structure:

**Attacker Goal → Observable Behavior → Key Logs → Tier-1 Triage → Actions → Escalation Notes**


## Core Windows Event IDs

### Authentication & Kerberos
- **4624** – Successful logon  
- **4625** – Failed logon  
- **4768** – Kerberos TGT request (AS)  
- **4769** – Kerberos TGS request  
- **4771** – Kerberos pre-authentication failure  

### Privilege & Persistence Indicators
- **4672** – Special privileges assigned  
- **4673** – Sensitive privilege use  
- **7045** – New service installed  


## How To Use This Repository

1. Choose a scenario under **attack-scenarios/**  
2. Use **detection/event-correlation.md** to correlate logs into a timeline  
3. Apply the Tier-1 workflow in **incident-response/tier1-workflow.md**  
4. Document the case using **incident-response/incident-template.md**  
5. Escalate with clear evidence and structured notes  


## Example Detection Snapshot

```text
4625 Failed Logon | user: alice | src_ip: 192.168.56.20
4625 Failed Logon | user: bob   | src_ip: 192.168.56.20
4624 Successful   | user: bob   | logon_type: 3 | auth: NTLM | src_ip: 192.168.56.20
```
## Tier-1 Classification

- Repeated failures across multiple users from one source = **Exposure**
- Successful authentication following that pattern = **Incident**


## Detection & Correlation Approach

Detection focuses on **patterns**, not isolated events:

- Multiple `4625` events followed by `4624`
- `4624` (NTLM, Logon Type 3) followed by `4672` or `7045`
- Abnormal `4769` Kerberos service ticket behavior

See: [event-correlation.md](detection/event-correlation.md)


## SOC Tier-1 Incident Response Workflow

The lab follows a realistic SOC Tier-1 process:

- Alert validation
- Triage (Noise / Exposure / Incident)
- Evidence collection and enrichment
- Scope and impact assessment
- Escalation with structured documentation

See: [tier1-workflow.md](incident-response/tier1-workflow.md)


## Incident Documentation

A reusable SOC Tier-1 incident report template, including:

- Classification rationale
- Evidence summary
- Timeline
- Impact assessment
- Escalation recommendation

See: [incident-template.md](incident-response/incident-template.md)


## Key Takeaways

This project demonstrates:

- Understanding of Active Directory authentication flows (TGT / TGS)
- Detection of identity-based attacks using Windows Security logs
- SOC Tier-1 triage and decision-making
- Clear, escalation-ready incident documentation


## Disclaimer

All activities were performed in an isolated lab environment for defensive security learning purposes only.