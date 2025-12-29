# Active Directory Attack & Detection Lab (SOC Tier-1)

## Overview
This project is a hands-on **Active Directory authentication attack and detection lab**
designed from a **SOC Tier-1 analyst perspective**.

The goal of this lab is not offensive exploitation, but:
- understanding **identity-based attacks**
- detecting them using **Windows Security logs**
- performing **triage, correlation, and escalation**
- documenting incidents in a structured SOC-ready format

This repository demonstrates how common AD authentication attacks
appear **in logs**, how they are **classified**, and how a Tier-1 analyst
should respond before escalation.

---

## Lab Environment

- **Domain Controller:** Windows Server  
  (AD DS, DNS, Kerberos / KDC)
- **Client:** Windows 10 (domain-joined)
- **Attacker:** Kali Linux (isolated lab only)

**Domain:** `lab.loc`

---

## Lab Topology (High Level)
[Kali Linux]
|
v
[Windows Domain Controller - lab.loc]
^
|
[Windows 10 Client]
---

## Project Structure
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
Each folder focuses on a different responsibility of a SOC Tier-1 analyst:
- **attack-scenarios:** what the activity looks like
- **detection:** how logs are correlated
- **incident-response:** how decisions and escalation are performed

 HEAD
- Attack-scenarios/  
  Each scenario is written as: **Goal → What it looks like → Key logs → Tier-1 triage → Actions → Escalation notes**.

---
 5c45d94 (Add SOC Tier-1 incident report template)

## Attack Scenarios Covered

- Password Spraying
- Pass-the-Hash
- Pass-the-Ticket
- Golden / Silver Ticket (detection-focused)

Each scenario includes:
- attacker goal
- observable log patterns
- key Windows Event IDs
- Tier-1 classification (Noise / Exposure / Incident)
- recommended actions and escalation notes

---

## Core Windows Event IDs Used

Authentication & Kerberos:
- **4624** – Successful logon
- **4625** – Failed logon
- **4768** – Kerberos TGT request (AS)
- **4769** – Kerberos TGS request
- **4771** – Kerberos pre-authentication failure

Privilege & persistence indicators:
- **4672** – Special privileges assigned
- **4673** – Sensitive privilege use
- **7045** – New service installed

<<<<<<< HEAD
## How to use this repo
1. Pick a scenario in Attack-scenarios/
2. Use the detection/ guides to correlate events into a timeline
3. Apply the Tier-1 workflow under incident-response/
4. Document the incident note using the template
---
5c45d94 (Add SOC Tier-1 incident report template)

## Example Detection Snapshot

Below is a simplified example of how authentication abuse was detected:
4625 - Failed Logon
User: alice
Source IP: 192.168.56.20

4625 - Failed Logon
User: bob
Source IP: 192.168.56.20

4624 - Successful Logon
User: bob
Logon Type: 3
Authentication Package: NTLM
Source IP: 192.168.56.20
This pattern was classified as **Password Spraying → Incident**
due to a successful authentication following repeated failures
from the same source.

---

## Detection & Correlation Approach

Detection in this lab focuses on **correlation**, not single events.

Examples:
- Multiple **4625** events → followed by **4624**
- **4624 (NTLM, Type 3)** → followed by **4672 / 7045**
- Abnormal **4769** Kerberos service tickets → privilege usage

See:
➡️ `detection/event-correlation.md`

---

## SOC Tier-1 Incident Response

This lab practices a real SOC Tier-1 workflow:
1. Alert / log validation
2. Triage (Noise / Exposure / Incident)
3. Evidence collection and enrichment
4. Scope and impact assessment
5. Escalation with clear documentation

SOC workflow reference:
➡️ `incident-response/tier1-workflow.md`

---

## Incident Documentation

A structured incident report template used throughout the lab:

➡️ **[Incident Report Template](incident-response/incident-template.md)**

The template includes:
- classification rationale
- key evidence
- timeline
- impact assessment
- escalation recommendations

---

## Key Takeaways

This project demonstrates:
- understanding of Active Directory authentication flows (TGT / TGS)
- ability to detect identity-based attacks using Windows logs
- SOC Tier-1 triage and decision-making
- structured, escalation-ready documentation

The focus is on **defensive analysis and response**, not exploitation.

---

## Disclaimer

All activities were performed in an isolated lab environment
for learning and defensive security purposes only.