# SOC Home Lab (VirtualBox) — Windows Telemetry & Tier 1 Triage

## What this is
A beginner SOC lab built in VirtualBox to practice:
- Windows Security Event Log analysis (4625 failed logons)
- Sysmon endpoint telemetry (process creation)
- Writing a Tier 1 incident report with evidence and remediation

## Lab setup
- Host: Windows 10
- Guest: Windows 10 VM (endpoint)
- Telemetry: Windows Security logs + Sysmon (Sysmon/Operational)

## Evidence (screenshots)
**Failed logons (Security Event ID 4625):**
![Failed logons](docs/04-security-4625-failed-logons.PNG)

**Triage proof (Sysmon EID 1 showing Event Viewer opened via mmc.exe / eventvwr.msc):**
![Sysmon triage proof](docs/05-sysmon-triage-proof.PNG)

**Suspicious PowerShell (Sysmon EID 1 – EncodedCommand):**
![Suspicious PowerShell](docs/06-sysmon-powershell-encodedcommand.PNG)


## Incident reports
- [IR-001: Failed Logons (4625)](incident-reports/IR-001-Failed-Logons.md)
- [IR-002: Suspicious PowerShell (Sysmon EID 1)](incident-reports/IR-002-Suspicious-PowerShell.md)

## What I learned
- How to generate and validate authentication-related events in Windows
- How to confirm investigator actions using Sysmon telemetry
- How to document triage findings like a SOC Tier 1 analyst

## Notes
All data is from a controlled lab environment (no real user/client data).





