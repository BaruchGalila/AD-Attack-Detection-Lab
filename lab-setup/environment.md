# Lab Setup – Environment Notes

## Machines
1) Domain Controller (Windows Server)
- Role: AD DS + DNS (AD-integrated zones), Kerberos KDC
- Purpose: Authentication issuance (TGT/TGS), directory services, SYSVOL/GPO distribution

2) Windows 10 Client (Domain Joined)
- Purpose: Normal user workstation and a target for authentication events

3) Kali Linux (Attacker – Lab only)
- Purpose: simulate attacker behavior inside isolated lab network

## Domain
- Domain name: lab.loc
- Typical admin used in lab: LAB\Administrator
- Important dependency chain:
  DNS → SRV records → DC discovery → Kerberos (TGT/TGS) → logons / tickets

## What “success” looks like for this project
You should be able to:
- Read Windows security events and Kerberos events
- Identify patterns for identity abuse
- Build a short timeline (start → movement → impact)
- Decide Noise/Exposure/Incident
- Escalate with a structured, short incident note
