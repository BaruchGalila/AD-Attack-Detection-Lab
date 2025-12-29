# Active Directory Attack & Detection Lab (SOC Tier-1)

## What this is
A hands-on lab project that focuses on **identity and authentication attacks** in an Active Directory environment,
and documents them from a **SOC Tier-1** perspective: **detection, triage, correlation, and escalation**.

This repository is intentionally written to help a recruiter / SOC lead understand quickly:
- what the lab contains
- what attack patterns were simulated
- what Windows logs to look at
- how to decide **Noise vs Exposure vs Incident**
- what actions a Tier-1 analyst should take before escalating

## Lab Environment
- **Domain Controller:** Windows Server (AD DS + DNS + Kerberos / KDC)
- **Client:** Windows 10 (domain-joined workstation)
- **Attacker:** Kali Linux (used only inside the isolated lab)

## Project Structure
- lab-setup/  
  Lab architecture and environment notes.

- Attack-scenarios/  
  Each scenario is written as: **Goal → What it looks like → Key logs → Tier-1 triage → Actions → Escalation notes**.

- detection/  
  Correlation guides: how to connect event IDs into a meaningful timeline.

- incident-response/  
  Tier-1 playbook style workflow + a clean incident note template.

## Scenarios Included
- Password Spraying
- Pass-the-Hash
- Pass-the-Ticket
- Golden/Silver Ticket (SOC detection-focused notes)

## Core Windows Event IDs Used
Authentication / Kerberos:
- **4624** Successful logon
- **4625** Failed logon
- **4768** Kerberos TGT request (AS)
- **4769** Kerberos TGS request (service ticket)
- **4771** Kerberos pre-auth failed (often symptom / user impact)

Privilege / persistence indicators:
- **4672** Special privileges assigned
- **4673** Sensitive privilege use
- **7045** New service installed (common persistence pattern)

## Tier-1 Decision Model (short)
- **Noise:** expected behavior, low risk, no suspicious pattern
- **Exposure:** suspicious pattern but **no confirmed compromise**
- **Incident:** confirmed compromise indicators, clear attacker progress, or high-impact access

## How to use this repo
1. Pick a scenario in Attack-scenarios/
2. Use the detection/ guides to correlate events into a timeline
3. Apply the Tier-1 workflow under incident-response/
4. Document the incident note using the template

