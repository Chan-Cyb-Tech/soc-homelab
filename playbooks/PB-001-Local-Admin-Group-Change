# PB-001 — Local Administrator Group Change

**Type:** Tier 1 SOC alert triage playbook  
**Scenario:** A user account is created, added to the local Administrators group, or granted elevated access unexpectedly  
**Platform:** Windows  
**Status:** Portfolio playbook / lab exercise  
**Related investigation:** IR-003 (see the `incident-reports` folder)

> **Scope:** This document describes an investigation workflow. It is not evidence that each step was performed in the lab. A Tier 1 analyst should follow the organization's approved escalation and containment procedures.

## 1. Objective

Determine whether a local administrator group membership change was authorized, identify who made it and on which host, check for related activity before and after the change, preserve evidence, and escalate suspicious findings.

## 2. Detection and relevant Windows events

| Event ID | Log source | Why it matters |
|---|---|---|
| **4732** | Windows Security | A member was added to a security-enabled local group (check whether the group is **Administrators**) |
| **4720** | Windows Security | A user account was created |
| **4726** | Windows Security | A user account was deleted |
| **4733** | Windows Security | A member was removed from a security-enabled local group |
| **4688** | Windows Security, when process creation auditing is enabled | A process was created; command-line detail depends on audit settings |
| **4624** | Windows Security | A successful logon |
| **4672** | Windows Security | Special privileges were assigned to a new logon |
| **1** | Sysmon, if installed | Process creation, including image, command line, parent process, and user |

**Note:** Event availability depends on the host, audit policy, and Sysmon configuration. Event 4732 alone does not prove malicious intent.

## 3. Initial triage checklist

1. Record the alert time, hostname, source log, and event record ID.
2. Review Event **4732**. Confirm that the affected group is the local **Administrators** group.
3. Capture the **subject (actor)**, **member added (target)**, group name/SID, and host. Resolve SIDs only when supporting data is available.
4. Check for a matching change ticket, approved administrator request, or maintenance window.
5. Look for nearby **4720**, **4726**, **4733**, **4624**, **4672**, and process creation events.
6. Label observations as **confirmed**, **possible**, or **not available**; never infer missing logs.

## 4. Decision branches (Tier 1)

### Branch A — Expected / authorized change

Use this branch **only if** the change matches a known ticket, approved change window, or confirmed administrative action.

- Validate the actor and target account against the authorization.
- Confirm the timestamp, hostname, and scope are consistent with the request.
- Check for unexpected follow-on activity even if the change was authorized.
- Document the validating evidence and close as authorized **only according to local procedure**.
- If any detail does not match, switch to **Branch B**.

### Branch B — Suspicious or unverified change

Use this branch if authorization cannot be established, the actor is unexpected, the account is unfamiliar, or activity after the change appears anomalous.

- Identify the actor, target account, local group, host, and time of change.
- Determine whether the target account was newly created or already existed.
- Trace the process or tool used to make the change, where logging allows.
- Examine subsequent successful logons, remote access, administrative commands, and persistence activity.
- Preserve supporting log entries and escalate to Tier 2 or the incident responder.
- Do **not** remove accounts, isolate hosts, or reset credentials without authorization under the incident response process.

### Branch C — Insufficient evidence

- Record which logs, event fields, approvals, or systems are unavailable.
- Request the missing information from the relevant owner or escalate for additional visibility.
- Keep the disposition **unverified** rather than calling it benign or malicious.

## 5. Trace the change — who and how

- **Who:** Compare the **Subject** account in 4732 with known administrators and approved change records.
- **Whom:** Identify the member added and whether there is a corresponding **4720** account-creation event.
- **Where/when:** Record hostname and timestamps, using a consistent time zone.
- **How:** Review **4688** or **Sysmon Event ID 1** for process image, command line, parent process, and user. Relevant tools may include administrative consoles, `net localgroup`, PowerShell, or management software; tool presence by itself is not proof of misuse.
- **Context:** Correlate activity by host, user, and time; avoid attributing a process to a change without supporting evidence.

## 6. Intent checks — what happened after access was granted?

Review the period immediately before and after the group change for:

- **Successful logons (4624):** interactive (type **2**), network (type **3**), unlock (type **7**), or Remote Desktop (type **10**) as applicable.
- **Privileged logons (4672):** whether elevated access was subsequently used.
- **Process execution:** unexpected PowerShell commands, encoded/obfuscated commands, or suspicious parent-child process relationships in **4688** or **Sysmon 1**.
- **Persistence:** new scheduled tasks, services, startup mechanisms, or repeated additions to privileged groups, if logs are available.
- **Account lifecycle:** a sequence such as **4720 → 4732 → 4726** may merit closer examination, but does not establish malicious intent on its own.

For every observation, capture its timestamp, source, account, host, and supporting event ID.

## 7. Escalation criteria

Escalate according to organization policy when any of the following occurs:

- There is no verifiable authorization for a privileged group change.
- An unexpected account or actor has been granted local administrator rights.
- There are suspicious logons, remote sessions, scripts, or persistence indicators after the change.
- Multiple hosts or privileged groups are affected.
- The event affects a sensitive or critical host, including a system supporting operational technology.
- Available evidence is insufficient to rule out unauthorized access and further investigation is required.

**OT/ICS caution:** In critical infrastructure, host isolation and other containment actions may affect availability and safety. Escalate to the appropriate security and operations owners before acting.

## 8. Escalation package (send to Tier 2)

- **Alert summary:** what was detected and why it needs review.
- **Timeline:** event timestamps with time zone.
- **Host and identities:** hostname, actor, added account/member, affected group, SIDs if available.
- **Key event references:** 4732, 4720, 4726, 4733, 4624, 4672, 4688, Sysmon 1 as applicable.
- **Process context:** image, command line, parent process, user, and available related artifacts.
- **Authorization check:** ticket/maintenance window found, not found, or unavailable.
- **Follow-on activity:** logons, remote access, scripts, persistence, and scope.
- **Assessment:** confirmed facts, unresolved questions, proposed severity with reasoning.
- **Actions taken:** evidence preservation, notifications, and any approved response actions.

## 9. Documentation and closure

Record the final disposition (**authorized**, **suspicious/escalated**, or **unverified**), the evidence supporting that decision, who reviewed the case, and any follow-up actions. Do not mark the case resolved until required review and documentation are complete.

---

*Portfolio note: This is a Tier 1 workflow written for a Windows SOC homelab and intended to demonstrate analytical reasoning, contextual triage, decision points, and escalation discipline.*
